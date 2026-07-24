# Old Paperclip Instance -- Full Ticket Audit

**Date:** July 17, 2026
**Source:** paperclip-production-014f.up.railway.app
**Total open issues:** 66 (17 blocked, 7 in_review, 18 todo, 24 in_progress)

---

## Summary

Most of the 66 open issues are **noise from the broken system**, not real work. The breakdown:

| Category | Count | Action |
|---|---|---|
| Funding/Outreach (duplicated escalations) | 32 | Triage -- most are duplicates of same 3 items |
| System Admin (restart/stability requests) | 3 | DROP -- these are about the old instance |
| Content/Hub | 5 | MIGRATE real items |
| Marketing/Social | ~4 | TRIAGE -- deadlines may have passed |
| Engineering | ~3 | MIGRATE if still relevant |
| Other | ~19 | TRIAGE individually |

---

## MIGRATE TO NEW INSTANCE (Real Work)

### Funding -- Deduplicate first

The 32 funding issues are mostly duplicate escalations about **3 core items**:

**1. Paula Poche / Walmart Spark Good email**
- Original: TEA-7384, TEA-7387, TEA-7401
- Escalations: TEA-8201, TEA-8203, TEA-8205, TEA-8277, TEA-8292, TEA-8361
- **Status:** Walmart deadline was Aug 1. Still actionable.
- **Action:** Create ONE task in new instance: "Send Paula Poche Walmart Spark Good email from TDI grants Gmail. Deadline Aug 1."
- **Assign to:** Vanessa (drafts) -> Rae (sends from grants Gmail)

**2. DuPage Foundation eligibility email to Barb Szczepaniak**
- Original: TEA-8116, TEA-8130
- **Status:** Email was authorized but never sent
- **Action:** Create ONE task: "Send DuPage Foundation eligibility email to Barb Szczepaniak"
- **Assign to:** Vanessa

**3. GNOF/St. Peter Chanel/JCF outreach**
- Multiple tickets from June-July about St. Peter Chanel school outreach
- JCF window closed June 26 -- **STALE**
- GNOF email may still be relevant
- **Action:** Check with Rae if GNOF is still active, create one task if so

### Content/Hub

| Old ID | Title | Action |
|---|---|---|
| TEA-8420 | Products without thumbnails | **MIGRATE** -- Lily should handle this |
| TEA-8447 | YIKES! MAJOR ISSUE | **CHECK** -- what is this? Need to read details |
| TEA-8450 | Ship NEXT_PUBLIC_LEARNING_HUB_* env-var fix | **MIGRATE** -- engineering task for Chris |
| TEA-7140 | D3 Instruction Series HTML -- 8 Files (Dr. Jasmine Cole) | **CHECK** -- may be completed (Nora approved D3 resources 2d ago) |
| TEA-7141 | D1 HTML Resource Files -- 8 Pieces | **CHECK** -- same question |

### Marketing/Social

| Old ID | Title | Action |
|---|---|---|
| TEA-8156 | Schedule reel in Buffer (July 11 deadline) | **STALE** -- deadline passed |
| TEA-8149 | Buffer reel window closing | **STALE** -- deadline passed |
| TEA-7119 | Alternate Buffer scheduling (June 25) | **STALE** -- deadline passed |

**Marketing verdict:** All marketing tickets had passed deadlines. No migration needed -- the new instance has TEA-5 (Marketing Pipeline Audit) assigned to Izzy to report current status fresh.

### Engineering

| Old ID | Title | Action |
|---|---|---|
| TEA-8439 | Apply migration 077 -- thumbnail enforcement gate | **MIGRATE** if TEA-8420 is still needed |
| TEA-8450 | Ship NEXT_PUBLIC_LEARNING_HUB_* env-var fix | **MIGRATE** -- this is a real code fix |

---

## DROP (Don't Migrate)

### System Admin (3 items) -- All about old instance problems
- TEA-8026: Execute board-locked stability actions -- OLD INSTANCE problem, not relevant
- TEA-8279: 3 agents in error + 47% run failure rate -- OLD INSTANCE, now fixed
- TEA-8271: Admin restart needed for Sophia/Amara/Nora -- OLD INSTANCE, now fixed

### Duplicate Escalations (~25 items)
The funding section has ~25 duplicate escalations about the same 3 email sends. These were created because agents kept re-escalating when Rae didn't respond (because the system was broken). Only the core 3 items need migration.

### Stale Marketing (~3 items)
Buffer/reel deadlines that passed in June-July.

---

## ITEMS NEEDING RAE'S DECISION

1. **Paula Poche / Walmart Spark Good:** Is this still active? Aug 1 deadline approaching.
2. **DuPage Foundation / Barb Szczepaniak:** Was this email ever sent manually?
3. **GNOF outreach:** Still relevant or stale?
4. **TEA-8447 "YIKES! MAJOR ISSUE":** What is this? Need to check details.
5. **D3/D1 HTML resources (TEA-7140, TEA-7141):** Were these completed? Nora approved D3 recently.

---

## Recommended New Instance Tasks to Create

Based on this audit, create these in the new instance:

1. **"Send Paula Poche Walmart Spark Good email -- Aug 1 deadline"** -> Vanessa, Funding & Grants project
2. **"Send DuPage Foundation eligibility email to Barb Szczepaniak"** -> Vanessa, Funding & Grants project
3. **"Products without thumbnails -- audit and create missing ones"** -> Lily, Learning Hub project
4. **"Ship NEXT_PUBLIC_LEARNING_HUB env-var fix"** -> Chris, Website Build project
5. **"Apply migration 077 thumbnail enforcement gate"** -> Chris, Website Build project (if #3 confirms needed)

Wait for Rae's answers on the decision items before creating more.
