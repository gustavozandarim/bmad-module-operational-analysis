---
name: oam-format-delivery
description: Re-render a finished operational analysis for a target audience (CFO, ops director, board) without altering substance. Use when the user requests OAM audience formatting, or when an agent invokes format-delivery at delivery.
---

# OAM Format & Delivery

## Overview

This workflow adapts a finished analysis to a **target audience** — a CFO, an operations director, a board — by changing *emphasis and presentation*, never substance. It is a cross-cutting capability any OAM agent can invoke (usually Sofia at delivery); the audience parameter comes from Owen's per-run prompt. The default OAM output is the living-dialogue `.md` itself, so audience formatting is an **optional layer** on top of it, not a replacement.

**Role:** Act as an editor who knows the reader. Same facts, same numbers, same conclusions — but led with what *this* reader needs first, in the register they expect.

**The substance lock (non-negotiable):** Never change a number, a confidence label, a source citation, or a conclusion. You reorder, re-weight, summarize, and re-voice — you do not re-analyze. Traceability and `fact`/`inference`/`assumption` labels survive every rendering intact. If the audience framing seems to require a different conclusion, that is a signal to send it back to Sofia, not to alter it here.

## Conventions

- Bare paths resolve from the skill root.
- `{project-root}`-prefixed paths resolve from the project working directory.

## Inputs

- **report** (required) — the finished living-dialogue report (`{oam_output_folder}/report.md`) or any approved analysis. If empty, **HALT** and ask for it.
- **audience** (required) — the target reader. CFO / operations director / board are worked examples below; any audience description works.
- **exec_summary** (optional) — whether the audience needs an executive summary led up front (Owen surfaces this per run).

## Execution

### Step 1: Read the analysis and the audience

Load the report. Identify what is fixed substance (numbers, labels, sources, conclusions) versus what is presentation (order, emphasis, length, vocabulary). Establish the audience lens:

- **CFO / finance:** lead with the bottom-line numbers, variance vs. budget/baseline, and allocation defensibility; precise, quantitative, assumptions explicit.
- **Operations director:** lead with operational drivers, what changed and why, and the actionable levers; concrete, process-oriented.
- **Board / executive:** lead with the decision and its few material facts; short, strategic, low jargon, exec summary first.
- **Other:** infer the lens from the audience description — what does this reader decide, and what do they need first?

### Step 2: Re-render for the audience

Re-order and re-weight so the audience's priorities come first; adjust length and vocabulary to the register. If `exec_summary` is set (or the audience is a board), lead with a tight executive summary drawn only from approved content. Keep every confidence label and source citation attached to its claim — emphasis changes, provenance does not.

### Step 3: Substance-preservation check

Before writing, verify against the source report: every number, label, source, and conclusion in your rendering matches the original exactly. If anything would have to change to fit the audience, stop and flag it rather than altering it.

### Step 4: Write the rendering

Write to `{oam_output_folder}/report-{audience}.md` (e.g. `report-cfo.md`), leaving the canonical `report.md` untouched. Return the path.

## HALT Conditions

- **HALT** if no report is provided.
- **HALT** if faithful formatting is impossible without changing substance — return to Sofia instead of bending a number or a conclusion to the audience.
