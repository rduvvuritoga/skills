# Cloudflare publication

## Resolve completion state

Determine whether the explained branch or commit belongs to a pull request.

- For an open pull request, keep the artifact local and register the deferred follow-up below.
- For a merged pull request, publish immediately.
- For a closed-unmerged pull request, stop its follow-up and keep the local artifact unless the user explicitly says that closed branch is the finished result.
- For the final commit produced by a completed implementation with no pull request, publish immediately.
- For an uncommitted diff, staged diff, or intermediate commit, keep the artifact local.

## Register an open-PR follow-up

Use the agent host's recurring automation or task-wakeup facility. Check the exact repository and pull-request number hourly without keeping the current agent turn alive.

On each run:

1. Read the pull request's current state.
2. If it remains open, make no change.
3. If it is merged, deploy the saved artifact, report the URL in the originating task, and remove or archive the monitor.
4. If it is closed without merge, keep the artifact, report that publication was withheld, and remove or archive the monitor.

If the host has no durable scheduling facility, report that automatic publication is unavailable and leave the artifact in `.scratch`. Do not claim a future deploy was scheduled.

## Resolve Cloudflare configuration

Use `EXPLAIN_DIFF_CLOUDFLARE_PROJECT`, then the repository-local Git config key `explainDiff.cloudflareProject`. If neither exists, ask once for the Pages project and save it with:

```bash
git config --local explainDiff.cloudflareProject <project>
```

Require `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`. Never print their values.

## Deploy and verify

Use a deterministic Pages branch:

- pull request: `<owner>-<repo>-pr-<number>`
- no-PR commit: `<owner>-<repo>-<short-commit-sha>`

Normalize names to lowercase letters, digits, and hyphens. Deploy the directory that contains `index.html`:

```bash
npx wrangler@latest pages deploy <artifact-directory> \
  --project-name=<project> \
  --branch=<pages-branch>
```

Capture the deployment URL and make an unauthenticated request to it. For a private repository, require Cloudflare Access to challenge or deny the request; fail closed if it is publicly readable. A public repository may publish publicly.

Only after a successful deploy and verification, remove the local artifact directory. Preserve it on any failure so publication can be retried. Return the verified URL with the pull-request number and merged state when applicable.
