---
title: 'Operational Analysis Method (OAM) — Validate Module Report'
module_code: 'oam'
module_name: 'Operational Analysis Method'
module_version: '1.0.0'
status: 'PASS — ready for use'
date: '2026-05-21'
---

# Operational Analysis Method (OAM) — Validation Report

## Module Identity
- **Name:** Operational Analysis Method   ·   **Code:** `oam`   ·   **Version:** 1.0.0
- **Type:** Standalone BMad module (multi-skill, dedicated `oam-setup`).
- **Purpose:** Bring the BMad Method's rigor to operational/analytical work — turn scattered operational data into structured, defensible analyses through diagnosis → planning/structuring → execution, under an Analytical Integrity Constitution and an adversarial-review quality gate.
- **Shared law:** `_bmad/memory/oam/analytical-integrity-constitution.md` (Six Laws) + `module-conventions.md`, loaded by every skill on activation.

## Skills Inventory (8)
| Skill | Type | Role | Lint |
| --- | --- | --- | --- |
| `oam-agent-owen` 🧭 | agent | Orchestrator — mode detection (structuring/recurring, full/quick), routing, first-run onboarding, party-mode host | clean |
| `oam-agent-diana` 🔍 | agent | Diagnostic Analyst — incremental ingestion, cross-document reconciliation, evidence base; originates confidence chain | clean |
| `oam-agent-helena` 📐 | agent | Analytical Architect — scope/KPIs/hypotheses + taxonomy/allocation/workplan; owns durable `taxonomy.md` | clean |
| `oam-agent-sofia` ✍️ | agent | Senior Analyst — section execution, confidence labeling, adversarial review, living-dialogue report, post-delivery distillation | clean |
| `oam-adversarial-review` | workflow | Quality gate — general cynical pass (→ core `bmad-review-adversarial-general`) + Constitution checklist; `depth: full|light` | clean |
| `oam-format-delivery` | workflow | Audience re-render (CFO/ops/board) with a hard substance-lock | clean |
| `oam-quick-analysis` | workflow | Scale-adaptive single-pass flow; escalates to full lifecycle; self-contained by design | clean |
| `oam-setup` | workflow | Installer — config, help registration, memory scaffold, law install, Owen onboarding handoff | template-style findings only* |

\* `oam-setup` carries 12 path-lint findings inherent to the official BMad setup-skill template (`./scripts/`+`./assets/` style + one prose line) and the law-doc illustration paths — not defects.

## Structural Validation (`validate-module.py`)
- **Result: PASS, 0 findings.** `setup_skill=oam-setup`, `module_code=oam`, 8 CSV capability entries for 8 skills, 7 skill folders + setup.
- **Fixed during VM:** `module-help.csv` header used the outdated `preceded-by`/`followed-by`; corrected to `after`/`before` (required by both `merge-help-csv.py` and the validator).
- **Roster:** no drift — all 4 agents' `module.yaml` entries match their `customize.toml`. **Menu codes unique:** SU/OW/DI/HE/SO/QA/AR/FD. **Artifact chain encoded:** `diana:ingest → helena:design → sofia:execute`.
- **Known validator quirk (not a defect):** `validate-module.py` displays `module_name: "Sofia"` — its naive parser grabs the last nested `name:`. Real `yaml.safe_load` (used by `merge-config.py` and the installer) reads "Operational Analysis Method" correctly.

## Dogfood — Flagship Use Case (cost structuring)
A synthetic ACME dataset (cost-master CSV + March ERP export + lease excerpt) with a deliberate **conflict**, **gap**, and **misallocation** was driven end-to-end.

**Structuring run (Q1):** Diana hard-stopped on a 3-source rent conflict → reconciled to the lease; Helena built the taxonomy and reclassified the trade show (Production → Sales & Marketing); Sofia captured 914,400, issued a Law-4 stop on the unsourced IT line ("captured, not complete"), and assembled the living-dialogue report. Adversarial review: **PASS WITH FIXES** (caught the IT completeness gap + the unexplained ERP rent).

**Recurring run (Q2):** Diana light-ingested one document (IT gap closed); Helena reused the taxonomy unchanged; Sofia went straight to variance vs. the Q1 baseline and fired the "explain the variance" prompt on Raw Materials +22% → labeled volume-driven (`Inference`, pending Operations confirmation). Adversarial review: **PASS**. The second run was **materially lighter** — the two-mode design validated.

### Constitution checklist — all six laws fired
| Law | Evidence in dogfood |
| --- | --- |
| 1 — never accept questionable data | Trade-show misclassification flagged, not silently fixed |
| 2 — reconcile conflicts first | Hard-stop on the 3-source rent conflict |
| 3 — label fact/inference/assumption | Extraction labels → claim labels, verified consistent |
| 4 — refuse on insufficient data | IT gap stop; total reported as "captured, not complete" |
| 5 — trace every number | Every figure cites a source; rent reconciled to the lease |
| 6 — log every decision | Decision log D1–D6 travelled with the deliverable |

### Adversarial-review behavior
The gate **found real issues on both clean runs** (IT completeness, unexplained ERP rent, single-sourced months) — confirming the "reviewer must find issues" mandate works.

## Bugs Caught & Fixed During Build/Validation
1. **Resolver emoji crash (Windows cp1252):** `resolve_customization.py` threw `UnicodeEncodeError` writing emoji icons → added a UTF-8 stdout reconfigure guard. (Core script — re-apply after reinstall.)
2. **Constitution deletable by cleanup:** the Constitution sat in `_bmad/oam/`, which `cleanup-legacy.py` removes → relocated to the persistent `_bmad/memory/oam/`, shipped via `oam-setup/assets`.
3. **Config path:** consolidated agents to `_bmad/config.yaml` (+ `config.user.yaml`) instead of a per-module file.
4. **CSV header:** `preceded-by`/`followed-by` → `after`/`before`.

## Overall Assessment
**PASS — ready for use.** All 7 authored skills are lint-clean; the module validates structurally; the flagship use case works end-to-end in both modes with full Constitution conformance. Install in any project by running `oam-setup`.
