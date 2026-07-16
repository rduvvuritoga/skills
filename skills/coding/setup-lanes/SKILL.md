---
name: setup-lanes
description: Set up stacked lane branches off a feature integration branch so multiple agents can work the same feature in parallel from separate Conductor workspaces. Use when asked to "set up lanes", "create lane branches", "stack branches for parallel agents", or splitting a feature across multiple workspaces.
user-invocable: true
disable-model-invocation: true
argument-hint: <feature-name> [lane1 lane2 ...]
allowed-tools: Bash Read Write AskUserQuestion
---

Set up the stacked-branch structure for parallel-agent work on a feature.

Target structure:

```
main
└── <feature>                ← integration branch (single PR target → main)
    ├── <feature>-lane-a     ← agent 1 workspace
    ├── <feature>-lane-b     ← agent 2 workspace
    └── <feature>-lane-c     ← agent 3 workspace
```

## Why this pattern

- Each agent works in its own Conductor workspace with a narrow scope.
- Lane branches all start from the same `<feature>` baseline, not from `main`, so they share any feature-level scaffolding (design docs, schema, shared types) that already lives on `<feature>`.
- Lanes merge into `<feature>` first; conflicts are resolved once. Only one PR (`<feature> → main`) lands the whole feature.

## Instructions

### 1. Resolve inputs

From `$ARGUMENTS` or by asking:

- `<feature-name>` — kebab-case, e.g. `preview-mode`. Used as the integration-branch name.
- Lane names — pick one shape:
  - **Generic**: `lane-a`, `lane-b`, `lane-c` — good when scope is documented in the design doc.
  - **Descriptive**: `ui`, `api`, `email` — good when the lane name *is* the scope.
  - Default to 3 generic lanes (`lane-a`, `lane-b`, `lane-c`) if the user doesn't specify.

### 2. Pre-flight

```bash
git fetch origin --prune
```

If `origin/<feature>` doesn't exist, create it from `origin/main` and push:

```bash
git branch <feature> origin/main
git push -u origin <feature>
```

If `origin/<feature>` already exists, leave it alone — that's the integration branch and may already have feature-level commits on it.

### 3. Create each lane branch from `origin/<feature>`

For each lane, branch from `origin/<feature>` (not from `HEAD`, not from `main`):

```bash
git branch <feature>-<lane> origin/<feature>
```

Skip any lane that already exists at origin (idempotent re-run):

```bash
git ls-remote --exit-code --heads origin "<feature>-<lane>" >/dev/null 2>&1 && echo "already exists, skipping"
```

Push them all with upstream tracking:

```bash
git push -u origin <feature>-<lane1> <feature>-<lane2> <feature>-<lane3>
```

### 4. Verify alignment

All lane branches must point at the same commit as `origin/<feature>`:

```bash
git for-each-ref --format='%(refname:short) %(objectname:short)' \
  refs/heads/<feature>* refs/remotes/origin/<feature>*
```

If a lane branch already existed locally and drifted, ask the user before doing `git reset --hard origin/<lane>` — that's destructive and should be confirmed.

### 5. If running inside one of the lane workspaces

If the current workspace's branch matches a lane that just got created (e.g. you're already on `<feature>-lane-a`), offer to hard-reset it to `origin/<feature>` to guarantee a clean baseline. Confirm first if the working tree has any local commits or uncommitted changes.

### 6. Output the workspace plan

Print:

- The branch tree with real names filled in.
- Suggested lane → workspace mapping (one Conductor workspace per lane).
- The PR-target reminder: each lane PR uses `gh pr create --base <feature>`, not `main`.
- Diff baseline reminder: `git diff origin/<feature>...` (not `origin/main`).

## Lane hygiene rules (give each agent these)

- **Stay in scope.** If you need to touch a file outside the lane's assigned scope, surface it — don't silently edit.
- **No drive-bys.** No unrelated refactors, renames, or repo-wide formatting.
- **Shared types are shared.** If a lane must change a type used by another lane, post the change to the user first so the other lane can rebase.
- **PR target is `<feature>`, not `main`.**
- **After a sibling lane merges into `<feature>`**, rebase the still-open lanes onto the new `<feature>` tip:
  ```bash
  git fetch origin
  git rebase origin/<feature>
  ```

## Merging back

When all lanes are merged into `<feature>`:

```bash
git checkout <feature> && git pull
pnpm install && pnpm lint && pnpm typecheck && pnpm test && pnpm build
git push origin <feature>
gh pr create --base main --head <feature>
```

## Failure modes to flag

- **Integration branch is far ahead of `main`.** Surface this — the user may want to rebase `<feature>` onto `main` first so lanes start from a fresher base.
- **A lane branch already exists with commits on it.** Don't overwrite. Tell the user; let them decide rename vs. reset vs. reuse.
- **`<feature>` is checked out in another workspace.** Branch creation from `origin/<feature>` doesn't touch the other workspace's checkout — note it so the user knows.
- **No remote `origin/main` reference.** Run `git fetch origin main` first; abort with a clear message if the remote is missing.
