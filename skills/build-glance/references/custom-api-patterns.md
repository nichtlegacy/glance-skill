# Custom API Patterns

Source:
- `glance/docs/configuration.md#custom-api`
- `glance/docs/custom-api.md`
- upstream preconfigured pages and community widget conventions

Checked against upstream on 2026-03-20.

## Pattern 1: Compact stats for `small`

Use for:
- service totals
- compact health summaries
- small side-column status widgets

```yaml
- type: custom-api
  title: Media Stats
  cache: 10m
  url: https://${MEDIA_URL}/api/stats
  headers:
    x-api-key: ${MEDIA_API_KEY}
  template: |
    <div class="dynamic-columns list-gap-20">
      <div>
        <div class="color-highlight size-h3">{{ .JSON.Int "movies" | formatNumber }}</div>
        <div class="size-h6">MOVIES</div>
      </div>
      <div>
        <div class="color-highlight size-h3">{{ .JSON.Int "shows" | formatNumber }}</div>
        <div class="size-h6">SHOWS</div>
      </div>
    </div>
```

Why it works:
- quick to scan
- no decorative wrapper needed
- suited to a narrow column

## Pattern 2: Rich list for `full`

Use for:
- bookmarks
- release feeds
- event or queue lists
- article lists with lightweight metadata

```yaml
- type: custom-api
  title: Latest Items
  cache: 15m
  url: https://${ITEMS_URL}/api/items
  options:
    collapse-after: 6
  template: |
    <ul class="list list-gap-10 collapsible-container" data-collapse-after="{{ .Options.IntOr "collapse-after" 6 }}">
    {{ range .JSON.Array "items" }}
      <li>
        <a class="size-h4 color-primary-if-not-visited block text-truncate" href="{{ .String "url" }}">{{ .String "title" }}</a>
        <ul class="list-horizontal-text">
          <li data-dynamic-relative-time="{{ .String "published" | parseTime "rfc3339" | unix }}"></li>
          <li>{{ .String "source" }}</li>
        </ul>
      </li>
    {{ end }}
    </ul>
```

Why it works:
- uses standard Glance list rhythm
- keeps metadata compact
- exposes collapse behavior cleanly

## Pattern 3: Media cards for `full`

Use for:
- recent videos
- media discovery
- thumbnail-led content that would feel cramped in `small`

```yaml
- type: custom-api
  title: Recent Videos
  cache: 30m
  url: https://${VIDEO_API}/recent.json
  frameless: true
  template: |
    <div class="cards-grid collapsible-container" data-collapse-after-rows="3">
    {{ range .JSON.Array "items" }}
      <div class="card widget-content-frame thumbnail-parent">
        <img class="thumbnail" src="{{ .String "thumbnail" }}" alt="" loading="lazy">
        <div class="margin-bottom-widget padding-inline-widget flex flex-column grow">
          <div class="margin-top-10 margin-bottom-auto">
            <a class="color-primary-if-not-visited text-truncate-2-lines" href="{{ .String "url" }}">{{ .String "title" }}</a>
          </div>
          <ul class="list-horizontal-text flex-nowrap margin-top-7">
            <li data-dynamic-relative-time="{{ .String "published" | parseTime "rfc3339" | unix }}"></li>
            <li>{{ .String "author" }}</li>
          </ul>
        </div>
      </div>
    {{ end }}
    </div>
```

Why it works:
- width is used intentionally
- frameless outer shell plus framed inner cards matches Glance patterns
- avoids a fake standalone design language

## Pattern 4: Reusable dual-mode widget

Use for community widgets or project widgets that may live in either `small` or `full`.

Pattern:

```yaml
- type: custom-api
  title: Queue
  cache: 5m
  url: https://${QUEUE_URL}/api/queue
  options:
    small-column: false
    collapse-after: 5
  template: |
    {{ $small := .Options.BoolOr "small-column" false }}
    <ul class="list {{ if $small }}list-gap-8{{ else }}list-gap-10{{ end }} collapsible-container" data-collapse-after="{{ .Options.IntOr "collapse-after" 5 }}">
    {{ range .JSON.Array "items" }}
      <li>
        <a class="{{ if $small }}size-h5{{ else }}size-h4{{ end }} color-primary-if-not-visited block text-truncate" href="{{ .String "url" }}">{{ .String "title" }}</a>
        <ul class="list-horizontal-text">
          <li>{{ .String "status" }}</li>
          <li data-dynamic-relative-time="{{ .String "updated" | parseTime "rfc3339" | unix }}"></li>
        </ul>
      </li>
    {{ end }}
    </ul>
```

Use this when the same YAML should survive both project use and community sharing.

## Pattern 5: Subrequest-backed summary

Use when one widget needs two related endpoints but should still remain a single `custom-api` widget.

```yaml
- type: custom-api
  title: Build Queue
  cache: 2m
  url: https://${CI_URL}/api/queue
  subrequests:
    stats:
      url: https://${CI_URL}/api/queue/stats
  template: |
    {{ $stats := .Subrequest "stats" }}
    <div class="margin-bottom-10">
      <span class="color-highlight size-h3">{{ $stats.JSON.Int "running" }}</span>
      <span class="color-subdue"> running</span>
    </div>
    <ul class="list list-gap-10">
    {{ range .JSON.Array "items" }}
      <li>{{ .String "name" }}</li>
    {{ end }}
    </ul>
```

Keep this restrained. If the widget needs complex merging, transformation, or nontrivial business logic, re-check whether it should remain `custom-api`.

## Pattern 6: Long widget extracted with `$include`

Use when:
- the widget template is long
- the same widget is reused across pages
- indentation would become fragile inline

Pattern:

```yaml
widgets:
  - $include: widgets/media-stats.yml
```

This is especially useful for community-friendly snippets and multi-page project configs.
