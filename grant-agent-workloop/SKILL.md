# TDI-Grant-Agent-Workloop-Skill
**Created July 9, 2026. How grant-writer agents (Vanessa, Amara) pick up portal-assigned work via the funding sync API and act on it. Pairs with grant-followup-engine, grants-catalog, and grant-positioning skills.**

---

## Heartbeat work loop — how to pick up portal-assigned work

On each heartbeat, check the funding portal for work that Bella (or Rae) has assigned to you, do it to standard, and push the result back. The portal and you share one database via the **Funding Sync API** (`/api/funding/sync`, bearer `PAPERCLIP_SYNC_KEY`).

### Step 1 — Ask what's assigned to you
Call the **`find_work`** action with your agent name: GET /api/funding/sync?action=find_work&agent=vanessa
(Amara uses `&agent=amara`.) It returns only *actionable* work assigned to you, each item tagged with a `request_type`.

### Step 2 — For `request_type: 'draft_narrative'`
An opportunity Bella asked you to draft a grant narrative for. Note: `find_work` only returns draft work for opportunities whose **funding window is verified open** — so if you receive it, the window is confirmed and it's safe to draft.

1. Mark it in progress: `update_opportunity` → set `narrative_status = 'drafting'` (portal shows "Vanessa is drafting…").
2. Draft the narrative to the **Grant Excellence & QA Spec** and **grant-positioning** standards, per line item, using **grants-catalog** knowledge for that funder. Position TDI as an implementation partner, use the funder's trigger phrases, avoid the kill-words.
3. Push the draft back: `update_narrative` with your drafted content.
4. Mark ready for review: `update_opportunity` → set `narrative_status = 'review'` (portal shows Bella a "Draft ready — review" + Approve button).
5. **Stop there.** Do NOT mark it `ready` or send anything to a client. Bella's approval (setting `ready`) and any client send are human-gated in the portal. You draft; Bella approves.

### Step 3 — For `request_type: 'research_funders'`
Bella asked you (Amara) to find more funding sources for a pursuit.
1. Mark in progress: set `research_status = 'researching'`.
2. Research using **grants-catalog** + the **Timing & Windows** knowledge — prioritize sources whose windows are currently open (don't surface a source whose window closed for the year unless it's for next cycle). Apply the A/B/C/D diversification rule (pair fast local with slow federal).
3. Push each new source in via `create_opportunity` (with plan_category, amount estimate, and — if known — window_opens/window_closes; default window_status to `unknown` so a human verifies before the engine acts).
4. Mark done: set `research_status = 'found'`.
5. **Stop there.** New opportunities surface in the portal for Bella to review and pursue. You find and propose; humans decide.

### Hard rules (same as the rest of the system)
- **Never** advance a narrative to `ready` — that's Bella's approval.
- **Never** send anything to a client — all client sends are human-gated in the portal (allowlist + window-gate + Bella's click).
- **Respect the window-gate:** don't draft for a closed/unknown window (find_work enforces this for you); when researching, default new sources to `window_status='unknown'`.
- Escalations, approvals, and policy questions → route to Rae (per your existing escalation instructions).

### The loop in one line
`find_work(you)` → mark in-progress → do it to spec → push result back → mark ready-for-review → stop. Bella approves; humans send.
