# Community Packaging

Source:
- `glanceapp/community-widgets/CONTRIBUTING.md`

Checked against upstream on 2026-03-20.

## Scope

This file is for shareable community `custom-api` widgets.

If the task is a community `extension`, read `boundary-extension.md` as well because the submission path is different.

## Required community widget contents

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

## README expectations

A good `README.md` should include:
- what the widget does
- a preview image
- the full YAML
- required environment variables
- any setup notes needed to make it work
- multiple variants, if the widget intentionally supports them

## Shareability rules

Do:
- use descriptive env vars
- provide a reasonable default cache
- expose likely customization points with `options`
- keep the YAML copy-pasteable
- mention preview generation expectations when relevant

Do not:
- submit local domains or IPs
- submit literal API keys
- require an extra local API just to make a `custom-api` widget function
- rely on arbitrary built-in widget CSS classes
- hardcode colors when semantic theme-aware classes would work

## Packaging output to return

When asked to package a community widget, usually return:
- widget YAML
- env vars
- `meta.yml` contents
- README-ready usage/setup text
- preview guidance

Only expand into full file contents when the user actually asks for them.

## Naming and reuse guidance

- Use descriptive env var names such as `IMMICH_URL` and `IMMICH_API_KEY`
- Prefer option names that read cleanly in YAML, such as `small-column`, `compact`, `collapse-after`, or `show-thumbnails`
- Mention multiple variants in one README when the widget intentionally supports them
