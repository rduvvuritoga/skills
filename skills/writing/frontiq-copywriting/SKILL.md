---
name: frontiq-copywriting
description: Write, rewrite, diagnose, or review FrontIQ marketing and product copy using the FrontIQ Writing System. Use when the user asks for FrontIQ homepage copy, landing pages, SEO/AEO copy, blog posts, thought-leadership articles, newsletters, LinkedIn posts, X/Twitter posts, ads, sales emails, feature pages, positioning copy, taglines, CTAs, content briefs, or copy critique. Also use when adapting generic B2B SaaS copy to FrontIQ's voice, turning positioning into page copy or educational content, or checking whether FrontIQ copy explains the operational workflow instead of relying on vague AI/revenue claims.
---

# FrontIQ Copywriting

Use the FrontIQ Writing System: copy should help the reader understand what work FrontIQ performs, how it performs it, and what business outcome follows. Prefer clear operational explanation over cleverness, hype, or broad SaaS language.

## Source Of Truth

Before writing, look for current FrontIQ positioning and GTM context in the repo. Common sources include `docs/GTM_STRATEGY.md`, `docs/PLAN.md`, `docs/PROGRESS.md`, `.agents/product-marketing.md`, `.claude/product-marketing.md`, and marketing-site copy. Treat those as authoritative for audience, offer, proof, and current status.

If source context is missing, ask only for the missing facts that materially affect the copy: audience, page/channel, desired action, current proof, and what workflow FrontIQ handles.

## The Spine

Every substantial piece of copy should progress:

```text
Problem -> Workflow -> Outcome
```

Do not jump directly from problem to benefit. Always show the mechanism.

Example:

```text
Problem: A customer calls after hours.
Workflow: FrontIQ captures the request, follows up automatically, and schedules the appointment.
Outcome: The business recovers revenue that would otherwise be lost.
```

When reviewing copy, diagnose which part is missing. Most weak FrontIQ copy either names an outcome with no workflow, or says "AI" without naming the customer operation being completed.

## Core Rules

- Write to explain, not impress. If a sentence teaches the reader something about customer operations, keep it. If it only sounds persuasive, rewrite it.
- Make every claim concrete. "Sends appointment reminders automatically" beats "improves customer experience."
- Show the work FrontIQ performs: books appointments, reschedules visits, requests missing documents, follows up with customers, updates CRM records, triggers workflows, sends reminders.
- Use operational nouns: appointment, booking, estimate, invoice, lead, customer, schedule, reminder, inspection, technician, workflow, request.
- Explain before persuading. Every claim needs a mechanism.
- Contrast categories, not named competitors. It is fine to say AI receptionists answer calls while FrontIQ completes customer workflows. Do not attack specific companies or create feature-by-feature comparisons against named competitors unless the user explicitly supplies approved positioning.
- Be concise without becoming formulaic. Short sentences are good; canned lines are not.
- Give each section one job. Do not mix definition, use cases, differentiation, proof, and CTA in the same section.
- Never fabricate proof. Use placeholders such as `[NEED: after-hours missed-call rate]` or `[NEED: customer quote about recovered bookings]`.

## Avoid

Avoid abstract nouns and empty SaaS language:

```text
transformation, innovation, engagement, enablement, ecosystem, synergy,
revolutionary, next-gen, cutting-edge, game-changing, seamless,
AI-powered, unlock, supercharge, leverage, robust, world-class
```

Use these only when a specific operational clause immediately earns them.

Avoid formulaic copy:

```text
Anyone can answer the phone.
The hard part is everything after hello.
More than just...
Don't take our word for it...
It's not X, it's Y...
```

Avoid vague claims:

```text
FrontIQ increases revenue.
FrontIQ improves customer experience.
FrontIQ transforms customer operations.
```

Rewrite with mechanism:

```text
FrontIQ captures after-hours requests and follows up automatically, helping businesses recover bookings that would otherwise be missed.
```

## Page Structure

Use this default ordering for marketing pages unless the source context calls for a different flow:

1. What FrontIQ is.
2. What customer workflow it completes.
3. Why that workflow matters to the buyer.
4. How it differs from category alternatives such as AI receptionists or support bots.
5. Proof, integration detail, or implementation reassurance.
6. One clear CTA.

For section-level copy, keep one idea per section. A section about after-hours capture should not also explain CRM updates, implementation, and pricing.

## Blog Posts

Use blog posts to teach customer operations, not to publish generic AI trend commentary. A strong FrontIQ post should name a practical operational problem, unpack the workflow behind it, and then show the business consequence.

Good blog topics sound like:

- Why missed calls turn into missed work.
- What happens after a customer asks to reschedule.
- How after-hours requests become lost bookings.
- Why answering the phone is only the first step in customer operations.

For SEO/AEO posts, answer the query directly, use plain section headings, and include self-contained sentences that explain what FrontIQ does. Do not pad with broad market commentary unless it helps the reader understand the workflow.

## Social Posts

For LinkedIn and X/Twitter, keep the same problem -> workflow -> outcome logic, but compress it to one operational insight. Do not write generic founder wisdom, AI hype, or engagement-bait.

LinkedIn posts can briefly teach the workflow:

```text
Problem: The visible issue.
Workflow: What actually has to happen behind the scenes.
Outcome: Why it matters for operators or customers.
```

X/Twitter posts should make one crisp point. Prefer concrete observations over threads unless the user asks for a thread. Keep hooks plain and specific, not dramatic.

Good social angles:

- A missed call is not just a missed conversation. It is an unscheduled job, an uncollected estimate, or a customer waiting for follow-up.
- AI receptionists answer. Customer operations systems complete the work that follows.
- The hard part of customer service is often not the answer. It is the update, reminder, reschedule, booking, and handoff.

## Useful Lines

Use these as patterns, not templates:

- FrontIQ handles customer requests after your staff goes home.
- Every request creates work.
- Calls create tasks.
- Bookings are only the beginning.
- Customer requests do not stop at answering.
- AI receptionists answer calls. FrontIQ completes customer workflows.
- Support bots answer questions. FrontIQ executes customer requests.

Adapt them to the real audience and workflow. Do not stack too many aphorisms together.

## Review Checklist

Before returning copy, check:

- Does the reader know what FrontIQ is within a few seconds?
- Does the copy name the customer workflow, not just the outcome?
- Does each benefit have a mechanism?
- Are the nouns operational and concrete?
- Are named competitor attacks absent?
- Is every section about one idea?
- Did every sentence add information that would disappear if removed?
- Are proof points real, sourced, or clearly marked as needed?
- Is the CTA singular and tied to what the reader gets next?

## Output

For critique or rewrite requests, return:

1. **Diagnosis** - Name what is unclear or missing, usually in terms of problem, workflow, outcome, specificity, proof, or category contrast.
2. **Rewrite** - Provide the improved copy. Lead with this when the user asked for a direct fix.
3. **Why it works** - Briefly explain the writing-system move so the pattern is reusable.

For new page or campaign copy, include only the sections needed for the requested artifact. Do not dump every possible variation. Give 2-3 variants only when the user asks for options or the strategic angle is genuinely ambiguous.
