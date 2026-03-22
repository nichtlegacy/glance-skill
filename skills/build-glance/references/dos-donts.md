# Dos And Don'ts

Use this file as the final guardrail before returning a Glance answer.

## Do

- Start with a native-first capability check.
- Prefer built-ins, `group`, `split-column`, `head-widgets`, and grouped bookmarks before `custom-api`.
- Use native disclosure patterns such as `data-popover-*`, `<details>`, `collapsible-container`, and relative-time helpers.
- Use environment variables for URLs, hosts, ports, tokens, and secrets.
- Expose reusable behavior through `options` such as `small-column`, `compact`, `collapse-after`, and `show-*`.
- Distinguish transport failure, misconfiguration, empty/no-usable-data, and healthy render states.
- Pick cache durations based on data volatility and reload behavior.
- Split a service into a widget suite when overview, live activity, and recent items naturally want different slots.
- Treat README, example composition, and packaging notes as part of the design when the widget is shareable.

## Don't

- Do not hardcode local IPs, local domains, local ports, or literal API keys in examples.
- Do not default to `custom-api` when a built-in widget or page recipe already fits.
- Do not recommend `extension` unless you clearly explain why `custom-api` is insufficient.
- Do not fork separate small/full templates when one template plus `options` would be cleaner.
- Do not depend on unrelated built-in widget CSS classes or unstable internal selectors.
- Do not invent custom JavaScript-like interaction in templates when native popovers or disclosure already exist.
- Do not create a large bespoke CSS system if utility classes and a few local styles are enough.
- Do not keep legacy files or hardcoded examples as if they were current best practice.
- Do not mix `parameters` with query strings embedded in `url` and then rely on the embedded values.

## Inline CSS Rule

Inline CSS is acceptable when:
- Glance utility classes do not cover a small local need
- the styling is tightly scoped to one element or one widget-local class
- the same effect would be awkward or impossible with native classes alone

Inline CSS is a smell when:
- it becomes the main layout system
- it recreates card, grid, spacing, or typography primitives Glance already has
- it forces separate templates for width variants instead of using options and classes

## Legacy And Migration Notes

Use older or legacy-near files as anti-pattern material only.

Call out these issues explicitly when they appear:
- local demo hosts in committed widget files
- literal tokens in example YAML
- duplicate legacy and modern files that should be one option-driven template
- large style-heavy widgets that could be simplified with native primitives

## Shareability Rule

If the answer is meant to travel outside the author's machine, it should be:
- env-var driven
- copy-pasteable
- Glance-native in layout and interaction
- clear about setup and cache behavior
