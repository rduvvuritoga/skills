---
name: save-for-later
description: Capture deferred implementation work as a durable GitHub issue. Use when the user asks to save, park, defer, or preserve a feature, plan, or technical task for later.
allowed-tools: Bash Read Grep Glob
---

# Save for Later

Preserve enough current evidence and implementation intent that another agent can resume the work without reconstructing the conversation.

## Workflow

1. Resolve the target repository with `gh repo view`. If the project has no GitHub issue tracker, report that limitation and ask where the durable task should live.

2. Use `$ARGUMENTS` as the requested focus when present. Gather the rest from conversation and repository evidence:

   - Problem or opportunity and why it is deferred.
   - Current behavior and relevant files, symbols, services, or commands.
   - Proposed implementation sequence.
   - Decisions, trade-offs, dependencies, and blockers.
   - Checkable acceptance criteria and validation commands.

3. Inspect available labels with `gh label list --json name`. Use a user- or repository-specified label; otherwise use `enhancement` only when that label exists. Label absence must not block issue creation.

4. Create the issue with `gh issue create` using a concise title and this body:

```markdown
## Problem
What needs to change and why.

## Current state
Evidence from the repository, including affected paths and symbols.

## Implementation plan
Ordered steps with the expected files or services.

## Acceptance criteria
- [ ] Observable completion condition
- [ ] Required validation

## Notes
Decisions, trade-offs, dependencies, and blockers.
```

Add the issue to a GitHub Project only when the user or repository instructions identify the project. Preserve the normal repository issue otherwise.

5. Verify the created issue with `gh issue view <url> --json url,title,body,state`. Completion requires an open issue whose title, plan, evidence, and acceptance criteria match the deferred work.

Return the verified issue URL and one sentence describing what was preserved.
