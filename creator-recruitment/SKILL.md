# TDI-Creator-Recruitment-Skill
**Created July 19, 2026. How Anne Marie identifies content gaps, researches potential creators, drafts outreach, and manages the recruitment pipeline. Uses the Creator Recruitment Sync API.**

---

## Weekly work loop: how to recruit creators

Every Monday (or when assigned a recruitment task), read the gap board, research candidates, and draft outreach. The portal and you share one database via the **Creator Recruitment Sync API** (`/api/creator-recruitment/sync`, bearer `PAPERCLIP_SYNC_KEY`).

### Step 1: Read the gap board, do not rebuild it

**The portal now detects content gaps on its own.** A scheduled Hub content scan runs every Monday, counts published courses and quick wins in every category, and writes the gaps itself. You no longer inventory the Hub.

Start every cycle by reading what is already on the board:
```
GET /api/creator-recruitment/sync?action=get_gaps
```

Each gap returns `category`, `priority`, `hub_course_count`, `hub_quick_win_count`, `candidate_count`, and `identified_by`. Work the CRITICAL and HIGH gaps that have `candidate_count: 0` first. Those are the ones nobody is covering.

**Only submit a gap the scan cannot see.** The scan counts catalog content. It cannot hear a sales call. If schools keep asking for something, that is real demand the counts will never show:
```
POST /api/creator-recruitment/sync
{
  "action": "submit_gap",
  "category": "Paraprofessional Support",
  "priority": "critical",
  "demand_signal": "12 active sales leads asked for para training in the last 30 days.",
  "sales_mentions": 12,
  "recommended_content_path": "download",
  "notes": "Demand heard on sales calls. Not visible in the catalog counts."
}
```

Never resubmit a category that already has an active gap, and never change the priority on a gap where `identified_by` is `admin`, because a human set that on purpose.

Priority levels (the scan applies these; use the same scale for demand you submit):
- **critical**: No courses at all, or leads asking with nothing to point them at
- **high**: Content is thin and no creator is covering it
- **medium**: Category underrepresented but no immediate pressure
- **low**: Healthy. The scan retires these on its own.

### Step 2: Research candidates

This is the part only you can do, and it is now the main job. The portal finds the gaps. You find the people.

**Do not treat Hub community response counts as evidence of expertise.** Those tables contain seeded community content attributed to real accounts, so a high response count does not mean that person wrote those posts. Course completion data is also too new to lean on. If you cite Hub engagement as a reason, cite something you verified directly and say what you checked.

For each HIGH or CRITICAL gap, research potential creators from these sources:

**Hub members:** Warm, because they already know TDI. Read what a person actually wrote and judge the substance of it. Do not rank people by response count or streak length, for the reason above. If you cannot point to specific writing of theirs that shows expertise in the gap topic, they are not a candidate yet.

**Social media:** Search for teachers creating content about the gap topic on TikTok, Instagram, LinkedIn. Look for: follower count, content quality, teaching expertise, engagement.

**Substack readers:** Look for engaged subscribers who respond to posts about the gap topic.

**Referrals:** Check if any existing creators have mentioned colleagues who'd be a good fit.

**For each candidate, draft a profile and outreach:**
```
POST /api/creator-recruitment/sync
{
  "action": "submit_candidate",
  "name": "Maria Chen",
  "email": "maria.chen@school.edu",
  "school_org": "Lincoln Elementary",
  "role": "3rd Grade Teacher / Assessment Lead",
  "expertise_area": "Formative assessment, data-driven instruction",
  "gap_id": "[UUID of the Assessment gap]",
  "content_path": "download",
  "source": "hub_user",
  "source_detail": "Wrote a detailed adaptation on the exit ticket quick win explaining how she reworked it for a co-taught room. Verified by reading the post.",
  "why_good_fit": "Her writing on assessment shows she has actually built and revised these systems, not just used them. Runs an assessment focused Instagram with original templates.",
  "social_url": "https://instagram.com/mariachen_teach",
  "outreach_draft": "Hi Maria, I noticed you've been crushing it on the Hub, especially the assessment content. Your responses show real depth and expertise. We're looking for educators like you to create downloadable resources for our community. It's a 50/50 revenue share, we handle all the production, and you'd be helping thousands of teachers. Would you be open to a quick conversation?\n\nThe TDI Team"
}
```

### Step 3: Content-path-aware outreach

Tailor the pitch based on what you're recruiting for:

**Download/Quick Win (low commitment):**
- Pitch: "Turn your best strategy into a downloadable resource"
- Commitment: 2-3 weeks
- Best for: First-time creators, social media finds, people who seem interested but might be intimidated by a full course

**Blog (lowest commitment):**
- Pitch: "Share your expertise with our educator community"
- Commitment: 1-2 weeks
- Best for: People who are unsure, testing the waters

**Course (high commitment):**
- Pitch: "Build a course that reaches thousands of educators"
- Commitment: 3-6 months
- Best for: Hub power users with demonstrated expertise, people who've already created content elsewhere

**Strategy:** For cold outreach or social media finds, suggest a download first. If they do well, invite them to create a course. Don't lead with a 6-month commitment to someone who's never heard of TDI.

### Step 4: Monitor pipeline health

Check the pipeline stats periodically:
```
GET /api/creator-recruitment/sync?action=get_stats
```

Watch for:
- **critical_gaps_without_candidates > 0**: Every critical gap should have at least one candidate. If not, prioritize research.
- **Candidates stuck in "outreach_sent" for 14+ days**: System should auto-follow-up, but flag if 3+ candidates are non-responsive.
- **Conversions this month = 0**: If nobody is converting, review the outreach quality or gap priorities.

### Hard rules

- **Never send outreach directly.** Always draft via `submit_candidate` with `outreach_draft`. Bella approves and sends.
- **Always include an `outreach_draft`.** Submitting a candidate without one leaves Bella a blank page. The draft is the work product, not the name.
- **Never contact candidates.** You research and recommend. Bella is the human voice.
- **Always link candidates to a gap.** Pull the `gap_id` from `get_gaps` and pass it. If you can't articulate which gap they fill, don't submit them.
- **Never claim engagement you did not verify.** Every specific in `why_good_fit` and `source_detail` must be something you actually read.
- **Quality over quantity.** 3 strong candidates with specific evidence > 10 generic suggestions.
- **Respect the "Revisit" status.** If Bella marks someone as "revisit with date," don't re-suggest them before that date.
- **Max 5 candidates per gap per week.** Don't flood the pipeline. Bella has limited bandwidth.
- **Sign outreach as "The TDI Team"** on its own line, never as Anne Marie or AI. No dash before the name.

### Outreach tone guide

- Warm, not corporate. These are teachers, not enterprise clients.
- Specific, not generic. Reference their actual work, their Hub engagement, their social content.
- Brief. 3-4 sentences max for initial outreach.
- Empowering. "Your students are lucky to have you" not "We need content creators."
- Honest about the model. 50/50 revenue share, we handle production, they bring the expertise.

### The loop in one line

`Read the gap board` -> `research candidates for the CRITICAL and HIGH gaps with no candidates` -> `draft outreach with verified evidence` -> `submit to portal, which pings Bella in #bella-actions` -> `monitor pipeline health` -> repeat weekly.

Submitting a candidate is the handoff. The portal posts it to Bella with a link straight to the approval screen, so once you submit, the work is with her and not with you.
