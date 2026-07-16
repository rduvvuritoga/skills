---
name: resolve-pr-comments
description: Mark PR review threads as resolved on GitHub after addressing the comments. Use when the user says "mark resolved", "resolve the comments", "resolve review threads", or "close out the review", typically after pushing fixes for review feedback.
user-invocable: true
disable-model-invocation: true
argument-hint: [optional PR number — defaults to PR for current branch]
allowed-tools: Bash Read
---

Resolve open PR review threads via the GitHub GraphQL API. Reply on threads you're closing as "false positive" with a brief explanation before resolving.

## Instructions

1. **Determine the PR number.**
   - If `$ARGUMENTS` is a number, use it.
   - Otherwise, auto-detect from the current branch:
     ```bash
     gh pr view --json number -q .number
     ```
   - If neither works, ask the user.

2. **Determine the repo (owner/name)** for GraphQL calls:
   ```bash
   gh repo view --json owner,name -q '.owner.login + "/" + .name'
   ```

3. **List all open review threads.** Capture both the thread id and the first comment (path, line, author, body) so you can match them against what was fixed.

   ```bash
   gh api graphql -f query='
   query($owner: String!, $repo: String!, $pr: Int!) {
     repository(owner: $owner, name: $repo) {
       pullRequest(number: $pr) {
         reviewThreads(first: 100) {
           nodes {
             id isResolved path line
             comments(first: 1) {
               nodes { author { login } body }
             }
           }
         }
       }
     }
   }' -f owner=OWNER -f repo=REPO -F pr=PR_NUMBER
   ```

   Filter to `isResolved == false`. If there are zero open threads, tell the user and stop.

4. **Decide which threads to resolve.** For each open thread:
   - If the comment's concern is clearly addressed by the latest commits on the branch (you can see the fix in the diff or in conversation context), mark it for resolution.
   - If you're not sure, ask the user.
   - If the comment is a false positive (the reviewer is wrong — e.g. flagged behavior is intentional or the analysis is incorrect), mark it for resolution **with a reply** explaining why before resolving.
   - If the comment hasn't been addressed yet, leave it open.

5. **Reply on false-positive threads** before resolving:
   ```bash
   gh api graphql -f query='
   mutation($threadId: ID!, $body: String!) {
     addPullRequestReviewThreadReply(input: { pullRequestReviewThreadId: $threadId, body: $body }) {
       comment { id }
     }
   }' -f threadId=THREAD_ID -f body="Brief explanation of why this is a false positive."
   ```

6. **Resolve the threads.** Iterate over your selected thread ids and call:
   ```bash
   gh api graphql -f query='
   mutation($tid: ID!) {
     resolveReviewThread(input: { threadId: $tid }) {
       thread { isResolved }
     }
   }' -f tid=THREAD_ID
   ```

   Bash word-splitting can be flaky in zsh — if you're resolving more than 3-4 at once, use a Python helper to iterate cleanly:
   ```bash
   echo "$THREAD_IDS" | python3 -c "
   import sys, subprocess
   for tid in sys.stdin.read().split():
       subprocess.run(['gh', 'api', 'graphql',
           '-f', 'query=mutation(\$t: ID!) { resolveReviewThread(input: { threadId: \$t }) { thread { isResolved } } }',
           '-f', f't={tid}'])
   "
   ```

7. **Verify.** Re-query and confirm `Unresolved: 0` (or whatever was intentionally skipped). Report counts back to the user:
   - `N resolved, M skipped (still open), K replied as false positive`.

## Notes

- The GitHub web UI shows resolved threads collapsed under "Resolved conversations" — the user will see the change immediately on refresh.
- Resolving a thread doesn't dismiss the review itself or affect approval state. It just marks the conversation closed.
- For Copilot review threads, resolving them is purely informational — Copilot doesn't act on it.
- This skill never resolves a thread that's not yet addressed. If you can't trace the fix, ask the user before marking it.
