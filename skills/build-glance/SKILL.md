---
name: build-glance
description: Build practical, native-first Glance YAML that prefers built-in widgets and page composition before `custom-api`, grounds custom work in real Glance primitives and proven widget archetypes, packages community-shareable widgets cleanly, and chooses `extension` only when it is explicitly justified.
---

# Build Glance

## Overview

Build Glance artifacts that feel like they belong in upstream Glance first, your own widget ecosystem second, and the community-widgets world third.

The skill now uses four distinct knowledge sources:
- official Glance capabilities and layout rules
- upstream template and component patterns from `internal/glance/templates`
- proven patterns from your current `glance-*` widget repos plus representative `glance/widgets` examples
- community packaging and shareability patterns

Default posture: `native-first`, not `custom-api-first`.

## Workflow

1. Classify the request.
   Common targets are built-in widget configuration, page composition, a `custom-api` widget, a widget suite, a community-shareable package, or an `extension` boundary decision.
2. Run the native-first check.
   Before inventing custom work, check whether built-ins, `group`, `split-column`, `head-widgets`, grouped bookmarks, or native interaction helpers already solve the problem cleanly.
3. Choose the lowest-complexity Glance primitive.
   Preferred order: built-ins and composition, then `custom-api`, then `extension`.
4. If `custom-api` is justified, choose the right shape.
   Decide whether the result should be `small`, `full`, `dual-mode`, or a suite of widgets rather than a monolith.
5. Choose the right upstream component family.
   Pick the real Glance pattern that best fits the slot: compact rows, rich lists, horizontal cards, grid cards, tabs, grouped links, split-column masonry, or centered utility blocks.
6. Ground the answer in the right pattern source.
   Use upstream references for capability truth and component vocabulary, your own widget archetypes for project-quality patterns, and community references for shareable packaging expectations.
7. Produce paste-ready output.
   Return valid Glance YAML first, then env vars, README/meta/preview guidance, or rationale text only as needed.
8. Run the guardrails.
   Check `dos-donts.md` and `quality-checklist.md` before finishing.

## Native-First Decision

Before recommending `custom-api`, always check these questions in order:
- Can a built-in widget already solve the request?
- Can page composition solve it with `group`, `split-column`, or `head-widgets`?
- Can native disclosure patterns solve the interaction?
  Relevant primitives: `data-popover-*`, `<details>`, `collapsible-container`, `data-dynamic-relative-time`.
- Is the missing piece really the data source, not the layout?

If the answer is yes at any earlier stage, prefer the earlier primitive.

Read first:
- `references/native-capabilities.md`
- `references/widget-catalog.md`
- `references/pages-and-layouts.md`
- `references/upstream-template-patterns.md`
- `references/page-recipes.md` when the request is page-level

## Custom API Path

Choose `custom-api` when:
- built-ins and page composition are not enough
- Glance can fetch the source directly over HTTP
- the response is JSON or otherwise still workable with the template layer
- the rendering can stay Glance-native without inventing a backend just for formatting

When you stay on this path, read:
- `references/custom-api-core.md`
- `references/custom-api-patterns.md`
- `references/styling-and-primitives.md`
- `references/upstream-template-patterns.md`
- `references/own-widget-patterns.md`
- `references/operational-notes.md` when env vars, cache, secrets, includes, or reload behavior matter

## Shape Decision: Small, Full, Dual-Mode, Or Suite

Pick the widget shape deliberately before writing markup:

- `small`
  Compact stats, utility widgets, quick monitors, short lists, or concise dashboards.
- `full`
  Rich lists, card-led content, media discovery, comparison layouts, or denser feed surfaces.
- `dual-mode`
  One reusable widget that adapts with `options` such as `small-column`, `compact`, `layout`, or `collapse-after`.
- `suite`
  Multiple widgets when the service naturally splits into overview, current activity, recent items, or alternative views.

Use your own archetypes as defaults:
- PlayStation for dual-mode Home Assistant entity widgets
- Tracearr for overview/live/dashboard suites
- Tautulli for dashboard-plus-lists patterns
- Audiobookshelf for group-ready suites
- SABnzbd for compact small-column monitors
- Discord for layered optional rich-presence blocks
- Cowboy for strict validation and resilient value handling

## Community Packaging Path

When the task is meant for `glanceapp/community-widgets`, produce the minimum useful community package:
- widget YAML
- required env vars
- `meta.yml` contents
- README-ready usage/setup text
- preview guidance for `preview.png`

Read:
- `references/community-packaging.md`
- `references/community-patterns.md`
- `references/dos-donts.md`

Community output should stay copy-pasteable, env-var driven, and free of local URLs or literal credentials.

## Extension Boundary

Use `extension` only when at least one of these is true:
- the service already owns server-side aggregation or trusted HTML rendering
- the output does not fit Glance's `custom-api` request/template model cleanly
- real server-side processing is required and should not be faked in YAML
- the widget depends on trusted HTML that Glance should render as-is

Always explain why `custom-api` is insufficient.

Read:
- `references/boundary-extension.md`
- `references/upstream-template-patterns.md`
- `references/native-capabilities.md`
- `references/community-packaging.md` only if the user also wants repo/docs guidance around the service

## Component-First Composition

After choosing the primitive, choose the visual family before writing markup.

Strong upstream families:
- compact rows for utility/status widgets
- rich vertical lists for feeds and recent activity
- horizontal cards for broad media or discovery surfaces
- grid cards for full-width browsing
- grouped tabs through `group`
- grouped links for startpages and launchers
- centered utility blocks for concise `small` widgets

Be creative by recombining official Glance families, not by inventing foreign HTML structures.

## Output Rules

### Always optimize for Glance-native output

- Use real widget types and real config keys
- Follow upstream layout and naming patterns
- Prefer built-ins and composition before custom work
- Prefer real upstream component families before bespoke wrapper hierarchies
- Use utility classes, theme-aware variables, and native disclosure helpers before bespoke CSS
- Keep templates readable after one pass

### Prefer reusable widget design

- Expose likely variations through `options`
- Prefer one adaptable template over separate small/full files when options are enough
- Use `small-column`, `compact`, `collapse-after`, `show-*`, or a small number of semantic `layout`/`style` options
- Use `frameless: true` only when the inner markup intentionally rebuilds the widget from Glance-native inner frames
- Use `$include` or YAML anchors when that reduces repetition cleanly

### Prefer resilient rendering

- Distinguish transport/auth errors, misconfiguration, and empty/no-usable-data states
- Normalize obviously unusable states instead of rendering broken placeholders
- Clamp or sanitize numeric values where appropriate
- Omit secondary metadata when it is missing instead of adding noise

### Prefer operational hygiene

- Use environment variables for URLs, hosts, tokens, and secrets
- Mention `${secret:...}` or `${readFileFromEnv:...}` when the task or deployment model warrants it
- Choose cache durations by volatility, not aesthetics
- Avoid companion backends unless they are genuinely necessary

## Reference Map

Start narrow. Do not load every reference by default.

- `references/native-capabilities.md`
  Official built-ins, composition primitives, native interaction helpers, and when native solutions should win.
- `references/own-widget-patterns.md`
  Canonical patterns from your modern widget repos and the representative main `glance/widgets` examples.
- `references/community-patterns.md`
  Real-world community patterns, where they help, and where they drift.
- `references/dos-donts.md`
  Anti-pattern guardrails, migration notes, and explicit rejection rules.
- `references/custom-api-core.md`
  Request fields, template model, options, parameters, and subrequests.
- `references/custom-api-patterns.md`
  Reusable custom widget shapes, including dual-mode and suite decisions.
- `references/styling-and-primitives.md`
  Utility classes, disclosure patterns, spacing primitives, and CSS boundaries.
- `references/upstream-template-patterns.md`
  Real component families from upstream templates: shell patterns, tabs, masonry, rows, lists, cards, and grouped navigation blocks.
- `references/community-packaging.md`
  `README.md`, `preview.png`, `meta.yml`, and community-shareable output expectations.
- `references/boundary-extension.md`
  Extension protocol, trust boundary, and when not to use it.
- `references/glance-core.md`
  Top-level structure, shared properties, and page constraints.
- `references/pages-and-layouts.md`
  Page composition rules and nesting limits.
- `references/page-recipes.md`
  Canonical page families and when to use them.
- `references/widget-catalog.md`
  Compact fit guide for built-ins and composition primitives.
- `references/quality-checklist.md`
  Final pass before returning any non-trivial result.

## Failure Modes To Prevent

- Reaching for `custom-api` when built-ins or page composition already solve the request
- Recommending `extension` without proving why `custom-api` is insufficient
- Returning invalid widget nesting or impossible page layouts
- Duplicating separate small/full templates where options would be cleaner
- Writing arbitrary bespoke HTML when an upstream template family already fits
- Hardcoding local IPs, domains, ports, or literal API keys in examples
- Using unstable CSS classes borrowed from unrelated built-in widgets
- Inventing custom JS-like interaction instead of native popovers, `<details>`, or collapse helpers
- Preserving legacy patterns as if they were current best practice
- Shipping community output without env vars, README/meta/preview guidance, or setup notes

## Completion Standard

A finished result should:
- choose the correct Glance primitive for the request
- argue from native-first capability truth when that decision matters
- choose a real upstream component family rather than freehanding the markup
- fit the intended `small`, `full`, `dual-mode`, or suite shape
- feel upstream-native in layout and styling
- reflect your current widget-quality patterns rather than legacy defaults
- stay operationally sane and copy-pasteable
- reject avoidable anti-patterns instead of silently reproducing them
