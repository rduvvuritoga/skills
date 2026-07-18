---
name: codex-computer-use
description: Delegate independent local UI and runtime verification to Codex CLI. Use when a task requires real browser, simulator, device, or app interaction, screenshots, visual reproduction, app launching, or a second agent's evidence from a running system.
---

# Codex Computer Use

Use Codex CLI as a separate verification agent when the answer depends on interacting with a running system rather than reading code alone.

## Workflow

### 1. Define the verification contract

Write one observable question and its pass condition. Identify the relevant project path, startup command or URL, test data, viewport or device, and evidence needed. The contract is ready when another agent can distinguish pass, fail, and blocked without guessing.

### 2. Prepare the test state

Confirm the app, server, simulator, or browser state exists or include exact startup instructions. Prefer local, test, or disposable data. Preserve the user's existing app and browser state unless the task explicitly authorizes changing it.

### 3. Choose the narrowest execution boundary

Use `read-only` for inspection that produces no local artifact. Use `workspace-write` when Codex must save screenshots, logs, or other evidence in the project. Use the configured default model unless the user or repository requires a specific one.

```bash
codex exec --cd "$PWD" --sandbox read-only -
```

The locally supported command contract is `codex exec --cd <project> --sandbox <read-only|workspace-write> -`; read the prompt from stdin. Check `codex exec --help` when flags may have changed.

The bypass flag is reserved for an externally sandboxed environment and requires explicit authorization for that run.

### 4. Run a narrow verification prompt

Read [the prompt template](references/verification-prompt.md), fill every applicable field, and run Codex from the project under test. Request interaction with the real running flow, evidence, and a read-only report unless the user explicitly requested a fix.

### 5. Check the returned evidence

Accept the report only when:

- It states `pass`, `fail`, or `blocked` against the original question.
- It describes the actual interaction performed.
- It includes the evidence that determines the result: screenshot path, visible state, URL, log, console error, selector, or exact reproduction steps.
- A blocked result names the missing state, permission, or command.

Relay the result and decisive evidence rather than the full transcript. Treat a code-only inference as incomplete when the contract required runtime interaction.

## Safety boundary

Proceed with local apps, test accounts or data, screenshots, and local logs. Obtain explicit authorization before the delegated run would use sensitive or production data, send an external message or form, make a purchase or booking, change a real account, delete data, modify system settings, close the user's apps, or perform broad cleanup. Put the approved boundary and the positive safe behavior in the prompt.
