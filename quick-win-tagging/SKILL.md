---
name: quick-win-tagging
description: >
  Tagging spec for Hub Quick Wins. Defines every metadata field, allowed values,
  decision criteria, and examples. Used by any agent or human creating or auditing
  Quick Win content in the Learning Hub.
---

# Quick Win Tagging Spec

**Owner:** Rae Hughart (CEO)
**Used by:** Any agent creating, importing, or auditing Quick Wins
**Database:** Supabase project `asdwpkcsbcnpknklchdq`, table `hub_quick_wins`

---

## Why tagging matters

Every Quick Win in the Hub gets filtered, sorted, and recommended based on its tags. Missing or wrong tags mean educators can't find the resource, the lift pills don't render, and the filtering UI breaks down. Every Quick Win must be fully tagged before publishing.

---

## Required fields for every Quick Win

Every published Quick Win (`is_published = true`) must have all of the following fields populated. A Quick Win missing any required field should not be published until tagged.

| Field | DB Column | Type | Required | Description |
|---|---|---|---|---|
| Title | `title` | text | Yes | Clear, specific name |
| Description | `description` | text | Yes | 1-2 sentences explaining what it is and why it helps |
| Category | `category` | text | Yes | Primary content bucket (see taxonomy below) |
| Lift | `lift` | text | Yes | How action-ready the resource is (see rubric below) |
| Quick Win Type | `quick_win_type` | text | Yes | Format of the resource |
| Resource Type | `resource_type` | text | Yes | Must match `quick_win_type` |
| Topic Tags | `topic_tags` | text[] | Yes | At least one tag from the approved list |
| Roles | `roles` | text[] | Yes | Who this resource is for |
| Danielson Domains | `danielson_domains` | text[] | Yes | Which Danielson Framework domain(s) apply |
| Tier | `tier` | text | Yes | Display tier name |
| Access Tier | `access_tier` | text | Yes | Access control tier (lowercase) |

### Optional fields

| Field | DB Column | Type | Notes |
|---|---|---|---|
| Spanish Title | `title_es` | text | Translated title |
| Spanish Description | `description_es` | text | Translated description |
| Duration | `duration_minutes` | integer | Estimated time to complete |
| Thumbnail | `thumbnail_url` | text | Cover image URL |
| File URL | `file_url` | text | Download link |
| Objectives | `objectives` | text | Learning objectives |

---

## Field 1: Category

The primary content bucket. Determines which tab/filter the Quick Win appears under.

| Value | Use when the resource primarily... | Examples |
|---|---|---|
| `Lesson Planning` | Helps design lessons, units, objectives, or curriculum | Lesson Flow Checklist, Writing Learning Objectives, Standards Clarity Toolkit |
| `Assessment` | Checks understanding, evaluates learning, or audits assessments | Formative Assessment Toolkit, The Assessment Audit, Retrieval Practice Checklist |
| `Instructional Strategies` | Teaching moves used during instruction -- questioning, modeling, grouping, scaffolding | Discussion Protocol Playbook, Scaffolding Without Spoon-Feeding, Flexible Grouping |
| `Classroom Setup` | Environment, routines, culture, physical space, systems | First 10 Minutes Framework, Noise Level System, Inclusive Classroom Setup Guide |
| `Classroom Management` | Behavior, de-escalation, conduct, intervention | Class Skipping Intervention, BIP Data Collection, De-Escalation Language Guide |
| `Communication` | Parent contact, colleague feedback, interpersonal skills, community building | Conversation Starter Cheat Sheet, Feedback Openness Audit, Restorative Circles |
| `Time Savers` | Reduces prep time, automates something, or streamlines workflow | Canva Bulk Create, Backwards Design in 30 Minutes, Pacing Guide Builder |
| `Leadership` | For admins, coaches, org-level planning, onboarding, funding | Culture-First Leadership Framework, PA Observation Guide, Funding PD |
| `Self-Care` | Supports educator wellness, rest, or personal sustainability | 7-Day Sustainable Sleep Challenge, Reset Without Guilt |
| `Stress Relief` | Provides immediate relief or coping during high-pressure moments | Calm Response Scripts, 3 Tiny Wellness Habits |
| `Games` | Interactive practice tools, quizzes, gamified activities | Energy Budget, Question Knockout, Behavior Bingo |
| `Vocational` | Specific to CTE, career-tech, or vocational ed contexts | Welding Lesson in a Box, Cosmetology Lab Card |

### Decision rules for category

- If a resource spans two categories, choose the one that best describes what the educator **does** with it, not what it's about.
- "Instructional Strategies" vs "Lesson Planning": Strategies are used **during** instruction. Planning is done **before** instruction.
- "Instructional Strategies" vs "Assessment": If it's about checking understanding, it's Assessment. If it's about teaching technique, it's Instructional Strategies.
- "Classroom Setup" vs "Classroom Management": Setup is about **systems and environment** (proactive). Management is about **behavior and conduct** (reactive).
- "Self-Care" vs "Stress Relief": Self-Care is **proactive and sustained** (habits, challenges, planning). Stress Relief is **reactive and immediate** (scripts, quick resets).
- "Communication" is always about **interpersonal exchange** -- if it's a form or template the educator fills out alone, it's not Communication.
- "Leadership" is for resources designed for **admins, coaches, or organizational** contexts, not classroom teachers.
- **"Classroom Tools" is retired.** Do not use this category. Map to one of the more specific categories above.

---

## Field 2: Lift

How action-ready the resource is. This determines the colored pill on the Quick Win card.

| DB Value | UI Label | UI Pill Color | Definition | Decision test |
|---|---|---|---|---|
| `LOW` | Grab & Go | Green (#D9E8E2) | Open it, use it, done. No planning, no context-gathering, no multi-step process. | Could an educator download this at 7:45 AM and use it by 8:00 AM with zero modification? |
| `MED` | Short Prep | Amber (#F4E9D0) | Needs a planning period. Read through it, think about your context, maybe fill in a few blanks, then implement. | Does this require the educator to sit down, think, customize, or plan before using it? |
| `HIGH` | Deeper Lift | (no pill currently) | Grab a coffee. Requires deeper reflection, multi-step implementation, or significant time investment. Usually frameworks, toolkits with multiple components, or resources that change practice over days/weeks. | Does this require multiple sessions, team coordination, or sustained effort across days? |

### Lift decision tree

```
START: Can an educator use this immediately with zero prep?
  |
  YES --> LOW (Grab & Go)
  |
  NO --> Does it require more than one planning period or multiple steps across days?
    |
    NO --> MED (Short Prep)
    |
    YES --> HIGH (Deeper Lift)
```

### Lift examples from existing data

**LOW (Grab & Go):**
- Lesson Flow Checklist -- print and use during planning
- Canva Bulk Create -- follow the steps, done
- Volume Awareness PA Guide -- read and apply today
- Calm Response Scripts -- pull out mid-conversation
- 7-Day Sustainable Sleep Challenge -- start tonight

**MED (Short Prep):**
- Equity-Centered Classroom Audit -- need to observe your classroom first, then assess
- SpEd Scenario Pack -- read scenarios, think about your students, plan responses
- Personalized PD Plan -- requires reflection on goals before filling it out
- Feedback Openness Audit -- need to gather input, then analyze
- Pacing Guide Builder -- requires knowing your curriculum to fill in

**HIGH (Deeper Lift):**
- Autism Acceptance Month Toolkit -- multi-component (teacher guide + parent handouts), plan across the month
- Executive Functioning Educator Guide -- comprehensive guide, multiple strategies to learn and implement over time
- Culture-First Leadership Framework -- organizational change, not a single-session resource
- Standards Clarity Toolkit -- deep alignment work across units

### When lift is uncertain

If you genuinely cannot determine the lift level from the title and description alone, set `lift_uncertain = true` and assign your best guess. This flags it for human review without blocking publication.

---

## Field 3: Quick Win Type / Resource Type

These two fields must always match. They describe the format of the resource.

| Value | Use when |
|---|---|
| `download` | A downloadable PDF, document, or printable (the vast majority of Quick Wins) |
| `game` | An interactive game or gamified activity |
| `quiz` | A self-assessment quiz or knowledge check |
| `quick_win` | A non-downloadable quick reference or tip (rare, legacy) |

---

## Field 4: Topic Tags

At least one tag from this approved list. Use multiple when applicable. Tags drive search and "related resources" recommendations.

| Tag | Use when the resource is about... |
|---|---|
| `general` | Broadly applicable to any educator, no specific specialization |
| `para` | Specifically designed for or relevant to paraprofessionals |
| `sped` | Special education contexts, IEPs, accommodations |
| `ell` | English language learners, multilingual support |
| `classroom-management` | Behavior, routines, environment (even if category is different) |
| `communication` | Parent contact, feedback, interpersonal skills |
| `wellness` | Educator health, self-care, sustainability |
| `leadership` | Admin, principals, instructional coaches |
| `planning` | Lesson planning, curriculum design, pacing |
| `assessment` | Formative/summative assessment, data use |
| `feedback` | Giving/receiving feedback, observation debriefs |
| `observation` | Classroom observations, walkthroughs |
| `K-12` | Relevant across all grade levels |
| `early-childhood` | Pre-K, kindergarten specific |
| `CTE` | Career and technical education |
| `vocational` | Vocational instruction |
| `credentials` | Certification, professional credentials |
| `autism` | Autism-specific resources |
| `executive-functioning` | EF skills, self-regulation |

### Tag rules

- Every Quick Win gets at least one tag.
- Use `general` only when no specific tag applies. Don't combine `general` with specific tags.
- A resource can have up to 5 tags. More than that dilutes search relevance.
- Tags describe the **content focus**, not the format. Don't tag based on file type.

---

## Field 5: Roles

Who this resource is designed for. Always an array, can include multiple.

| Value | Description |
|---|---|
| `teacher` | Classroom teachers (most resources) |
| `para` | Paraprofessionals, teaching assistants, aides |
| `leader` | Principals, assistant principals, deans |
| `coach` | Instructional coaches, mentors, department leads |

### Role rules

- Most resources should include `teacher` unless they are specifically NOT for classroom teachers.
- Para-specific resources (SpEd toolkits, PA guides) should include `para` and usually `teacher` too.
- Leadership resources should include `leader` and often `coach`.
- When in doubt, be inclusive rather than exclusive -- educators self-filter.

---

## Field 6: Danielson Domains

Which domain(s) of the Danielson Framework for Teaching this resource supports. Always an array.

| Value | Domain | Covers |
|---|---|---|
| `1-planning` | Planning & Preparation | Lesson design, curriculum, assessment design, content knowledge |
| `2-environment` | Classroom Environment | Culture, behavior management, physical space, respect, rapport |
| `3-instruction` | Instruction | Teaching strategies, questioning, engagement, differentiation |
| `4-professional` | Professional Responsibilities | Reflection, professional growth, communication with families, contributing to school |

### Domain rules

- Most resources map to 1-2 domains. Rarely more than 2.
- Self-Care and Wellness resources are almost always `4-professional`.
- Classroom Management resources are almost always `2-environment`.
- Assessment resources are usually `1-planning` (design) or `3-instruction` (formative/in-the-moment).
- Communication resources are usually `4-professional` (family communication) or `2-environment` (student rapport).

---

## Field 7: Tier / Access Tier

Content access level. These two fields must be kept in sync.

| `tier` (display) | `access_tier` (control) | Who can access |
|---|---|---|
| `Free` | `free` | Anyone (5 rotating items) |
| `Essentials` | `essentials` | Paid Substack subscribers, $5/mo (20 hand-picked items) |
| `Professional` | `professional` | Individual paid members, $10/mo (~40% of all Quick Wins) |
| `All-Access` | `all_access` | Partnership schools only, $25/mo (100% of everything) |

### Tier rules

- Default to `Professional` / `professional` for ALL new Quick Wins.
- Rae manually promotes items to `Essentials` (hand-picked) or `Free` (rotating).
- `Free` tier: only 5 items at a time, rotated periodically.
- `Essentials` tier: capped at 20 hand-picked best tools across categories.
- `All-Access` gets everything automatically, no action needed.
- When in doubt, use `Professional`.

---

## Tagging checklist

Before marking any Quick Win as `is_published = true`, verify:

- [ ] `title` is clear and specific (not generic like "Resource 1")
- [ ] `description` is 1-2 sentences, explains what it is and why it helps
- [ ] `category` is one of the 7 approved values
- [ ] `lift` is `LOW`, `MED`, or `HIGH` per the decision tree
- [ ] `quick_win_type` and `resource_type` match and are from the approved list
- [ ] `topic_tags` has at least 1 tag, max 5, all from the approved list
- [ ] `roles` has at least 1 role from the approved list
- [ ] `danielson_domains` has at least 1 domain from the approved list
- [ ] `tier` and `access_tier` are in sync
- [ ] `slug` is set (URL-safe, lowercase, hyphenated)

---

## Batch tagging process (for agents)

When tagging a batch of untagged Quick Wins:

1. Query for all Quick Wins missing required fields
2. For each item, read the title and description
3. Apply the decision rules above for each field
4. Set `lift_uncertain = true` on any item where lift is genuinely ambiguous
5. Run the update as a single SQL transaction
6. Report: how many tagged, how many flagged uncertain, any items that couldn't be categorized
7. Create a follow-up ticket for Rae to review any uncertain items

---

## Versioning

- **v1.0** -- July 18, 2026. Initial spec created from codebase analysis and existing data patterns.
- If categories, tags, or roles are added/changed, update this spec first, then update the DB and UI to match.
