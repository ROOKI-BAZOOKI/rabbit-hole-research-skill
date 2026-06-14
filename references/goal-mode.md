# Goal Mode for Long-Horizon Rabbit-Hole Research

Use this reference when a research task is too large for a normal Codex turn and the user wants Codex to keep working across many turns, possibly while the user is AFK. The purpose is not to make Codex loop forever. The purpose is to preserve a durable objective, work through checkpoints, and stop only when the evidence says the job is complete, blocked, budget-limited, or no longer worth continuing.

## Mental model

Goal mode is a thread-scoped completion contract. The goal belongs to the active thread, not to global memory, project instructions, or a generic backlog. The goal text functions as both the starting instruction and the completion criteria.

A normal prompt says: do the next thing and wait.

A Goal says: continue working, checking, and deciding the next useful action until the defined end state is true or a stop condition is reached.

For long research, this is useful only when the end state can be audited. A goal such as “research this deeply” is weak. A goal such as “produce an evidence-backed research report, source ledger, claim ledger, Obsidian note set when requested, and final audit that separates confirmed claims, weak evidence, blocked claims, contradictions, and remaining uncertainty” is strong.

## When to use `/goal`

Use Goal mode when all are true:

- The work may require many turns, hours, or days.
- The path depends on what Codex discovers along the way.
- There is a durable objective and a verifiable final artifact.
- There are explicit constraints and non-goals.
- Codex can write progress to files, not just remember it in chat.
- The user can tolerate autonomous progress between check-ins.

Good fits:

- existing tool, repo, workflow, or implementation comparisons when explicitly requested;
- technical, market, literature, or source-backed research reports;
- Obsidian vault/database construction;
- benchmarked comparisons of tools, repos, workflows, or methods;
- reproducing a paper/project claim where evidence quality must be labeled;
- iterative synthesis where new findings change the next search branch.

Avoid Goal mode when:

- the user wants one answer and a stop;
- the task is a simple lookup, edit, explanation, or summary;
- the finish line is vague;
- the user has not validated ambiguous direction;
- the objective is a list of unrelated tasks;
- Codex cannot inspect evidence or produce durable artifacts;
- continued work would wait on external events and no manual resume plan exists.

## Capability setup

Before relying on Goal mode for a multi-hour or multi-day run, confirm that the current Codex surface supports it. In CLI/TUI, check `codex --version` and the slash command list. If `/goal` is missing in a supported build, enable the feature if available:

```toml
# ~/.codex/config.toml or project config where appropriate
[features]
goals = true
```

or run:

```bash
codex features enable goals
```

Do not assume every environment has identical lifecycle controls, app-server APIs, budget reporting, or remote-computer behavior. If Goal controls are unavailable, fall back to a normal checkpointed workflow with explicit resume instructions and do not promise AFK continuation.

## Goal lifecycle states

Treat lifecycle states as execution states, not quality judgments:

```text
active / pursuing     Continue when the thread is idle and the next action has positive expected value.
paused                User/system paused continuation; do not continue until resumed.
blocked               Outside input, missing data, approval, failed state reconstruction, or unavailable evidence prevents defensible progress.
budget-limited        Budget is exhausted or nearly exhausted; summarize, do not continue substantive work.
usage-limited         Platform usage availability prevents continuation; summarize current state.
complete / achieved   Final audit passed against original goal and evidence.
unmet                 Goal not achieved; explain what failed and what would unlock progress.
```

A blocked, budget-limited, usage-limited, or unmet Goal is not a failed response if the partial result is honest, inspectable, and resumable.

## Required preflight before activating `/goal`

Do not start `/goal` directly from a vague user request. First run the normal skill phases:

1. Direction validator / mini-grill when intent is not safe enough.
2. Research contract.
3. Branch map.
4. Source priority and evidence standard.
5. Acceptance criteria.
6. Stop rules and budget rules.
7. Control-file scaffold.

If any of these are underspecified but not blocking, choose a default, record it under `Assumptions`, and continue. If a wrong default could waste hours, ask the user before starting the goal.

## Control-file architecture

For long-horizon research, create a durable workspace before Goal activation:

```text
research/<slug>/
  .control/
    goal.md
    control.md
    progress-log.md
    branch-map.md
    source-ledger.md
    claim-ledger.md
    contradiction-ledger.md
    parked.md
    blockers.md
    budget.md
    completion-audit.md
    checkpoints/
      001-baseline.md
      002-<branch>.md
  sources/
  claims/
  synthesis/
  comparisons/             # only when explicit prior-work comparison is part of scope
  obsidian/              # only when building a vault/export
```

Use files as the source of truth. Chat history is useful but not authoritative after compaction, resume, or long idle periods.

### `.control/goal.md`

```markdown
# Active Goal Contract

Root objective:
Final artifacts:
Acceptance criteria:
Verification surface:
Source/evidence standard:
Allowed tools/resources:
Boundaries and non-goals:
Iteration policy:
Subagent policy:
Obsidian policy:
Budget policy:
Stop conditions:
Blocked report requirements:
Completion audit requirements:
Last reviewed:
```

### `.control/progress-log.md`

Append one compact entry per meaningful step:

```markdown
## <timestamp> — <checkpoint or branch>
Action:
Evidence inspected:
Claims added/changed:
Branch decision:
Files updated:
What improved:
What got worse / risk:
Next action:
Blocked?: no | yes — <reason>
```

### `.control/branch-map.md`

Track branches with explicit status:

```markdown
| Branch | Status | Why it matters | Next action | Kill/park condition | Confidence |
|---|---|---|---|---|---|
| Official docs | active | Defines current feature behavior | inspect latest docs | no new semantics after 3 sources | medium |
| Repo issues/changelogs | parked | Real failure modes | resume if docs conflict | repeats known issues | medium |
```

### `.control/source-ledger.md`

Keep only sources that contribute to the answer. A source must add a material claim, primary evidence, contradiction, method, implementation detail, failure case, useful terminology, or strong bibliography lead.

### `.control/claim-ledger.md`

A long Goal should be claim-driven, not link-driven:

```markdown
| Claim | Evidence | Source quality | Status | Confidence | Used in final? |
|---|---|---|---|---|---|
| Goal completion must be evidence-checked | official docs + local audit | official | confirmed | high | yes |
```

## Goal-writing template

Use this shape:

```text
/goal <desired end state>, verified by <evidence surface>, while preserving <constraints>. Use <allowed inputs/tools/boundaries>. Work in checkpoints and maintain <control files/ledgers>. Between iterations, choose the next action by <expected-value rule>. Do not mark complete until <audit>. If blocked, budget-limited, uncertain, or no defensible path remains, stop and report <blocked report fields>.
```

## Default long research Goal

```text
/goal Complete the evidence-backed rabbit-hole research described in research/<slug>/.control/goal.md. Produce the requested final artifacts, source ledger, claim ledger, branch map, synthesis snapshots, implementation/prior-work comparison only when explicitly relevant, Obsidian output when requested, and final audit. Work in checkpoints. Before every continuation, compaction recovery, or resume, reconstruct state from the control files. Continue only while the next branch has positive expected value and can change the final output, reduce uncertainty, add material evidence, or improve the durable knowledge base. Do not mark complete until the original goal, acceptance criteria, ledgers, deliverables, contradictions, and final audit all agree that the work is done. If blocked, budget-limited, usage-limited, uncertain, or all remaining branches have low marginal value, stop substantive work and report completed work, evidence gathered, unresolved items, blockers, and the next input needed.
```

## Strong research Goal examples

### Existing implementation comparison

Use only when the goal itself asks to compare existing tools, repos, agents, workflows, implementations, or prior work. This is not a default branch for every research task.

```text
/goal Find and audit the strongest existing implementations relevant to <topic>, verified by inspected primary artifacts such as official docs, repo files, changelogs, issues, examples, and maintenance signals. Maintain research/<slug>/.control/* ledgers and produce a comparison that separates transferable mechanisms, rejected patterns, uncertainty, and final adopted changes. Continue only while new candidates add mechanisms or contradictions that could change the final output. Stop if remaining candidates are duplicates, irrelevant, abandoned, or low-value, and report the stop rationale.
```

### Obsidian knowledge base

```text
/goal Build an Obsidian-ready research base for <topic>, verified by an index, source notes, claim notes, synthesis notes, backlinks, frontmatter, contradiction notes, and an audit showing that every retained note has evidence and a purpose. Keep more useful evidence, not obvious junk. Continue branch research only while it adds non-obvious sourced claims, contradictions, methods, failure cases, or decision-changing context. Stop and report if evidence becomes repetitive or confidence is capped by unavailable sources.
```

### Evidence-backed technical report

```text
/goal Produce the strongest evidence-backed technical report on <topic>, verified by official/primary sources where possible, a claim ledger, contradiction ledger, repo, changelog, or issue evidence where relevant, and a final synthesis that separates confirmed claims, weak/proxy evidence, blocked claims, and remaining uncertainty. Work in checkpoints, keep logs compact, and do not declare completion until the final audit passes.
```

## AFK operating loop

When the user wants AFK behavior, Codex should run this loop:

1. Read `.control/goal.md`, `.control/control.md`, `.control/progress-log.md`, `.control/branch-map.md`, source ledger, claim ledger, and blockers.
2. Identify the current checkpoint and highest expected-value next action.
3. Do one focused research or synthesis step.
4. Write findings to ledgers immediately.
5. Update branch status: active, completed, killed, parked, blocked, unresolved.
6. Write a progress-log entry.
7. If the context is growing, create a synthesis snapshot.
8. Decide: continue, checkpoint, pause/blocked, budget-limited, or complete.
9. Before completion, run the full completion audit.

Never let the AFK loop become “search more because time remains.” Continue only when the next action has a reason.

## Checkpoint cadence

Use checkpoints to keep the run inspectable:

- after each major branch;
- after every 10-15 meaningful sources;
- before and after large subagent fan-out;
- before creating many Obsidian notes;
- before final synthesis;
- after any compaction or resume;
- whenever the branch map changes materially.

Checkpoint format:

```markdown
# Checkpoint <n>: <title>

Goal status:
Completed since last checkpoint:
Evidence added:
Claims added/changed:
Contradictions found:
Branches completed:
Branches killed/parked:
Current risks:
Next highest-value action:
Should continue?: yes/no and why
```

## Compaction and resume recovery

Long Codex sessions may compact context. After compaction/resume, the agent may over-focus on the last local subtask and forget the global research objective. Guard against that explicitly.

At the start of every continuation after compaction/resume, perform this recovery preflight:

```text
1. Re-read .control/goal.md and acceptance criteria.
2. Re-read the latest checkpoint and progress-log tail.
3. Re-read branch-map, source-ledger, claim-ledger, blockers, and completion-audit if present.
4. Reconstruct what is complete, active, parked, killed, blocked, and unresolved.
5. Verify current files on disk before editing or declaring completion.
6. Decide the next action from the reconstructed state, not from memory.
7. If reconstruction fails or files conflict, stop and report blocked.
```

Completion after compaction requires the same final audit as any other completion. Passing a local check, finishing one branch, seeing a stale progress panel, or finding no immediate next action is not enough.

## Budget and stop policy

A long Goal needs explicit budget behavior. Define at least one of these:

- maximum token budget, if available;
- maximum source count before checkpoint review;
- maximum branch depth;
- maximum duplicate/no-new-claim streak;
- maximum subagent fan-out;
- minimum evidence threshold for final claims;
- stopping rule for low marginal value.

Recommended defaults:

```markdown
Budget policy:
- Maximum active branches at once: 6.
- Maximum nested delegation: 1 level.
- Checkpoint after 10-15 meaningful sources or each major branch.
- Kill a branch after 3 credible sources add no material claim.
- Park tangents more than 1 hop from root unless expected value is explicit.
- Stop if remaining branches cannot change the final output, reduce uncertainty, add material evidence, or improve requested Obsidian artifacts.
```

When budget is hit, do not claim success. Report:

```markdown
Status: budget-limited | usage-limited | blocked | unmet
Completed:
Evidence gathered:
Deliverables produced:
Unfinished high-value branches:
Known contradictions:
Risks if user acts on current output:
Next best action if resumed:
```

## Waiting without token burn

Do not simulate a durable wait state by repeatedly polling inside the model. If research is blocked by an external event, CI, a download, a user decision, API quota, or an unavailable source, mark the goal blocked or ask the user to pause/resume later.

If the environment supports a real scheduler or future wait/wake primitive, use it only with a bounded wake condition. Until then, prefer manual `/goal pause` and later `/goal resume` rather than burning turns while nothing can be learned.

## User controls and lifecycle

Common lifecycle controls:

```text
/goal <objective>   Start or replace the active goal.
/goal               Inspect current goal/status where supported.
/goal pause         Pause autonomous continuation.
/goal resume        Resume a paused goal.
/goal clear         Remove the current goal.
```

Codex should suggest pausing when:

- the user is about to go offline and the next phase requires approval;
- the run is blocked by external input;
- the objective needs to change materially;
- a risky write/edit phase is about to start;
- there is no reliable evidence path without user preference.

Codex should suggest clearing/replacing the Goal when:

- the user changes the root objective;
- the research turns into implementation;
- the accepted output format changes completely;
- the current Goal was poorly scoped and would cause drift.

## Subagents under Goal mode

Goal mode should not turn subagents into an uncontrolled swarm. The parent thread owns the active Goal and all completion decisions.

Use subagents for bounded, read-heavy branches. Each subagent must receive:

```markdown
Parent goal:
Assigned branch:
Why this branch matters:
Allowed sources/tools:
Output schema:
Stop rules:
No-final-synthesis rule:
```

Subagents must write compact reports. The parent merges claims and ledgers. Avoid parallel writing to the same files unless one curator agent owns the write phase.

Under a long Goal, use this fan-out sequence:

1. Parent updates branch map.
2. Parent spawns scouts for independent branches.
3. Scouts return compact branch reports.
4. Parent validates source support for load-bearing claims.
5. Parent updates ledgers and synthesis snapshot.
6. Parent decides whether more fan-out is justified.

## Completion audit for Goal mode

Before marking the Goal complete, verify all items:

```markdown
## Goal completion audit

Original goal restated:
Acceptance criteria satisfied?: yes/no
Final artifacts exist?: yes/no and paths
Source ledger complete enough?: yes/no
Claim ledger reconciled?: yes/no
Contradictions surfaced?: yes/no
Weak/proxy evidence labeled?: yes/no
Branches all completed/killed/parked/blocked?: yes/no
Obsidian output valid, if requested?: yes/no
Implementation/prior-work comparison branch completed, if explicitly relevant?: yes/no
Subagent outputs consolidated?: yes/no
Compaction/resume recovery done before completion?: yes/no/not needed
Remaining uncertainty:
Reasons further research has low expected value:
Final status: complete | blocked | budget-limited | usage-limited | unmet
```

A Goal can be complete only when the final status is `complete`. If the status is `blocked`, `budget-limited`, `usage-limited`, or `unmet`, deliver the best partial result and the next exact action instead of pretending completion.

## Failure modes and fixes

### Premature completion

Symptom: Codex completes a local subtask and declares the whole Goal done.

Fix: re-read `.control/goal.md`, restate the original acceptance criteria, and run the completion audit before any completion claim.

### Compaction drift

Symptom: after compaction, Codex repeats old branches, forgets constraints, or changes progress estimates sharply.

Fix: run recovery preflight; trust control files over chat memory; mark uncertain state as blocked rather than guessing.

### Research trap

Symptom: more sources are being collected but the claim ledger is flat.

Fix: kill or park branch; synthesize; move to the branch with higher expected value.

### AFK token burn

Symptom: Codex keeps polling, waiting, or searching without new evidence.

Fix: set status blocked or pause; resume only when new information is available.

### Subagent overlap

Symptom: multiple scouts return the same sources or claims.

Fix: tighten branch boundaries, merge duplicates, stop one branch, and update the search vocabulary.

### Obsidian junk growth

Symptom: many notes exist but few are linked to claims, decisions, or synthesis.

Fix: stop note creation, run curator/audit, delete or quarantine junk notes, and resume only with claim-linked notes.

## Minimal non-compactable invariant to include in the Goal text

Because long sessions can compact context, embed a short invariant directly in the Goal and control file:

```text
Invariant: Before continuing after compaction/resume and before declaring completion, reconstruct state from research/<slug>/.control/goal.md, control.md, progress-log.md, branch-map.md, source-ledger.md, claim-ledger.md, blockers.md, and completion-audit.md. Do not rely on memory or the last local subtask. If reconstruction or evidence verification fails, stop and report blocked.
```

Keep this invariant short. Do not put the entire skill inside the Goal. The Goal should preserve critical execution rules, while detailed procedures stay in the skill and reference files.
