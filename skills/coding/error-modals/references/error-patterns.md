# Error Patterns

Read this reference when drafting or auditing error copy. Replace placeholders only with verified product facts and real recovery actions.

## Severity

| Severity | Typical situation | Tone |
|---|---|---|
| Low | Correctable input or minor local issue | Neutral and direct |
| Medium | Timeout, retry, or temporary feature failure | Calm and solution-focused |
| High | Payment failure, publishing failure, or data-loss risk | Serious and reassuring where facts allow |
| Critical | Account lockout, deletion, or irreversible consequence | Grave and explicit |

## Patterns

### Network or server failure

```text
Title: Couldn't [complete action]
Body: [Known cause, if useful]. [Verified preservation or impact].
Primary: Try again
Secondary: Dismiss
Support: [Escalation path when repeated retries cannot help]
```

### Validation failure

Prefer an inline message attached to the field:

```text
[Field label]: [What is required or how to correct the value]
```

When several fields need attention, use a summary that links or moves focus to the first invalid field.

### Payment failure

```text
Title: Couldn't process your payment
Body: [Verified reason, when safe]. [Whether a charge was made].
Primary: Update payment method
Secondary: Cancel
```

### Permission failure

```text
Title: You don't have access to [resource]
Body: Access requires [verified role, permission, or plan].
Primary: [Request access / Ask an administrator / View plans]
Secondary: Go back
```

### Unsaved changes

```text
Title: Leave without saving changes?
Body: Changes to [item] will be lost if you leave.
Primary: Leave without saving
Secondary: Keep editing
```

### Destructive confirmation

```text
Title: Delete [specific item]?
Body: This permanently deletes [scope]. This action cannot be undone.
Primary: Delete [item]
Secondary: Cancel
```

## Diagnostic rewrites

| Weak | Stronger direction |
|---|---|
| `Something went wrong.` | Name the failed action and available recovery. |
| `Error 403: Unauthorized` | Name the missing access and how to obtain it. |
| `Failed to fetch` | Name the content or action that could not load or complete. |
| `Stripe isn't responding` | Describe the payment effect without exposing an irrelevant vendor. |
| `You entered an invalid email` | State the expected email format beside the field. |
| `OK` | Label the actual result: retry, return, keep editing, or delete. |
