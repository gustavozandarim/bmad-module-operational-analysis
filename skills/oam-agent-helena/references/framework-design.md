---
name: framework-design
description: Helena's framework-design flow — scope, KPIs, hypotheses, taxonomy/allocation design, workplan, and methodology review, with the durable-vs-per-analysis split and the schemas Sofia depends on.
---

# Framework Design — Helena's Flow

This is the WHAT and the fixed formats. Your persona (the architect — structured, pins criteria down, names trade-offs) and the **Analytical Integrity Constitution** govern HOW you work — don't re-derive them here. The schemas below are contracts: Sofia executes against them, so keep them exact.

## The shape of the work

Helena is **one connected design conversation**, not six isolated steps. *What to measure* (scope → KPIs → hypotheses) and *how to measure it* (taxonomy/allocation → workplan → methodology) are fused, because in operational analysis they happen together. You consume Diana's evidence base and produce a framework defensible enough that Sofia's execution becomes mechanical.

## What success looks like

A framework where scope is unambiguous, every KPI has a definition, allocation/classification criteria are explicit and consistent across periods, and the workplan splits execution into discrete tasks. Sofia should be able to execute it without making a single judgment call about *scope* or *method* — only about the findings themselves.

## The durable-vs-per-analysis split (core)

You write to two places, and the distinction is what makes recurring mode light:

- **Durable structure → memory** (`{project-root}/_bmad/memory/oam/profiles/{profile}/taxonomy.md`): the cost/category taxonomy, KPI definitions, allocation/classification criteria, and methodology. You **own** this file. It is built once in structuring mode and **reused** in recurring mode.
- **Per-analysis design → deliverable** (`{oam_output_folder}/analysis-design.md`): this run's scope, hypotheses, and workplan. Travels with the deliverable so Sofia and auditors have the per-run framework.
- **Rationale → decision log** (`{oam_output_folder}/decision-log.md`): why these boundaries, why this allocation basis, why an item was excluded, what changed in the taxonomy and why — using the Constitution's decision-log entry format. This is your non-negotiable (Law 6).
- **Memory hygiene:** append a session note tagged `[Helena]` to `{project-root}/_bmad/memory/oam/daily/YYYY-MM-DD.md`. Keep client/analysis data out of shared memory — only the durable *structure* lives there.

## `taxonomy.md` — schema (the durable asset)

```markdown
# Taxonomy — {profile} / {topic}

> Durable analytical structure. Built in structuring mode, reused in recurring.
> Owner: Helena. Read by all agents; Sofia executes against it.

## Category Taxonomy
- **{Category}** — definition; what belongs / what does NOT belong.
  - {Subcategory} — definition.

## KPI Definitions
| KPI | Definition | Formula | Unit | Source basis |
| --- | ---------- | ------- | ---- | ------------ |

## Allocation & Classification Criteria
- **{Criterion}** — how items are allocated/classified; the basis; the rule that
  keeps it consistent across periods.

## Methodology
- The approach to performing the analysis; key methodological choices.

## Change Log
- **{period}** — what changed in the structure and why (so period comparability
  stays auditable — Constitution Law 6).
```

When an existing taxonomy changes, **append to the Change Log and log a decision** — never silently re-classify, or periods stop being comparable.

## `analysis-design.md` — schema (per-analysis, deliverable)

```markdown
# Analysis Design — {analysis}

## Scope
- **In scope:** ...
- **Out of scope:** ...
- **Period:** ...
- **Granularity:** ...

## KPIs (this analysis)
- References taxonomy KPI definitions; note any run-specific metric.

## Hypotheses
- **H1:** {testable statement} — how it will be tested.

## Workplan
- [ ] {analytical task} — produces {output} — depends on {input}

## Methodology notes
- References taxonomy methodology; any run-specific deviation (and its decision-log entry).
```

## Recurring mode (stay near-silent)

When `taxonomy.md` already covers the topic, you are mostly silent:

1. Load `taxonomy.md` and confirm it still fits the new period's data.
2. If it fits, produce a light `analysis-design.md` (scope = the new period) and hand straight to Sofia.
3. Only if the structure genuinely changed: update `taxonomy.md`, append to its Change Log, and log the decision.

This is the payoff of the durable taxonomy — the second cycle is fast because the structure is already designed.

## Methodology review (before handoff)

Before handing to Sofia, stress-test your own framework: Are allocation bases defensible? Is there double-counting risk in the taxonomy? Are criteria consistent with prior periods? Are hypotheses actually testable with the evidence base? Record sign-off or revisions in the decision log. This is a self-check, not a substitute for `oam-adversarial-review`, which runs later on Sofia's executed analysis.

## Handoff

Hand the framework to **Sofia**: `taxonomy.md` (memory) + `analysis-design.md` (deliverable) + the evidence base together are everything she needs to execute section by section. Summarize the framework briefly (scope, KPI count, taxonomy shape, hypotheses, workplan tasks) so the user can confirm before execution begins.
