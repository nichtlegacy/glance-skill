# Styling And Primitives

Source:
- `glance/docs/extensions.md`
- representative upstream templates in `glance/internal/glance/templates`
- representative upstream and local widget patterns

Checked against upstream on 2026-03-24.

## First Principle

Make the widget feel like Glance, not like an embedded mini app.

Prefer:
- Glance utility classes
- Glance spacing rhythm
- theme-aware color classes and variables
- real list, card, row, tab, and frame families already seen upstream
- native disclosure primitives

Avoid:
- borrowed widget-specific CSS classes from unrelated built-ins
- hardcoded colors unless explicitly required
- large bespoke wrapper hierarchies
- ornamental layout that ignores the target column width
- inventing JS-like interaction when markup primitives already exist

## Creative Rule: Recombine Official Families

Be creative by recombining real Glance component families, not by inventing foreign markup.

Strong upstream families to reach for:
- compact utility rows
- rich media lists
- horizontal cards in a frameless carousel
- grid cards for full-width discovery
- grouped tabs through `group`
- masonry-like wide content through `split-column`
- centered small-column utility blocks
- grouped link collections

The right question is usually:
- which upstream family already fits this surface?
- what density should it have in `small` versus `full`?
- what secondary information can move into popovers or disclosure?

## Shell Conventions From Real Templates

### Default shell

The normal widget shell is provided by `widget-base.html`:
- title and optional `title-url`
- notice and error indicators in the header
- a default error body when no content is available

Implication:
- if you can stay inside the default framed shell, do it
- `hide-header` also removes the normal header affordances, so do not hide it casually

### Frameless convention

Upstream uses a consistent pattern for richer layouts:
- set `frameless: true`
- add `widget-content-frameless`
- rebuild the inner structure with one or more `widget-content-frame` blocks

Use this when the widget needs:
- a carousel
- a card grid
- a tab strip or multi-frame composition
- several visually distinct inner cards inside one widget

Do not use `frameless: true` just to avoid learning the normal shell.

### Centered utility bounds

Use `widget-small-content-bounds` for compact, centered utility widgets in `small` columns.

This is a strong fit for:
- weather-like summaries
- concise status blocks
- small hero metrics with restrained supporting context

## Native Disclosure Primitives

Use these before inventing custom behavior:

- `data-popover-type="text"`
- `data-popover-type="html"`
- `data-popover-html`
- `data-popover-position`
- `data-popover-show-delay`
- `data-popover-max-width`
- `data-popover-margin`
- `<details>`
- `collapsible-container`
- `data-collapse-after`
- `data-collapse-after-rows`
- `data-dynamic-relative-time`

General rule:
- hover detail or tooltip-like context -> popover
- explicit expand/collapse -> `<details>`
- long lists -> collapse helpers
- "updated x ago" style metadata -> relative-time helper

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
- `items-start`
- `justify-between`
- `justify-center`
- `grow`
- `shrink-0`
- `min-width-0`
- `gap-8`, `gap-10`, `gap-12`, `gap-15`, `gap-20`
- `margin-top-5`, `margin-top-7`, `margin-top-10`, `margin-top-15`
- `margin-bottom-10`
- `margin-bottom-auto`
- `margin-bottom-widget`
- `padding-widget`
- `padding-inline-widget`

List and card structures:
- `list`
- `list-gap-8`, `list-gap-10`, `list-gap-14`, `list-gap-20`, `list-gap-24`
- `list-horizontal-text`
- `list-with-separator`
- `dynamic-columns`
- `cards-grid`
- `cards-horizontal`
- `cards-vertical`
- `card`
- `widget-content-frame`
- `widget-content-frameless`
- `widget-small-content-bounds`
- `carousel-container`
- `collapsible-container`
- `thumbnail-parent`
- `thumbnail`
- `attachments`

## Proven Layout Families

### Compact rows

Good for monitors, recent items, and narrow-column utilities.

Recurring structure:
- leading icon or thumbnail
- grow text block
- one metadata row with `list-horizontal-text`
- one trailing status, count, or small metric

### Rich lists

Good for `full` columns when the content is feed-like but should stay vertical.

Recurring structure:
- optional thumbnail
- stronger title line
- one lightweight metadata row
- optional description or tags underneath

### Horizontal cards

Good for `full` columns and `head-widgets` when content is visual or discovery-oriented.

Recurring structure:
- frameless outer widget
- `carousel-container`
- `cards-horizontal`
- each item wrapped in `card widget-content-frame`

### Grid cards

Good for full-width media or discovery surfaces where width materially improves scanning.

Recurring structure:
- frameless outer widget
- `cards-grid`
- `data-collapse-after-rows` when the grid can grow long

### Grouped links

Good for startpages and utility navigation.

Recurring structure:
- title block per group
- short list of links with optional icons
- optional one-line description kept secondary

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
- centered utility blocks
- restrained line length

Avoid:
- large card grids
- tall decorative hero areas
- heavy always-visible detail blocks
- multiple nested layout systems just to align a simple stat surface

### For `full`

Prefer:
- richer list layouts
- cards or thumbnail-led layouts
- grouped metadata rows
- tabs or split-column when several equal-weight views compete
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

## Classes To Treat As Inspiration, Not Default Reuse

Widget-specific selectors from templates can be useful inspiration, but they are not the first thing to copy into `custom-api` output.

Examples:
- `weather-*`
- `server-*`
- `monitor-site-*`
- `forum-post-*`
- `reddit-card-*`
- `bookmarks-*`

Use them only when:
- you are intentionally cloning that exact built-in visual language
- the benefit outweighs the maintenance coupling

Otherwise prefer generic classes and recreate the structure with Glance-native primitives.

## Practical Do And Don't

Do:
- use `frameless: true` only when inner markup intentionally rebuilds the shell
- use `list-horizontal-text` for lightweight metadata
- use `cards-grid` only when the content benefits from width
- expose density changes through `options` for reusable widgets
- push secondary details into popovers or disclosure instead of always-visible clutter

Do not:
- use unrelated widget-specific classes just because they look right today
- create a fake Tailwind-like system inside the template
- hardcode local brand colors when semantic colors would work
- preserve legacy style-heavy patterns as your default output
