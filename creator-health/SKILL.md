# TDI-Creator-Health-Skill
**Created July 13, 2026. How Anne Marie monitors Creator Studio creators, identifies who needs attention, and drafts check-in notes for Bella's approval. Uses the Creator Studio Sync API.**

---

## Heartbeat work loop -- how to monitor creator health

On each heartbeat, check the Creator Studio for creators needing attention, draft appropriate check-in notes, and flag urgent situations. The portal and you share one database via the **Creator Studio Sync API** (`/api/creator-studio/sync`, bearer `PAPERCLIP_SYNC_KEY`).

### Step 1 -- Ask what needs attention
Call the **`find_work`** action: `GET /api/creator-studio/sync?action=find_work&agent=anne-marie`

It returns work items tagged with a `request_type`:
- `stalled_creator` -- creator hasn't had portal activity in 14+ days (with severity: medium/high/critical)
- `approval_waiting` -- a milestone submission is waiting for TDI review (3+ days)
- `overdue_target` -- creator's target completion date has passed by 7+ days

The API already filters out:
- Creators with active re-engagement email sequences (steps 0-4) -- the drip is handling them
- Creators you already acted on in the past 7 days -- no piling on
- Published and paused creators

### Step 2 -- For `request_type: 'stalled_creator'`
A creator has gone quiet. Draft a warm, personalized check-in note.

1. Get their full profile: `GET /api/creator-studio/sync?action=get_creator&creatorId=[id]`
2. Review their milestones to understand exactly where they're stuck.
3. Draft a check-in note that:
   - References their specific project by name (e.g., "How's the outline for 'Classroom Management Strategies' coming along?")
   - Acknowledges that life happens (never guilt-trip)
   - Offers a specific next step or asks a specific question (not generic "how are you?")
   - Keeps it to 2-3 sentences max
   - Signs off as "The TDI Team" (never "Anne Marie" or "AI")
4. Push the draft: `POST /api/creator-studio/sync` with `{ action: 'draft_note', creator_id, content, reason: 'Stalled [X] days at [milestone]' }`
5. **Stop there.** Bella reviews and approves the note before the creator sees it.

For critical stalls (60+ days), also flag: `POST` with `{ action: 'flag_attention', creator_id, reason: 'Critical: [X] days inactive, re-engagement sequence exhausted' }`

### Step 3 -- For `request_type: 'approval_waiting'`
The TDI team owes a creator a response on their milestone submission.

1. Flag the creator for Bella's attention: `POST` with `{ action: 'flag_attention', creator_id, reason: 'Milestone [name] waiting [X] days for TDI review' }`
2. Do NOT draft a note to the creator -- this is a TDI-side action item, not a creator problem.

### Step 4 -- For `request_type: 'overdue_target'`
A creator's target date has passed.

1. Get their full profile to check context.
2. If they have recent activity (stalled < 14 days), skip -- they're actively working, just behind schedule.
3. If stalled AND overdue, this is already covered by the stalled_creator flow. Don't double-act.
4. If active but significantly overdue (30+ days past target), draft a gentle note suggesting they update their projected date in the dashboard.

### Hard rules
- **Never** make a note visible to creators directly -- always `draft_note` with `visible_to_creator: false`. Bella approves.
- **Never** send emails to creators -- only draft portal notes. The existing email automations (reminders, re-engagement, newsletter) handle email.
- **Never** change milestone status -- only flag and draft.
- **Never** draft a note if you already have a pending draft for that creator (API returns 409).
- **Sign off as "The TDI Team"** in note content, never as Anne Marie or AI.
- **Max 5 drafts per heartbeat** -- prioritize critical stalls, then approval waits, then overdue.
- If something looks wrong or unusual (e.g., a creator has been stalled 90+ days with no re-engagement record), escalate to Rae via `[APPROVE] [RAE NEEDED]` issue.

### Note tone guide
- Warm, not corporate. These are educators, not clients.
- Specific, not generic. Reference their project name, their current milestone, their content path.
- Brief. 2-3 sentences. No walls of text.
- Empowering. "Your outline was really strong" not "You need to finish your outline."
- Example: "Hey Catherine! Just checking in on your course 'Restorative Practices for Middle School.' You're so close -- your outline was approved and the next step is recording your first video module. No rush, but we're here if you want to hop on a quick call to plan it out. -- The TDI Team"

### The loop in one line
`find_work(anne-marie)` -> triage by priority -> draft notes for stalls / flag approvals -> stop. Bella approves; creators see notes only after human review.
