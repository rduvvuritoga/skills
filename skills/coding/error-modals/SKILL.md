---
name: error-modals
description: Design and implement actionable error experiences for web and mobile. Use when writing or auditing error copy, choosing a modal versus inline, toast, or page treatment, defining recovery actions, or building UI for validation, network, permission, payment, destructive, or data-loss failures.
---

# Error Experiences

Turn a failure into a clear explanation, an honest statement of impact, and the safest useful next action.

## Workflow

### 1. Choose the right surface

Match the interruption to the failure:

- **Inline validation** — a specific field can be corrected in place.
- **Inline or page error** — content failed to load or a persistent problem belongs near the affected area.
- **Toast or banner** — a non-blocking action failed and the user can continue elsewhere.
- **Modal or dialog** — the user must acknowledge impact, choose a recovery path, or confirm a consequential action before continuing.

Use a modal only when blocking the current flow is warranted. Treat an ordinary empty state as its own product state unless a failure caused the missing content.

### 2. Establish the facts

Determine:

- What action failed or consequence is about to occur.
- Whether the cause is known and safe to explain.
- What data, money, access, or progress was affected.
- What was definitely preserved or left unchanged.
- Which recovery actions actually work.
- How the user can get help when self-service recovery is unavailable.
- The severity: low, medium, high, or critical.

Use only verified facts. Omit an unknown cause or preservation claim instead of inventing reassurance.

### 3. Write the message

Build the message in this order:

1. **Title** — name the failed action or concrete consequence in roughly 5–8 words.
2. **Body** — explain the known cause, impact, and preserved state in 1–3 short sentences.
3. **Primary action** — offer the safest direct recovery or confirmation action.
4. **Secondary action** — provide a safe escape when one exists.
5. **Support path** — include it when the user cannot recover independently.

Calibrate tone to stakes: neutral for correctable input, calm for retries, serious for payment or data risk, and grave for irreversible loss. Acknowledge impact without blaming the user or exposing irrelevant vendor and implementation details.

For common copy structures, severity guidance, and before/after examples, read [error patterns](references/error-patterns.md).

### 4. Implement the selected behavior

Follow the project's existing design system, accessibility conventions, and error-state primitives. Preserve focus, keyboard navigation, screen-reader naming, loading/disabled states, and retry idempotency.

When the project uses React with shadcn/ui and needs a dialog implementation, read [the shadcn dialog reference](references/shadcn-dialog.md). Adapt it to the verified actions and existing component API rather than copying it blindly.

### 5. Verify the complete experience

Confirm every applicable condition:

- The chosen surface matches how blocking and severe the failure is.
- The title names the action or consequence rather than a generic error.
- The body distinguishes verified cause, impact, and preserved state.
- Every displayed action works and its label describes the result.
- A user who cannot self-recover has a support path.
- Tone matches severity and avoids blame, jargon, and false reassurance.
- Destructive actions state scope and irreversibility before confirmation.
- Implemented UI meets the project's accessibility and interaction conventions.

Revise until each applicable condition passes. Report any missing product fact or recovery behavior that prevents an honest final message.
