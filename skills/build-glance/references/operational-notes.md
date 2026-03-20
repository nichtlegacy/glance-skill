# Operational Notes

Source:
- `glance/docs/configuration.md`
- `community-widgets/CONTRIBUTING.md`

Checked against upstream on 2026-03-20.

## Environment variables first

Use `${ENV_VAR}` syntax for:
- API URLs
- hostnames
- tokens
- passwords
- numeric settings that should remain configurable

Do not hardcode:
- local IPs
- local domains
- literal API keys

## File-backed secrets

Mention these when the task is sensitive enough that plain env vars are not ideal:

- Docker secret syntax:

```yaml
token: ${secret:github_token}
```

- File path via env var:

```yaml
token: ${readFileFromEnv:TOKEN_FILE}
```

Use these when the user clearly cares about secret hygiene or already runs Glance in Docker or a similar environment.

## Includes and reuse

Use `$include` when:
- a widget gets long
- the same widget is reused
- indentation becomes error-prone

Rules to remember:
- the included file should start at top-level indentation
- `$include` must be on its own line
- included files can still use env vars

If debugging include-related YAML errors, Glance documents `config:print` as the reliable way to inspect the fully expanded config.

## Cache heuristics

Choose cache values by volatility:
- local service status or short queues: often seconds to a few minutes
- dashboard summaries for self-hosted apps: often 5 to 30 minutes
- external slow-moving data: often hours

Do not choose cache durations only for visual freshness.

## Reload and rate-limit behavior

Remember:
- invalid config on startup exits immediately
- invalid config after a successful run leaves the old config active
- config reload clears cached data

That means frequent reloads can hammer APIs and trigger rate limits during development. Mention this when proposing low-cache widgets or rapid iteration workflows.

## Query and request gotchas

- `parameters` override query params already present in `url`
- `allow-insecure` is only for invalid or self-signed certificates
- `skip-json-validation` should be justified explicitly
- Glance fetches from the server side, not from the browser

## Icons and assets

Not every widget uses icons, but Glance projects often do.

Useful icon prefixes:
- `si:`
- `sh:`
- `di:`
- `mdi:`

Useful detail:
- `auto-invert` helps non-theme-aware icons work in both light and dark schemes

If users want self-hosted icons or images, Glance supports `server.assets-path` and serves them under `/assets/...`.

## When to mention page context

Even for a single widget, mention page context when it changes the answer:
- whether the widget belongs in `small` or `full`
- whether `head-widgets` would be a better placement
- whether `$include` would help keep a larger config sane
