# Custom API Core

Source:
- `glance/docs/configuration.md#custom-api`
- `glance/docs/custom-api.md`

Checked against upstream on 2026-03-22.

## Role Of `custom-api`

`custom-api` is the primary custom path after the native-first check, not before it.

Choose it when:
- built-ins and page composition are not enough
- Glance can fetch the source directly
- the data can be rendered with the template layer and native HTML/CSS primitives
- adding a backend would only exist to reshape already-usable data

Do not use `custom-api` just because it feels flexible. If a built-in widget or composition pattern already solves the request, prefer the native path.

## Request Fields That Matter Most

- `url`
  The upstream endpoint. It must be reachable from the machine where Glance runs.
- `headers`
  Use for auth, tokens, and explicit `Accept` headers.
- `method`
  Supports `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS`, and `HEAD`.
- `body-type`
  `json` or `string`. Default is `json`.
- `body`
  YAML map for JSON bodies or a raw string payload.
- `frameless`
  Remove the outer Glance frame only when the inner markup intentionally rebuilds the structure.
- `allow-insecure`
  Only for self-signed or otherwise invalid certificates.
- `skip-json-validation`
  Only when the response is still template-usable but not a single normal JSON document.
- `template`
  Required. This is the actual widget view.
- `options`
  The preferred place for reusable knobs and density toggles.
- `parameters`
  Query-string builder that overrides query params already present in `url`.
- `subrequests`
  Additional concurrent requests available through `.Subrequest`.

## Template Mental Model

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

## Options Are The Reusable Path

Prefer `options` over editing the template for likely variations.

Use them for:
- `collapse-after`
- `show-*` toggles
- `small-column`
- `compact`
- semantic `layout` or `style` choices when really needed

Available helpers:
- `.Options.StringOr`
- `.Options.IntOr`
- `.Options.BoolOr`
- `.Options.FloatOr`

Pattern:

```gotemplate
{{ $small := .Options.BoolOr "small-column" false }}
<ul class="list {{ if $small }}list-gap-8{{ else }}list-gap-10{{ end }} collapsible-container" data-collapse-after="{{ .Options.IntOr "collapse-after" 5 }}">
```

Prefer one adaptable template over separate small/full files unless the layouts are genuinely different products.

## `subrequests`

Use subrequests when one widget needs a small number of related supporting calls that still belong to one coherent widget.

Good uses:
- one main list plus one small supporting stats response
- one status call plus one metadata call
- small supporting data that enriches a single surface

Bad uses:
- rebuilding a backend in YAML
- merging many endpoints with complex transformation rules
- compensating for a system that should expose a cleaner API elsewhere

Pattern:

```gotemplate
{{ $stats := .Subrequest "stats" }}
{{ $stats.JSON.Int "count" }}
{{ $stats.Response.StatusCode }}
```

## Parameters And Query Override Gotcha

When you set `parameters`, Glance rebuilds the final query string.

That means:
- query params embedded in `url` are overridden
- you should prefer `parameters` when the upstream API depends on query arguments

## Skip JSON Validation Only With Intent

Use `skip-json-validation: true` only when:
- the response is not a single valid JSON document
- the response is still usable by the template layer
- you explicitly say why the flag is needed

Do not use it as a general error-suppression switch.

## Practical Defaults

- Prefer env vars for URLs and tokens.
- Add `Accept: application/json` when the API is inconsistent.
- Choose cache by volatility, not aesthetics.
- Keep the template readable after one pass.
- Use native disclosure and structure helpers before building custom interaction.
- If the widget wants trusted external HTML or nontrivial server-side logic, re-check the extension boundary.
