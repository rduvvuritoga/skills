---
name: error-modals
description: >
  Generate well-designed error modals, dialogs, and error messages for web and mobile UIs.
  Use this skill whenever the user asks to build, design, write copy for, or improve an error
  modal, error dialog, error toast, error state, or error message — including form validation
  errors, server errors, network errors, permission errors, and empty states. Also trigger when
  the user says "something went wrong" screen, error handling UI, or wants to audit/rewrite
  existing error copy. Even if the user just says "add an error modal", use this skill — it
  encodes the full framework for what makes error UX good vs. bad.
---

# Error Modals Skill

Generate error modals that are empathetic, actionable, and honest — not generic, jargon-filled,
or blame-shifting.

## The Framework: What Makes an Error Message Good or Bad

### ❌ Bad error messages have these traits

| Trait | Example | Why it's bad |
|---|---|---|
| **Generic** | "Something went wrong." | Tells the user nothing; forces them to guess |
| **Technical jargon** | "Your credentials were denied." / "Failed to fetch." | Users don't care about internals; they want to know what to do |
| **Blame-shifting** | "You didn't fill in the required fields correctly." | Shames the user; even if true, focus on the fix, not the fault |
| **Third-party blame** | "Stripe isn't responding right now." | Looks unprofessional; user came to YOU, not Stripe |
| **Wrong tone** | "Oops! 😬 Something broke lol" | Cutesy is inappropriate when stakes are high (income, data loss) |
| **No path forward** | Error with no button, no link, no next step | Leaves the user stranded |

### ✅ Good error messages do these five things

1. **Say what happened and why** — Be specific. If you know the cause, say it. If it's a
   technical issue on your end, own it ("due to an issue on our end").

2. **Provide reassurance** — Tell them what was *not* affected. "Your draft was saved."
   "No charges were made." This is often the most calming thing you can write.

3. **Be empathetic, not performatively apologetic** — Use "please" sparingly, in genuinely
   dire situations. Don't over-apologize; it reads as hollow. Do acknowledge it matters.

4. **Help them fix it** — Give a clear, specific action. If the fix is complex, link to a
   KB article with a descriptive label like "Learn how to resolve this" — not just "Help".

5. **Always give a way out** — If they can't fix it themselves, provide a path to customer
   support. Never leave the user with no recourse.

---

## Tone Calibration by Severity

| Severity | Situation | Tone |
|---|---|---|
| **Low** | Form validation, minor input error | Neutral, matter-of-fact, quick fix |
| **Medium** | Feature unavailable, timeout, retry needed | Calm, helpful, solution-focused |
| **High** | Data loss risk, payment failure, publish failed | Serious, empathetic, reassuring |
| **Critical** | Account locked, irreversible action, data deleted | Grave, no humor, full ownership |

At medium and above: never use humor, exclamation points, or casual slang.

---

## Modal Anatomy

A complete error modal has these slots:

```
┌─────────────────────────────────────────┐
│  [Icon]  Title (what happened, 5-8 words)│
│                                          │
│  Body: why it happened + reassurance     │
│  (2–3 sentences max)                     │
│                                          │
│  [Secondary CTA]    [Primary CTA]        │
└─────────────────────────────────────────┘
```

**Title**: Name the problem, not the symptom. "Couldn't save your changes" > "Error"  
**Body**: What happened → why (if known) → what was preserved → what to do next  
**Primary CTA**: The fix action. "Try Again", "Retry", "Contact Support"  
**Secondary CTA**: The escape hatch. "Cancel", "Dismiss", "Go Back"  
**Never**: An OK button with no action context, or a single "Close" with no explanation

---

## Copy Templates by Error Type

### Network / Server Error
```
Title:    We couldn't complete that action
Body:     There was an issue on our end while [attempting to X]. Your [data/work] 
          hasn't been affected. Please try again — if the problem continues, 
          [contact our support team →].
CTA:      Try Again | Dismiss
```

### Form / Validation Error (inline, not modal — but if modal is required)
```
Title:    A few things need your attention
Body:     [Field name] is missing [or: doesn't look right]. 
          Check the highlighted fields below and try again.
CTA:      Review Fields | Cancel
```

### Payment / Billing Failure
```
Title:    We couldn't process your payment
Body:     Your card ending in [XXXX] was declined. No charge was made. 
          Please check your card details or try a different payment method.
CTA:      Update Payment | Contact Support
```

### Permission / Access Denied
```
Title:    You don't have access to this
Body:     This [feature/page/file] is only available to [admins / Pro plan members].
          [Upgrade your plan →] or ask your account admin for access.
CTA:      Upgrade Plan | Go Back
```

### Data Loss Warning (confirmation modal)
```
Title:    Unsaved changes will be lost
Body:     If you leave now, the changes you made to [item name] won't be saved.
          This can't be undone.
CTA:      Leave Without Saving | Keep Editing
```

### Destructive Action Confirmation
```
Title:    Delete [item name]?
Body:     This will permanently delete [item name] and all its associated [data].
          This action cannot be undone.
CTA:      Delete Permanently | Cancel
```

---

## React Component (shadcn/ui + Tailwind)

When generating an error modal as code, use this structure as the base:

```jsx
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogDescription,
  DialogFooter,
} from "@/components/ui/dialog";
import { Button } from "@/components/ui/button";
import { AlertCircle } from "lucide-react";

export function ErrorModal({
  open,
  onClose,
  onRetry,
  title = "Something went wrong",
  description,
  primaryAction = "Try Again",
  secondaryAction = "Dismiss",
}) {
  return (
    <Dialog open={open} onOpenChange={onClose}>
      <DialogContent className="sm:max-w-md">
        <DialogHeader>
          <div className="flex items-center gap-3">
            <AlertCircle className="h-5 w-5 text-destructive shrink-0" />
            <DialogTitle>{title}</DialogTitle>
          </div>
          <DialogDescription className="pt-1 pl-8">
            {description}
          </DialogDescription>
        </DialogHeader>
        <DialogFooter className="gap-2 sm:gap-0">
          <Button variant="outline" onClick={onClose}>
            {secondaryAction}
          </Button>
          {onRetry && (
            <Button variant="destructive" onClick={onRetry}>
              {primaryAction}
            </Button>
          )}
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

Adapt `variant` on the primary button:
- Network/server errors → `default` (blue)
- Destructive confirmations → `destructive` (red)
- Warnings → `default` with yellow icon (`AlertTriangle`)

---

## Checklist Before Finalizing

Before presenting any error modal copy or component, verify:

- [ ] Title says *what* happened, not just "Error" or "Oops"
- [ ] Body explains *why* (or acknowledges "an issue on our end" if unknown)
- [ ] Reassurance line included if any data/work could be at risk
- [ ] No technical jargon (no "fetch failed", "403", "null reference")
- [ ] No blame on user or third parties
- [ ] Tone matches severity — no humor or casual language for medium/high
- [ ] Primary CTA is a specific action, not just "OK"
- [ ] Secondary CTA / escape hatch is present
- [ ] Support path offered if user cannot self-serve

---

## Anti-Pattern Rewrites

Use these as reference when auditing existing error messages:

| Before | After |
|---|---|
| "Something went wrong." | "We couldn't save your changes due to an issue on our end." |
| "Error 403: Unauthorized" | "You don't have permission to view this page." |
| "Oops! Your request failed 😬" | "We couldn't complete your request. Please try again." |
| "Stripe isn't responding right now." | "We're having trouble connecting to our payment provider." |
| "You didn't enter a valid email." | "This email address doesn't look right — check it and try again." |
| "An unexpected error occurred." | "Something went wrong on our end. Your work has been saved." |
