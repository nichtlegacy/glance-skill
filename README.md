# glance-skill

Glance-focused Codex skill repository with a primary emphasis on practical `custom-api` widget work.

## Install

Once this repository is published on GitHub in the `skills/build-glance` layout, install the skill with:

```bash
npx skills add https://github.com/nichtlegacy/glance-skill --skill build-glance
```

After installation, restart Codex so the new skill is loaded.

## Repository Layout

```text
skills/build-glance/   installable skill package
evals/                 prompt-level evaluation cases
fixtures/              example prompts and sample outputs
```

## Skill Scope

`build-glance` is designed for:
- building polished Glance `custom-api` widgets
- choosing Glance-native layout, utility classes, and `options`
- packaging `custom-api` widgets for `glanceapp/community-widgets`
- composing pages around widgets when page structure matters
- choosing `extension` only when `custom-api` is not sufficient

## Using Evals

The repository includes prompt-level evals in `evals/` and concrete prompt fixtures in `fixtures/prompts/`.

Minimal manual workflow:
1. Pick a prompt from `evals/evals.json`.
2. Run the skill against the referenced prompt file.
3. Review the output against the listed `must_haves`.
4. If the answer misses core Glance rules, update the skill references or examples before trying again.

Use `fixtures/sample-output/` if you want to store notes or reference outputs from review rounds.
