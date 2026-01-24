# Spec: Update Install Scripts

**Status:** Complete
**Created:** 2026-01-24
**Research:** [research.md](./research.md)

---

## Overview

Update Cursor installation scripts to include the new `assets/` folder files for the `spec-driven-work` skill.

**Current state:** Scripts download SKILL.md files only
**Target state:** Scripts also download assets folder contents

---

## Scope

### In Scope
- Update `install.ps1` to download assets files
- Update `install.sh` to download assets files
- Create assets directory during installation

### Out of Scope
- Version bump (will be done separately)
- Changes to skill content
- Changes to agent files

---

## Context

### Current safety characteristics

**Idempotency:** Both scripts are already idempotent:
- Directory creation uses `-Force` (PS) / `mkdir -p` (Bash)
- File downloads overwrite existing files
- Clean step removes only `$SKILLS_DIR` (specops folder), not entire skills dir

**Scoping:** Scripts only touch specops-owned paths:
- `$SKILLS_DIR` = `~/.cursor/skills/specops/` (owned by specops)
- `$AGENTS_DIR` = `~/.cursor/agents/` (shared, but only writes `spec-step-executor.md`)

**Uninstall instructions:** Currently show:
- Remove `$SKILLS_DIR` recursively (includes assets since they're inside)
- Remove `spec-step-executor.md` from agents

Assets folder is inside `$SKILLS_DIR/spec-driven-work/assets/`, so current uninstall instructions already cover it.

---

## Steps

### Step 1: Update install.ps1

**Action:** Add assets directory creation and file downloads to PowerShell script.

**Files:**
- `install.ps1`

**Changes:**

1. Add directory creation for assets:
```powershell
New-Item -ItemType Directory -Force -Path "$SKILLS_DIR\spec-driven-work\assets" | Out-Null
```

2. Add assets to download list:
```powershell
$assets = @(
    "research-template.md",
    "spec-template.md",
    "progress-template.md"
)

foreach ($asset in $assets) {
    $url = "$REPO_RAW/skills/spec-driven-work/assets/$asset"
    $dest = "$SKILLS_DIR\spec-driven-work\assets\$asset"
    
    try {
        Invoke-WebRequest -Uri $url -OutFile $dest -UseBasicParsing
        Write-Success "  Downloaded assets/$asset"
    } catch {
        Write-Error "  Failed to download assets/${asset}: $_"
        exit 1
    }
}
```

**Verification:**
- [ ] Assets directory created in script
- [ ] All 3 asset files downloaded
- [ ] Error handling present

---

### Step 2: Update install.sh

**Action:** Add assets directory creation and file downloads to Bash script.

**Files:**
- `install.sh`

**Changes:**

1. Add directory creation for assets:
```bash
mkdir -p "$SKILLS_DIR/spec-driven-work/assets"
```

2. Add assets download section:
```bash
# Download assets
status "Downloading assets..."

for asset in "research-template.md" "spec-template.md" "progress-template.md"; do
    url="$REPO_RAW/skills/spec-driven-work/assets/$asset"
    dest="$SKILLS_DIR/spec-driven-work/assets/$asset"
    
    if download "$url" "$dest"; then
        success "  Downloaded assets/$asset"
    else
        error "  Failed to download assets/$asset"
        exit 1
    fi
done
```

**Verification:**
- [ ] Assets directory created in script
- [ ] All 3 asset files downloaded
- [ ] Error handling present

---

## Verification Checklist

After all steps:

- [ ] `install.ps1` creates assets directory
- [ ] `install.ps1` downloads all 3 template files
- [ ] `install.sh` creates assets directory
- [ ] `install.sh` downloads all 3 template files
- [ ] Both scripts maintain consistent structure/style
- [ ] **Idempotency:** Scripts safe to run multiple times (re-running overwrites only specops files)
- [ ] **Uninstall instructions:** Updated to mention assets folder cleanup
- [ ] **Safety:** Scripts only delete/overwrite files within specops directories, not arbitrary files

---

## Execution Plan

**Mode:** Direct
**Rationale:** Small, focused changes to 2 files. Easy to review.
