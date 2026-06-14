# Codex Subagents for Rabbit-Hole Research

Use this reference when research can be split into independent read-heavy branches. The coordinator stays in the main thread and owns synthesis.

## Default Codex config

For most research runs, keep nested fan-out disabled and cap concurrency:

```toml
# .codex/config.toml
[agents]
max_threads = 6
max_depth = 1
```

Use more threads only when branches are truly independent and source-heavy. Do not raise depth unless the user explicitly wants recursive delegation.

## Parent prompt template

```text
Use the rabbit-hole-research skill. Spawn one read-only research subagent per branch below, wait for all results, then synthesize in the main thread. Do not let subagents write final conclusions. Each subagent must return: branch, scope searched, best sources inspected, key claims with source pointers, contradictions, dead ends, next terms, confidence.

Branches:
1. <branch A>
2. <branch B>
3. <branch C>
```

## Branch prompt template

```text
You are a read-only branch researcher. Investigate only this branch: <branch>.
Goal: <why this branch matters to final answer>.
Source priority: primary/official first, then peer-reviewed/standards, then maintained code/issues, then expert analysis.
Do not browse randomly. Each query/source must have a purpose. Stop when three credible sources add no material new claim or when the branch no longer affects the final output.
Return a compact report with source pointers, contradictions, killed tangents, and confidence. Do not write files unless the parent asks.
```

## Optional custom agents

Create these only if you repeatedly run this workflow. Save personal agents in `~/.codex/agents/` or project agents in `.codex/agents/`.

### `.codex/agents/research-scout.toml`

```toml
name = "research_scout"
description = "Read-only scout for one bounded research branch; gathers sources, claims, contradictions, and next search terms without final synthesis."
model_reasoning_effort = "high"
sandbox_mode = "read-only"
developer_instructions = """
Stay within the assigned branch.
Use targeted searches and source reads; do not collect raw link dumps.
Extract claims, source pointers, contradictions, and next search terms.
Kill low-value tangents with a reason.
Return a compact branch report. Do not write files or make final recommendations.
"""
```


### `.codex/agents/source-auditor.toml`

```toml
name = "source_auditor"
description = "Read-only auditor that checks source quality, citation support, contradictions, and overclaims before final synthesis."
model_reasoning_effort = "high"
sandbox_mode = "read-only"
developer_instructions = """
Audit whether claims are supported by the cited evidence.
Flag weak sources, circular citations, missing primary sources, stale facts, hidden contradictions, and unsupported inference.
Do not rewrite the final answer. Return ranked findings with severity and fix suggestions.
"""
```

### `.codex/agents/obsidian-curator.toml`

```toml
name = "obsidian_curator"
description = "Curates completed research into Obsidian-ready atomic notes, indexes, backlinks, and frontmatter. Use after synthesis, not during uncontrolled browsing."
model_reasoning_effort = "medium"
developer_instructions = """
Create or update only the assigned Obsidian folder.
Do not invent sources or claims. Convert the approved source/claim ledger into atomic notes with YAML frontmatter, backlinks, and status tags.
Prefer useful density over raw volume. Mark raw notes as raw and synthesized notes as synthesized.
"""
```

## When not to use subagents

Avoid subagents for one-source tasks, simple lookups, tasks where the research direction is still unclear, or workflows where several agents would edit the same files. First validate direction, then delegate.

## Result consolidation checklist

- Merge duplicate claims across subagents.
- Compare contradictions before writing synthesis.
- Track which branches found no useful evidence.
- Do not blindly trust a confident subagent; inspect source support for critical claims.
- Ask a source-auditor subagent to review the final claim table for high-stakes or long reports.
