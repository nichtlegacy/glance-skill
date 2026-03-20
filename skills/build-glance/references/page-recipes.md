# Page Recipes

Use this file when the task is not just “add a widget”, but compose a page that feels like a real Glance layout.

## Recipe 1: Startpage

Best for:
- browser homepages
- search-first personal dashboards
- minimal launchers

Key traits:
- `width: slim`
- `hide-desktop-navigation: true`
- `center-vertically: true`
- one `full` column
- `search` first
- utility widgets underneath

Canonical shape:

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

Why it works:
- the page has one obvious entry action
- utility content stays below the search box
- slim width keeps the composition tight and intentional

## Recipe 2: Markets / News Page

Best for:
- finance dashboards
- mixed feed pages
- content pages with one strong center spine

Key traits:
- `small` utility/stacked market data on the left
- a `full` content spine with `rss`, `group`, and/or `videos`
- optional right `small` column for overflow feed content

Canonical shape:

```yaml
- name: Markets
  columns:
    - size: small
      widgets:
        - type: markets
          title: Indices
          markets: ...
        - type: markets
          title: Crypto
          markets: ...

    - size: full
      widgets:
        - type: rss
          style: horizontal-cards
          feeds: ...
        - type: group
          widgets: ...
        - type: videos
          style: grid-cards
          collapse-after-rows: 3
          channels: ...

    - size: small
      widgets:
        - type: rss
          limit: 30
          collapse-after: 13
          feeds: ...
```

Why it works:
- the center column carries the rich content
- side columns act as utility rails
- card-heavy widgets get the width they need

## Recipe 3: Gaming / Media Page

Best for:
- gaming dashboards
- creator/media discovery layouts
- topic pages with mixed feed density

Canonical shape:

```yaml
- name: Gaming
  columns:
    - size: small
      widgets:
        - type: twitch-top-games
          limit: 20
          collapse-after: 13

    - size: full
      widgets:
        - type: group
          widgets: ...
        - type: videos
          style: grid-cards
          collapse-after-rows: 3
          channels: ...

    - size: small
      widgets:
        - type: reddit
          subreddit: gamingnews
          style: vertical-cards
          limit: 7
```

Why it works:
- side columns stay quick to scan
- the full column carries card-rich content
- the page has a strong topical focus without feeling visually flat

## Recipe 4: Wide Split-Column Feed Page

Best for:
- selfhosted news walls
- wide discovery dashboards
- masonry-like feed collections

Canonical shape:

```yaml
- name: Home
  width: wide
  columns:
    - size: full
      widgets:
        - type: split-column
          max-columns: 4
          widgets:
            - type: reddit
              subreddit: selfhosted
              collapse-after: 15
            - type: reddit
              subreddit: homelab
              collapse-after: 15
            - type: reddit
              subreddit: linux
              collapse-after: 15
            - type: reddit
              subreddit: sysadmin
              collapse-after: 15
```

Why it works:
- you get multiple equal-weight content streams without abandoning Glance's page model
- `split-column` handles responsive collapse better than inventing custom CSS grids in YAML

## Recipe 5: Head-Widget Landing Page

Best for:
- pages that need one broad “hero band” above the main columns
- horizontal market/video/news summaries

Use `head-widgets` when:
- the widget benefits from horizontal span
- the content is still readable across the page width
- it acts as a top summary, not a dense body block

Good candidates:
- `markets`
- `rss` with `horizontal-cards`
- `videos`

Avoid it for:
- cramped utility widgets
- highly vertical lists
- widgets that only make sense in a narrow utility rail

## Balancing heuristics

### Small column

Put things here that are:
- status-heavy
- utility-like
- short-form
- quick to scan

### Full column

Put things here that are:
- card-heavy
- media-rich
- feed-dense
- visually central to the page

### Use `group` when

- multiple views compete for the same slot
- you want to reduce vertical sprawl without deleting content

### Use `split-column` when

- you want multiple equal-weight content blocks inside a full-width spine
- the page would benefit from a masonry-like or side-by-side reading rhythm

### Use YAML anchors when

- repeated widget definitions differ only by subreddit, feed, or a small option set

