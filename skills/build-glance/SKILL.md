---
name: build-glance
description: Build practical, polished Glance YAML with a primary focus on `custom-api` widgets that follow upstream Glance patterns and community conventions. Use when Codex needs to create or refine Glance `custom-api` widgets, choose Glance-native layout and utility classes, expose reusable `options`, package shareable community widgets, compose pages around widgets, or decide when `extension` is truly justified.
---

# Build Glance

## Overview

Build Glance artifacts that are practical to paste into a real setup and that read like they belong in upstream Glance or the community-widgets ecosystem.

Treat `custom-api` as the default path for custom work. Only leave that path when Glance cannot realistically fetch and render the data itself.

## Workflow

1. Classify the request.
   Common targets are a single `custom-api` widget, a shareable community widget, a page built around one or more widgets, or an `extension` boundary decision.
2. Choose the lowest-complexity Glance primitive that can solve the task.
   Prefer built-in widgets or page composition first, then `custom-api`, then `extension`.
3. For `custom-api`, decide the target mode before writing markup.
   Determine whether the widget is primarily for a `small` column, a `full` column, or a reusable dual-mode/community case.
4. Read only the narrowest relevant references.
   Do not load all references by default.
5. Produce paste-ready output.
   Return Glance YAML first. Add env vars, setup notes, README text, `meta.yml`, or packaging notes only when the task calls for them.
6. Run the quality checklist mentally before finishing.
   Catch invalid config keys, poor cache defaults, hidden trust boundaries, fragile CSS, and unnecessary backend requirements.

## Primary Path: Custom API First

Use `custom-api` by default when:
- Glance can fetch the source directly over HTTP
- the response is JSON or can still be handled with request options and template logic
- the rendering can be done with HTML templates plus Glance utility classes
- the widget should remain copy-pasteable without requiring a companion service

For `custom-api` widget work, read:
- `references/custom-api-core.md`
- `references/custom-api-patterns.md`
- `references/styling-and-primitives.md`

Also read:
- `references/operational-notes.md` when env vars, secrets, includes, icons, reload behavior, or cache choices matter
- `references/community-packaging.md` when the widget is meant for `glanceapp/community-widgets`
- `references/pages-and-layouts.md` or `references/page-recipes.md` when placing the widget inside a full page matters

## Width And Density Decision

Decide layout mode before writing the template:

- `small`
  Prefer compact stats, short lists, quick-scan monitor-like structures, and restrained density.
- `full`
  Prefer richer lists, card layouts, thumbnail-led media blocks, or wider comparison layouts.
- dual-mode or reusable community widget
  Expose layout behavior through `options` such as `small-column`, `compact`, `style`, or `collapse-after`.

Read `references/custom-api-patterns.md` before inventing a new structure from scratch.

## Output Rules

### Always optimize for native Glance output

- Use real Glance widget types and real config keys
- Follow upstream naming and layout patterns
- Use Glance utility classes and variables instead of inventing a separate design language
- Keep YAML indentation and grouping clean enough to paste directly into a config file
- Prefer templates that are understandable after one read

### Prefer reusable widget design

- Use `options` instead of hardcoding likely customization points into the template
- Expose density or display-mode choices when a widget may be used in both `small` and `full` contexts
- Use YAML anchors or `$include` when repetition would otherwise make the config brittle
- Keep defaults operationally sane

### Prefer operationally simple solutions

- Use environment variables for hostnames, URLs, tokens, and secrets
- Mention `${secret:...}` or `${readFileFromEnv:...}` when file-backed secret handling is more appropriate
- Prefer a direct API call over a companion backend when Glance can handle it
- Do not recommend an `extension` if a `custom-api` widget is enough

### Prefer robust styling

- Use utility classes and theme-aware variables first
- Use inline styles only when utility classes are not enough and the styling is local to that element
- Do not depend on arbitrary internal CSS classes from unrelated built-in widgets
- Avoid hardcoded colors unless the task explicitly requires a fixed branded color
- Make collapse behavior, spacing, and content density match the chosen column width

## Packaging Rules

When the task is a shareable community `custom-api` widget, produce the minimum useful set:
- the widget YAML
- required env vars
- a short configuration note
- `README.md`-ready usage text when requested
- `meta.yml` contents
- preview guidance for `preview.png`

Follow community expectations:
- descriptive env var names
- reasonable cache durations
- widget-local CSS suffixes only when custom classes are truly needed
- no extra local API unless the solution is explicitly no longer a `custom-api` widget

Read `references/community-packaging.md` before writing shareable widget packaging.

## Extension Boundary

Use `extension` only when at least one of these is true:
- the output logic does not fit Glance's template and request model cleanly
- the widget needs server-side processing that Glance should not do inline
- the widget already exists as a standalone service and emits the final HTML
- the requested experience depends on trusted external HTML

Always explain why `custom-api` is not sufficient when choosing `extension`.

Read:
- `references/boundary-extension.md`
- `references/styling-and-primitives.md` if the extension should visually match Glance
- `references/community-packaging.md` if the user also wants a community sharing path

## Page Composition Path

Use page composition when the user wants a complete dashboard section or a page built around one or more widgets.

Read:
- `references/glance-core.md`
- `references/pages-and-layouts.md`
- `references/page-recipes.md` when the user wants a preconfigured-page-like result
- `references/widget-catalog.md` when choosing built-in widgets for `small`, `full`, or `head-widgets`

Reuse upstream layout patterns such as `head-widgets`, `group`, and `split-column`.

## Reference Map

- `references/custom-api-core.md`
  Read for request fields, template behavior, `options`, `parameters`, and `subrequests`.
- `references/custom-api-patterns.md`
  Read for compact, full-width, reusable, and subrequest-driven widget patterns.
- `references/styling-and-primitives.md`
  Read for Glance-native classes, frame primitives, layout patterns, and CSS do/don't guidance.
- `references/operational-notes.md`
  Read for env vars, secrets, includes, caching, icons, assets, and reload behavior.
- `references/community-packaging.md`
  Read for `README.md`, `meta.yml`, `preview.png`, env var rules, and shareability expectations.
- `references/boundary-extension.md`
  Read for extension headers, HTML trust boundaries, and community extension packaging differences.
- `references/glance-core.md`
  Read for top-level config structure, shared widget properties, and page constraints.
- `references/pages-and-layouts.md`
  Read for page width, column strategy, `head-widgets`, `group`, and `split-column`.
- `references/page-recipes.md`
  Read for canonical page compositions.
- `references/widget-catalog.md`
  Read for built-in widget fit and placement heuristics.
- `references/quality-checklist.md`
  Read before finalizing any non-trivial result.

## Failure Modes To Prevent

- Invalid Glance config keys or unsupported widget nesting
- Choosing `extension` for a problem that should stay a `custom-api` widget
- Building a widget for the wrong width and density target
- Hardcoding values that should be exposed through `options`
- Overcomplicated page layouts that ignore Glance's column model
- Secrets or local URLs hardcoded into examples
- HTML that looks generic rather than Glance-native
- Borrowing unstable CSS classes from unrelated built-in widgets
- Community widgets that are missing cache guidance, metadata, preview guidance, or setup notes
- Forgetting that `parameters` override query params already embedded in the URL

## Completion Standard

A finished result should:
- use the correct Glance primitive
- be immediately understandable and nearly paste-ready
- match upstream and community conventions
- explain any required environment variables, secrets, or companion services
- fit the intended `small` or `full` context
- avoid avoidable complexity
