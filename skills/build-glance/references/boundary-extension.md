# Boundary Extension

Source:
- `glance/docs/configuration.md#extension`
- `glanceapp/community-widgets/CONTRIBUTING.md`

Checked against upstream on 2026-03-20.

## Use extension only when custom-api is no longer the right tool

Choose `extension` when:
- the widget needs real server-side aggregation or transformation
- the output is trusted HTML emitted by a service
- the service already exists and is the natural owner of the rendering
- the `custom-api` path would become awkward, unsafe, or misleading

Always explain why `custom-api` is insufficient.

## Core extension protocol

An extension is an HTTP endpoint that returns HTML plus special response headers.

Relevant headers:
- `Widget-Title`
- `Widget-Title-URL`
- `Widget-Content-Type`
- `Widget-Content-Frameless`

Important details:
- the only supported extension content type is `html`
- if `Widget-Content-Type` is missing or invalid, Glance can fall back to `fallback-content-type`
- `allow-potentially-dangerous-html: true` is a trust boundary, not a casual convenience flag

## Security rules

- never enable `allow-potentially-dangerous-html` without saying why the source is trusted
- never treat arbitrary third-party HTML as safe
- prefer `custom-api` whenever the same result can be achieved with fetched data plus Glance template logic

## Query behavior

Just like `custom-api`, extension `parameters` rebuild the query string.
Do not rely on query params already embedded in `url` if you also set `parameters`.

## Development guidance

- the extension widget defaults to a `30m` cache
- during development, a very low cache such as `1s` can be reasonable
- mention the maintenance burden because upstream documents the extension API as work in progress

## Community packaging difference

Community `extension` widgets do not follow the same packaging path as community `custom-api` widgets.

The official contribution path is:
- add the widget to `widgets/extensions.yml`

Do not package a community extension as if it should live in its own `widgets/<name>/` directory with `README.md`, `preview.png`, and `meta.yml` unless the user explicitly wants extra docs for their own repo.

## Quick rejection test

Stay with `custom-api` if all of the following are true:
- Glance can fetch the data directly
- the response is JSON or similar structured data
- template logic and utility classes are enough
- the only reason to add a service would be convenience
