# TDI-Grant-Followup-Engine-Skill
**Created July 9, 2026. Covers the funding follow-up engine, the submitter/backup escalation gate, and lead fit scoring. These are new systems built in the admin portal (funding_action_items, pursuit_gate, four-factor scoring) — none existed in the skill library before.**

---

## Purpose

This skill gives TDI agents the *process* behind the grant funding workflow's three newest systems: the follow-up engine that keeps every funding pursuit moving, the escalation gate that ensures a named submitter and backup exist, and the fit-scoring router that prioritizes (never rejects) interested schools. This skill defines the *cadence, rungs, and mechanics* that execute the policy. Nora owns orchestration; Rae owns final decisions.

Use it when tracking grant action items, deciding when/whom to escalate a stalled funding task, or scoring an inbound/outbound school lead.

---

## Part 1 — The follow-up engine (funding action items)

Every funding action item is always either progressing on schedule or actively surfaced for attention — never silently rotting. The system remembers so people don't have to.

**Color state (auto-computed):**
- **Green** — on track, not yet in the reminder window
- **Yellow** — within the reminder lead window (approaching due)
- **Red** — overdue

**Reminder lead time (scaled by action size):**
- Light action (send an email, make a call): remind 1–2 business days before due
- Standard action: 3 business days before
- Heavy action (assemble a packet, gather client docs): 5–7 business days before

**Nudge rule:** if an item goes past due, it gets nudged (at most once per day) — it never lapses silently. The reminder re-fires and escalates rather than disappearing.

**Funder-nudge cadence (outward check-ins, by source type — do NOT over-nudge formal funders):**
- Federal / state (Plan A/B): light, ~every 3–4 weeks; none during a formal review blackout
- Association (Plan C, e.g. NEA): ~every 2–3 weeks
- Local / community / corporate (Plan D): ~every 1–2 weeks, relational — these can genuinely be moved with a warm check-in

---

## Part 2 — The submitter/backup escalation gate

Every grant pursuit must have a named submitter AND a named backup before submission work proceeds. This is the lesson from the Allenwood stall: the submitter went silent and there was no backup, so the pursuit died.

**The escalation ladder (rung by rung, fires automatically on non-response):**
1. **Submitter** — the named person who actually submits
2. **Backup** — the named second person (required; no backup = the gate is not satisfied)
3. **Admin sponsor** — the administrator who signed the contract
4. **Rae** — final escalation endpoint

**Rules (as built and verified in the admin portal engine):**
- A rung advances only when the current rung hasn't acted within its window (**non-response**), not merely because raw time passed. The engine tracks the last escalation time and waits a full window before climbing.
- **Window size scales with runway** (calendar days from now to the due date): runway >14 days = 5 business days per rung; 7–14 days = 3 business days; under 7 days = 1 business day.
- **Runway floor:** any runway of 0 or negative (e.g. items created with same-day due dates) is treated as a floor of 7 calendar days, so the ladder steps through rungs instead of jumping straight to the top. (Without this, bulk/same-day items instantly max out to Rae — the specific bug we fixed.)
- **Note — two distinct mechanisms, don't confuse them:** (a) *reminder lead time* (Part 1) is how far *before* a due date the owner first gets pinged, scaled by action size; (b) *escalation window* (here) is how long a rung has to respond *after* going overdue before it climbs. They are separate.
- **Collapse duplicate rungs:** if the backup and admin sponsor are the same person (common at small schools — e.g. Allenwood, where Dr. Porter is both), skip the redundant rung; notify the distinct real people only, then Rae. Null/empty rungs are also skipped.
- A submission that only reached a district inbox is NOT confirmed submitted to the funder. For district-routed paths (Plan A/B), confirm it advanced past district routing before marking it done (the Title II-A / Kevin Thompson bounce lesson).

---

## Part 3 — Lead fit scoring (routes priority, never rejects)

Every interested school is served. The four-factor score sets priority and approach — it never decides whether TDI helps.

**Four factors, each 0–25 (total 0–100):**
- **Fit** — how well the school matches TDI's model
- **Pain** — how acute their need (retention, culture, SpEd gaps, etc.)
- **Warmth** — engagement / relationship signal
- **Funding** — realistic path to money

**Priority lanes (starting bands; Rae tunes over time):**
- **Tier 1 (70–100)** — move fast, standard path
- **Tier 2 (45–69)** — work in normal sequence
- **Tier 3 (below 45)** — still served, usually via a creative play + often a January-cycle nurture

**The router rule:** a low *funding* score never disqualifies — it triggers a **creative funding hunt** (local/community sources, braided funding, phased delivery, a smaller start now with a bigger dream contract later). Low warmth → relationship-building moves. Low fit → route to the right shape of support. Every weak factor summons a tailored play; none withdraws help.

---

## Escalation Flags
- Funding action item hits red (overdue) with no submitter response → escalation ladder engages automatically; surface to Nora if it reaches the Rae rung
- A pursuit missing a named backup → flag: the gate is not satisfied, submission work should not proceed without it
- District-routed submission stuck (reached district but not funder) → flag as stalled, escalate to admin sponsor, do not mark submitted
- Low-funding-score school → do not deprioritize; trigger the creative funding hunt (local/community sourcing — route to whoever owns local/foundation grant research)

## Maintenance
New skill as of July 9, 2026. Nora owns orchestration policy. Cross-references: grants-catalog (source library + A/B/C/D strategy), grant-positioning (narrative standards), partner-health (scoring-model structure analog).
