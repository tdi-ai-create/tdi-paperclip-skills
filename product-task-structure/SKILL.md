# TDI-Product-Task-Structure-Skill
**Lead: Marcus Thompson — Reviewed by Rae Hughart — Last updated March 30, 2026**

---

## Purpose

This skill document explains how TDI's product function works so Erin Pope can coordinate product work accurately. TDI does not run traditional sprint ceremonies. The build process is CCP-driven. Every feature ships through a defined loop. Erin Pope uses this document to track product work, surface blockers, and protect Rae's time on product decisions.

---

## How TDI builds — the CCP model

TDI's development process works as follows:

1. Rae and Marcus T. identify what needs to be built
2. A Claude Code Prompt (CCP) is written as a structured markdown file specifying exactly what to build
3. Chris Copypaste executes the CCP using Claude Code
4. Marcus T. reviews the output against the spec
5. If correct, the milestone is marked complete. If not, a follow-up CCP is written.

Before any CCP is written for a significant feature, Rae reviews an HTML visual mockup and approves the direction. Rae does not see code — she sees what it will look like and approves from there.

All completed CCPs and deliverables are pushed to the GitHub repo. The Admin Portal lives at teachersdeserveit.com/tdi-admin.

---

## Current build state — March 30, 2026

**Admin Portal — active build**

| Area | Status | Notes |
|---|---|---|
| Creator Studio | ~82% complete | Full pipeline built. Pub dates, stalled flow, target timeline complete. |
| Learning Hub | ~42% complete | Visual elevation complete. Analytics infrastructure built. UI translation debug ongoing. |
| Operations Command Center | Active | Tabbed layout live. All 8 partner contracts seeded with real data. |
| Sales CRM | Active | 131 opportunities, 82 contacts imported. GHL cutover target June 1, 2026. |
| Lead Dashboard | ~87% complete | Near complete. |
| Partner Dashboards | Stable | 7 legacy dashboards stay untouched through summer 2026. |
| Desi AI | Pre-build | MVP scope defined. No active build yet. |
| Roosevelt Hub pilot | Scheduled | Hub-only pilot April-June 2026. Dashboard experience needed. |
| Full platform launch | Scheduled | Target: July 1, 2026. |

---

## Recurring coordination tasks — weekly

**CCP queue review**
Marcus T. reviews what's been built, what's in progress, and what needs to be written next. Prioritized against the July 1 launch date. Happens at the start of each week.

**Build status check with Chris**
Marcus T. and Chris sync on what's deployed, what's blocked, and what's next. Chris flags anything that didn't work as expected after a CCP execution. Happens as needed, at minimum weekly.

**Rae decision queue**
Any feature requiring Rae's approval before build proceeds gets staged for her review. Marcus T. frames the decision, provides context, and presents a clear yes/no ask. Rae should never receive a product decision without enough context to decide in under 2 minutes.

---

## Recurring coordination tasks — milestone-triggered

**CCP execution review**
When Chris completes a CCP, Marcus T. reviews the output against the original spec. If off: follow-up CCP written. If correct: milestone marked complete.

**Rae mockup approval**
Before any significant feature CCP is written, Rae sees and approves an HTML visual mockup. This is non-negotiable for anything that changes what a school partner sees.

**Partner dashboard update coordination**
When a new school partner goes live or a platform change affects partner visibility, the relevant partner dashboard gets updated. Marcus T. coordinates timing with Priya.

---

## What "blocked" means precisely

**Genuine blocker — surfaces to Rae:**
- Rae's approval is required before build can proceed
- A product decision affects a school partner relationship
- A Desi AI scope or positioning decision needs Rae's sign-off
- A feature that changes what a school partner sees needs Rae approval before deployment
- A partner dashboard change could affect a live school
- Any new build area not previously scoped

**Not a blocker — stays with Marcus T.:**
- Chris is mid-CCP execution and hasn't pushed yet
- A CCP needs a second pass to fix a minor issue
- A build is taking longer than estimated
- A dependency between two features needs sequencing
- A feature works but needs UX polish
- Supabase migrations and schema decisions

---

## What Rae needs to unblock a product decision

When Marcus T. stages a product decision for Rae, it must include:
- What the decision is in one sentence
- Why it's blocked — what specific question needs her answer
- The two or three options with a recommendation
- What happens if she approves option A vs. option B
- How quickly the decision is needed

Rae should never have to ask for more context on a product decision. Everything she needs should be in the task when it arrives.

---

## Desi AI — current scope and status

**Desi v1 — MVP scope (agreed)**
Search and retrieval only. Not generation. Desi answers teacher questions by finding relevant content from TDI's knowledge base. Does not generate new coaching content.

No active build yet. Pre-build planning stage as of March 30, 2026.

**Desi v2 — deferred**
Implementation coaching — Desi actively guides teachers through applying what they've learned. Deferred to avoid scope creep. No timeline set.

**Desi's position**
Desi is available in the Sustain phase of the Blueprint. Priced at $30,000 flat fee. It is TDI's competitive moat and primary differentiator in the investor narrative. Any scope or positioning change routes to Rae immediately.

**Desi milestones — to be confirmed by Marcus T.**

| Milestone | Target date | Status | Rae approval needed? |
|---|---|---|---|
| Knowledge base architecture defined | TBD | Not started | Yes |
| RAG search prototype | TBD | Not started | No |
| Chat UI built | TBD | Not started | No |
| First school pilot | TBD | Not started | Yes |
| v1 launch | TBD | Not started | Yes |

---

## What goes to Rae vs. stays with Marcus T.

**Always goes to Rae:**
- Any Desi scope or positioning change
- Any feature that changes what a school partner sees
- Any product decision with pricing implications
- The Roosevelt Hub pilot dashboard experience
- The July 1 launch go/no-go decision
- Any new build area not previously scoped

**Stays with Marcus T.:**
- Routine CCP execution and review
- Build sequencing and dependency management
- Mockup iteration with Chris before Rae sees it
- Feature QA and polish after CCP execution
- Bug fixes and minor UX adjustments
- Supabase migrations and schema decisions

---

## Key technical context

- Platform: Next.js / Supabase / Vercel
- URL: teachersdeserveit.com
- Admin portal: teachersdeserveit.com/tdi-admin
- CRM: GHL (transitioning to internal Supabase-based CRM, cutover June 1, 2026)
- Developer: Chris Copypaste — executes all CCPs via Claude Code
- Database: Supabase with Postgres

---

## Flags for Erin Pope

- Desi milestone delay identified → same-day surface to Marcus T. and Rae
- Any Desi scope question → immediate route to Rae's approval queue
- CCP stuck without movement for 48+ hours → flag to Marcus T.
- Build area approaching July 1 with significant incomplete work → flag to Rae
- Partner dashboard change pending → confirm Priya is in the loop before Chris deploys
