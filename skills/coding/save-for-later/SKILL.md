---
name: save-for-later
description: Save a planned feature or task as a GitHub issue for later implementation. Use when the user says "save this for later", "park this", "we'll do this later", or similar.
user-invocable: true
disable-model-invocation: true
argument-hint: [optional title or description]
allowed-tools: Bash Read Grep Glob
---

Create a GitHub issue to capture a planned feature or task for later implementation.

## Instructions

1. If `$ARGUMENTS` is provided, use it as context for the issue. Otherwise, use the conversation context to determine what feature/task to save.

2. Gather all relevant details from the conversation:
   - What was discussed and planned
   - Implementation approach and steps
   - Affected files and services
   - Any trade-offs or considerations noted

3. Create a well-structured GitHub issue using `gh issue create`:
   - **Title**: Clear, concise summary (under 70 chars)
   - **Label**: `enhancement` (unless the user specifies a different label)
   - **Body**: Use this structure:

```
## Summary
Brief description of the feature/task.

## Plan
Detailed implementation steps with affected files.

## Notes
- Considerations, trade-offs, blockers
- Any relevant context from the discussion

🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

4. Add the issue to the **Front IQ MVP** project (project number 4, owner togasoftware) unless the user specifies otherwise:
   ```
   gh project item-add 4 --owner togasoftware --url <issue-url>
   ```

5. Return the issue URL to the user.
