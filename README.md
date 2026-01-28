# SpecOps

Work on projects through well-defined specs. Spec-first approach with no ambiguity — every process gets specced, cross-referenced, and followed.

## What it does

**Two modes:**
- **Spec Development** — Collaborative discussion to create and refine specs
- **Spec Execution** — Follow existing specs literally, flag gaps

Modes blend fluidly: execute a spec, hit a gap, develop spec to fill it, resume execution.

**Core principles:**
- Discussion by default — when input is ambiguous, discuss don't act
- Define before use — no vague or undefined terms
- Propose, show, confirm — never change without explicit approval
- One thing at a time — no bundled changes
- Don't assume, verify — research and check, never guess

## Contents

| Type | Name | Description |
|------|------|-------------|
| Skill | `spec-development` | Research, discuss, and write specs |
| Skill | `spec-execution` | Execute specs literally, track progress |
| Skill | `spec-step-execution` | Execute a single step from a spec |
| Skill | `spec-step-review` | Review implementation against spec |
| Agent | `spec-step-executor` | Subagent for executing individual spec steps |
| Agent | `spec-step-reviewer` | Subagent for reviewing implementation against spec |

Skills are usually automatically exposed as commands (`/spec-development`, `/spec-execution`).

## Installation

### Universal (recommended)

```bash
npx skills add wilddeer/specops
```

See [skills.sh](https://skills.sh/) for details.

### Cursor (manual install)

Windows (PowerShell):
```powershell
irm https://raw.githubusercontent.com/wilddeer/specops/v3.1.0/install.ps1 | iex
```

macOS/Linux:
```bash
curl -fsSL https://raw.githubusercontent.com/wilddeer/specops/v3.1.0/install.sh | bash
```

This installs skills to `~/.cursor/skills/specops/` and agents to `~/.cursor/agents/`.

To uninstall, remove `~/.cursor/skills/specops/` and `~/.cursor/agents/spec-step-executor.md`.

### Claude Code

```bash
# Add marketplace
/plugin marketplace add wilddeer/specops

# Install the plugin
/plugin install specops@wilddeer-specops
```

## License

MIT
