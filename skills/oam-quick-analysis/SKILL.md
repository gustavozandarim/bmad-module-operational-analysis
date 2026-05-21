---
name: oam-quick-analysis
description: Single-pass operational analysis for a simple question — scope, analysis, light review, deliver — still fully Constitution-compliant. Use when the user has a quick one-page operational question, or when Owen routes a small job to the quick track.
---

# OAM Quick Analysis

## Overview

This is the Operational Analysis Method's scale-adaptive **Quick Flow**. A simple, one-page operational question shouldn't require the full four-phase lifecycle (Diana → Helena → Sofia). This workflow handles it end to end in a **single pass**: quick scope, quick analysis, a light integrity check, and a usable deliverable. It uses the profile and taxonomy from memory if they exist, and it **escalates** to the full lifecycle the moment the task proves bigger than it looked.

**Role:** Act as a capable operational analyst handling a focused question from start to finish in one pass — fast, but never sloppy. The full **Analytical Integrity Constitution** still applies: every claim labeled, every number sourced, no conclusion on insufficient data.

**Design rationale (do not "fix" this):** This workflow deliberately duplicates a *compressed* version of the full lifecycle. That overlap is intentional — the BMad "duplicate for coherence" pattern. The quick track must stay **self-contained** to remain coherent as a standalone workflow; do **not** refactor it to call the specialists' internals or DRY it against their references. Restating the essentials inline here is correct.

## Conventions

- Bare paths resolve from the skill root.
- `{project-root}`-prefixed paths resolve from the project working directory.

## Setup (on invocation)

1. Load config from `{project-root}/_bmad/config.yaml` (core at root + the `oam:` section) and `{project-root}/_bmad/config.user.yaml`, falling back to `{project-root}/_bmad/core/config.yaml` if the module isn't set up yet; resolve `{oam_output_folder}` (default `{output_folder}/oam/{profile}/{analysis}`) and `{default_profile}` (default `default`). Apply `{communication_language}` / `{document_output_language}`.
2. Load the **Analytical Integrity Constitution** from `{project-root}/_bmad/memory/oam/analytical-integrity-constitution.md` and the **module conventions** from `{project-root}/_bmad/memory/oam/module-conventions.md` — they bind even the quick track.
3. If memory exists, load the active profile's `org-profile.md` and `taxonomy.md` from `{project-root}/_bmad/memory/oam/profiles/{default_profile}/` — reuse the structure rather than re-deriving it.

## Inputs

- **question** (required) — the operational question to answer. If empty, **HALT** and ask.
- **docs** (any) — whatever source documents the user brings.
- **audience** (optional) — if set, the deliverable can be formatted for it at the end.

## Stage 1: Quick Scope

Frame just enough to answer well: the question, what's in and out, the documents in play, and the period. A mini-scope — not Helena's full framework. State it back in a sentence or two so the user can correct you.

## Stage 2: Quick Analysis

Read the documents and answer the question. Apply the Constitution as you go:

- **Source everything** — every figure traces to a document + location (Law 5).
- **Label every claim** `fact` / `inference` / `assumption` (Law 3).
- **Reconcile conflicts** — if two documents disagree, record both, flag it, and resolve before continuing; never silently pick one (Laws 1–2).
- **Refuse on insufficient data** — if the question can't be answered from what's available, hard-stop:
  ```
  ⛔ Cannot conclude: [the question]
  Missing: [what's absent]   Why it matters: [the dependency]   To unblock: [smallest next step]
  ```
- Use the taxonomy from memory if present, so a quick answer stays consistent with prior periods.

## Stage 3: Escalation Check

If the task proves bigger than a quick pass — many conflicting documents, a taxonomy that needs designing, or genuine diagnostic depth — **stop and escalate** to the full lifecycle (Diana for ingestion/reconciliation → Helena for the framework → Sofia for execution), via Owen or directly. Carry over what you've already produced. A quick track that pretends a complex job is simple is worse than no quick track.

## Stage 4: Quick Review

Run the findings through **`oam-adversarial-review`** with `depth: light` — the essential Constitution checks (traceability, no double-counting, labels, sufficiency). Address what it flags before delivering.

## Stage 5: Deliver

Write a concise deliverable bundle to `{oam_output_folder}`:

- **`report.md`** — a concise living-dialogue report. Use the standard per-section schema so it's interchangeable with a full report:
  ```markdown
  ## [Section / Answer Title]

  [Concise analytical content]

  **Confidence:** [Fact | Inference | Assumption]

  **Sources used:**
  - [document name, location]

  ---
  > **Review field**
  > [ ] Approved
  > [ ] Contested — reason: ___
  > **Additional context:** ___
  ---
  ```
- **`decision-log.md`** — any analytical decision made (allocation, exclusion, assumption, conflict resolution).
- **`actions.md`** — any follow-up the analysis surfaced.

If an **audience** was given, optionally invoke **`oam-format-delivery`** to render `report.md` for that reader (substance unchanged).

## HALT Conditions

- **HALT** if there is no question to answer.
- **HALT (escalate)** when the task is too complex for a single pass — hand off to the full lifecycle.
- **HALT** on insufficient data — state what's missing rather than guessing.
