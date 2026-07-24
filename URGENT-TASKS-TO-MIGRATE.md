# Urgent Tasks to Recreate in Fresh Instance

**Source:** Old Paperclip instance (paperclip-production-014f), captured July 16, 2026
**Note:** The old Paperclip UI keeps dropping company context, making it impossible to fully browse. These are captured from screenshots of Nora's dashboard and the Inbox.

---

## From Nora's Dashboard (Recent Tasks)

| TEA ID | Title | Status | Notes |
|---|---|---|---|
| TEA-8371 | Auto-assign unassigned tasks to Nora | done | Recurring self-referential task -- DO NOT migrate |
| TEA-8367 | Design & Format 20 Classroom Management Resources into HTML Files | in progress | **MIGRATE** -- Lily Chen is working on this |
| TEA-8366 | Research & Write 20 Classroom Management, Behavior & Engagement Resources | done | Completed |
| TEA-8364 | Classroom Management, Behavior, & Engagement Strategies (content need) | blocked | **MIGRATE** -- parent task, blocked |
| TEA-8368 | Review & QA -- 20 Classroom Management Resources Before Hub Upload | blocked | **MIGRATE** -- waiting on TEA-8367 |
| TEA-8351 | [UPDATE] CEO Daily Brief -- 2026-07-15 | in review | Recent brief, may not need migration |
| TEA-8316 | Check the funding portal for work assigned to you and complete it | open | **MIGRATE** -- funding work |
| TEA-8252 | [URGENT] Jim Ford outreach -- July 11 deadline passed, action needed NOW | blocked | **REVIEW** -- Jim is human, not agent. This may be a real outreach item that needs Rae's attention even though deadline passed |

## From Inbox (Board Approvals)

### Needs Rae's attention NOW:
| Item | Requester | Status | Action Needed |
|---|---|---|---|
| Approve D3 HTML Resources -- Dr. Jasmine Cole Series (8 Files) | Nora Reeves | Approved 2d ago | Already approved -- verify files made it through |
| Failed run -- Amara Obi (Process lost, server restarted) | System | Failed 1h ago | Don't retry on old instance -- this work moves to new instance |

### Funding/Outreach (all rejected 1w ago -- may be stale):
| Item | Requester | Notes |
|---|---|---|
| Send GNOF email (3 min) + Paula Poche package | Elena Vasquez | **CHECK** -- was this ever sent manually? |
| EXECUTE NOW: GNOF email + St. Peter Chanel outreach | Elena Vasquez | JCF missed, these were "still live" 1w ago |
| CRITICAL: St. Peter Chanel JCF/GNOF/JPPS outreach | Elena Vasquez | JCF window closed June 26 -- likely stale |
| DEADLINE TOMORROW: JCF call + GNOF email for St. Peter Chanel | Elena Vasquez | Likely stale (1w+ old) |
| URGENT: St. Peter Chanel outreach (Walmart Spark Good) | Elena Vasquez | July 5 delivery deadline -- **past due, check status** |
| Authorize interim approver -- Rae unreachable 4+ days | Quinn Nakamura | Rejected -- Nora couldn't get approval authority |

### System items (don't migrate):
| Item | Notes |
|---|---|
| Hire Agent: QA Engineer (x2) | Already approved -- Julie Lynn exists |
| Unblock Phase 2A-4 Learning Hub build | Already approved |
| COO ESCALATION: Authorize Rae proxy restart (18+ items blocked) | System problem, not task |

## Active Agents at Time of Capture

6 agents with live heartbeat runs:
- Nora Reeves (1 live) -- orchestrator
- Elena Vasquez (1 live) -- sales/pipeline
- Olivia Smith (1 live) -- EA/comms
- Vanessa Thornton (1 live) -- grant writing
- Izzy Reeves (1 live) -- content/CMO prep
- Zara Okonkwo (2 live) -- social/Buffer

## Marketing Tasks (Kristin)

**NOT CAPTURED** -- Paperclip UI kept dropping company context before we could browse the Marketing project. Need to check:
- What Substack posts are in the pipeline (should be working 1 week ahead)
- Buffer queue status across all 3 outlets
- Any reel script batches in progress (Izzy's work)
- TikTok hook list currency
- Any content calendar items with deadlines

**Recommended:** In the new instance, create a "Marketing Audit" task for Kristin/Izzy to report current pipeline status as their first action.

## 23 Blocked Inbox Items

The Blocked tab showed 23 items but the UI wouldn't render them. These are likely a mix of:
- Real blocked work (funding, outreach, content)
- Recovery/escalation junk (Erin Pope dead-end loops)
- Self-referential Nora tasks

**Recommended:** Don't try to migrate these blindly. In the new instance, create fresh tasks for the real work items listed above. The junk will die with the old DB.
