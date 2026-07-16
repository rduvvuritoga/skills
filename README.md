# Rduvvuri Skills

Personal coding-agent skills shared through
[`rduvvuritoga/skills`](https://github.com/rduvvuritoga/skills).

The repository is the source of truth. The skills CLI installs the published
skills into `~/.agents/skills` and makes them available to Claude Code, Codex,
Cursor, GitHub Copilot, and Warp.

## Create a skill

You only need to clone this repository once. On Ravi's Mac, the existing clone
is at:

```text
/Users/ravi/work/claude-skills
```

From that checkout:

1. Create a lowercase, hyphenated folder at the repository root. The folder
   must contain a `SKILL.md` with `name` and `description` frontmatter.
2. Add only the scripts, references, or assets the skill actually needs.
3. Add the new folder to the `skills` array in
   `.claude-plugin/plugin.json`. This keeps it grouped under **Rduvvuri
   Skills** in the skills CLI.
4. Validate the package without installing it:

   ```bash
   npx skills add . --list --full-depth
   ```

5. Commit the finished skill, then publish it:

   ```bash
   git add <skill-name> .claude-plugin/plugin.json
   git commit -m "feat: add <skill-name> skill"
   ./publish-skills
   ```

`publish-skills` pushes `main` and immediately installs the latest published
collection globally. There is no separate build step.

When creating a skill with a coding agent, ask it to use the `skill-creator`
skill so the new skill is scaffolded and validated consistently.

## Work from another computer

Clone the repository once:

```bash
git clone https://github.com/rduvvuritoga/skills.git
cd skills
```

Create, validate, commit, and push the skill normally. Ravi's Mac checks the
GitHub repository hourly and installs a newer `main` automatically.

## Update installed skills

Install the latest published version immediately:

```bash
./sync-skills
```

Force a reinstall even when the recorded revision is unchanged:

```bash
./sync-skills --force
```

