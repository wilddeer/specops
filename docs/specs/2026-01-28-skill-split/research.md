# Research Progress: Skill Split (spec-driven-work)

**Started:** 2026-01-28
**Status:** complete

---

## Objective

Split the `spec-driven-work` skill into two separate skills:
1. Research & Development skill
2. Execution skill

Core principles should remain in both skills.

Key questions to answer before spec can be written:
- How should the skills be named?
- What content goes in each skill?
- How do the skills reference each other?
- Where should the new skill files live?

---

## Defined Terms

| Term | Definition |
|------|------------|
| | |

---

## Findings

### Finding 1: Relationship with spec-step-execution
**Date:** 2026-01-28

The existing `spec-step-execution` skill remains separate. The hierarchy is:
- `spec-development` - research, discuss, write specs
- `spec-execution` - orchestrate execution of a full spec (choose execution mode, track progress, handle gaps)
- `spec-step-execution` - execute a single step (already exists, stays separate - referenced by `spec-execution` during direct execution)

### Finding 2: Installation scripts need updating
**Date:** 2026-01-28

`install.ps1` and `install.sh` reference `spec-driven-work` and need to be updated to install the new skill structure. Scripts are already idempotent (clean previous installation before reinstalling).

---

## Decisions

### Decision 1: Skill Names
**Date:** 2026-01-28

**Decision:** The two skills will be named `spec-development` and `spec-execution`
**Rationale:** These names match the existing mode names in the current skill ("Spec Development Mode" and "Spec Execution Mode")

### Decision 2: Core Principles Handling
**Date:** 2026-01-28

**Decision:** Full duplication - copy the 10 core principles into both skills
**Rationale:** Skills spec doesn't have a good mechanism for sharing content between skills. Each skill should be self-contained.

### Decision 3: File Structure
**Date:** 2026-01-28

**Decision:** Separate folders - `skills/spec-development/SKILL.md` and `skills/spec-execution/SKILL.md`
**Rationale:** Clean split with fresh folders for both. Assets folder goes with spec-development (since research/spec templates are used during development).

### Decision 4: Cross-Referencing
**Date:** 2026-01-28

**Decision:** Each skill references the other by name - e.g., "If you hit a gap during execution, switch to the `spec-development` skill"
**Rationale:** Preserves the "modes blend fluidly" behavior. Agent knows when to suggest switching skills.

### Decision 5: Existing Skill Handling
**Date:** 2026-01-28

**Decision:** Delete `skills/spec-driven-work/` entirely after creating the new skills
**Rationale:** New skills fully replace it. Keeping the old one would create confusion about which to use.

---

## Open Questions

1. ~~What should the two skills be named?~~ **ANSWERED:** See Decision 1
2. ~~Should core principles be duplicated or referenced from a shared location?~~ **ANSWERED:** See Decision 2
3. ~~Where should the new skill files live (same folder structure, new folders)?~~ **ANSWERED:** See Decision 3
4. ~~How should the skills cross-reference each other for mode transitions?~~ **ANSWERED:** See Decision 4
5. ~~What happens to the existing `spec-driven-work` skill - delete, deprecate, or keep as an umbrella?~~ **ANSWERED:** See Decision 5
