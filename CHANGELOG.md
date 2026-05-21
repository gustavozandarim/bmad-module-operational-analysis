# Changelog

All notable changes to the Operational Analysis Method (OAM) module are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-21

### Added

- **Initial release of the Operational Analysis Method (OAM)** — a standalone, community-distributable BMad module.
- **8 skills:**
  - `oam-agent-owen` 🧭 — Orchestrator (mode detection, routing, first-run onboarding, party-mode host)
  - `oam-agent-diana` 🔍 — Diagnostic Analyst (incremental ingestion, cross-document reconciliation, evidence base)
  - `oam-agent-helena` 📐 — Analytical Architect (scope, KPIs, hypotheses, taxonomy, allocation methodology, workplan)
  - `oam-agent-sofia` ✍️ — Senior Analyst (section execution, confidence labeling, report assembly, distillation)
  - `oam-adversarial-review` — Constitution checklist quality gate (general pass + domain checks)
  - `oam-format-delivery` — audience rendering without altering substance
  - `oam-quick-analysis` — self-contained single-pass track that escalates when needed
  - `oam-setup` — installer (config, help registration, memory scaffold, law install, onboarding handoff)
- **Analytical Integrity Constitution** — the Six Laws, loaded by every skill on activation.
- **Profile-based memory system** supporting internal (1 profile), consultant (many, isolated), and community (0 → onboarding) deployments.
- **Two-mode spine:** structuring (builds the taxonomy + baseline) and recurring (light variance/trend/exception pass).
- **Deliverable bundle:** evidence base, analysis design, decision log, adversarial-review findings, living-dialogue report, and `actions.md`.
- **Validated** with Validate Module (VM): PASS, 0 structural findings.
- **Dogfooded** on the cost-structuring flagship use case in both structuring and recurring modes, with all six Constitution laws verified end-to-end.

### Fixed (during build)

- **Resolver UTF-8 crash on Windows (cp1252):** `_bmad/scripts/resolve_customization.py` threw `UnicodeEncodeError` writing emoji agent icons to stdout. Added a UTF-8 stdout-reconfigure guard. (Core script — see README for the post-install fix.)
- **Constitution location:** relocated the Constitution and module conventions from `_bmad/oam/` to `_bmad/memory/oam/` so the installer's `oam-setup` cleanup (which removes the per-module package dir `_bmad/{code}/`) cannot delete them.
- **Config path:** consolidated all agents and workflows to load `_bmad/config.yaml` (+ `_bmad/config.user.yaml`, root core + `oam:` section) instead of a per-module config file.
- **Help registry:** corrected the `module-help.csv` header columns to `after` / `before` (the schema required by both `merge-help-csv.py` and the module validator).

[1.0.0]: https://github.com/gustavozandarim/bmad-module-operational-analysis/releases/tag/v1.0.0
