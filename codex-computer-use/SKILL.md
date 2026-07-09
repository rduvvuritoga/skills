---
name: codex-computer-use
description: Ask Codex CLI, usually with gpt-5.5, to run local app verification that needs computer use, browser automation, simulators, screenshots, app launching, or independent runtime inspection. Use when Claude should have Codex test a flow, verify UI behavior, inspect a running app, capture screenshots, exercise a browser or simulator, reproduce visual bugs, or report independent confirmation and feedback about implemented behavior.
---

# Codex Computer Use

Use Codex as a separate local verification agent when the task needs real UI interaction, screenshots, simulator/browser/device state, app launching, or an independent runtime check outside Claude's current context.

Do not use this for ordinary code reading, typechecking, linting, or tests Claude can run directly. Launching apps, browsers, or simulators to verify the requested work is fine without asking; ask first only if the run could disrupt the user's environment beyond that, such as closing their apps, changing system settings, sending messages, making purchases, or acting on real accounts or data.

## Workflow

1. Identify the exact verification question.
2. Make sure the app or server state Codex needs is available, or include startup instructions in the prompt.
3. Run Codex CLI from the relevant project directory with a narrow prompt.
4. Ask Codex to use browser/computer-use tools as needed, collect evidence, and avoid unrelated code edits.
5. Read Codex's final report and relay the result to the user with any important screenshots, findings, or reproduction steps.

Prefer a read-only verification prompt unless the user explicitly asked Codex to fix something. If Codex finds a bug, have it report the issue and the evidence rather than patching by default.

## Command Pattern

Use `codex exec` non-interactively. Pass the prompt on stdin for anything longer than one sentence.

```bash
codex exec --model gpt-5.5 --cd "$PWD" --sandbox workspace-write --ask-for-approval on-request -
```

If `gpt-5.5` is unavailable, use the strongest available Codex model and say that you did so. Use `--cd <project-path>` for the app under test. Use `--sandbox read-only` for pure inspection. Use `workspace-write` only when Codex may need to create temporary screenshots, logs, or small artifacts in the repo.

Do not use `--dangerously-bypass-approvals-and-sandbox` unless the user explicitly approved that exact risk for this run.

## Prompt Template

Use this structure:

```text
You are a local verification agent. Use computer-use, browser automation, app launching, screenshots, and runtime inspection as needed.

Goal:
[one sentence describing what to verify]

Context:
- Project path: [path]
- App/server command or current URL: [command or URL]
- Relevant user request: [brief request]
- Areas to avoid changing: [files/accounts/settings, if any]

Instructions:
- Verify behavior by interacting with the real running app, not just reading code.
- Capture screenshots or describe visual state when useful.
- Do not make code changes unless explicitly asked.
- Do not act on real accounts, send messages, make purchases, delete data, or change system settings.
- If something cannot be verified, say exactly what blocked verification.

Report:
- Result: pass/fail/blocked
- Evidence: screenshots, observed UI state, logs, URLs, or exact reproduction steps
- Findings: bugs, mismatches, or risks
- Recommended next action
```

## Good Uses

- "Open the app and verify the signup flow works on mobile."
- "Use the browser to reproduce this modal overlap and screenshot it."
- "Launch the iOS simulator and confirm the onboarding screen renders."
- "Check whether the fixed dashboard chart is actually visible at `/reports`."
- "Act as a second agent and verify my UI change without relying on my description."

## Safety Boundaries

Ask the user before delegating if verification would require:

- Using real customer, financial, medical, legal, or production account data.
- Sending external messages, emails, Slack posts, texts, or form submissions.
- Making purchases, bookings, payments, trades, or irreversible account changes.
- Closing or modifying the user's open apps, browser profiles, system settings, or files outside the project.
- Running destructive commands or broad cleanup.

If verification only opens a local app/browser/simulator, captures screenshots, clicks through local or test data, or reads local logs, proceed.

## Output Back To User

Summarize Codex's result, not the entire transcript. Include the evidence that changes the answer: screenshot paths, exact visual observations, reproduction steps, failing selectors, console errors, or logs. If Codex was blocked, state the concrete blocker and the next command or permission needed.
