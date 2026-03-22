# Community Patterns

Source:
- `glanceapp/community-widgets` README and contribution flow
- representative community examples mirrored locally for study

Checked against community patterns on 2026-03-22.

## What Community Widgets Usually Optimize For

Community widgets tend to optimize for:
- copy-pasteability
- shareability across unknown environments
- a small number of meaningful options
- README-driven setup clarity
- preview-first discoverability

This means the skill should think about packaging and onboarding, not just raw YAML.

## Patterns Worth Copying

The most useful recurring community patterns are:
- long widgets live in their own file and are included from `glance.yml`
- README shows the preview, env vars, full YAML, options, and setup notes
- `collapsible-container` is used for long lists instead of custom show-more mechanics
- popovers or `<details>` carry secondary detail when the main row should stay compact
- env vars are descriptive and never tied to the author's machine
- cache choice is explained implicitly by the widget's purpose

## Shareable Structure To Aim For

For community-targeted `custom-api` work, the default shape should be:
- `widgets/<name>/README.md`
- `widgets/<name>/preview.png`
- `widgets/<name>/meta.yml`
- one or more YAML snippets that stay copy-pasteable

For local or one-off work, do not force this structure unless the user asks for packaging.

## Where Community Patterns Drift

Not every community pattern should become default skill guidance.

Examples of drift:
- very large inline style blocks
- widgets that mimic built-ins too literally with brittle CSS
- examples that are safe enough for the repo but not especially elegant
- README prose that is complete but visually inconsistent

The skill should borrow the good operational habits without cargo-culting every stylistic decision.

## When To Use Community Patterns

Lean on community patterns when:
- the user explicitly wants to publish or share the widget
- the request needs README/meta/preview output
- the widget should be robust for unknown third-party setups
- the result will be consumed as a reusable package rather than a local snippet

## When Not To Overfit To Community Patterns

Do not overfit to community conventions when:
- the request is just a local config edit
- Glance already has a built-in widget for the job
- the user needs a quick page composition answer rather than a repo package
- the community example solves the problem with avoidable hacks

## Community Guardrail

Community conventions should make the result easier to share, not less native.

The skill should still prefer:
- native Glance layout primitives
- env-var hygiene
- reusable options
- restrained CSS
- explicit rejection of local URLs, literal tokens, and unnecessary backends
