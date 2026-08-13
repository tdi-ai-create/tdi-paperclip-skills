# TDI-Creator-Recruitment-Skill
**Created July 19, 2026. How Anne Marie identifies content gaps, researches potential creators, drafts outreach, and manages the recruitment pipeline. Uses the Creator Recruitment Sync API.**

---

## Weekly work loop: how to recruit creators

Every Monday (or when assigned a recruitment task), identify content gaps, research candidates, and draft outreach. The portal and you share one database via the **Creator Recruitment Sync API** (`/api/creator-recruitment/sync`, bearer `PAPERCLIP_SYNC_KEY`).

### Step 1: Analyze content gaps

Cross-reference three data sources to identify where the Hub needs content:

**Source A: Hub content inventory**
Check what courses and quick wins exist. Categories with low content are gaps. Use your knowledge of the Hub structure. Key categories are:
- Classroom Management, Communication, Instructional Strategies, Lesson Planning, Assessment, Classroom Setup, Time Savers, Leadership, Self-Care, Stress Relief, Vocational, SPED/Inclusion, Early Childhood, Technology Integration, ELL/Multilingual

**Source B: Sales pipeline pain points**
Check the sales CRM via the Funding Sync API or by reviewing Olivia's daily briefs. What are schools asking for? If 10+ leads mention "paraprofessional training" and we have minimal content there, that's a CRITICAL gap.

**Source C: Current creator coverage**
Check which topics active creators are already covering via the Creator Studio Sync API (`find_work` or `get_dashboard`). If a gap has 0 creators and 0 content, it's higher priority than one where someone is already working on it.

**Submit each gap:**
```
POST /api/creator-recruitment/sync
{
  "action": "submit_gap",
  "category": "Assessment",
  "priority": "critical",
  "demand_signal": "12 active sales leads mention assessment PD. Hub has 6 quick wins but 0 courses.",
  "hub_course_count": 0,
  "hub_quick_win_count": 6,
  "sales_mentions": 12,
  "recommended_content_path": "download",
  "notes": "Fastest fill: recruit for 2-3 assessment downloads while searching for a course creator."
}
```

Priority levels:
- **critical**: Active sales leads asking for this AND no courses/creators covering it
- **high**: Hub users engage heavily in this category but content is thin, OR creator covering this is stalled 30+ days
- **medium**: Category underrepresented but no immediate sales pressure
- **low**: Nice to have, not urgent

### Step 2: Research candidates

For each HIGH or CRITICAL gap, research potential creators from these sources:

**Hub power users:** Look for teachers who completed many lessons in the gap category, have streaks, earned field notes. These are warm, because they already know TDI.

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
  "source_detail": "Completed 12 lessons, 45-day streak, left detailed responses on assessment content",
  "why_good_fit": "Deep assessment expertise demonstrated through Hub engagement. Completed every assessment quick win with thoughtful responses. Active on Instagram with assessment content.",
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
- **Never contact candidates.** You research and recommend. Bella is the human voice.
- **Always link candidates to a gap.** Every candidate should be tied to a content gap. If you can't articulate which gap they fill, don't submit them.
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

`Analyze gaps (Hub + sales + creators)` -> `research candidates for HIGH/CRITICAL gaps` -> `draft outreach with evidence` -> `submit to portal for Bella` -> `monitor pipeline health` -> repeat weekly.
