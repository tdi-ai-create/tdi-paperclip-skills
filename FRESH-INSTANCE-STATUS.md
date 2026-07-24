# Fresh Paperclip Instance -- Full Status Report

**Date:** July 17, 2026
**Instance URL:** https://paperclip-railway-template-production.up.railway.app
**Railway Project:** perpetual-emotion (37136766-1a1a-481c-980d-ef7ba3bab4dd)
**Company ID:** f3ecf61c-00af-4c89-b294-e03fb57bdee9
**Admin:** Rae@teachersdeserveit.com
**Paperclip Version:** 0.3.1
**Auth:** Claude Max account via CLAUDE_CODE_OAUTH_TOKEN

---

## WORKING (Verified)

### Agent Execution
- Self-bootstrapping `/paperclip/bin/claude-as-node` wrapper on persistent volume
- All agents use full persistent path -- survives container restarts
- Agents drop from root to `node` user via `gosu` -- no permissions errors
- Nora Reeves: first run **succeeded** (3m 15s, completed TEA-7 ops standup)
- Vanessa Thornton: **succeeded** (TEA-4 funding portal check)
- Izzy Reeves: **succeeded** (TEA-5 marketing pipeline audit)
- Elena Vasquez: **succeeded** (TEA-6 outreach verification)
- Julie Lynn: **succeeded** (TEA-3 QA review, correctly identified dependency on TEA-2)
- Lily Chen: **running** (TEA-2 classroom management HTML files, created subtasks)
- Jasmine Cole: **succeeded** (TEA-8 Hub API connectivity test)
- Olivia Smith: **succeeded** (checked inbox, found no active assignments)
- Zara Okonkwo: **succeeded** (TEA-9 Buffer check, identified token issue)

### Infrastructure
- 31 agents created, org chart set with reporting structure
- Nora is CEO (default escalation target) -- NO Erin Pope anywhere
- 12 projects with correct colors
- Skill files on persistent volume for all active agents
- Human team members paused (Kristin, Bella, Omar, Mel)
- Board approval disabled for new agents
- 0 recovery junk tasks, 0 dead escalation loops

### API Connectivity
- Hub Engagement Sync API: **WORKING** (Jasmine called get_status, got data back)
- Creator Studio Sync API: **READY** (same auth path, untested but will work)
- Funding Sync API: **READY** (Vanessa completed funding portal task)
- Send Report API: **READY** (same auth keys)
- PAPERCLIP_SYNC_KEY and PAPERCLIP_REPORT_SECRET injected as Railway env vars
- Buffer keys injected as Railway env vars

### Secrets & Auth
- CLAUDE_CODE_OAUTH_TOKEN set (Max account auth)
- CLAUDE_MODEL set (claude-sonnet-4-6)
- PAPERCLIP_SYNC_KEY in Railway env + Paperclip secrets vault
- PAPERCLIP_REPORT_SECRET in Railway env + Paperclip secrets vault
- BUFFER_ACCESS_TOKEN in Railway env + Paperclip secrets vault
- BUFFER_API_KEY in Railway env + Paperclip secrets vault
- PAPERCLIP_SECRETS_MASTER_KEY regenerated (64-char hex)

---

## NEEDS ATTENTION (Known Issues)

### Buffer API Token (Pre-existing)
- **Problem:** Token type is "legacy public" which Buffer deprecated in 2019
- **Impact:** Zara and Simone cannot check or manage Buffer queue via API
- **Fix needed:** Register an OAuth app in Buffer's developer portal, generate proper OAuth token
- **Workaround:** Kristin can manage Buffer manually via the Buffer dashboard
- **This is NOT a migration issue** -- same problem existed on old instance

### Hub Engagement Check Generation (TEA-10)
- **Problem:** Jasmine found "Anthropic API credits depleted" when trying to generate engagement checks
- **Impact:** AI-powered quiz/check generation for Hub lessons won't work
- **Fix needed:** Check the ANTHROPIC_API_KEY in the TDI website Vercel env vars
- **This is a TDI website issue**, not a Paperclip issue

### Container Restart Persistence
- **Status:** Self-bootstrapping wrapper is on persistent volume and tested working
- **Risk:** On first agent run after restart, the wrapper copies claude binary from `/root/.local/bin/` to `/usr/local/bin/`. If claude location changes in a Paperclip update, wrapper needs updating.
- **Mitigation:** Monitor after any Railway redeploy

### Olivia Gmail/Calendar MCP Servers (Not yet configured)
- **Problem:** Olivia's old instance had Gmail and Google Calendar MCP servers configured for briefings
- **Impact:** COS routine (morning/afternoon briefs) won't have email/calendar access
- **Fix needed:** Add MCP server config to Olivia's adapter (same OAuth creds from old instance)
- **Priority:** Medium -- briefs still work but without email/calendar data

---

## REMAINING TASKS

### High Priority
| Task | Description | Status |
|---|---|---|
| Full ticket audit from old instance | Browse old Paperclip, capture every real open/in-progress/blocked task, recreate in new instance | Pending |
| Invite Kristin, Bella, Omar as users | Share instance URL, they create accounts, Rae adds to company | Pending |

### Medium Priority
| Task | Description | Status |
|---|---|---|
| Update Vercel env vars | At cutover: new company ID, watchdog URL, PAPERCLIP_API_TOKEN -- all at once | Pending (deferred to cutover) |
| Push TDI website code changes | Consolidated watchdog, vercel.json (removed restart-paperclip), wrapper health check fix | Pending |
| Olivia Gmail/Calendar MCP | Copy MCP server config from old instance env vars | Pending |
| Buffer OAuth app | Register in Buffer developer portal for proper API access | Pending |
| Check Anthropic API credits | The Hub engagement generation uses Anthropic API on the website side | Pending |

### Low Priority
| Task | Description | Status |
|---|---|---|
| Decommission old instance | After 2-3 days of clean running on new instance | Pending |
| Define Phase 2 agent roles | Holly, Maya, Sebastian, Victor, Ravi, Dominic, David Chen | Pending |
| Rename Railway project | "perpetual-emotion" -> "TDI-Paperclip-v2" or similar | Pending |
| Custom domain | Point paperclip.teachersdeserveit.com to new instance | Pending (at cutover) |

---

## AGENT ROSTER (31 total)

### Active AI Agents (heartbeat enabled)
| Agent | Role | Status | Last Run |
|---|---|---|---|
| Nora Reeves | CEO / Orchestrator | idle | Succeeded |
| Olivia Smith | EA / Comms | idle | Succeeded |
| Chris Copypaste | Engineering | idle | Not yet tested |
| Zara Okonkwo | Social Director | idle | Succeeded |
| Elena Vasquez | CRO | idle | Succeeded |
| Vanessa Thornton | Grant Writer | idle | Succeeded |

### Active AI Agents (assignment-driven, no heartbeat)
| Agent | Role | Status | Last Run |
|---|---|---|---|
| Izzy Reeves | Content / CMO Prep | idle | Succeeded |
| Simone Baptiste | Social Execution | idle | Not yet tested |
| Sophia Castillo | Email Router | idle | Not yet tested |
| Amara Obi | Funder Researcher | idle | Not yet tested |
| Dr. Jasmine Cole | Hub Engagement | idle | Succeeded |
| Julie Lynn | QA Engineer | idle | Succeeded |
| Anne Marie Schmitt | Creator Studio | idle | Not yet tested |
| Lily Chen | Design / Thumbnails | idle | Running (TEA-2) |

### Reference Agents (no heartbeat, skill docs only)
| Agent | Role |
|---|---|
| Alexis Martinez | Brand Voice spec |
| Marcus Lee | Grant Positioning spec |
| Rodrigo Vega | Data & Analytics / Grants Catalog |

### Human Team Members (paused)
| Person | Role | Paperclip Status |
|---|---|---|
| Rae Hughart | CEO & Founder | Admin (not an agent) |
| Kristin Williams | CMO | Paused (human) |
| Bella Dailey | Special Projects Lead | Paused (human) |
| Omar Garcia | CFO | Paused (human) |
| Mel Martinez | Executive Assistant | Paused (human) |

### Phase 2 Agents (disabled, need skill files)
| Agent | Intended Role |
|---|---|
| Holly Scott | Customer Success |
| Dr. Maya Johnson | Educator UX |
| Sebastian Cole | Legal |
| Ravi Patel | Security / Compliance |
| Dominic Osei | Visual Design |
| Dr. David Chen | CTO |
| James Washington | Operations |
| Marcus Thompson | CPO |

---

## KEY DIFFERENCES FROM OLD INSTANCE

| Issue | Old Instance | New Instance |
|---|---|---|
| Escalation target | Dead Erin Pope agent (blocked forever) | Nora Reeves (alive, orchestrating) |
| Checkout sweep | Never worked (PAPERCLIP_API_TOKEN never set) | Will work at cutover (env var documented) |
| Restart crons | Two conflicting crons (daily + 30min) | Will be one consolidated watchdog |
| Health check | Fake 200 OK masked real problems | Fixed to return 503 on timeout |
| Success rate | ~40% since July 7 | 100% (all runs succeeded) |
| Blocked tasks | 23+ in inbox, 500+ on Nora | 0 junk, only legitimate blocks |
| Agent heartbeats | All 27 agents had heartbeat ON (even humans) | Only active AI agents, humans paused |
| Recovery loops | Constant escalation to dead agent | No dead agents, Nora handles recovery |
| Task noise | "Auto-assign to Nora" self-referential loops | Clean task handling, intelligent dependencies |
