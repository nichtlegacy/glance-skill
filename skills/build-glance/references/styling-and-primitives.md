# Styling And Primitives

Source:
- `glance/docs/configuration.md`
- `community-widgets/CONTRIBUTING.md`

Checked against upstream on 2026-03-20.

## First principle

Make the widget feel like Glance, not like an embedded mini app.

Prefer:
- Glance utility classes
- Glance spacing rhythm
- theme-aware color classes and variables
- simple list, card, and frame structures

Avoid:
- borrowed internal CSS classes from unrelated built-in widgets
- hardcoded colors unless explicitly required
- large bespoke wrapper hierarchies
- ornamental layout that ignores column width

## Stable utility classes worth reusing

Typography and links:
- `size-title-dynamic`
- `size-h3`, `size-h4`, `size-h5`, `size-h6`
- `uppercase`
- `text-truncate`
- `text-truncate-2-lines`
- `text-truncate-3-lines`
- `color-primary-if-not-visited`
- `visited-indicator`

Layout and spacing:
- `flex`
- `flex-column`
- `items-center`
- `justify-between`
- `grow`
- `shrink-0`
- `min-width-0`
- `gap-8`, `gap-10`, `gap-12`, `gap-15`, `gap-20`
- `margin-top-7`
- `margin-top-10`
- `margin-bottom-10`
- `margin-bottom-widget`
- `padding-inline-widget`

List and card structures:
- `list`
- `list-gap-10`
- `list-horizontal-text`
- `list-with-separator`
- `dynamic-columns`
- `cards-grid`
- `cards-horizontal`
- `cards-vertical`
- `card`
- `widget-content-frame`
- `widget-small-content-bounds`

Behavior helpers:
- `collapsible-container`
- `data-collapse-after`
- `data-collapse-after-rows`
- `data-dynamic-relative-time`

## Theme-aware values

Prefer these before fixed colors:
- `color-primary`
- `color-positive`
- `color-negative`
- `color-highlight`
- `color-subdue`
- `var(--color-primary)`
- `var(--color-widget-background)`
- `var(--color-widget-background-highlight)`
- `var(--color-text-base)`
- `var(--color-text-highlight)`
- `var(--color-text-subdue)`
- `var(--border-radius)`

## Width-specific guidance

### For `small`

Prefer:
- compact stats
- short vertical lists
- concise metadata
- restrained line length

Avoid:
- large card grids
- tall decorative hero areas
- three layers of nested wrappers just to align a stat block

### For `full`

Prefer:
- richer list layouts
- cards or thumbnail-led layouts
- grouped metadata rows
- clearer use of width

Avoid:
- stretching a tiny stats pattern until it feels empty
- card density that collapses badly on mobile

## Custom CSS rules

Use custom CSS only when utility classes and local inline styles are not enough.

If custom classes are necessary:
- suffix them with the widget name
- keep them widget-local
- do not assume internal Glance widget class names are stable

Community contribution guidance explicitly prefers applying necessary style directly to elements rather than depending on widget-specific CSS from built-in widgets.

## Practical do and don't

Do:
- use `frameless: true` only when inner markup intentionally rebuilds the shell
- use `list-horizontal-text` for lightweight metadata
- use `cards-grid` only when the content benefits from width
- expose density changes through `options` for reusable widgets

Do not:
- use classes from other widgets just because they look right today
- create a fake Tailwind-like system inside the template
- hardcode local brand colors when semantic colors would work
