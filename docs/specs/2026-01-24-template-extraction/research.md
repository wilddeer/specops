# Research Progress: Template Extraction

**Started:** 2026-01-24
**Status:** complete

---

## Objective

Extract embedded templates from `skills/spec-driven-work/SKILL.md` into separate files in an `assets/` folder, following the [Agent Skills specification](https://agentskills.io/specification). Ensure all templates are detailed to the same extent.

Key questions:
1. What templates exist currently?
2. Which templates need expansion?
3. What folder structure/naming conventions should be defined?

---

## Defined Terms

| Term | Definition |
|------|------------|
| Template | A reusable document structure with placeholders for project-specific content |
| Assets folder | Directory for static resources per Agent Skills spec (`assets/`) |
| Spec folder | Project-specific directory containing spec.md, research.md, progress.md |

---

## Findings

### Finding 1: Current templates in SKILL.md
**Date:** 2026-01-24

Five templates are currently embedded in SKILL.md:

| Template | Section | Detail Level |
|----------|---------|--------------|
| Spec Document Structure | Spec Development Mode | **Minimal** - bare skeleton |
| Execution Plan | Execution Plan Requirement | Moderate |
| Research File | Research Tracking | **Detailed** |
| Progress File | Progress Tracking | **Detailed** |
| Verification Section | Verification Tracking | **Detailed** |

### Finding 2: Agent Skills spec on assets folder
**Date:** 2026-01-24

From https://agentskills.io/specification:
> `assets/` - Contains static resources: Templates (document templates, configuration templates), Images (diagrams, examples), Data files (lookup tables, schemas)

Templates belong in `assets/` directory at skill root.

---

## Decisions

### Decision 1: Three templates only
**Date:** 2026-01-24

**Decision:** Extract only 3 templates: research.md, spec.md, progress.md
**Rationale:** These are the 3 files created per spec. Verification section is part of progress.md. Execution plan is part of spec.md.

### Decision 2: Template file naming
**Date:** 2026-01-24

**Decision:** Name template files `research-template.md`, `spec-template.md`, `progress-template.md`
**Rationale:** Clear purpose, consistent naming, distinguishable from actual spec files.

### Decision 3: Spec folder naming convention
**Date:** 2026-01-24

**Decision:** Spec folders follow pattern `[year]-[month]-[day]-[work-name]`
**Rationale:** Chronological ordering with day precision, easy to identify, avoids naming conflicts.

### Decision 4: Expand spec template
**Date:** 2026-01-24

**Decision:** Expand the Spec Document Structure template to match the detail level of research/progress templates.
**Rationale:** Current spec template is minimal (bare skeleton) while research/progress are detailed with all sections and placeholders.

### Finding 3: Simple spec structure (execution-mode-default)
**Date:** 2026-01-24

Examined `.specs/2026-01-execution-mode-default/spec.md`. Structure:

1. Overview (what + rationale)
2. Current State
3. Target State
4. Changes Required
5. Steps
6. Execution Plan

### Finding 4: Comprehensive spec structure (visitor-chatbot-migration)
**Date:** 2026-01-24

User shared a more detailed real-world spec. Structure:

**Header:**
- Status (Draft/In Progress/Complete)
- Created date
- Research link

**Core sections:**
1. **Overview** - What + current state/target state (concise)
2. **Scope** - In Scope / Out of Scope (explicit boundaries)
3. **Implementation Steps** - Detailed per step:
   - Action (what to do)
   - Files (affected)
   - Changes (code snippets)
   - Verification checklist per step

**Optional sections (as needed):**
- Context/Background (domain-specific info, e.g., "Features Preserved")
- Verification Checklist (overall, after all steps)
- Testing Plan (manual testing, what to compare)
- Rollback Plan (if issues arise)
- Future Work / Phase 2

**Key insight:** Specs can be flexible. Core structure is required, domain-specific sections are added as needed.

### Decision 5: Spec storage location
**Date:** 2026-01-24

**Decision:** Ask user where to store specs if unclear, no default assumption.
**Rationale:** Projects vary. Don't assume `.specs/` or any other location.

### Decision 6: Spec template sections
**Date:** 2026-01-24

**Decision:** Spec template has required + optional sections.

**Required sections:**
| Section | Purpose |
|---------|---------|
| Header | Status, Created, Research link |
| Overview | What we're doing + current/target state |
| Scope | In scope / Out of scope (explicit boundaries) |
| Steps | Actionable implementation steps |
| Verification Checklist | Overall checks after all steps |
| Execution Plan | Mode (direct/subagents) + rationale |

**Optional sections (add as needed):**
| Section | When to use |
|---------|-------------|
| Context/Background | Domain-specific info needed to understand the work |
| Testing Plan | When manual/automated testing is needed |
| Rollback Plan | When changes are risky or reversible |

**Step format:**
```markdown
### Step N: [Title]

**Action:** [What to do]

**Files:**
- [files affected]

**Changes:**
[code snippets if applicable]

**Verification:**
- [ ] [check 1]
- [ ] [check 2]
```

**Rationale:** Based on real-world spec examples. Verification Checklist required because specs need clear success criteria. Future Work removed as too arbitrary.

---

## Open Questions

~~1. Where should project specs be stored by default?~~ **ANSWERED:** Decision 5
~~2. Should the spec template include all 6 sections from the example?~~ **ANSWERED:** Decision 6
