# Skills Index

Detailed behavior and output reference for the skills cataloged in
[`README.md`](./README.md).

## Coding

### `codex-computer-use`

[Source](./skills/coding/codex-computer-use/SKILL.md) · Model-invoked

Delegates a narrow verification question to Codex CLI when the answer requires interaction with a running app, browser, simulator, or device. It establishes an observable pass condition, chooses a safe execution boundary, and requires screenshots, runtime state, logs, or reproduction steps as evidence.

Use it for visual regression checks, browser flows, simulator rendering, app launching, screenshot capture, and independent confirmation that an implementation works outside the code diff.

### `error-modals`

[Source](./skills/coding/error-modals/SKILL.md) · Model-invoked

Designs error experiences that explain what happened, state verified impact, and provide a useful recovery path. It first chooses the appropriate surface—inline message, page error, toast, banner, or modal—then guides copy, tone, actions, accessibility, and implementation.

Use it for validation, network, permission, payment, destructive-action, and data-loss failures, or when auditing existing error copy and recovery behavior.

### `explain-diff`

[Source](./skills/coding/explain-diff/SKILL.md) · Model-invoked

Creates a dated, self-contained HTML walkthrough of a pull request, branch, commit range, working-tree diff, or patch. The explainer teaches the existing system, the intuition behind the change, the important code, and five interactive quiz questions.

It verifies the rendered artifact, links repository paths to the exact GitHub revision, keeps work-in-progress explainers local, and applies the configured Cloudflare publication lifecycle after completion.

### `save-for-later`

[Source](./skills/coding/save-for-later/SKILL.md) · Model-invoked

Turns a deferred feature, plan, or technical task into a durable GitHub issue. The issue records the problem, current repository evidence, affected files and symbols, implementation sequence, trade-offs, blockers, acceptance criteria, and validation commands.

Use it when work should be parked without losing the context another agent will need to resume it later.

### `setup-lanes`

[Source](./skills/coding/setup-lanes/SKILL.md) · User-invoked as `$setup-lanes <feature-name> [lanes...]`

Creates one integration branch and aligned lane branches for parallel-agent feature work. It inspects local and remote refs, preserves existing branches, creates only missing lanes, verifies every new ref against the integration commit, and reports the exact PR and diff baselines.

The skill stops after safe branch setup; rebasing, merging, release checks, and destructive branch repair remain separate deliberate operations.

### `sync-personal-skills`

[Source](./skills/coding/sync-personal-skills/SKILL.md) · User-invoked as `$sync-personal-skills [--force]`

Runs `/Users/ravi/work/skills/sync-skills` to update Ravi's globally installed skills from `rduvvuritoga/skills`. It records repository status before and after the run, accepts only the optional `--force` argument, and verifies that the source checkout remains unchanged.

The result reports whether the installation was already current or includes the new revision and installed skill count.

## Writing and marketing

### `marketing-copy`

[Source](./skills/writing/marketing-copy/SKILL.md) · Model-invoked

Diagnoses and writes conversion-focused website copy using the project's positioning, audience, proof, and voice as the source of truth. It supports headlines, CTAs, value propositions, landing pages, pricing pages, feature pages, about pages, buyer-specific messaging, and AI-extractable copy.

The core workflow establishes product context, identifies the main conversion problem, writes one coherent conversion argument, protects evidence integrity, and verifies the result. Framework and page-specific guidance load only for the relevant branch.

## Learning

### `learn`

[Source](./skills/learning/learn/SKILL.md) · User-invoked as `$learn [mode] [topic]`

Coaches one topic through a selected structured mode. Available modes are:

- `80-20` — a ten-block, twenty-hour learning plan.
- `onepager` — a compact printable topic map.
- `ladder` — five observable skill levels.
- `mistakes` — five high-cost beginner traps.
- `newbie` — a beginner explanation with understanding checks.
- `teachback` — an iterative explain-and-correct loop.
- `test` — a ten-question practical understanding test.
- `scenarios` — guided practice across five realistic situations.
- `bridge` — transfer from a familiar topic to a new one.

Static modes return their complete artifact and a way to test understanding. Interactive modes stop at the learner's next action and continue from the response in a later turn.
