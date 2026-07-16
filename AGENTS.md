# Repository instructions

This repository is the source of truth for Ravi's reusable coding-agent skills.
Read [`README.md`](./README.md) for the catalog and publishing workflow.

Skills are organized by use:

- `skills/coding/` — engineering and repository workflows
- `skills/writing/` — writing, UX copy, and marketing workflows
- `skills/learning/` — teaching and learning workflows

When changing the collection:

- Use the globally installed `skill-creator` guidance to create or
  substantially update a skill.
- Keep every skill in a lowercase, hyphenated folder whose name matches the
  `name` in `SKILL.md`.
- Keep skill names unique across all categories; category folders do not
  namespace the installed `~/.agents/skills/<skill-name>` destination.
- Keep `SKILL.md` concise. Put reusable code in `scripts/`, detailed supporting
  material in `references/`, and output resources in `assets/` only when needed.
- Give every skill an `agents/openai.yaml` with `display_name`,
  `short_description`, and a `default_prompt` that mentions `$<skill-name>`.
- Pair `disable-model-invocation: true` in a user-invoked `SKILL.md` with
  `policy.allow_implicit_invocation: false` in its `agents/openai.yaml`.
- Add every published skill to `.claude-plugin/plugin.json` and the categorized
  catalog in `README.md`.
- Run `./check-skills` before committing. It verifies discovery, manifest and
  README coverage, UI metadata, and invocation-policy consistency.
- Do not edit installed copies under `~/.agents/skills`. Edit this repository
  and run `./publish-skills` from a committed `main` when publication is
  requested.
