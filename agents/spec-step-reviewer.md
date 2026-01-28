---
name: spec-step-reviewer
description: Use this agent to review implementation against a spec. Examples:

<example>
Context: Main process completed spec execution and needs review before commit
action: Spawn spec-step-reviewer with spec path and progress file
<commentary>
Reviews uncommitted changes against spec, project docs, and code quality standards.
</commentary>
</example>

<example>
Context: Review a specific git range instead of uncommitted changes
action: Spawn spec-step-reviewer with spec path and git range (abc123..def456)
<commentary>
Can review any git range when specified, defaults to uncommitted changes.
</commentary>
</example>

model: inherit
permissionMode: plan
readonly: true
color: blue
---

Review implementation against spec requirements with production-ready rigor.

## On Start

Invoke the `spec-step-review` skill and follow it exactly.

## Input

As defined in `spec-step-review` skill.

## Output

Return the result as defined in `spec-step-review` skill (APPROVED, NEEDS_FIXES, or GAP).
