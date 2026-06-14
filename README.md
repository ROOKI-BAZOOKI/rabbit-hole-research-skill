# rabbit-hole-research

Production Codex skill for controlled deep research, Obsidian-ready knowledge bases, and long-horizon `/goal` research.

This package is runtime-only and git-ready. It excludes development evals, package audit scripts, prior-art build notes, and other maintenance artifacts.

## Install

User-level install:

```bash
mkdir -p ~/.agents/skills
cp -R rabbit-hole-research ~/.agents/skills/
```

Repo-level install:

```bash
mkdir -p .agents/skills
cp -R rabbit-hole-research .agents/skills/
```

## Use

Invoke explicitly with `$rabbit-hole-research` or describe a deep research task that matches the skill description.

For long AFK runs, first let the skill create the research contract and durable control files, then activate `/goal` only after acceptance criteria, evidence standards, budget behavior, and stop rules are clear.

If the task explicitly asks for existing tools, repos, implementations, workflows, or prior work, treat that as an ordinary evidence branch inside the base pipeline. There is no separate GitHub/prior-art mode in the product build.

## Included files

- `SKILL.md` — compact activation, base pipeline, overlays, stop rules, and final audit.
- `references/goal-mode.md` — long-horizon `/goal` and AFK lifecycle.
- `references/subagents.md` — bounded parallel research branches.
- `references/obsidian-output.md` — Obsidian note and vault structure.
- `assets/goal-contract-template.md` — durable Goal contract template.
- `assets/research-control-template.md` — durable research control template.
- `LICENSE` — MIT license matching the `SKILL.md` metadata.
- `.gitignore` — minimal local scratch/cache ignores for repo use.

## Architecture

Version 1.0.0 is the minimal git-ready runtime build. It keeps the base-pipeline-plus-overlays architecture, removes the dedicated GitHub/prior-art overlay, and folds the final quality gate into `SKILL.md` instead of shipping a separate audit reference.

## License

MIT.
