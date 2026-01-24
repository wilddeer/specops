# Research Progress: Skill Structure Refactor

**Started:** 2026-01-24
**Status:** complete

---

## Objective

Investigate whether the `spec-driven-work` SKILL.md should be split into separate files using the `references/` folder pattern from the Agent Skills specification.

Key questions:
1. Does the current SKILL.md exceed recommended limits?
2. Would splitting Dev mode and Execution mode into separate files improve usability?
3. What are best practices for skill structure?
4. What are the tradeoffs?

---

## Defined Terms

| Term | Definition |
|------|------------|
| Progressive disclosure | Loading only metadata initially, then full skill on activation, then references on demand |
| References folder | Optional `references/` directory for additional documentation agents can read when needed |

---

## Findings

### Finding 1: Current SKILL.md size
**Date:** 2026-01-24

Measured current `skills/spec-driven-work/SKILL.md`:
- **Lines:** 407 (per IDE) / 282 (content lines per PowerShell)
- **Words:** ~2,062
- **Characters:** ~13,116
- **Estimated tokens:** ~2,500-3,500

Agent Skills spec recommendations:
- < 500 lines ✓ (we're at 407)
- < 5,000 tokens recommended ✓ (we're at ~3,000)

**Conclusion:** Currently within limits, but on the higher side.

### Finding 2: Agent Skills progressive disclosure pattern
**Date:** 2026-01-24

From [agentskills.io](https://agentskills.io/what-are-skills) and web search:

**Progressive disclosure works in 3 stages:**
1. **Discovery** - Only name + description loaded at startup (~50-100 tokens per skill)
2. **Activation** - Full SKILL.md loads when task matches skill
3. **Execution** - Referenced files load on-demand as needed

**Key insight:** The `references/` folder is for content that's NOT always needed. Agents load references only when required during execution.

**When to use references/ (per spec):**
- Detailed technical reference (REFERENCE.md)
- Domain-specific files (finance.md, legal.md)
- Form templates
- Content that's needed for specific subtasks, not every activation

### Finding 3: Best practices on splitting prompts
**Date:** 2026-01-24

From OpenAI and multi-agent orchestration guides:

**Key principle:** Split by functional responsibility, not just size.

**Routines pattern:**
- Self-contained sets of steps + tools
- Each routine has its own system prompt
- Clear conditional logic for when to use

**When splitting helps:**
- Different modes that don't overlap (one active at a time)
- Specialized subtasks with their own tools
- Content needed only for specific branches

**When to keep together:**
- Core principles that apply everywhere
- Mode transition logic
- Shared context/terminology

### Finding 4: spec-driven-work structure analysis
**Date:** 2026-01-24

Current SKILL.md sections and their scope:

| Section | Lines | Scope |
|---------|-------|-------|
| Overview | ~15 | Both modes (introduces fluid blending) |
| Core Principles | ~100 | Both modes (universal behaviors) |
| Spec Development Mode | ~110 | Dev only |
| Spec Execution Mode | ~135 | Exec only |
| Mode Transitions | ~10 | Both (explicitly references both modes) |
| Anti-Patterns | ~15 | Both modes |
| Checklist | ~10 | Both modes |

**Critical observation:** The Overview explicitly states "Modes blend fluidly: you might execute, hit a gap, develop spec to fill it, resume execution."

Mode Transitions table shows 6 scenarios where modes switch mid-work. This means both modes need to be accessible during any skill activation - they're not independent branches.

### Finding 5: Context engineering tradeoffs (2025 best practices)
**Date:** 2026-01-24

From [context engineering guides](https://promptbuilder.cc/blog/context-engineering-agents-guide-2025) and [agentic systems research](https://blog.jroddev.com/context-window-management-in-agentic-systems/):

**When modular/split helps:**
- Content can be selectively included based on current task
- Different branches are mutually exclusive
- Components have independent lifecycles
- Reduces "context rot" from irrelevant info

**When monolithic is better:**
- Content is always needed together
- Modes/branches blend and switch frequently
- Shared principles apply across all scenarios
- Splitting would require loading both files anyway

**Key insight:** "Stuffing entire documents into a single prompt is expensive, but fragmenting tightly-coupled context across components creates coherence problems."

### Finding 6: Current spec-step-executor structure
**Date:** 2026-01-24

Current setup involves two files:

**1. `agents/spec-step-executor.md`** (~46 lines)
- Agent definition file (not a skill)
- Contains: description, examples, model, color
- Instructions: "Invoke the `spec-step-execution` skill and follow it exactly"
- Restrictions: no user interaction, no TodoWrite

**2. `skills/spec-step-execution/SKILL.md`** (~125 lines)
- Separate skill with full instructions
- Contains: Input requirements, 5-step process, gap detection, result formats
- Used by: spec-step-executor agent AND main process (direct execution)

**Key observation:** The skill is used in two contexts:
1. By subagents (spec-step-executor) running autonomously
2. By main process when executing steps directly

### Finding 7: Agents vs Skills - separate systems
**Date:** 2026-01-24

From [Cursor docs](https://cursor.com/docs/context/subagents) and [Claude Code docs](https://code.claude.com/docs/en/sub-agents):

**Agents and Skills are separate systems:**

| Aspect | Agents (subagents) | Skills |
|--------|-------------------|--------|
| Location | `.claude/agents/` or `~/.claude/agents/` | Skill directories with SKILL.md |
| Format | Markdown + YAML frontmatter | Markdown + YAML frontmatter |
| Purpose | Define subprocess with own context | Extend main agent's capabilities |
| Loading | Spawned as Task with own context window | Loaded into main agent's context |
| Prompt | Markdown body = system prompt | SKILL.md body = instructions for main agent |

**Key insight:** A subagent's system prompt comes from the agent file's markdown body. Skills are loaded into the calling agent's context, not the subprocess.

**Implication:** If spec-step-execution moved to `references/`:
- Main process could still read and follow it ✓
- But subagents would NOT automatically get it as their prompt ✗
- Would need to either:
  a) Keep agent file that explicitly reads the reference, OR
  b) Embed full instructions in agent file (duplicating content)

### Finding 8: Three architectural options
**Date:** 2026-01-24

**Option A: Current structure (separate skill + agent)**
```
skills/spec-step-execution/SKILL.md  (125 lines - the instructions)
agents/spec-step-executor.md         (46 lines - "invoke the skill")
```
- Pro: Skill usable by both main process and subagent
- Pro: Single source of truth
- Con: Two files to maintain

**Option B: Move to references folder**
```
skills/spec-driven-work/references/step-execution.md
agents/spec-step-executor.md (would need full instructions OR "read the reference")
```
- Pro: All spec-driven-work content in one skill folder
- Con: Subagent can't invoke skills, would need to read reference file explicitly
- Con: Reference file isn't auto-loaded for subagent

**Option C: Embed in agent file, eliminate skill**
```
agents/spec-step-executor.md (embed full 125 lines)
```
- Pro: Single file for subagent
- Con: Main process can't invoke it as a skill
- Con: Duplicates logic if main process also needs direct execution

### Finding 9: Task tool requires defined subagent types
**Date:** 2026-01-24

The Task tool (for spawning subagents) has a `subagent_type` parameter that must match a defined agent. Custom agents in `agents/` folder become available subagent types.

**If we remove the agent file:**
- Can't spawn that subagent type via Task tool
- Would need to use `generalPurpose` with custom prompt
- Loses the agent-specific configuration (tools, model, restrictions)

**Conclusion:** Agent files in `agents/` folder serve a distinct purpose - they define spawnable subagent types. Moving content to references doesn't eliminate the need for an agent definition.

---

## Decisions

### Decision 1: Do NOT split Dev/Execution modes into references/
**Date:** 2026-01-24

**Decision:** Keep Dev mode and Execution mode in the main SKILL.md. Do not split into `references/` folder.

### Decision 2: Restructure Spec Development Mode section
**Date:** 2026-01-24

**Decision:** Restructured into clear ordered steps:
1. Create Research File (mandatory first)
2. Research & Discuss
3. Confirm Research Complete (explicit checkpoint)
4. Write the Spec

**Key changes:**
- Research file is now mandatory for all spec development
- Decisions must be made by user (agent proposes, user decides)
- Explicit confirmation required before moving from research to spec writing
- Removed redundant folder naming mentions
- Execution Plan template details moved to template file

### Decision 3: Keep spec-step-executor agent and spec-step-execution skill separate
**Date:** 2026-01-24

**Decision:** Keep the current structure with separate agent and skill files.

**Rationale:**

1. **Different purposes** - The agent file defines a spawnable subagent type (required for Task tool). The skill provides instructions usable by both main process and subagents.

2. **Both usage contexts matter:**
   - Main process uses skill for direct execution
   - Subagent uses agent file which invokes the skill
   
3. **Moving to references doesn't help:**
   - Would still need agent file (can't eliminate)
   - Would add complexity (agent reads reference vs invokes skill)
   - No real benefit

4. **Embedding in agent breaks direct execution:**
   - Main process couldn't invoke it as a skill
   - Would need duplication

**Current structure is correct:**
```
skills/spec-step-execution/SKILL.md  → Used by main + subagent
agents/spec-step-executor.md         → Defines spawnable subagent type
```

**Rationale (Decision 1):**

1. **Modes blend fluidly** - The skill explicitly states modes switch mid-work (6 transition scenarios). Splitting would require loading both files on almost every activation, negating the benefit.

2. **Within recommended limits** - At ~407 lines and ~3000 tokens, we're under the 500 line / 5000 token recommendations.

3. **Shared context** - Core Principles, Anti-Patterns, Checklist, and Mode Transitions apply to both modes. Splitting creates duplication or cross-reference complexity.

4. **Progressive disclosure already achieved** - We already extracted templates to `assets/`. The skill now references files on-demand for the verbose template content.

5. **No clear "optional" content** - References folder is for content needed by specific subtasks, not core operational modes. Both modes are core.

**What WOULD justify splitting:**
- If skill exceeded 500 lines / 5000 tokens significantly
- If modes were mutually exclusive (not fluid)
- If there were domain-specific variants (e.g., research-focused vs code-focused versions)

---

## Open Questions

1. ~~Current SKILL.md size (lines, tokens) vs recommended limits?~~ **ANSWERED:** Finding 1 - 407 lines, ~3000 tokens, within limits
2. ~~How would Dev mode and Execution mode be split?~~ **ANSWERED:** Finding 4 - analyzed, but modes blend fluidly making split counterproductive
3. ~~Online opinions on skill/prompt structure for agents?~~ **ANSWERED:** Findings 2, 3, 5 - split by function when branches are independent, keep together when they blend
4. ~~Pros/cons of splitting vs keeping monolithic?~~ **ANSWERED:** Decision 1 - keep monolithic for this skill
5. ~~Should spec-step-executor agent/skill remain separate or move to references folder?~~ **ANSWERED:** Decision 3 - keep separate, serves distinct purposes
