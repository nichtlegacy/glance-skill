# Quality Checklist

Run this before finalizing any non-trivial result.

## Capability Choice

- Did you check built-in widgets and page composition before recommending `custom-api`?
- If the answer is `custom-api`, did you make it clear why native Glance primitives were not enough?
- If the answer is `extension`, did you explain why `custom-api` is insufficient?

## Shape And Architecture

- Is this best as `small`, `full`, `dual-mode`, or a widget suite?
- Did you avoid creating a monolith when overview/live/recent surfaces should be separate?
- Did you avoid separate small/full file forks when options would be cleaner?
- Did you choose a real upstream component family such as compact rows, rich lists, horizontal cards, grid cards, tabs, or grouped links?

## Config Validity

- Are the widget types real Glance widget types?
- Are the field names real upstream config keys?
- Are nesting rules respected?
- Are page width and column choices valid?
- If `parameters` are present, did you avoid also depending on embedded query params in `url`?

## Visual And Interaction Quality

- Does the output feel Glance-native?
- Does it use real utility classes and theme-aware values before bespoke styling?
- Does it reuse a fitting upstream shell pattern such as the default frame or frameless plus inner frames?
- Is the content density suitable for the intended column width?
- Does the layout still make sense on mobile collapse?
- Did you use native disclosure such as `data-popover-*`, `<details>`, or collapse helpers where appropriate?

## Operational Quality

- Are secrets and local hostnames replaced with env vars?
- Should `${secret:...}` or `${readFileFromEnv:...}` be mentioned for better secret hygiene?
- Is the cache duration reasonable?
- Is the result still copy-pasteable?
- Did you avoid adding a companion service unless it is justified?
- If `skip-json-validation` is used, is the reason explicit?
- If template-side requests are used, is the request chain short and are status codes checked manually?
- If `extension` is used, did you mention the maintenance burden, `allow-potentially-dangerous-html`, and the HTML-only content type?

## Community Packaging

If the output is meant to be shared:
- Is `meta.yml` covered?
- Are preview expectations covered?
- Are README/setup notes present if needed?
- Are community packaging rules respected?
- If it is a community extension rather than a `custom-api` widget, did you switch to the correct contribution path?

## Anti-Pattern Rejection

- Did you remove local IPs, domains, ports, and literal API keys from examples?
- Did you avoid unstable CSS borrowed from unrelated built-in widgets?
- Did you keep inline CSS limited to clearly justified local cases?
- Did you treat legacy examples as migration material rather than as the default standard?
