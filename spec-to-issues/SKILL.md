---
name: spec-to-issues
description: Break a Linear spec into independently-grabbable GitHub issues using vertical slices (tracer bullets). Use when user wants to convert a spec into implementation issues, create GitHub issues from a Linear spec, or plan work as vertical slices.
---

# Spec to Issues

Break a Linear spec into independently-grabbable GitHub issues using vertical slices (tracer bullets).

## Prerequisites

This skill requires the Linear MCP server. If Linear MCP tools are not available, stop and ask the user to set it up:

```bash
claude mcp add --transport http linear https://mcp.linear.app/mcp
```

Do NOT run this command yourself. The user must run it and restart Claude Code.

## Process

### 1. Locate the spec

Ask the user for the spec title or area. Use Linear MCP to search for issues with the `Spec` label.

If multiple specs match, present the list and ask the user to pick one.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.

### 3. Draft vertical slices

Break the spec into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices may be 'HITL' or 'AFK'. HITL slices require human interaction, such as an architectural decision or a design review. AFK slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories from the spec this addresses

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

### 5. Create the GitHub issues

For each approved slice, create a GitHub issue using `gh issue create`. Use the issue body template below.

Create issues in dependency order (blockers first) so you can reference real issue numbers in the "Blocked by" field.

<issue-template>
## Parent Spec

Linear: <spec-title> (<link-to-linear-issue>)

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation. Reference specific sections of the parent spec rather than duplicating content.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- Blocked by #<issue-number> (if any)

Or "None - can start immediately" if no blockers.

## User stories addressed

Reference by number from the parent spec:

- User story 3
- User story 7

</issue-template>

### 6. Link back to the spec

After creating all GitHub issues, add a comment on the Linear spec issue listing all the created GitHub issues:

```markdown
## Implementation Issues

Created the following GitHub issues from this spec:

- [ ] owner/repo#123 — <title>
- [ ] owner/repo#124 — <title>
- [ ] owner/repo#125 — <title>
```

This keeps the spec as the single source of truth with traceability to implementation work.
