# Upstream Template Patterns

Source:
- `glance/internal/glance/templates/widget-base.html`
- `glance/internal/glance/templates/group.html`
- `glance/internal/glance/templates/split-column.html`
- `glance/internal/glance/templates/page.html`
- `glance/internal/glance/templates/page-content.html`
- `glance/internal/glance/templates/custom-api.html`
- `glance/internal/glance/templates/extension.html`
- representative content templates such as `rss-*`, `videos-*`, `forum-posts.html`, `bookmarks.html`, `search.html`, `monitor*.html`, and `server-stats.html`

Checked against upstream on 2026-03-24.

## Why This File Exists

The upstream templates are the best source for how Glance really composes UI.

Use them as:
- component vocabulary
- density guidance
- shell conventions
- evidence for what feels native

Do not treat them as a license to copy arbitrary internal class names blindly.

## Shell Patterns

### `widget-base.html`

This is the canonical widget shell.

It provides:
- the outer `widget widget-type-*` wrapper
- header title and optional linked title
- WIP badge support
- notice and error indicators
- standard empty/error rendering when content is unavailable

Implication:
- default to the standard shell unless the widget genuinely wants a frameless composite surface

### `custom-api.html`

`custom-api` itself is intentionally thin:
- it delegates to `widget-base.html`
- `frameless` only toggles `widget-content-frameless`
- the actual visual quality comes from the template you write

Implication:
- "creative" `custom-api` work should still reuse upstream families rather than inventing a mini frontend

### `extension.html`

`extension` is equally thin:
- it delegates to `widget-base.html`
- frameless behavior comes from the extension response header
- the HTML is rendered as provided

Implication:
- a polished extension still needs conscious use of Glance-native structure and utilities

## Page-Level Patterns

### `page-content.html`

The page body has three major bands:
- optional mobile reachability header
- optional `head-widgets`
- the column grid

Implication:
- broad, visually horizontal widgets belong in `head-widgets`
- column content should be arranged as coherent rails, not random blocks

### `page.html`

The full page shell shows:
- desktop navigation
- mobile navigation pills per column
- theme picker integration
- loading state inside `page-content`

Implication:
- each column should make sense when viewed more independently on mobile
- single-page dashboards can justify hiding desktop navigation

## Composition Primitives

### `group.html`

This is the real Glance tab pattern:
- header buttons across the top
- one active panel at a time
- good for alternate views in one slot

Design takeaway:
- if the user wants toggles between related views, `group` is almost always better than custom switch logic

### `split-column.html`

This is the real Glance masonry-like wide layout:
- `masonry`
- `data-max-columns`
- children rendered directly inside

Design takeaway:
- if the user wants multi-stream wide content, use `split-column` before inventing custom multi-column markup

## Reusable Content Families

### Utility rows

Strong examples:
- `monitor.html`
- `monitor-compact.html`

Shared pattern:
- concise title link
- one or two supporting metrics
- trailing status glyph
- small gaps and strong scanability

Use for:
- health checks
- queues
- recent short activity
- compact operational widgets

### Rich vertical lists

Strong examples:
- `rss-list.html`
- `rss-detailed-list.html`
- `videos-vertical-list.html`
- `forum-posts.html`

Shared pattern:
- title-first reading order
- one metadata row with `list-horizontal-text`
- optional thumbnail
- optional description or tags
- `collapsible-container` when the list can get long

Use for:
- news
- recent media
- community/forum activity
- event timelines

### Horizontal card rails

Strong examples:
- `rss-horizontal-cards.html`
- `rss-horizontal-cards-2.html`
- `videos.html`
- `reddit-horizontal-cards.html`

Shared pattern:
- frameless outer shell
- `carousel-container`
- `cards-horizontal`
- each card wrapped in `widget-content-frame`

Use for:
- discovery surfaces
- media-heavy content
- head-widget bands
- full-width visual summaries

### Grid cards

Strong examples:
- `videos-grid.html`

Shared pattern:
- frameless outer shell
- `cards-grid`
- optional row collapsing

Use for:
- full-width content exploration
- media libraries
- visual dashboards that benefit from width

### Vertical cards

Strong examples:
- `reddit-vertical-cards.html`

Shared pattern:
- stacked framed cards
- image optional
- short metadata row

Use for:
- rich content that is still easier to scan vertically than horizontally

### Grouped navigation blocks

Strong examples:
- `bookmarks.html`
- `search.html`

Shared pattern:
- a strong primary action or grouped launch surface
- optional icons
- short descriptions kept secondary

Use for:
- startpages
- service launchers
- curated tool collections

### Metric blocks with hidden detail

Strong examples:
- `weather.html`
- `server-stats.html`

Shared pattern:
- one obvious headline metric
- compact supporting values
- secondary details in popovers instead of permanent clutter

Use for:
- host metrics
- concise analytics
- summary-plus-detail widgets

## Common Structural Ingredients

These ingredients appear repeatedly across upstream templates:

- `widget-content-frame`
  Inner card or frame inside frameless widgets.
- `list-horizontal-text`
  Metadata rows with compact separators.
- `dynamic-columns`
  Auto-balancing repeated row groups.
- `cards-horizontal`
  Horizontal card rails.
- `cards-grid`
  Grid discovery surface.
- `cards-vertical`
  Vertical stack of framed cards.
- `thumbnail-parent` and `thumbnail`
  Image-aware card and list composition.
- `attachments`
  Small tag-like metadata chips.
- `collapsible-container`
  Long-list control without custom interaction code.

## Safe Reuse Versus Inspiration Only

Usually safe to reuse:
- generic utility classes
- generic card and list families
- `widget-content-frame`
- `widget-content-frameless`
- `cards-*`
- `list-*`
- `thumbnail`
- `attachments`
- `collapsible-container`

Treat as inspiration first:
- widget-specific selectors like `weather-column-*`, `server-*`, `forum-post-*`, `reddit-card-*`, `bookmarks-*`

## Choosing A Family By Slot

### `small`

Reach for:
- compact utility rows
- concise vertical lists
- centered utility blocks
- grouped links

Avoid:
- wide carousels
- dense card grids
- overbuilt decorative wrappers

### `full`

Reach for:
- rich lists
- grouped views
- split-column layouts
- horizontal or grid cards

Avoid:
- stretching a small utility pattern until it feels empty

### `head-widgets`

Reach for:
- horizontal cards
- wide visual summaries
- broad market/media bands

Avoid:
- text-heavy vertical utilities

## Practical Creative Guidance

If the user asks for something "more creative", do not leave Glance behind.

Instead:
- choose a stronger upstream family
- tune density with `options`
- use frameless plus inner frames deliberately
- move secondary detail into popovers or collapse helpers
- make the page composition feel curated through `group`, `split-column`, and rail hierarchy
