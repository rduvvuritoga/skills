---
name: sync-personal-skills
description: Manually sync Ravi's global personal coding-agent skills from the rduvvuritoga/skills repository. Use only when explicitly invoked with $sync-personal-skills or when the user directly asks to run the personal skills sync on demand.
user-invocable: true
disable-model-invocation: true
argument-hint: [optional --force]
allowed-tools: Bash
---

# Sync Personal Skills

Run the personal skills sync script from the source skills repository and report the result without editing, committing, or pushing the repository.

## Workflow

1. Work from `/Users/ravi/work/skills`.

2. Run:

```bash
/Users/ravi/work/skills/sync-skills
```

If `$ARGUMENTS` is exactly `--force`, run:

```bash
/Users/ravi/work/skills/sync-skills --force
```

Do not pass other arguments unless the user explicitly provides them.

3. Treat the repository as read-only:
   - Do not edit files.
   - Do not commit.
   - Do not push.
   - Do not publish skills.

4. Report the outcome concisely:
   - If already current, say exactly that and include the SHA from the script output.
   - If changes are installed, report the new SHA and installed skill count from the script output.
   - If the script fails, report the exact blocker and the failing command.

## Notes

The sync script compares `rduvvuritoga/skills` main against the last installed SHA and only reinstalls when the remote changed.
