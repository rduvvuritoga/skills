---
name: sync-personal-skills
description: Sync Ravi's globally installed coding-agent skills from the rduvvuritoga/skills repository.
user-invocable: true
disable-model-invocation: true
argument-hint: [optional --force]
allowed-tools: Bash
---

# Sync Personal Skills

Run the source repository's sync script while preserving the repository exactly as found.

## Workflow

1. Work from `/Users/ravi/work/skills`. Capture `git status --short` before the run.

2. Select the command:

```bash
/Users/ravi/work/skills/sync-skills
```

When `$ARGUMENTS` is exactly `--force`, use:

```bash
/Users/ravi/work/skills/sync-skills --force
```

Treat any other argument as unsupported and report it without running the script.

3. The sync script is the only permitted repository operation. Preserve the source checkout: leave files, commits, branches, remotes, and publication state unchanged.

4. Capture `git status --short` again and compare it with the initial result. Report any unexpected repository change as a failed preservation check.

5. Report the script outcome concisely:

   - **Already current** — include the SHA from the script output.
   - **Installed changes** — include the new SHA and installed skill count.
   - **Failed** — include the failing command and exact blocker.

Completion requires a successful script result and an unchanged source checkout. The script's reported SHA and count are authoritative; do not infer them from local files.
