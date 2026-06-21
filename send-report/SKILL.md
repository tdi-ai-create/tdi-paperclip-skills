---
name: send-report
description: >
  Deliver formatted email reports to Rae via the TDI send-report API endpoint.
  Handles markdown content delivery with styled HTML rendering.
---

# Send Report -- Email Delivery

Deliver reports and briefs to Rae via the TDI send-report endpoint.

## Endpoint

```
curl -s -X POST "https://www.teachersdeserveit.com/api/paperclip/send-report" \
  -H "Authorization: Bearer [resolve secret PAPERCLIP_REPORT_SECRET]" \
  -H "Content-Type: application/json" \
  -d '{"subject": "...", "content": "markdown report here"}'
```

- **Method:** POST
- **Auth:** `Bearer [resolve secret PAPERCLIP_REPORT_SECRET]`
- **Recipient:** Hard-coded to rae@teachersdeserveit.com (no `to` field needed)
- **From:** Olivia Smith -- TDI EA

## Request Body

```json
{ "subject": "Subject line here", "content": "markdown report body" }
```

The `content` field accepts markdown. The endpoint renders it to styled HTML automatically.

## Formatting Rules

### Section Structure -- What / So What / Now What

Every report section uses this three-part structure when presenting data or metrics:

- `> **What:**` -- renders as blue callout box. The metric, data point, or fact.
- `> **So What:**` -- renders as amber callout box. Why it matters, trend direction.
- `> **Now What:**` -- renders as green callout box. Specific recommended action for Rae.

### Markdown Conventions

- `##` for section headers
- `**bold**` for key metrics and emphasis
- Tables with `|` for data grids
- Numbered and bulleted lists for action items
- Always include actual numbers -- "$1.35M pipeline, up from $1.30M" not "pipeline is growing"
- When a metric has zero data, report "No data" -- never skip the section
- Write for a CEO scanning on mobile -- lead with the headline, details below
- Escape `\<[alphanumeric]` sequences in all markdown

## Subject Line Conventions

| Report Type | Subject Format |
|---|---|
| Morning Brief | `Morning Brief -- YYYY-MM-DD` |
| Mid-Day Check | `Mid-Day Check -- YYYY-MM-DD` |
| EOD Wrap | `EOD Wrap -- YYYY-MM-DD` |
| Ad-Hoc Alert | `[ACTION] Brief description -- YYYY-MM-DD` |
| Urgent Alert | `[URGENT] Brief description -- YYYY-MM-DD` |

## Important Rules

- **Never fabricate data.** If a query returns nothing, say "No data" with context.
- **Mobile-first.** Lead with the headline number or decision. Details below.
- **One email per routine.** Don't split a morning brief into multiple sends.
- **Ad-hoc alerts are rare.** Only send outside scheduled times for genuinely time-sensitive items.
