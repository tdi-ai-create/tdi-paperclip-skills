# TDI-Hub-Engagement-Skill
**Created July 14, 2026.**
**How content agents (Jasmine) generate engagement checks for Learning Hub lessons via the Hub Engagement Sync API. Julie Lynn uses the same API to validate density.**

---

## Base URL and auth

All API calls go to the TDI website (NOT Paperclip, NOT Railway):
- **Base URL:** `https://www.teachersdeserveit.com`
- **Auth:** `Authorization: Bearer $PAPERCLIP_SYNC_KEY`

The key is in your company secrets. Every URL below is relative to the base URL above.

---

## Work loop — how to find and process lessons

### Step 1 — Find lessons that need engagement checks

```
GET /api/hub/engagement-sync?action=find_work
Authorization: Bearer $PAPERCLIP_SYNC_KEY
```

Returns lessons with content but fewer than 4 engagement checks:
```json
{
  "work": [
    {
      "lesson_id": "uuid",
      "lesson_title": "Building Trust Through Consistency",
      "course_title": "Classroom Culture",
      "has_body_html": true,
      "has_video": true,
      "existing_checks": 0,
      "request_type": "generate_checks"
    }
  ],
  "count": 3
}
```

If `count` is 0, all lessons are covered. Stop.

### Step 2 — Generate checks for a lesson

For each item where `request_type` is `generate_checks`:

```
POST /api/hub/engagement-sync
Authorization: Bearer $PAPERCLIP_SYNC_KEY
Content-Type: application/json
{
  "action": "generate_checks",
  "lesson_id": "<lesson_id from find_work>"
}
```

The API reads the lesson content, uses AI to generate 5 engagement checks (2 comprehension, 1 reflection, 1 action step, 1 checkpoint), and inserts them into the database with correct positioning.

Returns:
```json
{
  "success": true,
  "lesson_id": "uuid",
  "lesson_title": "Building Trust Through Consistency",
  "checks_created": 5
}
```

### Step 3 — Validate density

After generating, verify the lesson meets minimum requirements:

```
GET /api/hub/engagement-sync?action=validate_density&lesson_id=<uuid>
Authorization: Bearer $PAPERCLIP_SYNC_KEY
```

Returns:
```json
{
  "lesson_id": "uuid",
  "passes": true,
  "total_checks": 5,
  "breakdown": {
    "comprehensionCount": 2,
    "reflectionCount": 1,
    "actionCount": 1,
    "checkpointCount": 1
  },
  "issues": []
}
```

If `passes` is false, `issues` tells you what's missing. Generate additional checks or escalate to Rae.

---

## Read-only queries

### Get overall coverage status
```
GET /api/hub/engagement-sync?action=get_status
```
Returns total lessons, how many have checks, coverage percentage.

### Get full lesson data
```
GET /api/hub/engagement-sync?action=get_lesson&lesson_id=<uuid>
```
Returns lesson content + all existing questions. Use this to review before generating.

---

## Minimum density requirements

Every lesson must have at minimum:
- 2 comprehension checks (multiple choice or true/false)
- 1 action step ("Try It" — a specific classroom action)
- 1 checkpoint (key takeaways summary)

Reflections are encouraged but not required.

---

## Important rules

1. **Never generate checks for a lesson that already passes density.** Call `validate_density` first.
2. **Never delete or modify existing checks.** Only add new ones.
3. **The API handles all AI generation.** You do NOT write the questions yourself — you call `generate_checks` and the API does it.
4. **Report results to Rae** via the send-report skill when you finish a batch.
5. **If a lesson has no content and no transcript**, skip it. The API will return an error — that's expected.
