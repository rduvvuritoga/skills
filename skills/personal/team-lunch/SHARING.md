# Sharing the `team-lunch` skill

A paste-ready setup note for the team, plus the dependencies that live outside
this repo. Fill in the `dd-cli` download link before posting.

## Slack message

> **🍱 New: order team lunch straight from Claude Code / Codex**
>
> I built a `team-lunch` skill — say *"I'm hungry"* or *"order lunch for the
> team"* and it walks the whole order (cuisine → restaurant → headcount →
> dietary needs → checkout) on DoorDash. It only places the order after you
> explicitly confirm.
>
> **One-time setup (~5 min):**
>
> **1. Install `dd-cli`** (the DoorDash CLI it drives):
> `<paste the dd-cli download/install link here>`
> Then confirm it works: `dd-cli --version`
>
> **2. Log into your own DoorDash account:**
> `dd-cli login`
> (Your account, your saved cards — nothing shared.)
>
> **3. Install the skill:**
> ```
> npx skills add rduvvuritoga/skills \
>   --skill team-lunch \
>   --agent claude-code codex \
>   --global --yes --full-depth
> ```
>
> **That's it.** Open a fresh session and type *"order lunch for the team."*
>
> Notes:
> • Safe to try — the flow is free up to the price preview; nothing is charged
>   until you say "place it."
> • Want it billed to the company? That needs DoorDash for Business set up on
>   your account — otherwise it uses your personal card.
> • Update later: `npx skills update --global --yes`

## Dependencies (not carried by the skill)

The skill is only the workflow. Each teammate also needs:

1. **`dd-cli` on PATH** — a standalone binary, distributed separately from this
   repo. Share your install link.
2. **`dd-cli login`** — into their own DoorDash account. Orders and cards are
   per-account.
3. **DoorDash for Business** (optional) — required for the work-benefits budget
   step to offer anything; without it, orders bill a personal card.

The skill's `SKILL.md` contains every `dd-cli` command it needs, so it runs
without the personal `dd-cli-usage` helper skill.
