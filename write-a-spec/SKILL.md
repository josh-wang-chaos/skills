---
name: write-a-spec
description: Author or update a living spec ticket in Linear. Use when user wants to create a spec, document system behavior, capture architectural decisions, or update an existing spec. Specs are long-lived artifacts that evolve over time.
---

# Write a Spec

Create or update a living specification ticket in Linear. Specs are long-lived artifacts that serve as the knowledge base for the system.

## Prerequisites

This skill requires the Linear MCP server. If Linear MCP tools are not available, stop and ask the user to set it up:

```bash
claude mcp add --transport http linear https://mcp.linear.app/mcp
```

Do NOT run this command yourself. The user must run it and restart Claude Code.

## Creating a New Spec

### 1. Gather context

Ask the user for:
- Which area/module does this spec cover? (e.g., auth, payments, API gateway)
- Is this for an existing system (documenting current behavior) or a new design?

### 2. Explore the codebase

If speccing an existing system, explore the repo to understand current behavior. Verify the user's assertions.

### 3. Interview the user

Walk through each section of the spec template below. Ask targeted questions:
- What are the invariants that must always hold?
- What are the known edge cases?
- What are the API contracts or interfaces?
- What constraints exist (performance, compatibility, security)?

### 4. Create the Linear ticket

Use the Linear MCP to create an issue in the user's team with:
- **Label**: `Spec`
- **Title**: `[Spec] <Module/Area Name>`
- **Body**: Use the spec template below

### Spec Template

Use the same structure as a PRD:

```markdown
## Problem Statement

The problem or domain this spec covers, from the user's perspective.

## Solution

The current or proposed solution, from the user's perspective.

## User Stories

A LONG, numbered list of user stories:

1. As an <actor>, I want a <feature>, so that <benefit>

This list should be extensive and cover all aspects of the system area.

## Implementation Decisions

- The modules involved
- The interfaces and their contracts
- Architectural decisions and rationale
- Schema details
- API contracts
- Key interactions and data flows
- Invariants that must always hold
- Known edge cases and how they're handled

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## Testing Decisions

- What makes a good test for this area
- Which modules have test coverage
- Prior art for tests in the codebase

## Out of Scope

What this spec intentionally does NOT cover.

## Further Notes

Any additional context, links, or references.
```

## Updating an Existing Spec

### 1. Find the spec

Ask the user for the spec title or area. Use Linear MCP to search for issues with the `Spec` label.

### 2. Review current state

Read the existing spec issue body and comments to understand the current state.

### 3. Make updates

- **Update the issue body** with the new canonical version of the spec
- **Add a comment** explaining what changed and why, so the evolution timeline is preserved

Comment format:
```markdown
## Spec Update — <date>

### What changed
- <list of changes>

### Why
<rationale for the changes>

### Triggered by
<link to PR, issue, or conversation that prompted the update>
```

## Labeling Convention

Every spec issue MUST have the `Spec` label. Additional labels should indicate the area:
- Module/area labels (e.g., `auth`, `payments`, `api`)
- `frontend`, `backend`, or `fullstack` to indicate which codebase is involved
