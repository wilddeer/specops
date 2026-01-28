# Spec: Skill Split (spec-driven-work)

**Status:** Complete
**Created:** 2026-01-28
**Research:** [research.md](./research.md)

---

## Overview

Split the `spec-driven-work` skill into two separate skills: `spec-development` and `spec-execution`.

**Current state:** Single `spec-driven-work` skill containing both development and execution modes in one file.
**Target state:** Two separate skills, each self-contained with core principles duplicated, cross-referencing each other.

---

## Scope

### In Scope
- Create `skills/spec-development/SKILL.md` with development mode content
- Create `skills/spec-execution/SKILL.md` with execution mode content
- Move assets to appropriate skill folders
- Delete original `skills/spec-driven-work/` folder
- Update README.md if it references the old skill

### Out of Scope
- Changes to `spec-step-execution` skill (remains as-is)
- Changes to agents (`spec-step-executor.md`, `spec-step-reviewer.md`)

---

## Steps

### Step 1: Create spec-development skill folder and assets

**Action:** Create the folder structure and copy assets used by spec-development.

**Files:**
- `skills/spec-development/` (create folder)
- `skills/spec-development/assets/research-template.md` (copy from spec-driven-work)
- `skills/spec-development/assets/spec-template.md` (copy from spec-driven-work)

**Verification:**
- [ ] Folder `skills/spec-development/assets/` exists
- [ ] `research-template.md` copied correctly
- [ ] `spec-template.md` copied correctly

---

### Step 2: Create spec-development SKILL.md

**Action:** Create the spec-development skill with:
- Frontmatter (name: Spec Development, description, version: 1.0.0)
- Overview adapted for development focus
- All 10 Core Principles (copied verbatim)
- Spec Development Mode content (Steps 1-5 from original)
- Execution Plan Requirement section
- Anti-Patterns (filtered to development-relevant ones)
- Checklist (adapted for development)
- Cross-reference to `spec-execution` skill for when spec is ready to execute

**Files:**
- `skills/spec-development/SKILL.md`

**Content structure:**
```markdown
---
name: Spec Development
description: Research, discuss, and write specs. Use this when creating or refining specs.
version: 1.0.0
---

# spec-development

## Overview
[Development-focused overview]
[Announce at start message]

## Core Principles
[All 10 principles - copied verbatim from original]

## Process
[Steps 1-5 from Spec Development Mode]
[Execution Plan Requirement section]

## When to Switch to Execution
[Reference to spec-execution skill]

## Anti-Patterns
[Development-relevant anti-patterns]

## Checklist
[Development-focused checklist]
```

**Verification:**
- [ ] Frontmatter is valid
- [ ] All 10 core principles present
- [ ] Steps 1-5 from development mode present
- [ ] Execution Plan Requirement section present
- [ ] Cross-reference to `spec-execution` included
- [ ] Asset paths updated to `assets/research-template.md` and `assets/spec-template.md`

---

### Step 3: Create spec-execution skill folder and assets

**Action:** Create the folder structure and copy assets used by spec-execution.

**Files:**
- `skills/spec-execution/` (create folder)
- `skills/spec-execution/assets/progress-template.md` (copy from spec-driven-work)

**Verification:**
- [ ] Folder `skills/spec-execution/assets/` exists
- [ ] `progress-template.md` copied correctly

---

### Step 4: Create spec-execution SKILL.md

**Action:** Create the spec-execution skill with:
- Frontmatter (name: Spec Execution, description, version: 1.0.0)
- Overview adapted for execution focus
- All 10 Core Principles (copied verbatim)
- Spec Execution Mode content (Process, Progress Tracking, Verification Tracking, Completion, Choosing Execution Mode, Direct Execution, Subagent Execution)
- Anti-Patterns (filtered to execution-relevant ones)
- Checklist (adapted for execution)
- Cross-reference to `spec-development` skill for when gaps are found
- Reference to `spec-step-execution` skill for single step execution

**Files:**
- `skills/spec-execution/SKILL.md`

**Content structure:**
```markdown
---
name: Spec Execution
description: Execute specs literally with zero creativity. Use this when following an existing spec.
version: 1.0.0
---

# spec-execution

## Overview
[Execution-focused overview]
[Announce at start message]

## Core Principles
[All 10 principles - copied verbatim from original]

## Process
[Process section from Spec Execution Mode]

## Progress Tracking
[Progress Tracking section]

## Verification Tracking
[Verification Tracking section]

## Completion
[Completion section]

## Execution Modes
[Choosing Execution Mode, Direct Execution, Subagent Execution sections]

## When to Switch to Development
[Reference to spec-development skill for gaps]

## Anti-Patterns
[Execution-relevant anti-patterns]

## Checklist
[Execution-focused checklist]
```

**Verification:**
- [ ] Frontmatter is valid
- [ ] All 10 core principles present
- [ ] All execution mode sections present
- [ ] Cross-reference to `spec-development` included
- [ ] Reference to `spec-step-execution` included
- [ ] Asset path updated to `assets/progress-template.md`

---

### Step 5: Update README.md references

**Action:** Check README.md for references to `spec-driven-work` and update to reference the new skills.

**Files:**
- `README.md`

**Verification:**
- [ ] No references to `spec-driven-work` remain
- [ ] New skills are referenced appropriately (if README lists skills)

---

### Step 6: Update installation scripts

**Action:** Update both `install.ps1` and `install.sh` to install the new skill structure:
- Create `spec-development` and `spec-execution` folders instead of `spec-driven-work`
- Download assets to correct skill folders:
  - `research-template.md` → `spec-development/assets/`
  - `spec-template.md` → `spec-development/assets/`
  - `progress-template.md` → `spec-execution/assets/`
- Update uninstall instructions in output messages

**Files:**
- `install.ps1`
- `install.sh`

**Changes (install.ps1 - directory creation):**
```powershell
# Old:
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-driven-work" | Out-Null
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-driven-work\assets" | Out-Null
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-step-execution" | Out-Null

# New:
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-development" | Out-Null
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-development\assets" | Out-Null
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-execution" | Out-Null
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-execution\assets" | Out-Null
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-step-execution" | Out-Null
```

**Changes (install.ps1 - skills list):**
```powershell
# Old:
$skills = @(
    @{ name = "spec-driven-work"; file = "SKILL.md" },
    @{ name = "spec-step-execution"; file = "SKILL.md" }
)

# New:
$skills = @(
    @{ name = "spec-development"; file = "SKILL.md" },
    @{ name = "spec-execution"; file = "SKILL.md" },
    @{ name = "spec-step-execution"; file = "SKILL.md" }
)
```

**Changes (install.ps1 - assets download):**
```powershell
# Old: all assets from spec-driven-work/assets/
# New: download each asset to its correct skill folder

$devAssets = @("research-template.md", "spec-template.md")
foreach ($asset in $devAssets) {
    $url = "$REPO_RAW/skills/spec-development/assets/$asset"
    $dest = "$SKILLS_DIR\spec-development\assets\$asset"
    # ... download logic
}

$execAssets = @("progress-template.md")
foreach ($asset in $execAssets) {
    $url = "$REPO_RAW/skills/spec-execution/assets/$asset"
    $dest = "$SKILLS_DIR\spec-execution\assets\$asset"
    # ... download logic
}
```

**Changes (install.sh - similar pattern):**
- Update mkdir commands for new folder structure
- Update skills loop: `"spec-development" "spec-execution" "spec-step-execution"`
- Split assets download into two loops (dev assets, exec assets)

**Verification:**
- [ ] `install.ps1` creates correct folder structure
- [ ] `install.ps1` downloads skills from correct paths
- [ ] `install.ps1` downloads assets to correct skill folders
- [ ] `install.sh` creates correct folder structure
- [ ] `install.sh` downloads skills from correct paths
- [ ] `install.sh` downloads assets to correct skill folders
- [ ] Scripts remain idempotent: remove entire `$SKILLS_DIR` before reinstalling (already done - verify this behavior is preserved)
- [ ] Upgrade scenario works: running new script after old installation removes `spec-driven-work` and installs new structure cleanly

---

### Step 7: Delete original spec-driven-work folder

**Action:** Delete `skills/spec-driven-work/` folder and all its contents.

**Files:**
- `skills/spec-driven-work/` (delete entire folder)

**Verification:**
- [ ] Folder `skills/spec-driven-work/` no longer exists

---

## Verification Checklist

> Overall checks after all steps complete.

- [ ] `skills/spec-development/SKILL.md` exists and is valid
- [ ] `skills/spec-development/assets/` contains research-template.md and spec-template.md
- [ ] `skills/spec-execution/SKILL.md` exists and is valid
- [ ] `skills/spec-execution/assets/` contains progress-template.md
- [ ] `skills/spec-driven-work/` is deleted
- [ ] Both new skills contain all 10 core principles
- [ ] Skills cross-reference each other correctly
- [ ] `spec-execution` references `spec-step-execution` for single step execution
- [ ] Installation scripts updated and idempotent

---

## Execution Plan

**Mode:** Direct
**Rationale:** Steps require careful content adaptation and verification. Not suitable for parallel execution since each skill's content depends on understanding the split correctly.
