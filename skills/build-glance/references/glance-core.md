# Glance Core

## Top-level structure

Glance is configured with YAML. The core top-level entry for dashboard content is `pages`.

Typical shape:

```yaml
pages:
  - name: Home
    columns:
      - size: small
        widgets: ...
      - size: full
        widgets: ...
```

Related top-level configuration also exists for:
- `server`
- `document`
- `branding`
- `theme`

Use those only when the task clearly calls for them.

## Pages

Important page properties:
- `name`
- `slug`
- `width`: `default`, `slim`, `wide`
- `desktop-navigation-width`
- `center-vertically`
- `hide-desktop-navigation`
- `show-mobile-header`
- `head-widgets`
- `columns`

Constraints:
- a page contains up to 3 columns
- a page must have 1 or 2 `full` columns
- `slim` pages may only have up to 2 columns

Practical guidance:
- use `slim` for focused startpages and search-first pages
- use `wide` only when the content density actually needs it
- use `head-widgets` sparingly for broad, visually horizontal widgets such as markets, videos, or RSS card layouts

## Columns

Column sizes:
- `small`: fixed 300px
- `full`: consumes remaining width

Common valid patterns:
- `small` + `full` + `small`
- `full` + `small`
- `full` + `full`

## Shared widget properties

Every widget can use these shared fields:
- `type`
- `title`
- `title-url`
- `hide-header`
- `cache`
- `css-class`

Notes:
- `hide-header` removes the visible title area; it also hides the built-in error indicator
- `cache` uses duration strings like `30s`, `5m`, `2h`, `1d`
- `css-class` is instance-specific and useful when custom CSS is unavoidable

## Includes and env vars

Use environment variables for tokens, URLs, and secrets:

```yaml
headers:
  x-api-key: ${IMMICH_API_KEY}
```

Use `$include` when the result would otherwise become too large or error-prone to indent inline:

```yaml
widgets:
  - $include: widgets/my-widget.yml
```

## Theme and custom CSS

Use theme-aware Glance classes before recommending custom CSS.

Useful facts:
- upstream exposes theme presets and a theme picker
- custom CSS can be attached with `theme.custom-css-file`
- each widget gets a `widget-type-{name}` class

Do not default to custom CSS unless the task truly needs it.

## Operational behavior

Useful runtime behavior to remember:
- invalid config on startup exits with an error
- invalid config after a successful run leaves the previous config active
- frequent reloads clear cache and may trigger upstream API rate limits

