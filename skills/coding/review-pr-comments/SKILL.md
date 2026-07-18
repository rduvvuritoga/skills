---
name: review-pr-comments
description: Address and close out GitHub pull-request review feedback. Use when the user asks to fix or address review comments, resolve review threads, or finish a review round with validated changes and verified GitHub resolution.
allowed-tools: Bash Read Edit Write Grep Glob
---

Address one round of pull-request feedback end to end: fetch → classify → fix when needed → validate → push → resolve → verify.

Completion means every thread that was open at the start is accounted for. In full-remediation mode, actionable fixes are validated and pushed before resolution. In resolution-only mode, addressed threads are confirmed resolved while unaddressed concerns stay open and are reported.

## Workflow

### 1. Identify an open pull request

Use `$ARGUMENTS` when it is a PR number; otherwise detect the PR for the current branch:

```bash
gh pr view --json number,state -q '.number, .state'
gh repo view --json owner,name -q '.owner.login + "/" + .name'
```

Proceed once the owner, repository, PR number, and `OPEN` state are known. Report and stop when the PR cannot be found or is closed or merged.

### 2. Fetch every open review thread

```bash
gh api graphql -f query='
query($owner: String!, $repo: String!, $pr: Int!, $after: String) {
  repository(owner: $owner, name: $repo) {
    pullRequest(number: $pr) {
      reviewThreads(first: 100, after: $after) {
        nodes {
          id isResolved path line
          comments(first: 100) {
            nodes { author { login } body url }
          }
        }
        pageInfo { hasNextPage endCursor }
      }
    }
  }
}' -f owner=OWNER -f repo=REPO -F pr=PR_NUMBER
```

Run the first request without `after`. While `hasNextPage` is true, repeat it with `-f after=END_CURSOR` and append the returned nodes. Filter the complete result to `isResolved == false`. If none remain, report `No open review threads on PR #N.` and stop.

Create a ledger containing every open thread's id, path, line, concern, classification, and evidence. The ledger is the completion checklist for the run.

### 3. Classify every thread

Assign exactly one classification:

- **FIX** — a concrete concern that can be addressed with a focused change.
- **ASK** — a decision that needs user judgment because scope, architecture, or intent is ambiguous.
- **FALSE POSITIVE** — the concern does not apply; record the code or behavior that disproves it.
- **ALREADY FIXED** — the branch already addresses it; record the commit or exact code evidence.

Show one line per ledger entry:

```text
[FIX]  path:line — concern
[ASK]  path:line — decision needed
[FP]   path:line — reason it does not apply
[DONE] path:line — addressed by <commit or evidence>
```

Every open thread must appear once. When any entry is `ASK`, present the decision with its options and wait for the user's answer before changing code.

Choose the branch from the user's request:

- **Full remediation** — continue to step 4 when the user wants review feedback addressed. Resolve `FIX` entries only after their changes are validated and pushed.
- **Resolution only** — jump to step 6 when the user only wants addressed threads marked resolved. Resolve `FALSE POSITIVE` and `ALREADY FIXED` entries with recorded evidence; leave `FIX` and `ASK` entries open and report them.

### 4. Apply and validate focused fixes

For each `FIX`, read the surrounding code and repository instructions, then make the smallest change that addresses the recorded concern. Keep unrelated changes out of the patch.

After all fixes:

1. Inspect the complete diff and account for every changed line against a ledger entry.
2. Run the repository's required checks plus the affected tests for the changed behavior.
3. Record the commands and results in the ledger.

Proceed only when the relevant validation passes. On failure, report the command and output and leave the changes uncommitted and unpushed.

### 5. Commit and push the validated round

Match the repository's commit convention after inspecting recent commits. Use one focused commit for the review round unless repository instructions require a different structure.

Before committing, confirm that only intended files are staged. Push normally. If the push is rejected or would require a force push, report the rejection and wait for explicit authorization.

Completion for this step is a pushed commit SHA containing every `FIX` and no unrelated changes.

### 6. Reply, resolve, and verify

Reply to each `FALSE POSITIVE` before resolving it:

```bash
gh api graphql -f query='
mutation($threadId: ID!, $body: String!) {
  addPullRequestReviewThreadReply(input: {
    pullRequestReviewThreadId: $threadId,
    body: $body
  }) { comment { id url } }
}' -f threadId=THREAD_ID -f body="Brief evidence-backed explanation."
```

Resolve each eligible entry: `FIX`, `FALSE POSITIVE`, and `ALREADY FIXED` after full remediation; `FALSE POSITIVE` and `ALREADY FIXED` in resolution-only mode.

```bash
gh api graphql -f query='
mutation($threadId: ID!) {
  resolveReviewThread(input: { threadId: $threadId }) {
    thread { id isResolved }
  }
}' -f threadId=THREAD_ID
```

Repeat the query from step 2 after all mutations. Compare the result with the ledger and confirm:

- Every intended thread now has `isResolved == true`.
- Every `ASK` or intentionally skipped thread remains open.
- No thread disappeared from the accounting.

Treat any mismatch or failed mutation as incomplete work: report the affected thread ids instead of claiming success.

### 7. Report the verified outcome

Return:

```text
Addressed N review threads on PR #X<optional: in commit-sha>:
- M fixed, validated, pushed, and resolved
- K false positives explained and resolved
- D already-fixed threads verified and resolved
- A questions left open: <path:line and reason>
Validation: <commands and results>
PR: <url>
```

Include only categories that occurred. In resolution-only mode, omit commit and validation fields when no code changed. The resolved counts must come from the verification query, not from attempted mutations.
