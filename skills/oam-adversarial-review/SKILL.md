---
name: oam-adversarial-review
description: Adversarial quality gate for operational analyses — a general cynical pass then the Analytical Integrity Constitution checklist. Use when the user requests an OAM adversarial review or quality gate, or when Sofia or oam-quick-analysis invoke the review.
---

# OAM Adversarial Review

## Overview

This is the Operational Analysis Method's signature quality gate. It stress-tests an analytical artifact against the **Analytical Integrity Constitution** before it reaches leadership. It works in two layers: first a broad cynical pass (delegated to core `bmad-review-adversarial-general`), then an OAM-specific **Constitution checklist** that verifies the domain integrity leadership actually challenges — allocation correctness, no double-counting, variance explained, source traceability, and classification consistency across periods.

The output is a findings report with a pass/fail verdict per check and the required fix for each failure. It is invoked by **Sofia** before delivery and by **oam-quick-analysis** (in a lighter form); it also runs **standalone** on any analysis.

**Role:** Act as a cynical, exacting reviewer with zero patience for sloppy work. The analysis was produced by someone who wants it to pass; your job is to find where it does not. Be skeptical of every number and every conclusion. **"Looks fine" is not an acceptable result of a review** — if you find nothing, you have not looked hard enough.

**Design rationale:** The review is *layered* — the core general pass surfaces broad weaknesses; the OAM checklist enforces the domain logic the Constitution codifies. The domain logic lives here, not in core, so the module's quality bar does not depend on core for its differentiator. The reviewer's mandate to *find issues* is deliberate (mirrors BMad's adversarial review) — it is the antidote to a confident-but-wrong analysis.

## Conventions

- Bare paths resolve from the skill root.
- `{project-root}`-prefixed paths resolve from the project working directory.
- `{skill-name}` resolves to the skill directory's basename.

## Inputs

- **analysis** (required) — the executed analysis or draft report to review. If invoked standalone, the user provides it or names the deliverable folder.
- **evidence base** — `{oam_output_folder}/evidence-base.md`, for source-traceability checks.
- **taxonomy** — the active profile's `taxonomy.md`, for allocation and classification-consistency checks.
- **analysis design** — `{oam_output_folder}/analysis-design.md`, for scope and double-counting context.
- **baselines** (recurring mode) — prior `baselines/` results, for the variance-explained check.
- **depth** (optional) — `full` (default) or `light`. `light` is the compressed pass used by `oam-quick-analysis`: skip the broad general pass and run the essential Constitution checks only.

## Execution

### Step 1: Receive & Locate Inputs

Load the analysis to review. If it's empty or unreadable, **HALT** and ask for it. Load whatever of the evidence base, taxonomy, analysis design, and baselines are available — note any that are missing, because a missing input weakens a specific check (e.g. no evidence base → traceability cannot be fully verified).

### Step 2: Layer 1 — General Cynical Pass

*(Skip in `depth: light`.)* Invoke **`bmad-review-adversarial-general`** with the analysis as `content` and, as `also_consider`, the Constitution's six laws. Capture its findings. If it returns zero findings, that is suspicious — re-run or sharpen the input.

### Step 3: Layer 2 — Constitution Checklist

Evaluate each check below against the analysis and its supporting artifacts. For each: a **Pass/Fail** verdict, the **evidence** for the verdict (cite the section/number), and — for any Fail — the **required fix**. Be specific; a vague pass is a fail.

| # | Check | What "Fail" looks like |
| - | ----- | ---------------------- |
| 1 | **Allocation correctness** | An allocation basis is unstated, or applied inconsistently with `taxonomy.md`. |
| 2 | **No double-counting** | A datum is counted in two categories, or a total exceeds the sum of its sourced parts. |
| 3 | **Variance explained** | A material period-over-period movement has no stated cause (recurring mode especially). |
| 4 | **Source traceability** | A reported number has no traceable source document + location (Law 5). |
| 5 | **Classification consistency across periods** | Criteria changed without a logged decision + taxonomy Change Log entry (Law 6) — periods no longer comparable. |

Then verify Constitution conformance:

- **Labels (Law 3):** every claim carries `fact`/`inference`/`assumption`; extraction-level labels are consistent with claim-level ones.
- **Reconciliation (Laws 1–2):** no unresolved conflict was silently papered over.
- **Sufficiency (Law 4):** no conclusion rests on insufficient data; where data is short, the analysis hard-stops rather than hedging.

### Step 4: Assemble the Findings Report

Write the findings to `{oam_output_folder}/adversarial-review.md` using the template below. Lead with the verdict so the reader knows immediately whether the analysis is deliverable.

```markdown
# Adversarial Review — {analysis}

**Verdict:** [PASS | PASS WITH FIXES | FAIL]   ·   **Depth:** [full | light]   ·   **Date:** [YYYY-MM-DD]

## Constitution Checklist
| # | Check | Verdict | Evidence | Required fix |
| - | ----- | ------- | -------- | ------------ |
| 1 | Allocation correctness | Pass/Fail | ... | ... |
| 2 | No double-counting | ... | ... | ... |
| 3 | Variance explained | ... | ... | ... |
| 4 | Source traceability | ... | ... | ... |
| 5 | Classification consistency | ... | ... | ... |

## Constitution conformance
- Labels (Law 3): ...
- Reconciliation (Laws 1–2): ...
- Sufficiency (Law 4): ...

## General-pass findings (Layer 1)
- [finding] — [why it matters] — [fix]

## Required before delivery
- [the must-fix items, ordered by severity]
```

### Step 5: Return

Return the verdict and the path to the findings report. If invoked by Sofia, she addresses the required fixes and may re-run. **PASS WITH FIXES** means deliverable only once the listed fixes are made; **FAIL** means it is not.

## HALT Conditions

- **HALT** if the analysis is empty or unreadable — ask for it.
- **HALT** if the general pass (full depth) returns zero findings — re-analyze; that result is not credible.
