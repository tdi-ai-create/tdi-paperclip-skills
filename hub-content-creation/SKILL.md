---
name: hub-content-creation
description: >
  Complete workflow for creating, publishing, and maintaining Quick Wins
  and courses on the TDI Learning Hub. Covers every role, every gate,
  and every ongoing maintenance task.
---

# Hub Content Creation and Maintenance Workflow

## Pipeline: New Quick Win (Idea to Live)

```
Nora receives task, routes to Dr. Jasmine Cole
    |
Dr. Jasmine Cole researches, drafts content,
  suggests tags (category, lift, roles, danielson, topic_tags)
    |
Lily designs and polishes the PDF
  (layout, formatting, visual quality)
    |
Julie Lynn QA gate (validates tags, quality, completeness,
  verifies against tagging spec)
    |
Julie Lynn publishes via Content Sync API
  (POST /api/hub/content-sync, action: publish)
  DB trigger auto-seeds 5 community posts
    |
Dr. Jasmine Cole seeds 1-2 additional community posts within 24 hours
    |
Nora verifies live, card renders, community exists, notifies team
    |
Marketing gets weekly summary of published content
  (Izzy and Zara create social on their own schedule)
```

No human gate required. Agents run the full pipeline autonomously.

---

## Roles and Responsibilities

### Dr. Jasmine Cole (Curriculum Specialist) -- CONTENT CREATOR
- Researches topics and writes Hub content (Quick Wins, lesson resources)
- Creates downloadable PDFs, templates, checklists, toolkits
- Suggests all tags: category, lift rating, roles, danielson domains, topic tags
- Seeds community posts for new content within 24 hours of publish
- Audits existing Hub content for quality and standards alignment
- Flags content gaps or outdated resources to Rae
- Does NOT create social media posts (that is Izzy/Zara)

### Julie Lynn (QA Engineer) -- QUALITY GATE
- Validates every piece of content against the tagging spec before publish
- Checks: title, description, category, lift, roles, danielson, topic_tags, access_tier
- Verifies content is actionable (classroom-ready, not just theory)
- Checks spelling, grammar, PDF formatting
- Routes failed content back to Dr. Jasmine Cole with specific issues
- Does NOT write or edit content

### Maya (Educator UX) -- UX AUDITOR
- Audits how content displays on the Hub (cards, filters, detail pages)
- Catches UX issues: broken links, missing thumbnails, confusing labels
- Verifies new categories and filters work correctly
- Tests the educator experience end-to-end
- Does NOT write content or approve content

### Jasmine (Hub Engagement) -- ENGAGEMENT CHECKS
- Generates engagement checks for Hub LESSONS (courses), not Quick Wins
- Validates check density meets minimum requirements
- Reports coverage status to Rae
- Does NOT create Quick Win content

### Nora (COO) -- ORCHESTRATOR
- Verifies published content is live and rendering correctly
- Confirms community posts exist within 24-48 hours
- Notifies team (Slack) when new content goes live
- Routes content tasks to correct pipeline (Hub vs Social)
- Does NOT create or approve content

### Izzy (Content Marketing) -- SOCIAL ONLY
- Drafts social media posts, Substack pre-screens, reel scripts
- Does NOT create Hub Quick Wins or courses
- May create social posts ABOUT new Hub content (separate pipeline)

### Zara (Social Director) -- SOCIAL SCHEDULING
- Co-edits social drafts with Izzy
- Schedules to Buffer
- Does NOT create Hub content

---

## Tagging Responsibilities

### Who proposes tags:
**Dr. Jasmine Cole** -- when drafting, she includes:
- Suggested category (one of 12)
- Suggested lift rating (LOW/MED/HIGH)
- Suggested roles (teacher/para/leader/coach)
- Suggested danielson domains (1-planning/2-environment/3-instruction/4-professional)
- Suggested topic tags (1-5 from approved list)

### Who validates tags:
**Julie Lynn** -- checks against the tagging spec:
- Is the category correct for what this tool does?
- Is the lift rating accurate (can they really use this with zero prep?)
- Are the roles inclusive enough? (most tools should include teacher)
- Do the danielson domains match?
- Are topic tags from the approved list?

### Who can override tags:
**Rae** -- during approval, can change any tag

### Who enforces tags:
**The database** -- trigger `enforce_quick_win_tags_before_publish` rejects any publish with missing required fields

### Default access tier:
**professional** -- Rae manually promotes to essentials (hand-picked 20) or free (rotating 5)

---

## Content Quality Standards

Every Quick Win must be:
1. **Actionable** -- an educator can use it in their classroom, not just read about a concept
2. **Specific** -- solves a specific problem, not a vague overview
3. **Short** -- the "quick" in Quick Win means 5 minutes or less to use
4. **Complete** -- title, description, PDF, category, roles, danielson domains, community posts. No partial publishes. (Thumbnails NOT needed. Cards use colored category dots.)
5. **Honest about lift** -- if it requires 30 minutes of planning, it is not "Grab & Go"

---

## Ongoing Hub Maintenance

### Weekly (automated):
- Weekly digest to principals (Thursday 7 AM CT, only active partnerships)
- Onboarding reminders (Day 3-45 for new principals)
- Contract expiration sequence (30/14/3/0 days)

### Monthly (Dr. Jasmine Cole):
- Audit 10 random Quick Wins for accuracy and freshness
- Flag any that reference outdated tools, broken links, or stale strategies
- Report content gaps (what categories are thin? what roles are underserved?)

### Monthly (Maya):
- UX audit of Quick Wins page (filters working, cards rendering, variety showing)
- Test search functionality
- Check mobile responsiveness
- Report any educator-facing issues

### As needed (Julie Lynn):
- Pre-publish QA gate for every new Quick Win
- Spot-check community posts for quality (no spam, no off-topic)

### As needed (Nora):
- Verify new content is live after publish
- Route content requests to correct pipeline
- Cancel stale content tasks that have been blocked 7+ days

---

## Content for Hub vs Content for Social

| | Hub Content | Social Content |
|---|---|---|
| What | The Quick Win itself (PDF, template) | Posts ABOUT the Quick Win |
| Creator | Dr. Jasmine Cole | Izzy + Zara |
| Approval | Julie Lynn QA -> Rae | Kristin CMO (or Rae if paused) |
| Where it lives | Learning Hub | Buffer -> Facebook/LinkedIn/IG/TikTok |
| Depends on Kristin? | NO | YES |

---

## When Things Go Wrong

### Content stuck in draft:
Nora checks: is it waiting on Dr. Jasmine Cole, Julie Lynn, or Rae? Pings the blocker.

### QA keeps failing:
Dr. Jasmine Cole and Julie Lynn should discuss the issue directly. If it is a tagging spec question, escalate to Rae.

### Community posts missing after publish:
Nora flags to Dr. Jasmine Cole. This is a mandatory gate. Content should not be promoted on social until community posts exist.

### Wrong category or lift after publish:
Rae + Claude Code can update the database directly. No republish needed.

### Content request from community (educator asks for something):
Log it as a Paperclip task tagged [HUB CONTENT] [COMMUNITY REQUEST]. Dr. Jasmine Cole evaluates and drafts if approved.

---

## API Reference: Content Sync

Base URL: `https://www.teachersdeserveit.com`
Auth: `Authorization: Bearer $PAPERCLIP_SYNC_KEY`

### Check pipeline status
```
GET /api/hub/content-sync?action=get_status
```
Returns: `{ total, published, drafts, drafts_missing_pdf, drafts_missing_description }`

### List draft Quick Wins
```
GET /api/hub/content-sync?action=list_drafts
```
Returns: `{ drafts: [{ id, title, slug, category, has_pdf, has_description, has_tags, has_roles }], count }`

### Create a new draft
```
POST /api/hub/content-sync
{
  "action": "create_draft",
  "title": "Executive Functioning Guide for Paras",
  "slug": "executive-functioning-para-guide",
  "description": "A step-by-step guide for paraprofessionals...",
  "category": "Instructional Strategies",
  "capacity": "med",
  "lift": "med",
  "roles": ["para", "teacher"],
  "danielson_domains": ["3-instruction"],
  "topic_tags": ["executive-functioning", "inclusion"],
  "access_tier": "professional",
  "resource_type": "pdf",
  "duration_minutes": 5
}
```
Valid categories: Lesson Planning, Assessment, Instructional Strategies, Classroom Setup, Classroom Management, Communication, Time Savers, Leadership, Self-Care, Stress Relief, Games, Vocational

### Upload PDF resource
```
POST /api/hub/content-sync
{
  "action": "upload_pdf",
  "id": "uuid-from-create-draft",
  "pdf_base64": "JVBERi0xLjQK...",
  "filename": "executive-functioning-para-guide.pdf"
}
```
Max size: 10MB. Stores at `hub-assets/quick-wins/{id}/{filename}`

### Publish (Rae only, not agents)
```
POST /api/hub/content-sync
{
  "action": "publish",
  "id": "uuid"
}
```
Pre-validates: title, description, topic_tags (1+), roles (1+), file_url (must have PDF). Thumbnails NOT required (cards use colored category dots).
Returns `{ success: false, missing: [...] }` if validation fails.
Triggers auto_seed_community (5 community posts created automatically).

---

## API Reference: Community Seeding

### Seed a community post (after publish)
```
POST /api/hub/community/seed
{
  "quick_win_id": "uuid",
  "user_id": "c3c1c7a9-e084-47b8-9945-15423f154ca9",
  "contribution_type": "tried_it",
  "body": "Used this with my 3rd graders and the structure made such a difference..."
}
```

Valid seeded users (match role to content):
- `c3c1c7a9-e084-47b8-9945-15423f154ca9` (Pam, teacher)
- `7a502d0a-29e9-4490-b330-ea1131311d44` (Michelle, para)
- `4236f26b-88a7-4ae9-abf6-65cd09e9fdd9` (Christine, coach)
- `d532b342-5aff-420d-8201-ae1d6564650c` (Matilde, para)
- `63e924ff-dfc6-4f24-9da2-950dae9b65d9` (Todd, teacher)

Valid contribution types: tried_it, adapted_it, still_trying, got_stuck, didnt_land

Body: 10-2000 characters. Write in the voice of the seeded user's role.

---

## Dr. Jasmine Cole Work Loop

When assigned a content creation task:

1. **Research** the topic using web search and existing Hub content
2. **Draft** the Quick Win content (title, description, PDF body)
3. **Create draft** on the Hub by running this exact curl command (replace the values):
   ```bash
   curl -s -X POST "https://www.teachersdeserveit.com/api/hub/content-sync" \
     -H "Authorization: Bearer $PAPERCLIP_SYNC_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "action": "create_draft",
       "title": "YOUR TITLE HERE",
       "slug": "your-slug-here",
       "description": "Your description here.",
       "category": "Classroom Tools",
       "topic_tags": ["tag1", "tag2"],
       "roles": ["teacher", "para"],
       "danielson_domains": ["3-instruction"],
       "lift": "LOW",
       "access_tier": "professional"
     }'
   ```
4. **Save the ID** from the response JSON (`quick_win.id`). You need it for step 6.
5. **Generate the PDF** and save it to a file on disk.
6. **Upload PDF** to the Hub (replace ID and path):
   ```bash
   PDF_B64=$(base64 -w0 /path/to/your/file.pdf 2>/dev/null || base64 /path/to/your/file.pdf)
   curl -s -X POST "https://www.teachersdeserveit.com/api/hub/content-sync" \
     -H "Authorization: Bearer $PAPERCLIP_SYNC_KEY" \
     -H "Content-Type: application/json" \
     -d "{\"action\":\"upload_pdf\",\"id\":\"YOUR-QUICK-WIN-ID\",\"pdf_base64\":\"$PDF_B64\",\"filename\":\"your-file.pdf\"}"
   ```
7. **Verify** the draft appears on the Hub:
   ```bash
   curl -s "https://www.teachersdeserveit.com/api/hub/content-sync?action=get_status" \
     -H "Authorization: Bearer $PAPERCLIP_SYNC_KEY"
   ```
8. **Report to Rae** via send-report with draft summary for review
9. **Wait** for Julie Lynn QA and Rae approval
10. **After publish**, seed 1-2 community posts:
    ```bash
    curl -s -X POST "https://www.teachersdeserveit.com/api/hub/community/seed" \
      -H "Authorization: Bearer $PAPERCLIP_SYNC_KEY" \
      -H "Content-Type: application/json" \
      -d '{"quick_win_id":"YOUR-ID","user_id":"c3c1c7a9-e084-47b8-9945-15423f154ca9","contribution_type":"tried_it","body":"Post body from a teacher perspective"}'
    ```

**CRITICAL:** Steps 3 and 6 MUST happen. If you skip them, the content exists only inside Paperclip and educators cannot access it. Always run the status check (step 7) after uploading to verify.

Never publish yourself. Never skip the QA gate. Always suggest tags per the quick-win-tagging spec.

**Design note:** Quick Win cards do NOT use thumbnail images. Cards display with colored category dots (Option C design). Do not generate or upload thumbnails. Focus on PDF quality and accurate tagging.
