# Quality Checklist

Run this before finalizing any non-trivial result.

## Primitive choice

- Is this really a page, a `custom-api` widget, or an `extension`?
- If it is an extension, did you explain why `custom-api` is not enough?
- If it is `custom-api`, did you verify that Glance can fetch and render the data directly?

## Config validity

- Are the widget types real Glance widget types?
- Are the field names real upstream config keys?
- Are nesting rules respected?
- Are page width and column choices valid?
- If `parameters` are present, did you avoid also depending on embedded query params in `url`?

## Operational quality

- Are secrets and local hostnames replaced with env vars?
- Should `${secret:...}` or `${readFileFromEnv:...}` be mentioned for better secret hygiene?
- Is the cache duration reasonable?
- Is the result still copy-pasteable?
- Did you avoid adding a companion service unless it is justified?
- If `skip-json-validation` is used, is the reason explicit?

## Visual and UX quality

- Does the output feel Glance-native?
- Does it use Glance utility classes instead of random bespoke styling?
- Is the content density suitable for the intended column width?
- Does the layout still make sense on mobile collapse?
- Are likely display variations exposed through `options` instead of template edits?

## Community packaging

If the output is meant to be shared:
- Is `meta.yml` covered?
- Are preview expectations covered?
- Are README/setup notes present if needed?
- Are contribution rules respected?
- If it is a community extension rather than a `custom-api` widget, did you switch to the correct contribution path?
