# Research Progress: Update Install Scripts

**Started:** 2026-01-24
**Status:** complete

---

## Objective

Update the Cursor installation scripts (`install.ps1` and `install.sh`) to include the new `assets/` folder files that were added to `spec-driven-work` skill.

---

## Defined Terms

| Term | Definition |
|------|------------|
| Assets folder | Directory containing template files used by the skill |

---

## Findings

### Finding 1: Current installation scripts
**Date:** 2026-01-24

Both scripts (`install.ps1` and `install.sh`) currently download:

**Skills:**
- `skills/spec-driven-work/SKILL.md`
- `skills/spec-step-execution/SKILL.md`

**Agents:**
- `agents/spec-step-executor.md`

### Finding 2: New files to include
**Date:** 2026-01-24

The `spec-driven-work` skill now has an `assets/` folder with 3 templates:
- `assets/research-template.md`
- `assets/spec-template.md`
- `assets/progress-template.md`

These are referenced by the SKILL.md and need to be installed alongside it.

---

## Decisions

### Decision 1: No version bump
**Date:** 2026-01-24

**Decision:** Keep version at `v2.3.0`, do not bump yet.
**Rationale:** User decision - version bump will happen separately.

---

## Open Questions

1. ~~Should we bump the version number in the scripts?~~ **ANSWERED:** Decision 1 - no, not yet
