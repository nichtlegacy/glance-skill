# Widget Catalog

Use this file as the compact native-first fit guide for built-in Glance widgets and composition primitives.

These recommendations are reuse heuristics, not hard validation rules unless explicitly noted.

## Native-First Use Order

When choosing how to solve a Glance request, start here:
1. built-in widget already matches the data/problem
2. `group`, `split-column`, `head-widgets`, or grouped bookmarks solve the structure
3. one `custom-api` widget fills the remaining gap
4. `extension` only if the remaining gap truly needs it

## Composition Toolkit

| Primitive | Best fit | Reuse angle | Watch out for |
| --- | --- | --- | --- |
| `group` | any slot where several related views compete | native tabs for alternate views | cannot contain `group` or `split-column` |
| `split-column` | `full` columns | side-by-side or masonry-like content | do not place inside `group` |
| `head-widgets` | broad page summaries | horizontal hero band above columns | not for dense utility rails |
| `bookmarks` with groups | startpages, launchers, utility navigation | grouped links without custom work | keep groups concise |

## Built-In Widgets By Intent

| Widget | Best fit | Use when | Watch out for |
| --- | --- | --- | --- |
| `search` | slim or single-`full` pages | startpages and entry actions | strongest when it is visually primary |
| `rss` | `small`, `full`, `head-widgets` | feeds, blogs, news, mixed content pages | horizontal styles want width |
| `videos` | `full`, `head-widgets` | creator and media layouts | `grid-cards` needs width |
| `weather` | `small` | utility forecast widget | usually belongs in a side rail |
| `todo` | `small`, `full` | native task management | often better as utility than page anchor |
| `monitor` | `small`, `full` | uptime and service-health views | density choice matters a lot |
| `releases` | `small`, `full` | software release tracking | long repo lists need collapse tuning |
| `docker-containers` | `small`, `full` | container status and grouping | can get noisy if overstuffed |
| `dns-stats` | `small` | DNS/network utility sections | naturally utility-oriented |
| `server-stats` | `small` | host metrics and system utility rows | not usually a hero widget |
| `repository` | `small`, `full` | repo summaries and project side content | usually supporting content, not a full page spine |
| `calendar` | `small`, `full` | scheduling and upcoming events | default fit is utility unless density is high |
| `markets` | `small`, `head-widgets`, `full` | finance rows and broad summary bands | strong head-widget candidate |
| `twitch-channels` | `small`, `full` | creator/live dashboards | count and density change the fit |
| `twitch-top-games` | `small`, `full` | gaming discovery and ranked lists | upstream recipes often place it in `small` |
| `iframe` | depends on embed content | when Glance must host another view | operationally heavier than native widgets |
| `html` | any static slot | static snippets or local markup | not a substitute for remote data fetching |
| `custom-api` | any, if intentionally designed for the slot | one missing custom data surface | still must be shaped for `small` or `full` |
| `extension` | any, if justified | trusted external HTML or server-side rendering | last resort, not convenience path |

## Native Interaction And Disclosure

Built-in or native markup interactions to reuse before inventing custom behavior:
- group tabs via `group`
- weather hover behavior for richer detail within a compact widget
- todo hover/delete interactions as proof that native widgets can already be interactive
- container grouping patterns in `docker-containers`
- `data-popover-*` for hover details
- `<details>` for explicit disclosure
- `collapsible-container` for long lists
- `data-dynamic-relative-time` for relative timestamps

## Fit Heuristics

Choose `small`-column solutions when the content is:
- metric-dense
- line-based
- utility-focused
- naturally stacked

Choose `full`-column solutions when the content is:
- card-oriented
- media-heavy
- feed-dense
- visually central to the page

Choose dual-mode `custom-api` when the same information should survive both placements with option-driven density changes.

Choose a suite when overview/live/recent or status/discovery surfaces naturally diverge.

## Strong Native Recipes To Remember

- Startpage: `search` + `monitor` + `bookmarks`
- Markets/news: stacked `markets` plus `rss`/`videos` in a full spine
- Gaming/media: `twitch-top-games` in `small`, `group` and `videos` in `full`
- Wide discovery page: `split-column` inside a `full` column instead of custom CSS grids
