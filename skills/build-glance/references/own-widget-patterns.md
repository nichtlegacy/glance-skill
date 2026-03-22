# Own Widget Patterns

Source corpus:
- standalone repos: `glance-audiobookshelf`, `glance-cowboy`, `glance-discord`, `glance-playstation`, `glance-sabnzbd`, `glance-tautulli`, `glance-tracearr`
- representative local patterns from `glance/widgets`

Checked against local repos on 2026-03-22.

## Canonical Repo Structure

Your modern widget repos tend to follow this pattern:
- `widgets/` for shareable YAML files
- `examples/glance.yml` for integration examples
- `README.md` as the primary product document
- `docs/` for preview images when the repo is presentation-oriented
- `meta.yml` only when the package is meant for community-widget submission

This means the skill should treat README and example composition as part of the widget design, not as afterthoughts.

## Repeated Option Design Patterns

Across the current repos, the stable option vocabulary is:
- env-backed required connection options such as `base-url`, `api-key`, or entity IDs
- `show-*` booleans for optional blocks
- `small-column` for density/shape adaptation
- `compact` for spacing reduction inside the same general layout
- `collapse-after` for long lists
- a small number of semantic layout toggles such as `layout` or `hero-primary-metric`

The skill should default to:
- one adaptable template plus `options`
- a short, readable option surface
- option names that read cleanly in YAML

The skill should avoid:
- duplicating separate small/full templates when options are enough
- adding options for every microscopic style decision

## Resilience Patterns To Copy

Your stronger widgets consistently distinguish these states:
- upstream unavailable or HTTP failure
- required configuration missing
- response succeeded but returned no usable data
- normal data render

Other recurring resilience patterns:
- normalize Home Assistant values such as `unknown`, `unavailable`, `null`, `none`, `None`, `nan`, and boolean-like strings
- clamp percentages and non-negative metrics where needed
- use fallback chains for title, cover, or status fields
- omit secondary metadata when it is missing instead of printing placeholder clutter

## Archetype: `glance-playstation`

What to copy:
- dual-mode layout through `small-column` and `compact`
- strong Home Assistant entity normalization
- fallback chains for media title, cover art, platform, and session labels
- explicit configured/error/no-data states

What not to copy forward as the default:
- legacy files as living standards

Guidance:
- keep a single modern widget as the default answer
- treat `*-legacy.yml` only as migration or comparison material

## Archetype: `glance-tracearr`

What to copy:
- split a service into overview/live/dashboard when the slot fit changes materially
- make `small` and `full` decisions explicit
- expose detail density through `show-*` toggles and `collapse-after`
- build suites when overview and live activity have different natural surfaces

Guidance:
- prefer a suite over a giant monolith when the service has overview, active, and dashboard-level views

## Archetype: `glance-tautulli`

What to copy:
- dashboard plus recent-media list split
- popover-backed secondary details rather than always-visible technical noise
- one layout surface for monitoring, another for media discovery

Guidance:
- activity dashboards and recent-media lists often deserve separate widgets

## Archetype: `glance-sabnzbd`

What to copy:
- compact `small`-column-first monitor design
- proper use of `parameters` instead of fragile query strings
- clear queue-empty handling
- restrained option surface

Guidance:
- operational monitors should stay compact, quick to scan, and strongly typed around the API shape

## Archetype: `glance-audiobookshelf`

What to copy:
- several complementary widgets instead of one overstuffed widget
- README productization with recommended grouped usage
- a natural `group`/tab story for related widgets sharing a slot

Guidance:
- if a service has several equally valid views, consider a small suite plus `group` rather than one mega-template

## Archetype: `glance-discord`

What to copy:
- layered optional blocks through `show-*` options
- a selected primary presence surface with richer secondary context
- capability to be expressive without forcing every block to render all the time

Guidance:
- rich presence widgets should keep the main state legible and push secondary details into optional blocks or disclosure

## Archetype: `glance-cowboy`

What to copy:
- strict validation of required entity IDs and values
- clamping numeric values before display
- separate optional extras from core metrics
- hero metric decisions exposed as semantic options instead of duplicated templates

Guidance:
- validation-heavy widgets should fail clearly and keep optional data from polluting the core surface

## Archetype: Main `glance/widgets`

The representative upstream-like patterns in the main `glance/widgets` folder are especially useful for:
- `data-popover-*`
- `widget-content-frame`
- `list-horizontal-text`
- `collapsible-container`
- relative-time helpers
- restrained use of `<details>` for progressive disclosure

Guidance:
- treat these as native interaction and structure examples worth reusing
- do not inherit the hardcoded demo URLs or local values found in some older files

## README And Productization Patterns

Your stronger repos usually make the widget feel like a real package by including:
- a concise overview and recommended use case
- quick-start env vars
- example `glance.yml` composition
- option tables
- behavior notes
- recommended layout guidance for `small` versus `full`

The skill should mirror this when the user wants repo-quality or community-shareable output.

## Legacy As Anti-Pattern Material

Use legacy files and older hardcoded examples for migration guidance only.

Common anti-patterns to call out:
- hardcoded local URLs or IPs in sample YAML
- duplicated legacy and modern templates without a strong reason
- older widgets that solve density with extensive inline styles rather than reusable options
- examples that look workable locally but are not shareable
