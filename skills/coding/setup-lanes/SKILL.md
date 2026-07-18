---
name: setup-lanes
description: Create an integration branch and aligned lane branches for parallel-agent feature work.
user-invocable: true
disable-model-invocation: true
argument-hint: <feature-name> [lane1 lane2 ...]
allowed-tools: Bash Read Write AskUserQuestion
---

# Setup Lanes

Create a shared integration baseline and one branch per parallel work lane:

```text
main
└── <feature>
    ├── <feature>-<lane-1>
    ├── <feature>-<lane-2>
    └── <feature>-<lane-3>
```

Completion means every newly created lane points to the verified integration commit, existing branches remain untouched, and the user receives the exact PR and diff baselines.

## Workflow

### 1. Resolve names

Read `<feature-name>` and optional lane names from `$ARGUMENTS`. Use lowercase kebab-case. When lanes are omitted, use `lane-a`, `lane-b`, and `lane-c`.

Construct `<feature>-<lane>` for each lane and show the proposed names before any remote write when the input was inferred or normalized.

### 2. Inspect repository state

Run:

```bash
git status --short --branch
git remote get-url origin
git fetch origin --prune
git rev-parse --verify origin/main
git for-each-ref --format='%(refname:short) %(objectname)' refs/heads refs/remotes/origin
```

Proceed once `origin`, `origin/main`, and the local working state are known. Preserve uncommitted work; branch creation must not switch branches or alter tracked files.

### 3. Establish the integration branch

Inspect local and remote `<feature>` refs separately.

- When `origin/<feature>` exists, use its commit as the baseline and leave it unchanged.
- When neither ref exists, create `<feature>` from `origin/main` and push it with upstream tracking.
- When only a local `<feature>` exists, report its commit and ask before publishing or replacing it.

```bash
git branch <feature> origin/main
git push -u origin <feature>
```

Record the full baseline commit with `git rev-parse origin/<feature>` before creating lanes.

### 4. Create only missing lanes

For every proposed lane, inspect both `refs/heads/<feature>-<lane>` and `refs/remotes/origin/<feature>-<lane>`.

- Leave an existing lane untouched and record whether it matches the baseline.
- Create a lane only when neither local nor remote ref exists.

```bash
git branch <feature>-<lane> origin/<feature>
git push -u origin <feature>-<lane>
```

Never reset, force-update, or overwrite an existing branch as part of setup.

### 5. Verify exact alignment

Fetch again, then compare each newly created local and remote lane with the recorded integration commit:

```bash
git fetch origin --prune
git rev-parse origin/<feature>
git rev-parse <feature>-<lane>
git rev-parse origin/<feature>-<lane>
```

Each newly created ref must equal the recorded baseline. Report existing lanes that differ; they are outside automatic repair.

### 6. Return the workspace plan

Report:

- The branch tree with exact names and baseline SHA.
- Which branches were created and which already existed.
- Any existing lane that differs from the integration baseline.
- One workspace per lane.
- Lane PR base: `<feature>`.
- Lane diff baseline: `git diff origin/<feature>...`.
- Final integration PR base: `main`.

Stop after setup and verification. Merging, rebasing, and running the repository's release checks belong to the later integration workflow.
