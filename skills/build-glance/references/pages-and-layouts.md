# Pages And Layouts

Source:
- `glance/docs/glance.yml`
- `glance/internal/glance/templates/page.html`
- `glance/internal/glance/templates/page-content.html`

Checked against upstream on 2026-03-24.

## Compose Pages Like Glance Already Does

When building a page, start from upstream composition patterns instead of inventing a random layout.

## Strong Upstream Patterns

### Official starter pattern

The canonical `docs/glance.yml` example is:
- one `Home` page
- `small` / `full` / `small`
- compact utility rails
- richer grouped content in the center

Use it when the user wants:
- a general home dashboard
- clear utility sidebars
- one content spine in the middle
- an immediately recognizable Glance layout

### Startpage pattern

Use when the user wants a focused landing page:
- `width: slim`
- `hide-desktop-navigation: true`
- `center-vertically: true`
- one `full` column
- `search` first
- quick status or bookmark widgets underneath

This is appropriate for:
- browser homepages
- search-centric personal dashboards
- minimal service launchers

### Small-full-small pattern

Use when the user wants a balanced dashboard:
- left `small` column for compact status widgets
- center `full` column for content-heavy widgets
- right `small` column for supporting widgets

This is the strongest default for mixed utility plus content dashboards.

### Wide split-column pattern

Use `width: wide` plus `split-column` when the user wants a denser content surface such as:
- selfhosted homelab overview
- media dashboard
- multi-feed news layout

Do not force `wide` if the content would work fine with the default width.

## Head Widgets

`head-widgets` sit above the columns and span the combined width.

Good candidates:
- `markets`
- `videos`
- `rss` with horizontal card styles

Bad candidates:
- cramped widgets with lots of dense vertical text
- widgets that are only useful in a narrow column

Use head widgets to establish the page mood or primary discovery band before the column content starts.

## Group Widget

Use `group` when multiple related widgets should share one slot via tabs.

Good uses:
- several subreddits
- multiple news feeds of the same kind
- alternate views of one topic
- overview versus live view within the same slot

Constraints:
- you cannot place a `group` inside a `group`
- you cannot place a `split-column` inside a `group`
- you cannot hide the header of a `group`

## Split-Column Widget

Use `split-column` to place widgets side by side within a `full` column.

Good uses:
- two medium-density widgets next to each other
- three to five balanced columns on a wide page
- masonry-like feed collections

Rules:
- `group` can live inside `split-column`
- `split-column` cannot live inside `group`
- prefer it in `full` columns, not as a mental substitute for Glance's actual page columns

## Mobile And Density Heuristics

Always assume the layout will collapse on mobile.

Practical heuristics:
- place the most valuable widget near the top
- keep `small` columns utility-focused
- avoid stacking multiple huge card grids in one page unless the page is explicitly content-first
- prefer `collapse-after` or `collapse-after-rows` on long lists
- remember that mobile navigation emphasizes column switching, so each column should feel coherent on its own

## Intentional Page Composition

Make the page feel curated, not shuffled.

Useful mental model:
- left rail: quick scan, status, utility, weather, releases
- center spine: feed, media, recent activity, grouped content
- right rail: supporting utilities, markets, secondary monitoring, bookmarks

When the page gets too dense:
- move broad visual widgets into `head-widgets`
- use `group` instead of stacking alternate views
- use `split-column` inside the center `full` column rather than inventing custom grids

## YAML Reuse

When a page repeats similar widget settings, use YAML anchors instead of copy-pasting blocks.

This is especially useful for:
- grouped Reddit widgets
- multi-column repeated widgets
- repeated `custom-api` widget templates with different options

## Canonical Page Blocks

### Minimal startpage block

```yaml
- name: Startpage
  width: slim
  hide-desktop-navigation: true
  center-vertically: true
  columns:
    - size: full
      widgets:
        - type: search
          autofocus: true
        - type: monitor
          cache: 1m
          title: Services
          sites: ...
        - type: bookmarks
          groups: ...
```

### Balanced `small/full/small` block

```yaml
- name: Home
  columns:
    - size: small
      widgets:
        - type: weather
        - type: markets
    - size: full
      widgets:
        - type: rss
          style: horizontal-cards
          feeds: ...
        - type: group
          widgets: ...
    - size: small
      widgets:
        - type: releases
        - type: server-stats
```

### Wide `split-column` block

```yaml
- name: Discover
  width: wide
  columns:
    - size: full
      widgets:
        - type: split-column
          max-columns: 4
          widgets: ...
```

Use `page-recipes.md` when you need fuller examples and decision logic for which page family to choose.
