# Paperclip Fresh Instance -- Migration Roster

**Created:** July 16, 2026
**Purpose:** Complete checklist for standing up a new Paperclip instance and migrating agents from the broken instance.

---

## Critical Setup Rules (Apply Before Creating Any Agent)

1. **Set escalation target to Nora Reeves from day one.** The old instance had Erin Pope (deleted agent) as the default recovery target. Every failed task escalated to a dead end.
2. **Do NOT add GitHub repo links to projects unless the agent writes code.** Railway doesn't have git installed. Only Chris Copypaste needs repo access.
3. **Use persistent volume mount for all storage.** Ephemeral storage at `/home/paperclip/dev/` wipes on restart.
4. **Keep the restart cron consolidated.** One watchdog (every 30 min) with cooldown, checkout sweep, and alerting. No separate blind restart.

---

## Agents to Recreate

### Tier 1 -- Core Operations (recreate first)

| Agent | Role | Type | Skill File | API Keys Needed | Notes |
|---|---|---|---|---|---|
| **Nora Reeves** | COO / Orchestrator | Heartbeat | `nora/SKILL.md` | PAPERCLIP_SYNC_KEY, PAPERCLIP_REPORT_SECRET | NEW skill file. Set as default escalation target. Recovery junk sweeper. |
| **Olivia Smith** | EA / Comms / Efficiency | Heartbeat | `olivia/SKILL.md` | PAPERCLIP_REPORT_SECRET | Morning + afternoon briefs, email routing, approval batching. Also has `cos-routine/SKILL.md`. |
| **Chris Copypaste** | Director of Engineering | Heartbeat | `product-task-structure/SKILL.md` | GitHub access | Only agent that needs repo link on project. CCP execution model. |

### Tier 2 -- Revenue & Growth (recreate after Tier 1 is stable)

| Agent | Role | Type | Skill File | API Keys Needed | Notes |
|---|---|---|---|---|---|
| **Zara Okonkwo** | Social Director | Heartbeat | `zara/SKILL.md` | BUFFER_ACCESS_TOKEN, BUFFER_API_KEY | Buffer key "Paperclip-Zara-2026" valid through June 2027. Manages 3 outlets. |
| **Izzy Reeves** | Content / CMO Prep | Assignment | `izzy/SKILL.md` | None | NEW skill file. Reel scripts, social co-drafts, Substack pre-screen. Nora wakes her. |
| **Simone** | Social Execution | Assignment | `simone/SKILL.md` | Buffer (via Zara) | FB Group engagement, reel posting. Works under Zara. |
| **Kristin Williams** | CMO | Heartbeat | `kristin/SKILL.md` | None | Content strategy, Substack, brand voice enforcement. Human CMO but has agent presence. |
| **Elena Vasquez** | CRO / Strategic Sales | Heartbeat | None (inline instructions) | Gmail OAuth (DEFERRED) | Pipeline oversight and planning. Gmail OAuth still unresolved -- no sending from rae@ until fixed separately. |
| **Sophia Castillo** | Email Router | Assignment | None (inline instructions) | None | Routes emails between outreach and Olivia via sender decision tree. |

### Tier 3 -- Funding (recreate when grant work resumes)

| Agent | Role | Type | Skill File | API Keys Needed | Notes |
|---|---|---|---|---|---|
| **Vanessa** (Thornton) | Grant Writer | Heartbeat | `grant-agent-workloop/SKILL.md` | PAPERCLIP_SYNC_KEY, PAPERCLIP_REPORT_SECRET | Drafts grant narratives, saves to Google Drive. Also uses `grant-positioning/SKILL.md`. |
| **Amara** (Obi) | Funder Researcher | Heartbeat | `grant-agent-workloop/SKILL.md` | PAPERCLIP_SYNC_KEY | Researches funders, creates opportunities. Also uses `grants-catalog/SKILL.md`. |

### Tier 4 -- Hub & Creator Studio (recreate when content pipeline is active)

| Agent | Role | Type | Skill File | API Keys Needed | Notes |
|---|---|---|---|---|---|
| **Jasmine** (Cole) | Hub Engagement | Assignment | `jasmine/SKILL.md`, `hub-engagement/SKILL.md` | PAPERCLIP_SYNC_KEY, PAPERCLIP_REPORT_SECRET | AI-generates engagement checks for Hub lessons. |
| **Julie Lynn** | QA Engineer | Assignment | `julie-lynn/SKILL.md`, `hub-engagement/SKILL.md` | PAPERCLIP_SYNC_KEY, PAPERCLIP_REPORT_SECRET | Validates engagement density. Also QA-gates social, reel, email, funding outputs. Reports to Nora. |
| **Anne Marie** (Schmitt) | Creator Studio | Assignment | `creator-health/SKILL.md` | PAPERCLIP_SYNC_KEY | Monitors creator health, drafts check-in notes for Bella. Do NOT add GitHub repo to her project. |
| **Lily** (Chen) | Design / Thumbnails | Assignment | None (inline instructions) | None | Creates and uploads Hub thumbnails. |

---

## Agents to SKIP (do not recreate)

| Agent | Old Role | Why Skip |
|---|---|---|
| **Erin Pope** | Operating CEO | Dead agent poisoning escalation system. Her routing function is now split between Nora (orchestration) and Olivia (comms/efficiency). Do not recreate. |
| **Jim Ford** | SDR | Human, not an agent workflow. Jim does phone outreach and schedules leads on Rae's calendar. Remove from Paperclip entirely. |
| **Priya Nair** | Partner Health | Merged into Nora's scope. Partner health monitoring is now part of operations orchestration. |
| **Quinn Nakamura** | Policy / Structure | Quarterly structural audits. No active tasks. Reference-only role. Recreate only if/when quarterly audit cycle starts. |
| **Ravi Patel** | Strategy | Listed in team.ts but no skill file, no tasks, no references in any workflow. Unknown function. |
| **Holly Scott** | Customer Success | Listed in team.ts but no skill file, no active tasks found. |
| **Dr. Maya Johnson** | VP Educator UX / Curriculum | Listed in team.ts and org chart but no skill file, no active workflow. |
| **Sebastian Cole** | Legal | Listed in team.ts but no skill file or active tasks. |
| **Sandra Reyes** | Accounting | Listed in team.ts, referenced in partner-health for collections. No skill file. Recreate only if collections workflow activates. |
| **Victor Nash** | Finance | Listed in team.ts but no skill file or active tasks. |
| **Marcus Thompson** | CPO | Has `product-task-structure/SKILL.md` but that's more of a reference doc. His function is to coordinate with Rae on what to build. Consider whether this needs an active agent or just the reference doc. |
| **James Washington** | COO | Referenced in org chart but no skill file or active tasks. Nora fills the operational role now. |
| **Rodrigo Vega** | Data & Analytics | Maintains grants catalog. No active agent tasks -- just the `grants-catalog/SKILL.md` reference doc. |
| **Alexis Martinez** | Brand Voice | Not an agent -- just the `brand-voice/SKILL.md` reference doc. |
| **Marcus Lee** | Grant Positioning | Not an agent -- just the `grant-positioning/SKILL.md` reference doc. |

---

## Shared Skills (load into new instance, assign to relevant agents)

| Skill | Used By | Notes |
|---|---|---|
| `brand-voice/` | All content agents (Izzy, Zara, Kristin, Simone, Olivia) | Reference spec, not API-driven |
| `cos-routine/` | Olivia | Daily brief cadence definition |
| `send-report/` | All agents | Email delivery to Rae |
| `hub-engagement/` | Jasmine, Julie Lynn | Hub lesson engagement check API |
| `creator-health/` | Anne Marie | Creator Studio sync API |
| `grant-agent-workloop/` | Vanessa, Amara | Funding sync API + Google Drive |
| `grant-followup-engine/` | Funding agents | Follow-up cadence + escalation ladder |
| `grant-positioning/` | Vanessa, Amara | Reference spec for grant language |
| `grants-catalog/` | Amara | 39-source master catalog |
| `outreach-sequences/` | Jim | SDR call/email sequences |
| `partner-health/` | Priya Nair (if recreated) | Partner scoring model |
| `product-task-structure/` | Chris, Marcus T | CCP build model reference |
| `nora/` | Nora | NEW -- orchestration work loop |
| `izzy/` | Izzy | NEW -- content/CMO prep work loop |

---

## Environment Variables Needed

Transfer these from the old Railway instance to the new one:

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | New Postgres DB (fresh -- do not reuse old DB with blocked tasks) |
| `BETTER_AUTH_SECRET` | Generate new for fresh instance |
| `PAPERCLIP_PUBLIC_URL` | New Railway URL |
| `PAPERCLIP_ALLOWED_HOSTNAMES` | New Railway hostname |
| `ANTHROPIC_API_KEY` | Claude API access for agents |
| `OPENAI_API_KEY` | Optional, if any agents use OpenAI |
| `BUFFER_ACCESS_TOKEN` | Buffer integration (same key, valid through June 2027) |
| `BUFFER_API_KEY` | Buffer integration (same key) |

And on the TDI website (Vercel), update:
| Variable | Purpose |
|---|---|
| `PAPERCLIP_API_TOKEN` | Session token for new instance (for watchdog checkout sweep) |
| `PAPERCLIP_COMPANY_ID` | Company ID from new instance |

The `PAPERCLIP_SYNC_KEY` and `PAPERCLIP_REPORT_SECRET` on Vercel stay the same -- those authenticate agents calling TDI APIs, not the other way around.

---

## Vercel Cron Updates Needed

The watchdog URL is hardcoded. After new instance is live, update:
- `app/api/cron/paperclip-watchdog/route.ts` -- change `PAPERCLIP_URL` to new Railway URL
- `app/api/cron/olivia-daily-briefing/route.ts` -- change Paperclip health check URL if referenced
- Remove `app/api/cron/restart-paperclip` from `vercel.json` (already done in this session)

---

## Migration Order

1. Deploy fresh Paperclip on Railway with persistent volume
2. Set env vars, create admin account
3. Set default escalation target to Nora Reeves
4. Create Tier 1 agents (Nora, Olivia, Chris) with skill files
5. Verify heartbeats running, no recovery loops
6. Create Tier 2 agents (Zara, Izzy, Simone, Kristin, Jim, Sophia)
7. Resolve Elena's Gmail OAuth or defer her
8. Create Tier 3 agents (Vanessa, Amara) when grant work timing is right
9. Create Tier 4 agents (Jasmine, Julie Lynn, Anne Marie, Lily)
10. Update Vercel env vars and watchdog URL
11. Verify end-to-end: agent heartbeats, send-report delivery, API sync calls
12. Decommission old instance

---

## Decisions (Resolved July 16, 2026)

1. **Elena Vasquez:** Recreate WITHOUT Gmail OAuth. She does pipeline oversight and planning. OAuth gets resolved separately later.
2. **Priya Nair:** Do NOT recreate as standalone agent. Merge partner health monitoring into Nora's orchestration scope.
3. **Jim Ford:** REMOVE from Paperclip entirely. Jim is human, does phone outreach to schedule leads on Rae's calendar. Not an agent workflow.
4. **Holly, Maya, Sebastian, Victor, Ravi:** These agents have intended purposes but were never properly set up. Do NOT skip -- come back after core instance is stable to define their roles and create skill files. Flagged as "needs skill file" below.
5. **Old database:** Migrate useful data (task history, agent configs, routine definitions) to new instance before cutting over. Identify what's useful vs. junk before migration.

## Agents Needing Skill Files (Phase 2 -- after core instance is stable)

| Agent | Title (from team.ts) | Intended Area | Status |
|---|---|---|---|
| Holly Scott | Customer Success | Educator support? Hub user questions? | Needs role definition with Rae |
| Dr. Maya Johnson | Curriculum / VP Educator UX | Curriculum design? Content review? | Needs role definition with Rae |
| Sebastian Cole | Legal | Contract review? Compliance? | Needs role definition with Rae |
| Victor Nash | Finance | Financial ops? Budget tracking? | Needs role definition with Rae |
| Ravi Patel | Strategy | Strategic planning? Market research? | Needs role definition with Rae |
