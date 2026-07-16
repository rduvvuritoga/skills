---
name: review-pr-comments
description: End-to-end workflow for addressing PR review comments — fetch open threads, classify each, apply fixes, run type-check + tests, commit, push, and resolve the threads. Use when the user says "fix the review comments", "address the review", "fix and push", or after a PR review lands and they want one command to close it all out.
user-invocable: true
disable-model-invocation: true
argument-hint: [optional PR number — defaults to PR for current branch]
allowed-tools: Bash Read Edit Write Grep Glob
---

End-to-end review-comment workflow: fetch → classify → fix → test → commit → push → resolve.

## Instructions

### 1. Determine the PR

- If `$ARGUMENTS` is a number, use it.
- Otherwise auto-detect:
  ```bash
  gh pr view --json number -q .number
  ```
- Determine repo (owner/name):
  ```bash
  gh repo view --json owner,name -q '.owner.login + "/" + .name'
  ```
- If the PR is closed/merged, stop and tell the user.

### 2. Fetch open review threads

```bash
gh api graphql -f query='
query($owner: String!, $repo: String!, $pr: Int!) {
  repository(owner: $owner, name: $repo) {
    pullRequest(number: $pr) {
      reviewThreads(first: 100) {
        nodes {
          id isResolved path line
          comments(first: 5) {
            nodes { author { login } body }
          }
        }
      }
    }
  }
}' -f owner=OWNER -f repo=REPO -F pr=PR_NUMBER
```

Filter to `isResolved == false`. If zero open threads, tell the user "No open review threads on PR #N." and stop.

### 3. Classify each thread

For each open thread, decide one of:

- **fix** — the concern is concrete, you understand it, and you can apply a focused fix. Examples: missing null check, unused import, `let foo;` → explicit type, missing `try/finally`.
- **ask** — the comment needs human judgment (architectural disagreement, scope question, design preference, ambiguous "consider X").
- **false-positive** — the reviewer is wrong (lazy-eval pattern flagged as TDZ, deduplicate of an already-fixed item, misreads the code).
- **already-fixed** — earlier in this conversation or a recent commit on the branch already addresses it. Common when a reviewer re-runs after a previous round of fixes.

Print a one-line summary per thread:
```
[FIX]   path:line — short description
[ASK]   path:line — short description
[FP]    path:line — short description (will reply + resolve)
[DONE]  path:line — already addressed in <commit-sha or "earlier in this turn">
```

### 4. Confirm scope (only if needed)

Skip if every thread is FIX/FP/DONE — proceed straight to fixes.

If any are **ask**, present them via AskUserQuestion-style prose with options, and stop until the user answers. Do not silently skip them.

If the user previously said "fix all of them" or "go ahead", trust that and don't ask again.

### 5. Apply fixes

For each FIX:
1. Read the relevant file.
2. Make the smallest change that addresses the concern.
3. Move to the next.

After all fixes are applied:
- Run **type-check** — find the right command from the project. Common patterns: `pnpm type-check`, `pnpm tsc --noEmit`, `bun run type-check`, `npm run typecheck`. Look for project's `package.json` scripts or a `Makefile`.
- Run **affected tests** — focus on tests near the changed files. If you added new tests, run them too. Don't rerun the entire suite unless the project is small.
- If type-check or tests fail: STOP. Show the failures. Do not commit. Do not push.

### 6. Commit

One commit for the whole round of fixes. Title should match prior commits on the branch in style (look at `git log --oneline origin/main..HEAD` for the convention).

Standard pattern:
```
fix: address <reviewer> review on PR #N

- short bullet for each fix, naming the file/symbol
- another bullet
- (if false positives addressed) note them: "schema-ordering comments
  are false positives — relations() callback evaluated lazily"

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
```

Use a HEREDOC for the commit message:
```bash
git commit -m "$(cat <<'EOF'
<message>
EOF
)"
```

### 7. Push

```bash
git push
```

If the push fails (rejected, force-required, etc.), STOP and report. Don't retry with `--force` unless the user explicitly approves.

### 8. Resolve the threads

For each thread that was FIX, FP, or DONE:

**If false-positive**, reply first with a brief explanation:
```bash
gh api graphql -f query='
mutation($threadId: ID!, $body: String!) {
  addPullRequestReviewThreadReply(input: { pullRequestReviewThreadId: $threadId, body: $body }) {
    comment { id }
  }
}' -f threadId=THREAD_ID -f body="Brief one-paragraph explanation."
```

Then resolve. If you have many threads to resolve, use a Python loop — bash word-splitting is unreliable in zsh:
```bash
echo "$THREAD_IDS" | python3 -c "
import sys, subprocess
for tid in sys.stdin.read().split():
    subprocess.run(['gh', 'api', 'graphql',
        '-f', 'query=mutation(\$t: ID!) { resolveReviewThread(input: { threadId: \$t }) { thread { isResolved } } }',
        '-f', f't={tid}'])
"
```

### 9. Report

Print a tight summary the user can scan in 5 seconds:
```
Addressed N comments on PR #X (commit <sha>):
  ✓ M fixes applied (file:line list, one per line)
  ✓ K false positives explained + resolved
  → A asked, awaiting your input  (only if applicable)
Type-check: clean. Tests: <count> passed.
PR: <url>
```

## Stop conditions

- Type-check fails after fixes → don't commit, show errors.
- Tests fail after fixes → don't commit, show failures.
- Push rejected → don't force; show why.
- A reviewer's comment is genuinely ambiguous → ask the user.
- The PR diff would grow large enough that a single round of fixes feels risky (rough rule: >300 lines changed) → confirm with the user before committing.

## Notes

- Skip threads where `isResolved == true` — already done.
- Skip threads where the latest comment is from the user themselves (no action needed).
- Don't make drive-by changes outside the scope of the review comments — even if you spot something else.
- If the project uses a heavy review pipeline (large test suites, linting that takes minutes), prefer to scope tests narrowly. The user can run the full suite separately.
- The `/resolve-pr-comments` skill is a strict subset of step 8 — if the user only wants to mark things resolved without applying fixes, point them there.
