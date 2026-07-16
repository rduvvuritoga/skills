# Repository instructions

This repository is the source of truth for Ravi's reusable coding-agent skills.
Read [`README.md`](./README.md) for the publishing workflow.

- Use the globally installed `skill-creator` guidance when creating or
  substantially updating a skill.
- Keep every published skill in a lowercase, hyphenated root folder whose name
  matches the `name` in its `SKILL.md`.
- Keep `SKILL.md` concise. Put reusable code in `scripts/`, detailed supporting
  material in `references/`, and output resources in `assets/` only when needed.
- Add every new published skill to `.claude-plugin/plugin.json` so the CLI groups
  it under **Rduvvuri Skills**.
- Before committing, run `npx skills add . --list --full-depth` and fix any
  discovery or metadata errors.
- Do not edit installed copies under `~/.agents/skills`; edit this repository and
  publish from a committed `main` with `./publish-skills` when requested.
