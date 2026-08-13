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

**Safe to edit directly:** `category`, `lift`, `title`, `description`, `topic_tags`,
`roles`, `danielson_domains`, `objectives`, `duration_minutes`, translations.

**NEVER edit directly:** `is_published` or `status`. Publishing goes through
`POST /api/hub/content-sync` (action: `publish`) and nothing else.

Why this matters. The Hub renders on `is_published` alone. `status` is workflow
metadata for the admin and QA views and no educator-facing query ever reads it.
Writing `status = 'published'` on its own therefore makes the admin say published
while the item stays invisible to every educator, and it raises no error. That is
exactly what happened to five quizzes on 2026-08-06. They sat dark for a week while
the daily health check misreported them as drafts stuck in the pipeline.

Migration 108 now normalizes that state at the database level, so the bad write
self-corrects rather than sticking. Do not rely on it. Use the publish API.

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

## Quick Win PDF Content Structure

Every Quick Win PDF MUST contain these sections. Do not create placeholder or generic content. Each section must be specific to the topic and actionable.

| Section | What to Write | Min Length |
|---|---|---|
| **Overview** | What this is, who it's for, why it matters | 2-3 sentences |
| **Why This Works (rationale)** | Research basis, framework, or pedagogical reasoning | 1 full paragraph |
| **Steps** | Numbered, specific instructions an educator can follow without interpretation | 3-7 steps |
| **Adapt It** | How to customize for different grades, roles, or contexts | 2-3 bullets |
| **Try It Today** | One concrete action the educator can take this week | 1-2 sentences |
| **Reflection** | A question to help educators assess impact after trying it | 1 question |

**Quality bar:** If you removed the title, could an educator still use the tool effectively from the content alone? If not, it needs more detail.

---

## Dr. Jasmine Cole Work Loop

When assigned a content creation task:

1. **Research** the topic using web search and existing Hub content
2. **Draft** the structured content following the Content Structure table above. Write ALL six sections with real, specific content.
3. **Create draft** on the Hub:
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
       "access_tier": "professional",
       "quick_win_type": "download"
     }'
   ```
4. **Save the ID** from the response JSON (`quick_win.id`). You need it for step 5.
5. **Generate the branded PDF** using the generate_pdf action (the server creates a professionally designed PDF from your structured content):
   ```bash
   curl -s -X POST "https://www.teachersdeserveit.com/api/hub/generate-pdf" \
     -H "Authorization: Bearer $PAPERCLIP_SYNC_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "id": "YOUR-QUICK-WIN-ID",
       "sections": {
         "overview": "What this tool is and who it helps...",
         "rationale": "Research shows that... This approach works because...",
         "steps": [
           "Step 1: Do this specific thing...",
           "Step 2: Then do this...",
           "Step 3: Follow up with..."
         ],
         "adapt_it": [
           "For paras: Focus on...",
           "For new teachers: Start with..."
         ],
         "try_it": "This week, try using this with one class period and note what happens.",
         "reflection": "After trying this for a week, what changed about how your students respond?"
       }
     }'
   ```
6. **CRITICAL: Generate the actual TOOL PDF.** This is the printable resource educators use. Choose the right tool_type and write the actual content (not a guide about the content):
   ```bash
   curl -s -X POST "https://www.teachersdeserveit.com/api/hub/generate-pdf" \
     -H "Authorization: Bearer $PAPERCLIP_SYNC_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "action": "generate_tool",
       "id": "YOUR-QUICK-WIN-ID",
       "tool_type": "form",
       "tool_content": {
         "title": "The actual resource title",
         "description": "Brief instructions for the user",
         "sections": [
           {
             "heading": "Section Name",
             "fields": [
               {"label": "Field to fill in:", "type": "box"},
               {"label": "Another field:", "type": "line"}
             ]
           }
         ]
       }
     }'
   ```

   **Tool type decision:**
   | If the title says... | Use tool_type | What the PDF contains |
   |---|---|---|
   | Checklist, Protocol, Steps | `checklist` | Checkboxes with items, grouped by section |
   | Template, Planner, Log, Form | `form` | Labeled fields with lines/boxes to fill in |
   | Cheat Sheet, Quick Reference, Scripts, Guide | `reference_card` | Dense info boxes, key phrases, organized reference |
   | Kit, Toolkit, Pack, Collection | `toolkit` | Numbered items with full descriptions and instructions |

   **The tool must be THE THING an educator prints and uses.** Not instructions about how to do something. If the title says "Ice Breaker Kit" the tool PDF must contain actual ice breakers with instructions, timing, and group sizes. If it says "Check-in Protocol" the tool PDF must be a fillable check-in form.

7. **Verify** both PDFs exist:
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

**CRITICAL:** Steps 3, 5, AND 6 MUST ALL happen. Every Quick Win needs TWO PDFs:
- Step 5 creates the **guide** (why this works, how to use it)
- Step 6 creates the **tool** (the actual printable resource)
If you skip step 6, educators download an essay instead of a usable tool. This is a critical failure.

Never publish yourself. Never skip the QA gate. Always suggest tags per the quick-win-tagging spec.

**Design note:** Quick Win cards do NOT use thumbnail images. Cards display with colored category dots (Option C design). Do not generate or upload thumbnails.

**Format rule:** All new Quick Win downloads MUST use the generate_pdf action. Never upload raw PDF files or HTML files. The server handles all PDF design and branding.
