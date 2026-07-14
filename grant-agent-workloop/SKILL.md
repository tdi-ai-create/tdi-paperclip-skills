# TDI-Grant-Agent-Workloop-Skill
**Created July 9, 2026. Updated July 14, 2026: agents now create Google Docs for narratives.**
**How grant-writer agents (Vanessa, Amara) pick up portal-assigned work via the funding sync API and act on it. Pairs with grant-followup-engine, grants-catalog, and grant-positioning skills.**

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

3. **Create a Google Doc** with the narrative:
   ```
   POST /api/paperclip/save-to-drive
   Authorization: Bearer $PAPERCLIP_REPORT_SECRET
   Content-Type: application/json
   {
     "title": "<Grant Name> - <School Name>",
     "content": "<the narrative, in markdown>",
     "format": "doc",
     "folder": "TDI Grant Narratives",
     "agentName": "<your agent name>"
   }
   ```
   Returns: `{ "success": true, "url": "https://docs.google.com/document/d/.../edit", "title": "...", "folder": "TDI Grant Narratives" }`

   The URL is what Bella and Julie will open, read, and edit. The Doc is the living document.

4. **Push BOTH the URL and the text back** via `update_narrative`:
   ```
   POST /api/funding/sync
   Authorization: Bearer $PAPERCLIP_SYNC_KEY
   Content-Type: application/json
   {
     "action": "update_narrative",
     "opportunityId": "<opportunity id from find_work>",
     "narrativeStatus": "review",
     "narrativeUrl": "<url from save-to-drive>",
     "narrativeContent": "<plain text of the narrative>",
     "note": "Draft created as Google Doc by <agent name>"
   }
   ```

   **Both fields are required.** `narrativeUrl` is the Google Doc link that Bella opens. `narrativeContent` is the plain-text copy for the portal's inline reader and search. Always send both.

5. **If save-to-drive fails:** still call `update_narrative` with `narrativeContent` alone (no `narrativeUrl`). Include the error in the note: `"Draft saved as text only — Google Doc creation failed: <error>"`. A failed Doc must never mean a lost draft. The portal can still display the inline text, and the Doc can be created later.

6. **Stop there.** Do NOT mark it `ready` or send anything to a client. Bella's approval (setting `ready`) and any client send are human-gated in the portal. You draft; Bella approves.

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
- **Always push both URL and text** when creating a Google Doc draft. If the Doc fails, push text alone — never lose the draft.
- Escalations, approvals, and policy questions → route to Rae (per your existing escalation instructions).

### The loop in one line
`find_work(you)` → mark in-progress → draft to spec → create Google Doc → push URL + text back → mark review → stop. Bella approves; humans send.
