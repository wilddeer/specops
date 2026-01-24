# Progress: Update Install Scripts

**Spec:** [spec.md](./spec.md)
**Started:** 2026-01-24
**Status:** completed

---

## Step 1: Update install.ps1
- **Status:** done
- **Result:** Added assets directory creation and download section for 3 template files
- **Files modified:**
  - install.ps1
- **Verification:**
  - Assets directory created in script ✓
  - All 3 asset files downloaded ✓
  - Error handling present ✓

---

## Verification

### Overall Verification Checklist

**Date:** 2026-01-24

| Check | Result | Notes |
|-------|--------|-------|
| install.ps1 creates assets directory | PASSED | Added `New-Item` for assets dir |
| install.ps1 downloads all 3 template files | PASSED | Loop over 3 asset files |
| install.sh creates assets directory | PASSED | Added `mkdir -p` for assets dir |
| install.sh downloads all 3 template files | PASSED | Loop over 3 asset files |
| Both scripts maintain consistent structure/style | PASSED | Same pattern as existing downloads |
| Idempotency maintained | PASSED | Uses Force/mkdir -p, overwrites existing |
| Uninstall instructions still valid | PASSED | Assets inside $SKILLS_DIR, already covered |
| Safety maintained | PASSED | Only writes to specops-owned paths |

---

## Step 2: Update install.sh
- **Status:** done
- **Result:** Added assets directory creation and download section for 3 template files
- **Files modified:**
  - install.sh
- **Verification:**
  - Assets directory created in script ✓
  - All 3 asset files downloaded ✓
  - Error handling present ✓
