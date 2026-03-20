# Pages And Layouts

## Compose pages like Glance already does

When building a page, start from upstream composition patterns instead of inventing a random layout.

## Strong upstream patterns

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

This is how many official and preconfigured examples are structured.

### Wide split-column pattern

Use `width: wide` plus `split-column` when the user wants a denser content surface such as:
- selfhosted homelab overview
- media dashboard
- multi-feed news layout

Do not force `wide` if the content would work fine with the default width.

## Head widgets

`head-widgets` sit above the columns and span the combined width.

Good candidates:
- `markets`
- `videos`
- `rss` with horizontal card styles

Bad candidates:
- cramped widgets with lots of dense vertical text
- widgets that are only useful in a narrow column

## Group widget

Use `group` when multiple related widgets should share one slot via tabs.

Good uses:
- several subreddits
- multiple news feeds of the same kind
- alternate views of one topic

Constraints:
- you cannot place a `group` inside a `group`
- you cannot place a `split-column` inside a `group`
- you cannot hide the header of a `group`

## Split-column widget

Use `split-column` to place widgets side by side within a `full` column.

Good uses:
- two medium-density widgets next to each other
- three to five balanced columns on a wide page
- masonry-like feed collections

Rules:
- `group` can live inside `split-column`
- `split-column` cannot live inside `group`
- prefer it in `full` columns, not as a mental substitute for Glance's actual page columns

## Mobile and density heuristics

Always assume the layout will collapse on mobile.

Practical heuristics:
- place the most valuable widget near the top
- keep `small` columns utility-focused
- avoid stacking multiple huge card grids in one page unless the page is explicitly content-first
- prefer `collapse-after` or `collapse-after-rows` on long lists

## YAML reuse

When a page repeats similar widget settings, use YAML anchors instead of copy-pasting blocks.

This is especially useful for:
- grouped Reddit widgets
- multi-column repeated widgets
- repeated `custom-api` widget templates with different options

## Canonical page blocks

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
