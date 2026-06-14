# Obsidian Output Schema for Rabbit-Hole Research

Use this when the user wants durable notes, a database, or “the more useful material the better.” Keep more evidence, not more noise.

## Folder structure

```text
Research/<slug>/
  00-index.md
  01-research-map.md
  02-synthesis.md
  03-open-questions.md
  04-source-ledger.md
  sources/
  claims/
  concepts/
  decisions/
  parked/
  .control/
```

`.control/` is scratch state. Do not treat it as a polished deliverable. Remove it or summarize it when the user wants a clean vault.

## Note types

Use these types consistently:

- `index` — navigation hub.
- `map` — branch graph and status.
- `synthesis` — current integrated answer.
- `source` — one source, repo, paper, issue, doc page, video, or dataset.
- `claim` — one material claim with evidence.
- `concept` — reusable term, method, framework, or definition.
- `decision` — chosen recommendation with alternatives.
- `parked` — tangent saved because it may matter later but not now.

## Base frontmatter

```yaml
---
title: ""
type: source
status: raw
topic: ""
branch: ""
created: 2026-01-01
updated: 2026-01-01
confidence: medium
relevance: medium
tags:
  - research/rabbit-hole
---
```

Use `status` values: `raw`, `triaged`, `synthesized`, `parked`, `rejected`, `final`.

Use `confidence` values: `low`, `medium`, `high`.

Use `relevance` values: `low`, `medium`, `high`, `critical`.

## Source note template

```markdown
---
title: "<source title>"
type: source
status: triaged
topic: "<topic>"
branch: "<branch>"
source_type: official-docs
url: "<url or local path>"
author_org: "<author/org>"
published: 2026-01-01
accessed: 2026-01-01
confidence: high
relevance: critical
tags:
  - research/rabbit-hole
  - evidence/primary
---

# <source title>

## Why this source matters

## Extracted claims

- [[claims/<claim-slug>]] — <one-line claim>

## Limitations / bias

## Follow-up terms
```

## Claim note template

```markdown
---
title: "<claim>"
type: claim
status: synthesized
topic: "<topic>"
branch: "<branch>"
sources:
  - "[[sources/<source-slug>]]"
confidence: high
relevance: critical
tags:
  - research/rabbit-hole
  - claim
---

# <claim>

## Claim

## Evidence

## Supports

## Contradicts

## Use in synthesis
```


## Index template

```markdown
---
title: "<topic> research index"
type: index
status: final
topic: "<topic>"
tags:
  - research/rabbit-hole
---

# <topic> research index

## Current synthesis

[[02-synthesis]]

## Research map

[[01-research-map]]

## Critical claims

```dataview
TABLE confidence, relevance, branch
FROM "Research/<slug>/claims"
WHERE relevance = "critical"
SORT confidence DESC
```

## Sources by branch

```dataview
TABLE source_type, confidence, relevance, accessed
FROM "Research/<slug>/sources"
SORT branch ASC, relevance DESC
```

## Parked tangents

```dataview
TABLE branch, relevance
FROM "Research/<slug>/parked"
```
```

## Keep vs discard rules

Keep a note when it has one of these: a material sourced claim, contradiction, real-world failure mode, official behavior, method, term needed for future search, decision rationale, high-quality implementation example, or a parked tangent with plausible future value.

Discard or reject when it is a duplicate with no new angle, SEO filler, unsupported opinion, stale content superseded by a primary source, a trivial definition, or unrelated to any branch.

## Linking rules

- Every claim links to at least one source.
- Every source links to extracted claims or says why it was rejected.
- Every synthesis section links to critical claims.
- Every parked tangent explains what would make it worth reopening.
- Use quoted `"[[Note]]"` links in YAML properties when needed.
