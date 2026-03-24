# glance-skill

Glance-focused Codex skill repository for native-first dashboard composition, practical `custom-api` widget work, community-shareable packaging, and explicit `extension` boundary decisions.

## Install

Once this repository is published on GitHub in the `skills/build-glance` layout, install the skill with:

```bash
npx skills add https://github.com/nichtlegacy/glance-skill --skill build-glance
```

After installation, restart Codex so the new skill is loaded.

## Knowledge Architecture

`build-glance` now teaches from four distinct sources of truth:

- **Official Glance first**
  Built-in widgets, page composition, native disclosure helpers, layout rules, `docs/glance.yml`, and the real `custom-api`/`extension` boundary.
- **Upstream template patterns**
  Real component vocabulary from `internal/glance/templates`: shells, cards, lists, carousels, tabs, grouped links, masonry, and frameless inner-frame conventions.
- **Own widget archetypes second**
  Modern patterns from the standalone `glance-*` repos plus representative `glance/widgets` examples.
- **Community patterns third**
  Packaging, shareability, README/meta/preview expectations, and real-world conventions from `glanceapp/community-widgets`.

The result is intentionally `native-first`: built-ins and composition should win whenever they are sufficient. `custom-api` is the main custom path only after that check. `extension` remains the last resort.

## Skill Scope

`build-glance` is designed for:
- choosing between built-ins, page composition, `custom-api`, and `extension`
- composing native Glance pages with `group`, `split-column`, and `head-widgets`
- building polished `custom-api` widgets with Glance-native layout and interaction patterns
- deciding between `small`, `full`, `dual-mode`, and widget-suite architectures
- packaging `custom-api` widgets for `glanceapp/community-widgets`
- rejecting anti-patterns such as local URLs, literal secrets, duplicate templates, and unnecessary companion backends

It is still authoring-first, but it now encodes stronger review guardrails through explicit Do/Don't references and eval cases.

It also has a stronger design vocabulary now: not just "use native classes", but "choose the right upstream family" such as compact rows, rich lists, horizontal cards, grid cards, grouped tabs, split-column masonry, and grouped navigation blocks.

## Repository Layout

```text
skills/build-glance/   installable skill package
  references/          official capabilities, own archetypes, community patterns, guardrails
evals/                 prompt-level evaluation cases
fixtures/prompts/      concrete prompt fixtures for manual eval runs
fixtures/sample-output/ optional notes and review output
```

Key reference blocks inside `skills/build-glance/references/`:
- `native-capabilities.md`
- `upstream-template-patterns.md`
- `own-widget-patterns.md`
- `community-patterns.md`
- `dos-donts.md`

## What The Skill Should Do

A good `build-glance` response should:
- start with the lowest-complexity Glance primitive that actually solves the request
- choose a real upstream component family before freehanding markup
- use native composition and disclosure patterns before inventing custom behavior
- reflect current repo-quality widget patterns such as `options`, `small-column`, `compact`, `collapse-after`, `show-*`, and resilient empty/error/configured states
- know when `custom-api` should use `newRequest`, `withParameter`, JSON Lines, or a suite instead of a fake backend
- treat `extension` as an explicit trust and maintenance boundary, not a convenience shortcut
- package community widgets with complete env-var and repo guidance when requested
- refuse to normalize avoidable anti-patterns

## Using Evals

The repository includes prompt-level evals in `evals/evals.json` and supporting prompts in `fixtures/prompts/`.

Minimal manual workflow:
1. Pick a prompt from `evals/evals.json`.
2. Run the skill against the referenced prompt file.
3. Review the result against the listed `must_haves`.
4. If the answer misses native-first routing, packaging completeness, or anti-pattern guardrails, update the relevant references before trying again.

The eval set now covers these categories:
- built-in versus `custom-api` capability choice
- page composition and layout fit
- native popover/disclosure patterns
- upstream template family selection
- dual-mode and suite architecture decisions
- dynamic `custom-api` request chaining
- extension maintenance and trust-boundary decisions
- community packaging completeness
- anti-pattern rejection and operational hygiene

Use `fixtures/sample-output/` if you want to store notes or reference outputs from review rounds.
