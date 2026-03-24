# Boundary Extension

Source:
- `glance/docs/extensions.md`
- `glance/docs/configuration.md#extension`
- `glanceapp/community-widgets/CONTRIBUTING.md`

Checked against upstream on 2026-03-24.

## Use Extension Only When `custom-api` Is No Longer The Right Tool

Choose `extension` when:
- the widget needs real server-side aggregation or transformation
- the output is trusted HTML emitted by a service
- the service already exists and is the natural owner of the rendering
- the `custom-api` path would become awkward, unsafe, or misleading

Always explain why `custom-api` is insufficient.

## Upstream Status And Ownership

`glance/docs/extensions.md` explicitly says:
- the feature is still a work in progress
- the API may change in the future
- extension authors are responsible for maintaining their own extensions

Implication:
- do not recommend `extension` as a convenience shortcut
- mention the maintenance burden directly
- prefer `custom-api` if structured data plus native Glance markup are enough

## Core Extension Protocol

An extension is an HTTP endpoint that returns content plus special response headers.

Relevant headers:
- `Widget-Title`
- `Widget-Title-URL`
- `Widget-Content-Type`
- `Widget-Content-Frameless`

Important details:
- if `Widget-Title` is missing, Glance uses `Extension`
- if the user config sets `title-url`, it takes precedence over `Widget-Title-URL`
- currently the only supported extension content type is `html`
- if `Widget-Content-Type` is missing, the content is shown as plain text
- if `Widget-Content-Frameless` is `true`, the content is shown without the default widget frame

## HTML Trust Boundary

For HTML rendering, the user must set:

```yaml
allow-potentially-dangerous-html: true
```

If they do not, the HTML is shown as plain text.

This is a trust boundary, not a casual convenience flag.

Rules:
- never enable it without saying why the source is trusted
- never treat arbitrary third-party HTML as safe
- prefer `custom-api` whenever the same result can be achieved with fetched data plus Glance template logic

## Reusing Existing Classes And Features

Upstream explicitly encourages extension authors to reuse Glance classes and features such as:
- semantic color classes
- typography utilities
- visited-indicator links
- relative-time attributes
- list and collapsible patterns
- lazy-loaded images

But the same docs also warn that class names or features may change.

Implication:
- reuse Glance classes only when you accept the maintenance coupling
- prefer generic utility classes and primitives over obscure widget-specific selectors
- do not promise long-term API or CSS stability for extension HTML

## Query Behavior

Just like `custom-api`, extension `parameters` rebuild the query string.
Do not rely on query params already embedded in `url` if you also set `parameters`.

## Development Guidance

The extension widget defaults to a `30m` cache.

During development:
- a very low cache such as `1s` is reasonable
- this is the documented way to avoid restarting Glance after every extension change

In normal use:
- choose cache based on the volatility of the rendered HTML
- do not pin it to `1s` unless the widget genuinely needs near-live refresh

## Community Packaging Difference

Community `extension` widgets do not follow the same packaging path as community `custom-api` widgets.

The official contribution path is:
- add the widget to `widgets/extensions.yml`

Do not package a community extension as if it should live in its own `widgets/<name>/` directory with `README.md`, `preview.png`, and `meta.yml` unless the user explicitly wants extra docs for their own repo.

## Quick Rejection Test

Stay with `custom-api` if all of the following are true:
- Glance can fetch the data directly
- the response is JSON or similar structured data
- template logic and utility classes are enough
- the only reason to add a service would be convenience
