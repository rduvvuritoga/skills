---
name: marketing-copy
description: Diagnose and write conversion-focused website copy. Use for creating or improving headlines, CTAs, value propositions, landing, pricing, feature, or about pages, buyer-specific messaging, conversion clarity, or AI-extractable page copy. Defer line editing that does not involve positioning or persuasion.
---

# Marketing Copy

Write clear, specific copy that connects a buyer's real problem to a believable outcome and one next action.

## Workflow

### 1. Establish the product context

Look for `.agents/product-marketing.md`, `.claude/product-marketing.md`, or a repository positioning or brand document before asking questions. Treat the project's positioning, audience, differentiation, proof, and voice as authoritative.

Gather only the missing context that can materially change the copy:

- **Purpose** — page type and the one action the visitor should take.
- **Audience** — buyer segment, urgent problem, objections, and language they use.
- **Offer** — product, differentiation, promised outcome, and available proof.
- **Traffic** — source and what the visitor already knows, when relevant.

For a narrow rewrite, infer what the supplied copy safely establishes and ask only for blocking context. Begin drafting once the audience, offer, and intended action are clear enough to avoid inventing positioning.

### 2. Diagnose before drafting

Name the main conversion problem in one sentence: unclear value, weak specificity, feature-first framing, mismatched audience, unsupported claim, hidden action, excess friction, or unanswered anxiety.

Use the branch-specific reference only when it applies:

- For a weak headline, flat page, disconnected feature list, or conversion audit, read [framework and pattern selection](references/frameworks.md) and choose one fitting tool.
- For full homepage, landing, pricing, feature, or about-page work, buyer-segment tailoring, or AI-extractable copy, read [page and audience guidance](references/page-guidance.md).

### 3. Write around one conversion argument

Make the page speak to **one audience**, about **one problem**, with **one promise**, driving **one action**.

- Lead with the outcome before the mechanism or feature.
- Use the buyer's language from research, sales calls, reviews, or supplied context.
- Prefer concrete situations and named outcomes over category jargon.
- Give each section one job in a logical argument.
- Place proof and objection handling beside the claim they support.
- Make the headline and subhead explain what the product does, who it serves, and why it matters within five seconds.

Default to direct, confident, plainspoken prose, then follow the project's documented voice. Use concrete nouns and active verbs. Let headlines carry more energy than body copy; make CTAs name the value of the next step.

### 4. Keep proof honest

Use only substantiated statistics, customer names, testimonials, logos, and ROI claims. When stronger copy needs missing evidence, insert a specific request such as:

```text
[NEED: percentage of after-hours calls missed — source from analytics]
```

This evidence rule is a hard completion gate: every factual claim must be supported by supplied context or clearly marked as a placeholder.

### 5. Return the right output for the branch

For copy submitted for review, return:

1. **Rewrite** — lead with the improved copy when the user wants a direct fix. Give 2–3 options only for a headline or CTA where distinct angles are genuinely useful.
2. **Diagnosis** — identify the main conversion problem and the evidence in the original.
3. **Why it works** — briefly name the principle and specific change.

For new copy, return the requested artifact followed by a short rationale. Add variations only when the audience or tone has multiple plausible directions, and label each by its angle. For a full page, also provide a page title and meta description that summarize the page in extractable language.

### 6. Verify before returning

Confirm all applicable checks:

- The audience, problem, promise, and primary action are each identifiable.
- The headline and subhead pass the five-second test without surrounding context.
- Outcomes appear before implementation details.
- Claims are supported or marked with evidence placeholders.
- The copy uses one buyer's language rather than generic company language.
- Each section advances one idea and reduces either confusion, friction, or anxiety.
- Filler, hedges, jargon stacks, and unearned exclamation points are absent.
- The output format matches the requested branch.

Revise until every applicable check passes; report any check blocked by missing product evidence.
