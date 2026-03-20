# Custom API Core

Source:
- `glance/docs/configuration.md#custom-api`
- `glance/docs/custom-api.md`

Checked against upstream on 2026-03-20.

## Default stance

Use `custom-api` first for custom widgets.

Stay on this path when:
- Glance can fetch the data directly
- the response is JSON or still workable with request options
- the output can be expressed with template logic and Glance-native HTML/CSS
- a companion service would only exist to reshape already-usable data

## Request fields that matter most

- `url`
  The upstream endpoint. It must be reachable from the machine where Glance runs.
- `headers`
  Use for API keys, auth headers, and explicit `Accept` headers.
- `method`
  Supports `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS`, and `HEAD`.
- `body-type`
  `json` or `string`. The default is `json`.
- `body`
  Use a YAML map for JSON bodies or a multi-line string for raw payloads.
- `frameless`
  Remove the outer Glance frame when the inner markup intentionally rebuilds the visual shell.
- `allow-insecure`
  Only for self-signed or otherwise invalid certificates.
- `skip-json-validation`
  Only when the response is still parseable by the template layer but fails normal JSON validation, such as JSON Lines.
- `template`
  Required. This is the actual widget view.
- `options`
  Preferred place for reusable knobs such as density, collapse thresholds, and small/full display mode.
- `parameters`
  Query-string builder. Remember that it overrides query params already present in `url`.
- `subrequests`
  Additional concurrent requests available through `.Subrequest`.

## Template mental model

Glance uses Go `html/template` plus `gjson`.

Common access patterns:

```gotemplate
{{ .JSON.String "title" }}
{{ .JSON.Int "stats.movies" }}
{{ if .JSON.Exists "user.age" }}...{{ end }}

{{ range .JSON.Array "items" }}
  {{ .String "name" }}
{{ end }}
```

Important details:
- inside `range`, the context becomes the current array element
- use `$` to access the original top-level context from inside loops
- dot notation works for nested paths such as `user.address.city`
- indexed paths such as `items.0.name` work
- arrays of primitives use an empty string key: `.JSON.Array ""`, `.String ""`

## Options are the reusable path

Prefer `options` over editing the template for likely variations.

Use them for:
- `collapse-after`
- `show-thumbnails`
- `compact`
- `small-column`
- style variants

Available helpers:
- `.Options.StringOr`
- `.Options.IntOr`
- `.Options.BoolOr`
- `.Options.FloatOr`

Pattern:

```gotemplate
<ul class="list list-gap-10 collapsible-container" data-collapse-after="{{ .Options.IntOr "collapse-after" 5 }}">
```

If the same widget will be reused multiple times, combine `options` with YAML anchors.

## Subrequests

Use subrequests when one widget needs a small number of related API calls that still belong to one coherent widget.

Good uses:
- one main list plus a tiny summary endpoint
- one status call plus one supporting metadata call

Bad uses:
- rebuilding a mini backend
- compensating for a response that should really be pre-aggregated elsewhere

Pattern:

```gotemplate
{{ $stats := .Subrequest "stats" }}
{{ $stats.JSON.Int "count" }}
{{ $stats.Response.StatusCode }}
```

Subrequests support the same fields as the main request, except `subrequests` itself.

## Parameters and query override gotcha

When you set `parameters`, Glance rebuilds the final query string.

That means:
- query params already embedded in `url` are overridden
- if the upstream API depends on query params, prefer putting them in `parameters`

## Skip JSON validation only with intent

Use `skip-json-validation: true` only when:
- the response is not a single valid JSON document
- the response is still usable by the Glance template layer
- you mention why this flag is needed

Do not use it as a general “make errors go away” switch.

## Practical defaults

- Prefer env vars for URLs and tokens
- Add `Accept: application/json` when the API is inconsistent
- Pick cache duration based on data volatility, not aesthetics
- Keep the template readable after one pass
- If the widget feels like it wants arbitrary HTML or server-side aggregation, re-check the extension boundary
