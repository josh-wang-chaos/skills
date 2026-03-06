---
name: spec-context
description: Fetch relevant specs from Linear as context before working on bugs, enhancements, or new features. Use when starting a bugfix, investigating an issue, planning an enhancement, or when you need to understand how a system area is supposed to behave. Searches the Linear knowledge base of Spec-labeled tickets.
---

# Spec Context

Fetch relevant living specs from Linear to inform your work on bugs, enhancements, or features.

## Prerequisites

This skill requires the Linear MCP server. If Linear MCP tools are not available, stop and ask the user to set it up:

```bash
claude mcp add --transport http linear https://mcp.linear.app/mcp
```

Do NOT run this command yourself. The user must run it and restart Claude Code.

## When to Use

Use this skill BEFORE diving into implementation when:
- Fixing a bug — understand intended behavior and invariants
- Building an enhancement — understand current design and constraints
- Reviewing code — check against documented contracts
- Investigating an issue — find known edge cases

## Process

### 1. Identify the relevant area

From the task at hand, determine which module/area is involved (e.g., auth, payments, API).

### 2. Search for specs

Use Linear MCP to search for issues with the `Spec` label. Try:
- Search by area label (e.g., `auth`, `payments`)
- Search by keyword from the task description
- If unsure, list all `Spec`-labeled issues and scan titles

### 3. Read the spec

Read the full issue body (the canonical spec) and recent comments (for evolution history).

### 4. Extract relevant context

From the spec, pull out:
- **Invariants** — things that must remain true (don't break these)
- **Edge cases** — known tricky scenarios (check your fix handles these)
- **API contracts** — interfaces that must be preserved
- **Related specs** — other areas that might be affected

### 5. Apply to your task

- For **bugfixes**: Check if the bug violates a documented invariant. If the spec is wrong (not the code), update the spec.
- For **enhancements**: Ensure your change is compatible with documented constraints. Update the spec after implementation.
- For **new features**: Check for overlap or conflicts with existing specs.

### 6. Update the spec if needed

After completing your task, if you learned something new about the system (a new edge case, a corrected invariant, a new constraint), update the spec by:
- Updating the issue body with the new canonical version
- Adding a comment explaining what changed and why

This keeps the knowledge base alive and accurate.
