# Spec: Template Extraction

**Status:** Complete
**Created:** 2026-01-24
**Research:** [research.md](./research.md)

---

## Overview

Extract embedded templates from `skills/spec-driven-work/SKILL.md` into separate files in an `assets/` folder, following the [Agent Skills specification](https://agentskills.io/specification).

**Current state:** Templates (research, spec, progress) are embedded inline in SKILL.md. Spec template is minimal compared to research/progress.
**Target state:** Templates live in `assets/` folder. All three templates have equal detail. SKILL.md references templates instead of embedding them.

---

## Scope

### In Scope
- Create `assets/` folder in `skills/spec-driven-work/`
- Extract 3 templates: `research-template.md`, `spec-template.md`, `progress-template.md`
- Expand spec template to match detail level of research/progress
- Update SKILL.md to reference templates
- Document folder naming convention (`[year]-[month]-[day]-[work-name]`)

### Out of Scope
- Changes to core principles or execution logic
- Changes to spec-step-execution skill
- Renaming existing spec folders to new convention

---

## Steps

### Step 1: Create assets folder

**Action:** Create the assets directory for templates.

**Files:**
- `skills/spec-driven-work/assets/` (new directory)

**Verification:**
- [ ] Directory exists at `skills/spec-driven-work/assets/`

---

### Step 2: Create research template

**Action:** Extract research file format from SKILL.md into standalone template.

**Files:**
- `skills/spec-driven-work/assets/research-template.md` (new)

**Verification:**
- [ ] Template contains all sections: Objective, Defined Terms, Findings, Decisions, Open Questions
- [ ] Placeholders are clear
- [ ] Format matches current embedded version

---

### Step 3: Create progress template

**Action:** Extract progress file format from SKILL.md into standalone template, including verification section.

**Files:**
- `skills/spec-driven-work/assets/progress-template.md` (new)

**Verification:**
- [ ] Template contains: header, step entries, verification section, issues section
- [ ] All status values documented
- [ ] Format matches current embedded version

---

### Step 4: Create spec template

**Action:** Create expanded spec template based on research decisions.

**Files:**
- `skills/spec-driven-work/assets/spec-template.md` (new)

**Template must include:**

Required sections:
- Header (Status, Created, Research link)
- Overview (what + current/target state)
- Scope (In Scope / Out of Scope)
- Steps (with Action, Files, Changes, Verification per step)
- Verification Checklist (overall)
- Execution Plan (mode + rationale)

Optional sections (documented as available):
- Context/Background
- Testing Plan
- Rollback Plan

**Verification:**
- [ ] All required sections present with placeholders
- [ ] Optional sections listed with guidance on when to use
- [ ] Step format documented (Action, Files, Changes, Verification)
- [ ] Detail level matches research/progress templates

---

### Step 5: Update SKILL.md references

**Action:** Replace embedded template code blocks with references to asset files.

**Files:**
- `skills/spec-driven-work/SKILL.md`

**Changes:**
- Research Tracking section: replace embedded format with link to `assets/research-template.md`
- Progress Tracking section: replace embedded format with link to `assets/progress-template.md`
- Spec Document Structure section: replace minimal skeleton with link to `assets/spec-template.md`
- Add folder naming convention (`[year]-[month]-[day]-[work-name]`)

**Verification:**
- [ ] No embedded template code blocks remain (except small inline examples)
- [ ] Links to all 3 templates work
- [ ] Folder naming convention documented

---

## Verification Checklist

After all steps:

- [ ] `assets/` folder exists with 3 template files
- [ ] All templates have similar detail level
- [ ] SKILL.md references templates instead of embedding them
- [ ] Folder naming convention `[year]-[month]-[day]-[work-name]` is documented
- [ ] No broken links in SKILL.md

---

## Execution Plan

**Mode:** Direct
**Rationale:** Small, focused changes with user review at each step. Templates need careful wording review.
