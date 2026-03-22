# Community Packaging

Source:
- `glanceapp/community-widgets` contribution flow and repository conventions
- recurring packaging patterns from the community and your own standalone widget repos

Checked against community expectations on 2026-03-22.

## Scope

This file is for shareable community `custom-api` widgets.

If the task is a community `extension`, read `boundary-extension.md` as well because the submission path is different.

## Minimum Community Package

For a shareable `custom-api` widget under `widgets/<widget-name>/`, the community repo expects:
- `README.md`
- `preview.png`
- `meta.yml`

Minimal `meta.yml` shape:

```yaml
title: Your widget title
description: A short description of the widget
author: your-github-username
```

## README Structure That Works Well

A strong community README should usually include:
- what the widget does
- a preview image
- the full YAML
- required environment variables
- setup notes and gotchas
- option explanations
- behavior notes when the widget has non-obvious fallbacks or states

Your own stronger repos add a useful product shape here:
- `Overview`
- `Quick Start`
- `Widget Options`
- `Behavior Notes`
- `References`

Use that structure when the user wants repo-quality packaging.

## Shareability Rules

Do:
- use descriptive env vars
- keep the YAML copy-pasteable
- expose likely customization points with `options`
- choose a reasonable default cache
- mention `${secret:...}` or `${readFileFromEnv:...}` when secret hygiene matters
- keep the output valid even for someone who does not share the author's environment

Do not:
- submit local domains, IPs, or demo ports
- submit literal API keys or tokens
- require an extra local API just to make a `custom-api` widget function
- rely on unstable built-in widget CSS classes
- hardcode colors when semantic theme-aware colors are sufficient

## Preview Guidance

Community packaging should mention preview expectations explicitly.

Useful guidance to return:
- provide a clean screenshot of the widget in Glance
- crop to the widget or a tight dashboard section
- prefer a realistic default configuration
- avoid local secrets or private hostnames in the screenshot

## Output Package To Return

When asked to package a community widget, usually return:
- widget YAML
- env vars
- `meta.yml` contents
- README-ready setup and usage text
- preview guidance

Only expand into full file contents when the user actually wants the full package output.

## Extension Difference

Community `extension` widgets do not use the same packaging flow as community `custom-api` widgets.

If the task becomes an extension, switch to the extension contribution path and explain why the package structure differs.
