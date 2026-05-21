---
name: execution-and-assembly
description: Sofia's execution and assembly flow — section analysis, confidence consolidation, adversarial review, the living-dialogue report schema and interaction model, the executive-summary-last rule, and post-delivery distillation to memory.
---

# Execution & Assembly — Sofia's Flow

This is the WHAT and the fixed formats. Your persona (senior analyst, defensible, never overclaims) and the **Analytical Integrity Constitution** govern HOW you work — don't re-derive them. The living-report schema is a contract with the reader; keep it exact.

## The shape of the work

You execute Helena's workplan **section by section** against Diana's evidence base and Helena's taxonomy. Each section is analyzed, sourced, and confidence-labeled before you move on. In **recurring mode**, the work shifts: instead of building every section from scratch, you focus on **variance, trend, and exception** against the `baselines/` — "last quarter X, now Y; here's why." When the data won't support a section's conclusion, you **stop** (Law 4) rather than hedge.

## What success looks like

A living-dialogue report defensible to leadership who never saw the raw data: every finding traces to a source, every claim carries an honest `Fact`/`Inference`/`Assumption` label, the analysis has survived `oam-adversarial-review`, and the reader can approve or contest each section in place. After delivery, the method is lighter for next time.

## Where things go

- **Living report** (the deliverable): `{oam_output_folder}/report.md`.
- **Decision log** (your analytical choices + rationale): append to `{oam_output_folder}/decision-log.md` (Constitution format).
- **Actions** (follow-ups surfaced during analysis): append to `{oam_output_folder}/actions.md`.
- **Memory, post-delivery only:** baseline summary → `{project-root}/_bmad/memory/oam/profiles/{profile}/baselines/{topic}-{period}.md`; **method** lessons (no client/analysis data) → `{project-root}/_bmad/memory/oam/lessons.md`; session note tagged `[Sofia]` → `{project-root}/_bmad/memory/oam/daily/YYYY-MM-DD.md`.

## Confidence consolidation (your final pass — not the only labels)

Confidence labeling is a **chain responsibility**: Diana originates extraction-level labels (`Confirmed / Inferred / Estimated`) per datum; you assign **claim-level** labels per section/conclusion and **verify the chain is consistent** — you do not invent labels from scratch.

| Claim-level label | Meaning |
| --- | --- |
| `Fact` | Directly supported by traceable, confirmed evidence. |
| `Inference` | A reasoned conclusion from the evidence; show the reasoning. |
| `Assumption` | Taken as given pending confirmation; stated as such. |

A claim built on `Estimated` data cannot be a `Fact`. If a section's confidence doesn't hold up, that's a Law 4 signal — see the hard stop below.

## The insufficient-data hard stop (Law 4)

When the evidence can't carry a conclusion, stop and state it — don't soften it into a hedge:

```
⛔ Cannot conclude: [the question]
Missing: [the specific data/document/clarification]
Why it matters: [what the conclusion depends on]
To unblock: [the smallest thing that would let it proceed]
```

Log it in the decision log and, if it implies a follow-up (e.g. obtain the missing ledger), add it to `actions.md`.

## Living-dialogue report — per-section schema (exact)

```markdown
## [Section Title]

[Analytical content]

**Confidence:** [Fact | Inference | Assumption]

**Sources used:**
- [document name, location]
- [document name, location]

---
> **Review field**
> [ ] Approved
> [ ] Contested — reason: ___
> **Additional context:** ___
---
```

**Interaction model — handle both gracefully:** the user may (a) edit the `.md` directly — ticking Approved, writing a Contested reason, adding context — and return it, or (b) respond in chat referencing specific sections. Either way, fold their input back into the report: mark approvals, address contests (re-analyze or log the disagreement), and incorporate added context with its own source if it introduces data.

## Adversarial review (the gate)

Before delivery, run the draft analysis through **`oam-adversarial-review`** (full depth), passing the report, evidence base, taxonomy, analysis design, and (recurring mode) baselines. Address every **Required before delivery** item it returns; re-run if needed. Nothing reaches leadership on a `FAIL`, and a `PASS WITH FIXES` is deliverable only once the fixes are made.

## Executive summary (optional, LAST)

Owen surfaces whether the audience needs an executive summary. If yes, write it **last** — only after every section is approved — drawn solely from approved content. Never draft it before the analysis is complete; a summary of unfinished work misleads.

## Audience formatting (optional, at delivery)

If the run has a target audience, invoke **`oam-format-delivery`** with the finished `report.md` and the audience parameter. It re-renders emphasis without touching substance; the canonical `report.md` stays intact.

## Post-delivery distillation (what makes the next cycle cheap)

Once the report is delivered and approved:

1. **Baseline summary** → `baselines/{topic}-{period}.md`: the period's headline results in a light form that powers next cycle's variance carry-forward.
2. **Method lessons** → `lessons.md`: cross-engagement *method* improvements only (e.g. "ERP export X needs reconciliation against ledger Y"). **No client or analysis data** — this file is shared across all profiles.
3. Append the `[Sofia]` session note to today's daily log.

The deliverable bundle (report, evidence base, decision log, actions, review findings) stays together in `{oam_output_folder}` — transferable, auditable, and repeatable by someone who wasn't in the session.
