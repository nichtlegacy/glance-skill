# Native Capabilities

Source:
- `glance/docs/configuration.md`
- `glance/docs/custom-api.md`
- official widget docs and examples in the upstream configuration guide

Checked against upstream on 2026-03-22.

## Native-First Rule

Before recommending `custom-api`, check these layers in order:
- built-in widget already exists
- page composition solves the structure
- native disclosure solves the interaction
- only the data source is missing

If a built-in widget or composition pattern already solves the request cleanly, prefer it over custom work.

## Composition Primitives

### `group`

Use `group` when multiple related widgets should share one slot via tabs.

Best for:
- alternate views of one topic
- multiple feeds competing for the same slot
- reducing vertical sprawl without deleting content

Rules:
- cannot contain another `group`
- cannot contain `split-column`
- cannot hide the group header
- behaves like native tabbed Glance, so prefer it over inventing custom switchers

### `split-column`

Use `split-column` inside a `full` column when you need side-by-side widgets or masonry-like balance.

Best for:
- equal-weight feeds
- side-by-side dashboards
- wide pages with multiple content streams

Rules:
- belongs in `full` columns
- can contain `group`
- cannot live inside `group`

### `head-widgets`

Use `head-widgets` for broad, horizontal content above the page columns.

Good fits:
- `markets`
- `videos`
- `rss` in horizontal-card styles

Bad fits:
- dense utility widgets
- long vertical lists
- anything that only makes sense in a narrow rail

## Native Interaction Toolkit

These are real Glance interaction primitives and should be preferred over invented behavior.

### Popovers

Text popovers:

```html
<span data-popover-type="text" data-popover-text="WAN Connected"></span>
```

HTML popovers:

```html
<div data-popover-type="html">
  <div data-popover-html>
    ...
  </div>
</div>
```

Useful attributes seen in real Glance widgets and examples:
- `data-popover-position`
- `data-popover-show-delay`
- `data-popover-max-width`
- `data-popover-margin`

Use popovers for:
- hover details
- technical metadata
- richer context that should not clutter the main row

### `<details>`

Use HTML `<details>` when the interaction should be explicit click-to-expand disclosure rather than hover-only behavior.

Good uses:
- optional debug info
- secondary stats blocks
- sections that should stay collapsed by default

### Collapse helpers

Use:
- `collapsible-container`
- `data-collapse-after`
- `data-collapse-after-rows`

These should usually replace hand-rolled “show more” logic in templates.

### Relative time

Use `data-dynamic-relative-time` when the UI should show “updated 3m ago”-style output without custom client logic.

## Built-Ins Worth Reusing Before Custom Work

Start with these by intent:

- `search`
  Startpages, entry pages, simple launch surfaces.
- `rss`
  Feeds, blogs, news, many discovery surfaces without custom APIs.
- `videos`
  Channel or media rows, especially in `full` columns or `head-widgets`.
- `weather`
  Compact forecast utility with native hover behavior.
- `todo`
  Task lists with built-in interaction.
- `monitor`
  Service status rails and response-time oriented utility sections.
- `releases`
  Software release tracking without custom template work.
- `docker-containers`
  Container status and grouped-service views.
- `server-stats`
  Host-level utility metrics.
- `bookmarks`
  Grouped links and startpage navigation; prefer this over custom link widgets.
- `markets`
  Finance-like rows and horizontal overview bands.
- `calendar`
  Scheduling and upcoming-event views.
- `repository`
  Project/repo summaries that do not need custom fetching.

## Native-First Decision Hints

Prefer built-ins or composition when:
- the request is really a startpage, feed page, or launch page
- the user mainly wants layout rather than a novel data source
- the interaction is tabs, hover details, expand/collapse, or relative time
- the data maps directly to a documented widget category

Prefer `custom-api` when:
- the data source is not covered by built-ins
- the page is otherwise native but needs one custom surface
- the request needs project-specific presentation from direct JSON APIs

Prefer `extension` only when:
- the service already emits trusted HTML
- server-side aggregation is truly required
- `custom-api` would become awkward, misleading, or unsafe

## Important Nesting And Layout Limits

Keep these rules in mind:
- up to 3 page columns
- a page must have 1 or 2 `full` columns
- `slim` pages may only have up to 2 columns
- `group` cannot contain `group`
- `group` cannot contain `split-column`
- `split-column` can contain `group`

## Reusable Mindset

The right question is usually not “how do I custom-build this?” but:
- can Glance already display this natively?
- can I compose existing widgets to get the desired result?
- can I use native disclosure to keep the layout clean?
- if something is still missing, is the missing piece just one `custom-api` widget?
