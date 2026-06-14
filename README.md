# rabbit-hole-research

Agent skill for controlled deep research, source-backed synthesis, Obsidian-ready knowledge bases, and long-horizon Goal-mode work.

This package is runtime-only and git-ready. It excludes development evals, package audit scripts, prior-art build notes, and other maintenance artifacts.

## Install

Important: the installed skill directory should be named `rabbit-hole-research`.

The GitHub repository is named `rabbit-hole-research-skill`, but Claude Code uses the installed directory name as the slash command name, and Codex UI discovery is most reliable when the folder and `SKILL.md` name match.

### Codex

From Codex, install the repository root as the skill named `rabbit-hole-research`:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo ROOKI-BAZOOKI/rabbit-hole-research-skill \
  --path . \
  --name rabbit-hole-research
```

### Claude Code

Install into:

```text
~/.claude/skills/rabbit-hole-research/
```

or project-local:

```text
.claude/skills/rabbit-hole-research/
```

Invoke with:

```text
/rabbit-hole-research
```

No separate `.claude/commands/` file is needed. Claude Code skills already create slash commands.

## Use

Codex:

```text
$rabbit-hole-research
```

Claude Code:

```text
/rabbit-hole-research
```

You can also describe a deep research task that matches the skill description.

For long AFK runs, first let the skill create the research contract and durable control files, then activate `/goal` only after acceptance criteria, evidence standards, budget behavior, and stop rules are clear.

If the task explicitly asks for existing tools, repos, implementations, workflows, or prior work, treat that as an ordinary evidence branch inside the base pipeline. There is no separate GitHub/prior-art mode in the product build.

## Included files

- `SKILL.md` — compact activation, base pipeline, overlays, stop rules, and final audit.
- `references/goal-mode.md` — long-horizon `/goal` and AFK lifecycle.
- `references/subagents.md` — bounded parallel research branches.
- `references/obsidian-output.md` — Obsidian note and vault structure.
- `assets/goal-contract-template.md` — durable Goal contract template.
- `assets/research-control-template.md` — durable research control template.
- `agents/openai.yaml` — Codex UI metadata for skill lists and chips.
- `LICENSE` — MIT license matching the `SKILL.md` metadata.

## License

MIT.
