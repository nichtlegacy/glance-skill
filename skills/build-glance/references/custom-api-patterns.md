# Custom API Patterns

Source:
- `glance/docs/configuration.md#custom-api`
- `glance/docs/custom-api.md`
- your current `glance-*` widget archetypes

Checked against upstream and local patterns on 2026-03-22.

## Pattern 0: Native Gate Before Custom Work

Before using any pattern in this file, verify that the request is not already solved by:
- a built-in widget
- page composition with `group`, `split-column`, or `head-widgets`
- native popovers, `<details>`, or collapse helpers

If the problem is already native, stop there.

## Pattern 1: Compact Utility Monitor For `small`

Use for:
- service overviews
- queue/status monitors
- quick stats blocks
- side-rail utility widgets

Strong archetypes:
- `glance-sabnzbd`
- `glance-tracearr` overview
- parts of `glance-cowboy`

Traits:
- compact stats or short lists
- restrained metadata
- explicit empty/error/configured states
- quick scan rhythm

Typical controls:
- `small-column: true`
- `collapse-after`
- a small number of `show-*` flags

## Pattern 2: Dual-Mode Entity Or Presence Widget

Use when the same data may live in either `small` or `full`.

Strong archetypes:
- `glance-playstation`
- `glance-discord`

Traits:
- one template adapts by `small-column` and `compact`
- hero content stays readable in both modes
- optional blocks are controlled by `show-*`
- fallback chains and state normalization are central

Use this when the width changes density, not the fundamental product.

## Pattern 3: Widget Suite Instead Of One Monolith

Use when a service naturally splits into different surfaces.

Strong archetypes:
- `glance-tracearr` overview/live/dashboard
- `glance-tautulli` dashboard plus recent media lists
- `glance-audiobookshelf` grouped suite

Choose a suite when:
- overview, current activity, and recent items want different page slots
- one template would become overstuffed
- different views belong in `small` versus `full`
- a `group` makes the suite easier to consume

Do not force one widget just because all data comes from one service.

## Pattern 4: Rich List With Native Disclosure

Use for:
- recent media lists
- release feeds
- queue/job lists
- technical rows with optional detail

Strong archetypes:
- `glance-tautulli` recent lists
- representative release widgets using popovers and collapse

Traits:
- main row stays compact
- secondary detail moves into `data-popover-*` or `<details>`
- long lists use `collapsible-container`
- timestamps use relative-time helpers where useful

## Pattern 5: Validation-Heavy Metrics Widget

Use for:
- Home Assistant entity bundles
- dashboards where values must be normalized or clamped
- widgets with optional extras and hard-required core fields

Strong archetype:
- `glance-cowboy`

Traits:
- validate required identifiers early
- clamp percentages or prevent negative output where it matters
- keep optional sections independent from core rendering
- fail clearly instead of rendering broken values

## Pattern 6: Subrequest-Backed Summary

Use when one widget needs a main endpoint plus one small supporting endpoint.

Traits:
- small number of supporting requests
- response merge remains readable
- no backend-like transformation layer is being recreated

If the widget grows beyond that, step back and reconsider whether the answer should be a suite or an extension.

## Pattern 7: Long Widget Extracted With `$include`

Use when:
- the template is long
- the same widget is reused across pages
- indentation would be fragile inline
- the output is meant to be shared or documented

Pattern:

```yaml
widgets:
  - $include: widgets/my-widget.yml
```

This is especially appropriate for community-targeted widgets and repo-quality packages.

## Pattern Selection Heuristic

Use this quick rule:
- choose `small` utility monitors for compact operational data
- choose `full` rich lists or cards when width materially improves the experience
- choose dual-mode when the same widget should survive both placements
- choose a suite when the service really has multiple distinct surfaces
