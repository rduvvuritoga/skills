# Rduvvuri Skills

[![skills.sh](https://skills.sh/b/rduvvuritoga/skills)](https://skills.sh/rduvvuritoga/skills)

Ravi Duvvuri's reusable coding, writing, marketing, and learning skills for
coding agents. The repository is the source of truth; the skills CLI installs
the published collection into `~/.agents/skills` for Claude Code, Codex,
Cursor, GitHub Copilot, and Warp.

## Install

Install every published skill globally:

```bash
npx skills add rduvvuritoga/skills \
  --global \
  --skill '*' \
  --agent claude-code codex cursor github-copilot warp \
  --yes \
  --full-depth
```

Update already-installed global skills:

```bash
npx skills update --global --yes
```

## Skills

Model-invoked skills are available automatically when a request matches.
User-invoked skills run only when explicitly selected.

See [`index.md`](./index.md) for detailed descriptions, use cases, outputs, and
invocation syntax for every skill.

### Coding

**Model-invoked**

- [`codex-computer-use`](./skills/coding/codex-computer-use/SKILL.md) — Delegate local UI and runtime verification to Codex CLI.
- [`error-modals`](./skills/coding/error-modals/SKILL.md) — Design actionable error states, recovery messages, and dialogs.
- [`explain-diff`](./skills/coding/explain-diff/SKILL.md) — Create a self-contained HTML walkthrough of a code change.
- [`review-pr-comments`](./skills/coding/review-pr-comments/SKILL.md) — Fix or close out pull-request feedback with verified resolution.
- [`save-for-later`](./skills/coding/save-for-later/SKILL.md) — Preserve deferred work as a detailed GitHub issue.

**User-invoked**

- [`setup-lanes`](./skills/coding/setup-lanes/SKILL.md) — Create stacked branches for parallel-agent feature work.
- [`sync-personal-skills`](./skills/coding/sync-personal-skills/SKILL.md) — Manually sync Ravi's global skills.

### Writing and marketing

**Model-invoked**

- [`marketing-copy`](./skills/writing/marketing-copy/SKILL.md) — Diagnose and write conversion-focused marketing-site copy.

### Learning

**User-invoked**

- [`learn`](./skills/learning/learn/SKILL.md) — Coach a topic through structured learning modes.

## Create a skill

Clone this repository once and reuse that checkout. On Ravi's Mac, it lives at:

```text
/Users/ravi/work/skills
```

To add a skill:

1. Choose `skills/coding/`, `skills/writing/`, or `skills/learning/`.
2. Create a lowercase, hyphenated skill folder containing `SKILL.md` and
   `agents/openai.yaml`. Its name must be unique across every category because
   all skills install into the flat `~/.agents/skills/<skill-name>` namespace.
   Ask a coding agent to use `skill-creator` when scaffolding it.
3. Decide whether the skill is model-invoked or user-invoked. User-invoked
   skills must set `disable-model-invocation: true` in `SKILL.md` and
   `policy.allow_implicit_invocation: false` in `agents/openai.yaml`.
4. Add the folder to `.claude-plugin/plugin.json` and add its catalog entry
   above.
5. Validate everything:

   ```bash
   ./check-skills
   ```

6. Commit the finished change, then publish it:

   ```bash
   git add .
   git commit -m "feat: add <skill-name> skill"
   ./publish-skills
   ```

`publish-skills` validates the collection, pushes `main`, and immediately
installs the latest published revision globally. Skills have no separate build
step.

## Work from another computer

Clone the repository once:

```bash
git clone https://github.com/rduvvuritoga/skills.git
cd skills
```

Create, validate, commit, and push normally. On Ravi's Mac, run
`./sync-skills` or invoke `$sync-personal-skills` to install the newer `main`.

## Sync this computer

Install the latest published revision immediately:

```bash
./sync-skills
```

Force a reinstall even when the recorded revision is unchanged:

```bash
./sync-skills --force
```

## License

[MIT](./LICENSE)
