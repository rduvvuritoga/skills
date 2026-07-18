---
name: explain-diff
description: Create and manage a rich, self-contained HTML explanation of a code change, diff, commit range, branch, pull request, or reviewable patch. Use when the user asks to explain what changed, teach a PR, summarize a diff for onboarding, create an interactive walkthrough, produce a durable HTML artifact, or generate the post-commit explainer required by the implementation workflow.
---

# Explain Diff

## Overview

Create a dated HTML explainer that teaches a code change from first principles, then walks through the actual diff. The output should be useful to someone who wants to understand the system, not just skim the patch.

## Workflow

1. Identify the change to explain: PR, branch, commit range, staged diff, working tree diff, or files named by the user.
2. Inspect the diff and surrounding code. Read enough adjacent modules, tests, docs, and call sites to explain the existing system honestly.
3. For a pull request, resolve its head repository URL and head commit SHA from PR metadata. Use that exact head revision for source links; do not assume the base repository or a same-named local branch, especially for forked PRs.
4. If a PR head file is missing or stale in the local checkout, read the PR diff or head-file content through the available GitHub/API/fetch path and note that limitation in the output.
5. Build the explanation around four sections: `Background`, `Intuition`, `Code`, and `Quiz`.
6. Write a single self-contained `.html` file under the repository root at `.scratch/explain-diff/YYYY-MM-DD-<slug>/index.html`. Resolve the repository exclude file with `git rev-parse --git-path info/exclude` and add `.scratch/` when needed so the local artifact never enters the change being explained.
7. Verify the HTML source before returning: no external runtime dependencies, every eligible repository file path is linked to source, code blocks preserve whitespace, and quiz interactions work with embedded JavaScript.
8. Apply the lifecycle in [Cloudflare publication](references/cloudflare-publication.md).
9. Return the local path or published URL and a concise note on what change was explained.

## Content Structure

### Background

Explain the existing system relevant to the change.

- Start broad enough for a beginner to orient themselves.
- Then narrow to the specific modules, data structures, APIs, or workflows touched by the change.
- State which parts can be skipped by readers already familiar with the system.
- Ground claims in file paths, function names, tests, schemas, UI flows, or runtime behavior found locally.

### Intuition

Explain the essence of the change before the details.

- Use concrete examples, toy data, and before/after states.
- Prefer a few reusable diagram families over many one-off visuals.
- For UI changes, include simplified HTML mockups of the relevant UI state.
- For backend, data, or infrastructure changes, include system/data-flow diagrams with example payloads.

### Code

Walk through the changed code at a high level.

- Group changes by concept or workflow, not by raw file order when another order teaches better.
- Cite file paths and important functions/classes/components inline.
- Explain why each group exists, what behavior changes, and what risks or edge cases it addresses.
- Include short code excerpts only when they clarify the explanation. Do not paste large diffs.

### GitHub source links

Make every displayed repository file path clickable when explaining a pull request. This includes paths in prose, tables, diagrams, callouts, and code-block headers, not only paths in the `Code` section.

- Link files that exist at the PR head to `HEAD_REPO_URL/blob/HEAD_SHA/PATH`, with `#Lstart` or `#Lstart-Lend` when the relevant line range is known.
- Use the PR head repository, not automatically the base repository, so links work for forked pull requests.
- Prefer the head commit SHA over the branch name. It shows the exact code under review and remains valid if the branch is later deleted.
- Render the path itself as the anchor text. Open source links in a new tab and add `rel="noopener noreferrer"`.
- URL-encode path segments and HTML-escape both the label and attribute value.
- Do not invent a head-blob link for a deleted file. Link it to the corresponding PR file diff when practical; otherwise render it as text and label it deleted.
- Do not link paths that cannot be verified as repository files at the selected revision.

### Quiz

Always end with five interactive multiple-choice questions. The quiz is enabled by default for every explainer; omit it only when the user explicitly asks to do so in the current request. Do not carry forward an older preference to disable it.

- Questions should test real understanding of the change, not trivia or gotchas.
- Use 3-4 answer options per question.
- Provide immediate feedback after a click: correct/incorrect plus a short explanation.
- Vary answer lengths and positions so the correct answer is not visually obvious.

## HTML Requirements

- Output exactly one self-contained HTML file with embedded CSS and JavaScript.
- Use one long page with section headers and a table of contents. Do not use tabs for the top-level structure.
- Make it responsive enough to read on a phone.
- Use semantic HTML where practical: `section`, `nav`, `figure`, `figcaption`, `aside`, `details`, `button`.
- Do not use ASCII diagrams. Build diagrams with HTML and CSS.
- Use callouts for key concepts, definitions, assumptions, and edge cases.
- Style source links so they are visibly clickable and remain legible in code labels, tables, diagrams, and prose.
- Use `<pre>` for code blocks. If a custom code container is used, its CSS must include `white-space: pre` or `white-space: pre-wrap`.
- Before saving, scan every code block style in the HTML source and confirm whitespace is preserved.
- Before saving a PR explainer, scan the rendered file-path labels and confirm every eligible repository file path is wrapped in a valid GitHub source anchor at the PR head SHA.
- Do not depend on CDN CSS/JS, web fonts, network images, or repo-local assets unless the user explicitly asks.

## Writing Standard

Write in clear, narrative technical prose. Keep transitions smooth, define unfamiliar terms, and make the reader feel the change is inevitable once they understand the old system and the new requirement.

Avoid shallow PR-summary language such as "this updates X" without explaining why X matters. Prefer explanations that move from system behavior to design pressure to code mechanics.

## Artifact Lifecycle

Keep the artifact local while its pull request is open. Never commit the artifact or include it in the diff it explains.

- Keep open pull requests and unfinished diffs local.
- Schedule a durable follow-up for an open pull request; publish only after it is merged.
- Do not publish a closed-unmerged pull request unless the user explicitly identifies it as the finished result.
- Publish the final commit of a completed no-PR implementation immediately.

Read [Cloudflare publication](references/cloudflare-publication.md) for state detection, deferred automation, deployment naming, security checks, and cleanup.

## Final Response

Return the `.scratch` path while work is open, or the verified Cloudflare URL after publication. Mention the source change explained, its pull-request state when applicable, and any limitations such as unavailable scheduling, missing Cloudflare configuration, unreadable files, or tests that were not run.
