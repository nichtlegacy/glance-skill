# Custom API Core

Source:
- `glance/docs/custom-api.md`
- `glance/docs/configuration.md#custom-api`

Checked against upstream on 2026-03-24.

## Role Of `custom-api`

`custom-api` is the main custom path after the native-first check, not before it.

Choose it when:
- built-ins and page composition are not enough
- Glance can fetch the source directly over HTTP
- the response can still be rendered with Go templates plus Glance-native markup
- adding a backend would mostly exist to reshape already-usable data

Do not use `custom-api` just because it feels flexible. If a built-in widget, `group`, `split-column`, `head-widgets`, or grouped `bookmarks` already solve the request, prefer the native path.

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
- `parameters`
  Query-string builder that overrides query params already present in `url`.
- `subrequests`
  Additional fixed supporting requests available through `.Subrequest`.
- `frameless`
  Remove the outer Glance frame only when the inner markup intentionally rebuilds the shell.
- `allow-insecure`
  Only for self-signed or otherwise invalid certificates.
- `skip-json-validation`
  Only when the response is still template-usable but not a single normal JSON document.
- `template`
  Required. This is the actual widget view.
- `options`
  The preferred place for reusable knobs and density toggles.

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
- indexed paths such as `users.0.name` work
- arrays of primitives use an empty string key such as `.JSON.Array ""` and `.String ""`

## Response Object

The top-level response is available through `.Response`.

Useful fields:
- `.Response.StatusCode`
- `.Response.Status`
- `.Response.Header.Get "Content-Type"`

Use this when the API can fail, rate limit, or return empty-but-successful responses.

Pattern:

```gotemplate
{{ if eq .Response.StatusCode 200 }}
  ...
{{ else }}
  Failed to fetch data: {{ .Response.Status }}
{{ end }}
```

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
- `.Options.JSON`

Pattern:

```gotemplate
{{ $small := .Options.BoolOr "small-column" false }}
<ul class="list {{ if $small }}list-gap-8{{ else }}list-gap-10{{ end }} collapsible-container" data-collapse-after="{{ .Options.IntOr "collapse-after" 5 }}">
```

Prefer one adaptable template over separate small/full files unless the layouts are genuinely different products.

## `subrequests` Versus Template-Side Requests

Choose the mechanism deliberately:

- `subrequests`
  Best when the widget needs one or two fixed, related supporting calls known in advance.
- `newRequest ... | getResponse`
  Best when the second request depends on data from the first response, or the request URL/parameters must be computed at render time.

Do not use either path to rebuild a backend in YAML.

## Dynamic Requests Inside The Template

`glance/docs/custom-api.md` explicitly supports making additional HTTP requests from inside the template.

Available helpers:
- `newRequest`
- `withParameter`
- `withHeader`
- `getResponse`

Good uses:
- fetch an ID first, then request the detail endpoint
- compute a time window dynamically with `offsetNow`
- add optional auth or query parameters only when needed

Pattern:

```gotemplate
{{ $id := .JSON.String "id" }}
{{ $detail := newRequest (concat (.Options.StringOr "base-url" "") "/items/" $id) | withHeader "Authorization" (concat "Bearer " (.Options.StringOr "token" "")) | getResponse }}

{{ if eq $detail.Response.StatusCode 200 }}
  {{ $detail.JSON.String "title" }}
{{ else }}
  Failed to fetch detail: {{ $detail.Response.Status }}
{{ end }}
```

Important details:
- you must manually check the status code of template-side requests
- template-side requests are strongest when the chain is short and obvious
- if the logic becomes a multi-step pipeline, re-check whether the service should expose a cleaner API instead

You can also omit `url` entirely and do the whole request from the template when the endpoint needs dynamic parameters.

## JSON Lines And `skip-json-validation`

`custom-api.md` documents JSON Lines / NDJSON support.

Use this path when:
- the response is not a single valid JSON document
- each line is still valid JSON
- the result is easier to consume as JSON Lines than to proxy through another service

Requirements:
- set `skip-json-validation: true`
- iterate through `.JSONLines` or query the parsed data with `gjson`

Pattern:

```gotemplate
{{ range .JSONLines }}
  {{ .String "name" }} is {{ .Int "age" }} years old
{{ end }}
```

Do not use `skip-json-validation` as a generic error-suppression switch.

## Helper Functions Worth Remembering

These are easy wins that often avoid a companion backend:

### Time and date

- `now`
- `offsetNow`
- `duration`
- `parseTime`
- `parseLocalTime`
- `formatTime`
- `parseRelativeTime`
- `toRelativeTime`
- `startOfDay`
- `endOfDay`

### Math and numbers

- `add`
- `sub`
- `mul`
- `div`
- `mod`
- `toFloat`
- `toInt`
- `formatApproxNumber`
- `formatNumber`
- `percentChange`

### String cleanup

- `trimPrefix`
- `trimSuffix`
- `trimSpace`
- `replaceAll`
- `replaceMatches`
- `findMatch`
- `findSubmatch`
- `concat`

### Sorting and uniqueness

- `sortByString`
- `sortByInt`
- `sortByFloat`
- `sortByTime`
- `unique`

If one of these helpers solves the transformation cleanly, prefer it over inventing extra services.

## Parameters And Query Override Gotcha

When you set `parameters`, Glance rebuilds the final query string.

That means:
- query params embedded in `url` are overridden
- you should prefer `parameters` when the upstream API depends on query arguments

## Practical Defaults

- Prefer env vars for URLs and tokens.
- Add `Accept: application/json` when the API is inconsistent.
- Choose cache by volatility, not aesthetics.
- Keep the template readable after one pass.
- Use native disclosure and structure helpers before building custom interaction.
- If the widget wants trusted external HTML or nontrivial server-side logic, re-check the extension boundary.
