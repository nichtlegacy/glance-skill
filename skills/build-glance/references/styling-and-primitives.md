# Styling And Primitives

Source:
- `glance/docs/configuration.md`
- representative upstream and local widget patterns

Checked against upstream and local patterns on 2026-03-22.

## First Principle

Make the widget feel like Glance, not like an embedded mini app.

Prefer:
- Glance utility classes
- Glance spacing rhythm
- theme-aware color classes and variables
- simple list, card, and frame structures
- native disclosure primitives

Avoid:
- borrowed internal CSS classes from unrelated built-in widgets
- hardcoded colors unless explicitly required
- large bespoke wrapper hierarchies
- ornamental layout that ignores column width
- inventing JS-like interaction when markup primitives already exist

## Native Disclosure Primitives

Use these before inventing custom behavior:

- `data-popover-type="text"`
- `data-popover-type="html"`
- `data-popover-html`
- `data-popover-position`
- `data-popover-show-delay`
- `data-popover-max-width`
- `<details>`
- `collapsible-container`
- `data-collapse-after`
- `data-collapse-after-rows`
- `data-dynamic-relative-time`

General rule:
- hover detail or tooltip-like context -> popover
- explicit expand/collapse -> `<details>`
- long lists -> collapse helpers
- “updated x ago” style metadata -> relative-time helper

## Stable Utility Classes Worth Reusing

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

## Theme-Aware Values

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

## Width-Specific Guidance

### For `small`

Prefer:
- compact stats
- short vertical lists
- concise metadata
- restrained line length

Avoid:
- large card grids
- tall decorative hero areas
- heavy always-visible detail blocks
- multiple layout systems nested just to align a simple stat surface

### For `full`

Prefer:
- richer list layouts
- cards or thumbnail-led layouts
- grouped metadata rows
- clearer use of width

Avoid:
- stretching a tiny stats pattern until it feels empty
- card density that collapses badly on mobile
- using width for decoration rather than information

## Inline CSS And `<style>` Rules

Inline CSS is acceptable when:
- utility classes cannot express a small local need
- the adjustment is tightly scoped
- the markup still reads as Glance-native

A widget-local `<style>` block is acceptable when:
- the same local class is reused several times inside one widget
- utility classes alone would become more awkward than a tiny scoped stylesheet
- the class names are widget-local and not borrowed from built-ins

It becomes a problem when:
- the CSS acts like a separate design system
- most layout and spacing no longer come from native classes
- small/full differences are implemented as separate CSS-heavy templates instead of options

## Practical Do And Don't

Do:
- use `frameless: true` only when inner markup intentionally rebuilds the shell
- use `list-horizontal-text` for lightweight metadata
- use `cards-grid` only when the content benefits from width
- expose density changes through `options` for reusable widgets
- push secondary details into popovers or disclosure instead of always-visible clutter

Do not:
- use classes from other widgets just because they look right today
- create a fake Tailwind-like system inside the template
- hardcode local brand colors when semantic colors would work
- preserve legacy style-heavy patterns as your default output
