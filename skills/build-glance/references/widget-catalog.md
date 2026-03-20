# Widget Catalog

Use this file when you need a compact source of truth for built-in Glance widgets, their best-fit column types, and the styles that matter during layout decisions.

These fit recommendations are heuristics derived from upstream docs, templates, and preconfigured pages. They are not hard validation rules unless explicitly stated.

## Layout-first rules

- `small` columns are best for utility, status, and compact list widgets.
- `full` columns are best for content-heavy widgets, card layouts, and multi-item feeds.
- `head-widgets` are best for broad horizontal widgets that can span a page naturally.
- `split-column` only belongs inside a `full` column.
- `group` is a composition tool, not a content type. Its fit depends on the widgets inside it.

## Built-in widgets

| Widget | Best fit | Styles or variants | Use when | Watch out for |
| --- | --- | --- | --- | --- |
| `rss` | `small`, `full`, `head-widgets` | `vertical-list`, `detailed-list`, `horizontal-cards`, `horizontal-cards-2` | news, blog feeds, link-heavy content | `detailed-list` and horizontal styles are effectively `full` column patterns |
| `videos` | `full`, `head-widgets` | `horizontal-cards`, `vertical-list`, `grid-cards` | YouTube or playlist-driven media layouts | `grid-cards` and `horizontal-cards` want width; avoid cramming into `small` columns |
| `hacker-news` | `full`, `split-column` | list-based | developer/news feed in a content spine | long lists benefit from `collapse-after` |
| `lobsters` | `full`, `split-column` | list-based | engineering/news layouts similar to Hacker News | tag filtering changes feed density |
| `reddit` | `small` or `full` depending on style | `vertical-list`, `horizontal-cards`, `vertical-cards` | subreddit-driven dashboards | `vertical-cards` is the small-column style; `horizontal-cards` is full-column oriented |
| `search` | `full`, slim page focus | search box | startpages and entry pages | strongest in a single-column or centered layout |
| `group` | inherits inner widget fit | tabbed widget container | switch between related feeds or views | cannot contain `group` or `split-column` |
| `split-column` | `full` only | masonry via `max-columns` | side-by-side widgets, wide dashboards, masonry-like feeds | do not place inside `group` |
| `custom-api` | any, if designed for the target column | author-defined | custom widgets from JSON APIs | the template author must design for `small` vs `full` explicitly |
| `extension` | any, if justified | external HTML | custom widgets that need their own server-side output | only use when `custom-api` is not enough |
| `weather` | `small` | built-in forecast layout | compact utility/weather glance | best in utility columns |
| `todo` | `small`, `full` | built-in todo layout | personal task list | feels most natural in utility-heavy side columns |
| `monitor` | `small`, `full` | default and compact monitor variants | service status, uptime, response time | compact/full presentation matters a lot; see `styling-and-primitives.md` |
| `releases` | `small`, `full` | list-based | software release tracking | long repo lists need collapse tuning |
| `docker-containers` | `small`, `full` | status list | homelab/container monitoring | can become noisy if too many containers are shown |
| `dns-stats` | `small` | stats list | DNS and network utility pages | utility-focused, not a hero widget |
| `server-stats` | `small` | system stats | CPU, RAM, disk utility widgets | best near other status widgets |
| `repository` | `small`, `full` | repo summary | source control or project overview | good as supporting content, not usually a page anchor |
| `bookmarks` | `small`, slim startpages | grouped links | launchers, browser homepages, quick nav | strongest when groups stay concise |
| `change-detection` | `small`, `full` | list-based | monitoring changed pages | tune density with `collapse-after` |
| `clock` | `small` | timezone list | utility/time displays | naturally a side widget |
| `calendar` | `small`, `full` | current and legacy versions | scheduling and upcoming events | utility-first unless event density is high |
| `markets` | `small`, `head-widgets`, `full` | ticker list | finance-focused dashboards | works well in `head-widgets` or stacked small-column sections |
| `twitch-channels` | `full`, `small` with care | channel list/cards | creator/live dashboards | density depends on channel count |
| `twitch-top-games` | `small`, `full` | ranked list | gaming pages | official recipe places it in `small` |
| `iframe` | depends entirely on embed content | embedded site | when Glance just needs to host an external view | operationally heavier than native widgets |
| `html` | any, if static | raw static HTML | static snippets or small embeds | no remote fetching, so it is not a replacement for `custom-api` |

## Compact vs full heuristics

Use compact or narrow-friendly widgets in `small` columns when they are:
- metric-dense
- line-based
- quick to scan
- naturally stacked

Use full-width widgets when they are:
- card-oriented
- media-heavy
- feed-heavy
- tabbed or horizontally structured

Community patterns worth copying:
- `small-column: true|false` options for custom widgets that need dual-mode layouts
- `compact` or `style: compact` options for monitor-like widgets
- card grids for media widgets in full columns

## Strong upstream examples

- `Startpage` recipe: `search` + `monitor` + `bookmarks` in a slim single-column layout
- `Markets` recipe: stacked `markets` in a `small` column, `rss`/`group`/`videos` in the `full` spine
- `Gaming` recipe: `twitch-top-games` in `small`, `group` plus `videos` in `full`, compact `reddit` in `small`

Use `page-recipes.md` for full YAML-oriented compositions.
