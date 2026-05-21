# Operational Analysis Method (OAM)

![version](https://img.shields.io/badge/version-1.0.1-blue)
![BMad module](https://img.shields.io/badge/BMad-module-6E40C9)
![license](https://img.shields.io/badge/license-MIT-green)
![status](https://img.shields.io/badge/validated-VM%20PASS-brightgreen)

> A standalone [BMad](https://github.com/bmad-code-org/BMAD-METHOD) module that brings the BMad Method's rigor to **operational and analytical work** — turning scattered operational data into structured, defensible analyses through diagnosis, planning, structuring, and execution, under an enforced integrity constitution and an adversarial-review quality gate.

---

## What is OAM?

BMad's core ideas — progressive context building, specialized agents per domain, structured workflows with concrete artifacts, and adversarial review — are **domain-agnostic**. OAM applies them to *operational* analysis: cost structuring, KPI/OEE reviews, variance investigations, accounting report assembly, and similar work.

It solves the "structure and coordination" problem. Instead of a blank prompt (shallow, inconsistent output) or a rigid template (can't adapt), OAM guides you from a vague operational question through a rigorous process that produces a high-quality, **defensible** document — one every number of which traces to a source, every claim of which carries its confidence, and which a colleague who wasn't in the room can audit or re-run.

### The two-mode spine

OAM runs in one of two modes, detected automatically from what's already in memory:

- **Structuring mode** — the first, high-effort pass. Builds the cost taxonomy / KPI definitions / allocation criteria and a baseline *into memory*. This is where the durable structure is created.
- **Recurring mode** — the light, fast pass. *Reuses* the structure from memory and focuses only on **variance, trends, and exceptions** vs. the baseline ("last quarter X, now Y — explain the difference"). The second run is materially cheaper than the first.

### The Analytical Integrity Constitution

Six laws every agent and workflow obeys — the module's signature quality bar. When integrity and convenience conflict, integrity wins.

1. **Never silently accept questionable or conflicting data** — surface it and ask.
2. **Reconcile conflicts across documents before proceeding** — record all values, flag, and hard-stop.
3. **Label every claim** `fact` / `inference` / `assumption`.
4. **Refuse to conclude on insufficient data** — a hard stop stating what's missing and why, not a hedge.
5. **Trace every number to a source** — document + location.
6. **Log every analytical decision** — the decision log travels with the deliverable.

A dedicated workflow (`oam-adversarial-review`) enforces these as a checklist gate before anything reaches leadership.

### Who it's for

Business analysts and operational consultants · financial controllers and management accountants · strategy & planning professionals · risk/compliance/audit professionals · operations managers facing complex decisions or recurring reporting cycles.

---

## Agents

| Agent | Role | What it does |
| ----- | ---- | ------------ |
| **Owen** 🧭 | Orchestrator | The front door. Detects mode (structuring/recurring, full/quick), routes to the right specialist, runs first-run onboarding, and hosts party-mode debates. |
| **Diana** 🔍 | Diagnostic Analyst | Incremental, conversational ingestion of messy mixed-format documents; automatic cross-document reconciliation; builds the fully-sourced **evidence base**. Originates the confidence chain. |
| **Helena** 📐 | Analytical Architect | Fuses planning (scope, KPIs, hypotheses) and structuring (cost taxonomy, allocation methodology, workplan). Owns the durable `taxonomy.md`. |
| **Sofia** ✍️ | Senior Analyst | Executes the analysis section by section, consolidates confidence labels, runs adversarial review, assembles the living-dialogue report, and distills lessons back to memory. |

---

## Workflows

| Workflow | Purpose |
| -------- | ------- |
| **oam-adversarial-review** | The Constitution quality gate — a general cynical pass (delegated to core `bmad-review-adversarial-general`) plus the OAM checklist: allocation correctness, no double-counting, variance explained, source traceability, classification consistency. `depth: full | light`. |
| **oam-format-delivery** | Re-renders a finished analysis for a target audience (CFO, ops director, board) **without altering substance** — emphasis and presentation only; numbers, labels, and conclusions are locked. |
| **oam-quick-analysis** | A scale-adaptive, self-contained single-pass track (scope → analysis → light review → deliver) for simple questions, still fully Constitution-compliant. Escalates to the full lifecycle when a task proves complex. |

---

## Memory & artifacts

**Profile-based context.** OAM keeps a per-deployment context model under `_bmad/memory/oam/`:

- **Internal** → one profile (never re-explain your org).
- **Consultant** → one profile per client, fully isolated; *method* lessons accumulate across engagements without leaking client data.
- **Community** → ships empty; Owen's first-run onboarding builds the first profile.

**The memory-vs-deliverable split** is the core principle:

- **Lives in memory** (persists, makes recurring runs light): the org profile, the taxonomy / allocation criteria, historical baselines, and method lessons.
- **Travels with the deliverable** (per analysis, in your output folder): the evidence base, the analysis design, the decision log, the adversarial-review findings, the report, and `actions.md`.

**The deliverable bundle** (one folder per analysis, auditable and transferable):

```
evidence-base.md       # every datum + source + location + confidence (Diana)
analysis-design.md     # scope + KPIs + hypotheses + workplan (Helena)
decision-log.md        # every analytical decision + rationale (all agents)
adversarial-review.md  # the quality-gate findings
report.md              # the living-dialogue report with response fields (Sofia)
actions.md             # any follow-up the analysis surfaced
```

The signature output is a **living-dialogue report**: each section carries its confidence label, its sources, and a *response field* the reader can use to approve, contest, or add context — a dialogue, not a verdict.

---

## Installation

OAM installs as a custom BMad module alongside BMM. Replace `gustavozandarim` with your GitHub account/org:

```bash
npx bmad-method install \
  --modules bmm \
  --custom-source https://github.com/gustavozandarim/bmad-module-operational-analysis \
  --tools claude-code \
  --yes
```

See **[INSTALL.md](./INSTALL.md)** for prerequisites, the Windows post-install fix, and updating.

---

## Quick start

1. **Run setup** (first time in a project): invoke `oam-setup`. It scaffolds the shared memory tree, installs the Constitution, collects config (`oam_output_folder`, `default_profile`), registers help, and — on a fresh, profile-less install — hands you to Owen for onboarding.
2. **Cold-start onboarding:** with zero profiles, **Owen** asks a few questions about your organization (vocabulary, priorities, data sources) and builds your first profile + a seed taxonomy.
3. **Start an analysis:** talk to **Owen** — *"Owen, I need to structure our Q1 operating costs"* — and he detects the mode and routes you.
4. **Or summon a specialist directly** (power users): Diana to ingest documents, Helena to design the framework, Sofia to execute, or run `oam-quick-analysis` for a fast one-pass answer.

---

## ⚠️ Important — resolver UTF-8 fix (Windows)

OAM agents have emoji icons (🧭 🔍 📐 ✍️). The BMad resolver script `_bmad/scripts/resolve_customization.py` writes JSON to stdout with `ensure_ascii=False`; on a **Windows console using the legacy cp1252 encoding**, that crashes (`UnicodeEncodeError`) when it hits an emoji. Agents have a built-in fallback (they resolve their config manually if the script fails), so activation still works — but you'll see a traceback on every launch until you apply the fix.

**Fix (recommended)** — add a self-contained UTF-8 guard at the **top of `main()`** in `_bmad/scripts/resolve_customization.py`, before any stdout write. This needs no environment variable and no per-launch improvisation:

```python
def main():
    # Windows consoles default to cp1252, which cannot encode emoji icons.
    # Reconfigure stdout to UTF-8 at startup so every write is safe.
    if sys.platform == "win32":
        sys.stdout.reconfigure(encoding="utf-8")
    ...
```

**Alternative (no code change)** — set the environment variable before running:

```bash
# PowerShell
$env:PYTHONIOENCODING = "utf-8"
# bash
export PYTHONIOENCODING=utf-8
```

> Note: `resolve_customization.py` is a **core BMad script**, not part of this module — it is reinstalled by the BMad installer, so re-apply the code fix after any reinstall (or use the env-var approach). This fix has been reported upstream to BMAD-METHOD.

---

## Built with

- **[BMad Method](https://github.com/bmad-code-org/BMAD-METHOD)** v6.7.1
- **BMad Builder (BMB)** v1.8.1 — agents, workflows, and module scaffolding
- Reuses core BMad skills only: `bmad-party-mode`, `bmad-review-adversarial-general`.

This module was ideated, built, packaged (Create Module), validated (Validate Module — PASS, 0 findings), and dogfooded end-to-end (cost-structuring flagship, structuring + recurring) entirely within the BMad Builder toolchain.

## License

[MIT](./LICENSE) — same as the BMad ecosystem.
