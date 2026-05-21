---
title: 'Operational Intelligence Module — Plan'
status: 'complete'
module_name: 'Operational Analysis Method'
module_code: 'oam'
module_description: 'A standalone BMad module that brings the BMad Method''s rigor to operational and analytical work — turning scattered operational data into structured, defensible analyses and documents through progressive diagnosis, planning, structuring, and execution with specialized agents and adversarial review.'
architecture: 'Multiple named agents (orchestrator + 3 specialists: Diana, Helena, Sofia) over a single shared module memory; shared adversarial-review and delivery-formatting capabilities; orchestrator-first and direct-specialist access patterns; two operating modes (structuring vs recurring).'
standalone: true
expands_module: ''
skills_planned: ['oam-agent-owen', 'oam-agent-diana', 'oam-agent-helena', 'oam-agent-sofia', 'oam-adversarial-review', 'oam-format-delivery', 'oam-quick-analysis', 'oam-setup']
config_variables: ['oam_output_folder', 'default_profile']
created: '2026-05-21'
updated: '2026-05-21'
---

# Operational Intelligence Module — Plan

## Vision

An adaptation of the BMad Method for **non-software domains** — specifically, producing structured operational documents and analyses in a business or organizational context.

**Core insight:** BMad's fundamental principles — progressive context building, specialized agents per domain, structured workflows with concrete artifacts, and adversarial review — are domain-agnostic. They work for operational/analytical work as well as for software development.

**Problem it solves:** Today knowledge workers using AI for operational work get either (a) a blank prompt with no structure → inconsistent, shallow outputs, or (b) a rigid template-filler that can't adapt to complexity. This module solves the "coordination and structure" problem: guide the user from a vague operational question through a rigorous process that produces high-quality, defensible documents — just as BMad guides a developer from idea to production-ready code.

**Target users:**
- Business analysts and operational consultants
- Strategy and planning professionals
- Financial controllers and management accountants
- Risk, compliance, and audit professionals
- Operations managers facing complex decisions or recurring reporting cycles

## Architecture

**Decision:** Multiple named agents over a **single shared module memory**, fronted by an **orchestrator** with **direct specialist access** also available.

**Rationale:**
- **Party mode requires distinct personas** — a single agent can't convene a roundtable with itself. This alone settles single-vs-multi.
- **The expertise is genuinely different** — wrangling a messy PDF export (Diana) is a different skill from designing a cost taxonomy (Helena) or stress-testing a conclusion under the Constitution (Sofia).
- **The agents are tightly coupled** — they all work the same analysis and share taxonomy, evidence, history, and decisions → BMad's *single shared memory* pattern (recommended for tightly-coupled agents) fits exactly.
- **Two access patterns serve two audiences:** orchestrator-first for onboarding/complex work (and community users); direct specialist access for speed/precision (internal power users). Same machine, two doors.

**Agent roster (4 — 1 orchestrator + 3 specialists):**

| Agent | Name | Role | Phase(s) | Core job |
| ----- | ---- | ---- | -------- | -------- |
| Orchestrator | **Owen** | Front door / conductor | cross-cutting | Detects **structuring vs. recurring** mode, routes to the right specialist, hosts **party mode**, drives first-run onboarding. The natural landing point for new/community users. |
| Specialist | **Diana** | Diagnostic Analyst | 1 — Diagnostic | Incremental, conversational ingestion of mixed/messy documents; identifies what each doc is, extracts what's useful, **reconciles conflicts across documents**, flags ambiguities, asks rather than assumes. Builds the **evidence base** with provenance. |
| Specialist | **Helena** _(Marco+Helena fused)_ | Analytical Architect | 2+3 — Planning + Structuring | Scopes *what* to measure (KPIs, hypotheses, criteria) **and** designs *how* (cost taxonomy, allocation methodology, workplan). One mind for "what + how," since they happen in the same conversation — and both are near-silent in recurring mode (structure already in memory). |
| Specialist | **Sofia** | Senior Analyst | 4 — Execution & Delivery | Produces the analysis section by section under the Constitution; labels every claim `fact`/`inference`/`assumption`; invokes adversarial review; assembles the report; captures lessons. |

**Shared capabilities (workflows/skills, NOT agents):**
- **Adversarial review** — domain-adapted, reused from core; enforces the Constitution checklist (allocation correctness, no double-counting, variance explained, source traceability, classification consistency). Invokable by Sofia and standalone.
- **Delivery & audience formatting** (formerly "Rafael") — a cross-cutting capability any agent can invoke; the **living-dialogue .md with response fields** is the default output, audience-specific formatting (CFO vs. ops director) is an optional invocation.
- **Mode routing** — structuring vs. recurring; the two-mode spine.
- **Party mode** — convene the specialists to debate a complex question; hosted by the orchestrator.

**The Analytical Integrity Constitution** (shared law, baked into every agent — likely a shared reference file all agents load on activation):
1. Never silently accept questionable/conflicting data — ask.
2. Reconcile conflicts across documents before proceeding.
3. Label every claim `fact` / `inference` / `assumption`.
4. Refuse to conclude on insufficient data — hard stop with what's missing and why.
5. Trace every number to a source document.
6. Log every analytical decision (why this allocation, why that exclusion, what assumption).

**Two operating modes (the spine):**
- **Structuring mode** — first-time, high-effort, full lifecycle; *builds* the taxonomy + baseline into memory.
- **Recurring mode** — light, faster; *reuses* the structure from memory; focuses on variance, trends, exceptions.

### Memory Architecture

**Pattern:** Single shared module memory (recommended for tightly-coupled agents), with a **profile layer** so the same machine serves internal / consultant / community deployments.

**Location:** `{project-root}/_bmad/memory/oam/`

```
_bmad/memory/oam/
  index.md                         # orientation — every agent reads this first
  lessons.md                       # cross-engagement METHOD lessons (no client data)
  daily/
    YYYY-MM-DD.md                  # append-only session log, tagged by agent
  profiles/
    {profile}/                     # internal = 1 · consultant = many · community = 0-then-built
      org-profile.md               # vocabulary, strategic priorities, data sources, conventions
      taxonomy.md                  # cost taxonomy / KPI defs / allocation criteria — the durable STRUCTURE
      baselines/
        {topic}-{period}.md        # lightweight historical results → powers "last quarter X, now Y"
```

**The memory-vs-deliverable split (core principle):**

- **Lives in memory (persists across analyses):** org profile, taxonomy / allocation criteria, historical baselines, method lessons. This is what makes the *second* run light — **recurring mode mostly reads here.**
- **Travels with the deliverable (per-analysis, in the output folder):** the **evidence base** (provenance ledger), the **decision log**, the **living-dialogue report**, and **`actions.md`**. These travel together so the analysis is transferable / auditable / repeatable by someone who wasn't in the session. After delivery, a **baseline summary** + reusable **method lessons** are distilled back into memory.

**Profile layer in practice:**
- **Internal** → one profile (e.g. `profiles/acme/`); never re-explain context.
- **Consultant** → one profile per client (`profiles/client-a/`, `profiles/client-b/`), client data fully isolated; `lessons.md` accumulates *method* improvements across all engagements without leaking client data.
- **Community** → ships empty; Owen's first-run onboarding builds the first profile.

### Memory Contract

| File | Purpose | Read by | Written by |
| ---- | ------- | ------- | ---------- |
| `index.md` | Orientation: which profiles exist, what's in memory, recent activity. Read first by every agent. | All agents on activation | Owen (curates); any agent appends activity |
| `lessons.md` | Cross-engagement **method** lessons (e.g., "ERP export X needs reconciliation against ledger Y"). No client data. | All agents | Sofia (post-delivery distillation); any agent inline |
| `daily/YYYY-MM-DD.md` | Append-only chronological session log, tagged by agent name. | Owen (orientation), any agent for recent context | All agents, every session |
| `profiles/{profile}/org-profile.md` | Org vocabulary, strategic priorities, data sources, analytical conventions. | All agents (active profile) | Owen (onboarding); Diana/Helena refine |
| `profiles/{profile}/taxonomy.md` | The durable **structure** — cost taxonomy, KPI definitions, allocation criteria. Built in structuring mode, reused in recurring mode. | All agents; **Helena** owns it | Helena (builds/updates); Sofia references |
| `profiles/{profile}/baselines/{topic}-{period}.md` | Lightweight historical results to power variance carry-forward ("last quarter X, now Y"). | Sofia (recurring mode), Owen | Sofia (post-delivery) |

**Deliverable-side artifacts (output folder, per analysis — not memory):**

| Artifact | Purpose | Owner |
| -------- | ------- | ----- |
| Evidence base / provenance ledger | Every datum + its source document + location. The traceability spine. | Diana (builds), all agents append |
| Decision log | Every analytical decision + rationale (why this allocation, why that exclusion, what assumption). | All agents append; travels with deliverable |
| Living-dialogue report (`.md`) | The signature output — findings with `fact/inference/assumption` labels + **response fields** for the user to approve/challenge/add context. | Sofia assembles |
| `actions.md` | Any action identified during the analysis. Plain, tool-agnostic, travels with the deliverable. | Any agent appends |

### Cross-Agent Patterns

- **Owen routes; the artifact chain carries state.** Owen is the front door: detects structuring vs. recurring mode and hands off to the right specialist. Power users skip Owen and summon Diana / Helena / Sofia directly.
- **The artifact chain (structuring mode):** Diana's **evidence base** → Helena's **analysis design** (scope + taxonomy + methodology, written to memory) → Sofia's **executed analysis** → assembled **living-dialogue report**. Provenance and the decision log **accrue across all agents**, append-only, throughout.
- **Shared memory enables cross-domain awareness:** Helena writes `taxonomy.md`; Sofia reads it without re-derivation. In recurring mode, Sofia reads `taxonomy.md` + `baselines/` and goes straight to variance/trend analysis — Diana does a light ingest, Helena barely fires.
- **Handoff model:** orchestrator-first (Owen routes) for onboarding/complex work; direct-specialist for speed. Either way, agents communicate *through the shared memory and deliverable artifacts*, not by passing context manually.
- **Party mode:** Owen convenes Diana + Helena + Sofia to debate a complex operational question from their respective lenses (data reality / analytical design / conclusion integrity).
- **Future expansion:** `actions.md` stays portable now; integration with external task tools (Notion, Linear, Jira) via **MCP**, configurable per profile, is a future expansion — not in scope for v1.

## Skills

> All agents load the **Analytical Integrity Constitution** (shared reference file) and `index.md` on activation. Shared memory lives at `_bmad/memory/oam/`; per-analysis deliverables live at `oam_output_folder`.

### oam-agent-owen

**Type:** agent

**Persona:** Owen — the conductor. Calm, organized, decisive about routing without dominating. Talks like an experienced engagement lead: orients newcomers, lets experts skip ahead. Enforces the method quietly, never lectures.

**Core Outcome:** The user is always working with the right specialist, in the right mode, knowing where they are in the lifecycle — with minimal friction.

**The Non-Negotiable:** Correctly detect **structuring vs. recurring** and **full vs. quick** before routing. A wrong mode call wastes the entire run.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Intake & mode detection | Classify the job: structuring vs. recurring, full vs. quick | User request, profile count, taxonomy presence for the topic | Mode decision + rationale |
| Routing | User reaches the right specialist or quick track | Mode decision | Handoff to Diana/Helena/Sofia or `oam-quick-analysis` |
| First-run onboarding | A usable first profile exists | User answers about org/vocabulary/data sources | `org-profile.md` + seed `taxonomy.md` for new profile |
| Party-mode host | A complex question debated from all lenses | Question + active profile | Convened session via `bmad-party-mode` (scoped to OAM agents) |
| Audience prompt | The run's audience + whether an exec summary is needed are known | Per-run user input | Audience parameter + exec-summary flag passed downstream |

**Memory:** Reads `index.md` + active profile (org-profile, taxonomy presence, baselines) on activation. Writes daily-log entries; curates `index.md`. Onboarding writes a new profile.

**Init Responsibility:** 0 profiles → run onboarding to create the first. Otherwise → load index + active profile, summarize current state.

**Activation Modes:** Interactive (primary); headless can return a routing/mode decision.

**Tool Dependencies:** None beyond core; invokes `bmad-party-mode`.

**Design Notes:** Mode heuristics — taxonomy exists for the topic → recurring; absent → structuring; small/simple scope → quick. Owen produces a tangible artifact (the onboarded profile), so he is not a pure coordinator — satisfies the multi-agent output check.

**Relationships:** Front door to Diana → Helena → Sofia; routes small jobs to `oam-quick-analysis`; hosts `bmad-party-mode`. Power users may bypass Owen and summon specialists directly.

---

### oam-agent-diana

**Type:** agent

**Persona:** Diana — forensic and skeptical, relentless about provenance, warm but won't let a questionable number slide. She *drives* the data conversation.

**Core Outcome:** A complete, reconciled, fully-sourced **evidence base** downstream agents can trust without re-checking.

**The Non-Negotiable:** Never silently accept questionable or conflicting data; every datum is traceable to a source document (Constitution #1, #2, #5).

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Situational assessment | Shared understanding of why this analysis exists | User brief | Situation summary |
| Incremental ingestion | Messy mixed-format docs become structured, sourced data | Docs (Excel/CSV/PDF/exports/contracts/emails), one at a time or in batches | Growing structured evidence base |
| Data-landscape mapping | Known inventory of what data exists and what's missing | Ingested docs + user knowledge | Data landscape map + gap list |
| Cross-document reconciliation | Conflicts surfaced and resolved before proceeding | Multiple docs with overlapping figures | Conflict log + resolutions |
| Stakeholder mapping | Owners/consumers/decision-makers identified | User input | Stakeholder map |
| Evidence-base building | Provenance spine for the whole analysis | All of the above | Evidence base / provenance ledger (deliverable) |

**Memory:** Reads active profile (org-profile vocabulary, known data sources). Writes/refines `org-profile.md` data-sources; builds the deliverable-side evidence base. Daily tag `[Diana]`.

**Init Responsibility:** Ensure active profile loaded. In recurring mode, load prior evidence/baselines for continuity.

**Activation Modes:** Interactive (primary — ingestion is conversational); batch ingest possible.

**Tool Dependencies:** Harness file reading (PDF/CSV/text); light `.xlsx` parse step; fallback "export as CSV."

**Design Notes:**
- Ingestion is **stateful and incremental** — builds a growing picture rather than demanding everything upfront. Reconciliation drives resolution *before* moving on. The evidence base is the traceability spine all later artifacts cite.
- **Optional capabilities (Note 2):** *situational-assessment* and *stakeholder-mapping* are optional — Owen skips them in recurring mode or when the org profile already establishes full context. Lean runs stay lean.
- **Extraction-level confidence:** Diana labels every data point `Confirmed / Inferred / Estimated` at ingestion (see Artifact Schemas). These originate the confidence chain that Sofia later consolidates.

**Relationships:** Phase 1. Hands the evidence base to Helena. In recurring mode does a *light* ingest feeding Sofia directly.

---

### oam-agent-helena

**Type:** agent

**Persona:** Helena — the architect. Structured, methodical, thinks in frameworks and taxonomies. Her instinct: "What exactly are we measuring, and how will we defend it?"

**Core Outcome:** A defensible analytical framework — scope, KPIs, hypotheses, taxonomy, allocation methodology, workplan — that makes execution mechanical and the result defensible.

**The Non-Negotiable:** Allocation and classification criteria are explicit, consistent across periods, and recorded in the decision log (Constitution #3, #6) — so periods stay comparable.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Analysis scope | Clear boundaries: what's in/out, period, granularity | Evidence base, user objective | Scope statement |
| KPI framework | Agreed metrics + definitions | Scope, org-profile | KPI definitions |
| Hypothesis definition | Testable hypotheses to direct the analysis | Scope, situational context | Hypothesis set |
| Framework / taxonomy design | The durable analytical structure | Evidence base, KPIs | `taxonomy.md` (cost taxonomy / allocation criteria) |
| Workplan breakdown | Execution split into discrete analytical tasks | Framework | Workplan |
| Methodology review | Self-check that the approach is sound before execution | Framework + workplan | Methodology sign-off / revisions |

**Memory:** Reads evidence base + org-profile. **Writes `taxonomy.md`** (the durable structure) to the active profile; writes decision-log entries (criteria rationale). Daily tag `[Helena]`.

**Init Responsibility:** Load evidence base + org-profile. In recurring mode, load existing `taxonomy.md` and confirm it still fits (mostly silent).

**Activation Modes:** Interactive (primary).

**Tool Dependencies:** None beyond core.

**Design Notes:** Helena owns `taxonomy.md` — built in structuring mode, reused in recurring mode (which is why she's near-silent on repeat runs). She fuses *planning* (what to measure) and *structuring* (how) because in operational analysis they happen in the same conversation.

**Relationships:** Phases 2+3. Consumes Diana's evidence base; hands framework + taxonomy to Sofia. Near-silent in recurring mode.

---

### oam-agent-sofia

**Type:** agent

**Persona:** Sofia — senior analyst. Rigorous, writes for leadership, obsessed with defensibility. Labels her confidence and refuses to overclaim.

**Core Outcome:** A finished, defensible **living-dialogue report** where every finding is sourced, confidence-labeled, and survives adversarial review.

**The Non-Negotiable:** Refuse to conclude on insufficient data — a hard stop stating what's missing and why (Constitution #4); every claim labeled `fact`/`inference`/`assumption`.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Section analysis | Each part of the analysis produced and sourced | Framework, taxonomy, evidence base | Analyzed sections |
| Confidence labeling | Reader knows what's solid vs. needs verification | Findings | `fact`/`inference`/`assumption` labels |
| Adversarial review | Findings survive the Constitution checklist | Draft analysis | Findings report (invokes `oam-adversarial-review`) |
| Report assembly | The signature deliverable | Sections + labels + provenance | Living-dialogue `.md` with response fields (see Artifact Schemas) |
| Executive summary (optional) | Leadership-ready summary, only when the audience needs it | Approved sections | Exec summary written **last**, after all sections are approved |
| Lessons-learned distillation | Next run is lighter | Completed analysis | Baseline summary + method lessons → memory |

**Memory:** Reads taxonomy + evidence base + baselines (recurring mode). Writes living report + decision log + `actions.md` (deliverable). Post-delivery, writes baseline summary to `baselines/` + method lessons to `lessons.md`. Daily tag `[Sofia]`.

**Init Responsibility:** Load taxonomy, evidence base, framework. In recurring mode, load `baselines/` for variance carry-forward.

**Activation Modes:** Interactive (primary); headless drafting possible.

**Tool Dependencies:** Invokes `oam-adversarial-review` and `oam-format-delivery`.

**Design Notes:**
- Section-by-section execution; in recurring mode the focus shifts to **variance/trend/exception** against baselines. Assembles the living report with response fields. The post-delivery distillation is what makes the *next* cycle cheap.
- **Confidence labeling is a chain responsibility (Note 1):** every agent labels claims inline as they work. Sofia's *confidence-labeling* capability is the **final consolidation and verification** pass — not the only place labels appear. She verifies consistency against Diana's extraction-level labels; she does not generate them from scratch.
- **Executive summary is written last** — never before the analysis is complete and sections are approved.

**Relationships:** Phase 4. Consumes Helena's framework; invokes adversarial-review + format-delivery; produces the final deliverable bundle.

---

### oam-adversarial-review

**Type:** workflow

**Purpose:** Stress-test an analytical artifact against the **Analytical Integrity Constitution** — the module's signature quality gate. Reviewer MUST find issues.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| General cynical pass | Broad weaknesses surfaced | Analysis/report | Delegated to core `bmad-review-adversarial-general` |
| Constitution checklist | Domain integrity verified | Report + evidence base + taxonomy | Pass/fail per check + required fixes |

Checklist (minimum): **allocation correctness · no double-counting · variance explained · source traceability · classification consistency across periods.** Output: findings report (`.md` v1; HTML future).

**Design Notes:** Layered — core general pass first, then OAM-specific checks derived from the leadership challenge-triggers. The differentiator; does not depend on core for the domain logic.

**Relationships:** Invoked by Sofia and `oam-quick-analysis`; available standalone; uses core `bmad-review-adversarial-general`.

---

### oam-format-delivery

**Type:** workflow

**Purpose:** Adapt a finished analysis to a target audience (CFO, ops director, board) **without altering substance**.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Audience formatting | Same analysis, right emphasis for the reader | Living report + audience parameter | Audience-tailored rendering |

**Design Notes:** Cross-cutting — any agent invokes it. Default output is the living `.md`; audience formatting is an *optional* layer. Never changes numbers, labels, or conclusions — only presentation and emphasis. Preserves traceability + confidence labels.

**Relationships:** Invoked by any agent (usually Sofia at delivery). Audience parameter comes from Owen's per-run prompt.

---

### oam-quick-analysis

**Type:** workflow

**Purpose:** The scale-adaptive **Quick Flow** — handle a simple/one-page operational question in a single pass (scope + analysis + review) without the full four-phase lifecycle.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Quick scope | Just enough framing | Question + any docs | Mini-scope |
| Quick analysis | The answer, Constitution-compliant | Docs + active profile/taxonomy if present | Concise findings (labeled, sourced) |
| Quick review | Lightweight integrity check | Findings | Light adversarial pass |
| Deliver | Usable artifact | Findings | Concise living report + decision log + `actions.md` |

**Design Notes:** Mirrors `bmad-quick-dev`. Still obeys the full Constitution (labels, traceability, refuse-on-insufficient). Uses memory if present. **Escalates** to the full lifecycle (Diana/Helena/Sofia) if the task proves more complex than it looked. **Note 3 — the overlap with the full pipeline is the intentional BMad "duplicate for coherence" pattern. Do NOT DRY it up** — the quick track must stay self-contained to remain coherent as a standalone workflow.

**Relationships:** Owen routes small jobs here; can escalate to specialists; invokes `oam-adversarial-review` (light) + `oam-format-delivery`.

---

### oam-setup

**Type:** workflow (module setup skill)

**Purpose:** Make OAM installable — scaffold memory, write the Constitution, collect config, register help.

**Capabilities:**

| Capability | Outcome | Inputs | Outputs |
| --- | --- | --- | --- |
| Scaffold memory | Shared memory tree exists | — | `_bmad/memory/oam/` (`index.md`, `lessons.md`, `daily/`, `profiles/`) |
| Write Constitution | Shared law available to all agents | — | Constitution reference file |
| Collect config | Module configured | User answers | `oam_output_folder`, `default_profile` |
| Register help | Skills discoverable | — | `bmad-help.csv` rows for all OAM skills |
| Trigger onboarding | First profile bootstrapped | profile count = 0 | Handoff to Owen onboarding |

**Design Notes:** Generated when we run **Create Module (CM)**. Supports headless. Idempotent — safe to re-run for updates.

**Relationships:** Runs first (install/update). Hands off to Owen's onboarding.

---

### Artifact Schemas (deliverable formats)

**Confidence-label vocabularies — two levels, by design:**
- **Extraction level** (Diana, per data point in the evidence base): `Confirmed | Inferred | Estimated`
- **Claim level** (Sofia, per section/conclusion in the report): `Fact | Inference | Assumption`

Labels **originate at extraction with Diana**, travel through the chain, and **Sofia verifies consistency** at the end — she does not invent them from scratch.

**Living-dialogue report — per-section structure (Sofia assembles):**

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

- **Interaction model:** the user may edit the `.md` directly and return it, OR respond in chat referencing specific sections. Sofia handles **both** gracefully.
- **Executive summary:** optional. Owen asks at session start whether the audience needs one. If yes, Sofia writes it **last**, after all sections are approved — never before the analysis is complete.

**Evidence base — per-entry structure (Diana builds):**

```markdown
## Evidence Entry [ID]

- **Document:** [filename and type — e.g. planilha_custos_abril.xlsx]
- **Location:** [sheet/tab, cell range, page number, or line]
- **Period:** [date or period the document represents]
- **Value extracted:** [the actual data point]
- **Confidence:** [Confirmed | Inferred | Estimated]
- **Conflict flag:** [None | CONFLICT — see resolution note]
- **Resolution note:** [if conflict: both values recorded — source A says X, source B says Y — awaiting user resolution]
```

- **Conflict handling:** Diana records **both** values, flags the conflict explicitly, and **stops for resolution before continuing** — a hard stop under Constitution rule 2 (reconcile, don't paper over). She never silently chooses or proceeds with an unresolved conflict.

## Configuration

Deliberately minimal — the module runs on defaults. `oam-setup` collects:

| Variable | Prompt | Default | Result Template | User Setting |
| -------- | ------ | ------- | --------------- | ------------ |
| `oam_output_folder` | Where should deliverables be saved? | `{output_folder}/oam/{profile}/{analysis}` | per-analysis deliverable bundle (report + evidence base + decision log + actions.md) | yes |
| `default_profile` | Default profile name? | `default` (skip → triggers onboarding) | active profile under `_bmad/memory/oam/profiles/` | yes |

- **Audience is NOT a config variable** — it varies enough per analysis that Owen asks at the start of each run rather than assuming a standing default. (Session parameter, not config.)
- **Deployment mode is inferred, not asked** — profile count tells the story: 0 → community onboarding, 1 → internal, many → consultant.
- `communication_language` / `document_output_language` inherit from core BMad.

## External Dependencies

**v1: none required.** Data ingestion is harness-native + light scripting:
- Claude Code reads PDFs and text/CSV natively.
- Diana handles `.xlsx` with a small parse step; falls back to "export as CSV" when scripting isn't available.
- Fully portable, zero install, degrades gracefully.

**Future expansion (per profile, optional):**
- **MCP connectors for ERP / accounting systems** — direct data ingestion instead of file export.
- **MCP connectors for task tools** (Notion / Linear / Jira) — sync `actions.md` into a real task system.

Both are configured per profile when needed; not in v1 scope.

## UI and Visualization

**v1: none** — the primary (and only) output is the **living-dialogue `.md`**. The Constitution checklist and findings work well in markdown to start.

**Future enhancements (parked):**
- **HTML adversarial-review report** — findings + pass/fail on the Constitution checklist.
- **HTML cost-structure / variance dashboard** — taxonomy tree + period-over-period deltas.

## Setup Extensions

Beyond config collection, `oam-setup` must:
- **Scaffold the shared memory tree** at `_bmad/memory/oam/` (`index.md`, `lessons.md`, `daily/`, `profiles/`).
- **Write the Analytical Integrity Constitution** as a shared reference file all agents load on activation.
- **Register module help entries** (bmad-help.csv rows for Owen, Diana, Helena, Sofia + shared workflows).
- **If 0 profiles exist**, hand off to **Owen's first-run onboarding** to build the first profile (org-profile + initial taxonomy).

## Integration

**Standalone — independent value.** OAM has zero hard dependencies on BMM. It installs, scaffolds its own memory, and runs the full operational-analysis lifecycle on its own. It reuses **core** skills only (`bmad-party-mode`, `bmad-review-adversarial-general`, optionally `bmad-editorial-review-*` / `bmad-distillator`), which ship with BMad.

**Expansion hooks (optional, when BMM is present):**
- OAM's **Diagnostic** output (evidence base, situational assessment, stakeholder map) can inform a BMM **product brief / PRD** — an operational diagnosis feeding a product decision.
- OAM's operational KPI / variance analysis can ground BMM planning artifacts.
- Conversely, a BMM project can call OAM to analyze operational implications.

These hooks are **optional and non-breaking** — the absence of BMM never degrades OAM.

## Creative Use Cases

- **Monthly / quarterly cost refresh** — recurring mode: Diana light-ingests the new period, Sofia goes straight to variance vs. baseline. Minutes, not days.
- **Consultant practice** — one profile per client, fully isolated; `lessons.md` compounds *method* expertise across every engagement.
- **Party mode on a hard decision** — "Should we outsource line X?" Owen convenes Diana (data reality), Helena (the right framework to evaluate it), and Sofia (does the conclusion actually hold?).
- **Beyond cost** — OEE / operational KPI reviews, accounting report assembly, variance investigations — same machine.
- **Reproducible handover** — the evidence base + decision log let a colleague who wasn't in the session re-run or audit the analysis exactly.
- **Audit / compliance-ready by construction** — traceability spine + decision log = defensible to leadership and auditors.
- **Cross-module** — pipe an OAM operational diagnosis into a BMM PRD.

## Ideas Captured

_Raw material from the seed brief (`operations-idea.md`) and the live session. Generous and unstructured by design._

### From the seed brief

**Proposed lifecycle — 4 phases mirroring BMM:**
- **Phase 1 — Diagnostic** (mirrors Analysis): understand the situation before committing to a conclusion. Workflows: situational-assessment, data-landscape-mapping, stakeholder-mapping.
- **Phase 2 — Analytical Planning** (mirrors Planning): define what to analyze, for whom, by what criteria. Workflows: analysis-scope, kpi-framework, hypothesis-definition.
- **Phase 3 — Structuring** (mirrors Solutioning): design the analytical framework, break work into discrete analytical tasks. Workflows: analytical-framework-design, workplan-breakdown, methodology-review.
- **Phase 4 — Execution & Delivery** (mirrors Implementation): produce the analysis section by section, review critically, deliver. Workflows: section-analysis, adversarial-review (reused), report-assembly, lessons-learned.

**Proposed agents (5):**
1. **Diana** — Diagnostic Analyst (Phase 1): situational assessment, data landscape, stakeholder mapping.
2. **Marco** — Planning Strategist (Phase 2): scope definition, KPI framework, hypothesis structuring.
3. **Helena** — Analytical Architect (Phase 3): framework design, methodology, workplan.
4. **Sofia** — Senior Analyst (Phase 4): section execution, adversarial review, report assembly.
5. **Rafael** — Technical Writer (cross-cutting): document standards, formatting, final polish.

**Key design decisions to discuss:**
1. **Artifact chain** — each phase produces a document that feeds the next (like PRD → Architecture in BMM). Need to design this chain for operational work.
2. **Adversarial Review adaptation** — adapt BMM's "reviewer MUST find issues" for analytical documents: not just "is this accurate?" but "what assumptions are hidden?", "what data is missing?", "what alternative interpretation challenges this conclusion?"
3. **Scale-adaptive behavior** — small analyses (one-page briefing) shouldn't require all 4 phases. Want a Quick Flow track (like bmad-quick-dev) — a single workflow handling scope + analysis + review for simple tasks.
4. **Organizational context file** — equivalent to project-context.md in BMM: captures the org's vocabulary, strategic priorities, data sources, recurring analytical conventions, so agents stay aligned across sessions.
5. **Party Mode for operational decisions** — convene Diana, Marco, Helena, Sofia simultaneously to debate a complex question from their analytical perspectives (like BMM party mode with PM/Architect/Dev).

**Module type:** Standalone (not dependent on BMM), but with expansion hooks so users who also have BMM can cross-reference — e.g., use the operational diagnostic phase to inform a product PRD.

**What the user wants from this session:**
1. Validate and refine the agent architecture
2. Design the complete artifact chain (what each phase produces/consumes)
3. Define the skill structure for each workflow
4. Decide the module memory and personalization approach
5. Identify dependencies/integrations (MCP servers, external systems)
6. Produce the final plan document used to build each skill

### From the live session (2026-05-21)

- **Identity locked:** Module name "Operational Analysis Method", code `oam`. Deliberate parallel to BMad Method (BMM).
- **Deployment intent (multi-mode):** Primary use is **internal** (single organization). But the user also wants it to work for **community distribution** and for **consultant** use across clients. → Architecture must support a *profile-based* context model: cold-start gracefully (distribution), one rich profile (internal), or many per-engagement profiles (consultant). Lessons-learned ideally accumulate across engagements while keeping client data separate.
- **Flagship use case:** "**Structure operational costs spread around some documents.**" Take operational costs scattered across multiple source documents and consolidate them into a structured, defensible cost analysis. This concretely exercises the full lifecycle:
  - *Diagnostic:* where are all the cost documents, what's in them, who owns them (data-landscape + stakeholder mapping).
  - *Planning:* which costs/period/granularity (scope), cost categories & allocation bases (KPI framework), hypotheses (e.g. "costs concentrate in X").
  - *Structuring:* cost taxonomy + allocation methodology (framework design), workplan, methodology review.
  - *Execution:* analyze each cost category (section-analysis), check completeness/double-counting/allocation defensibility (adversarial review), assemble, capture lessons.
- **Open design implications to resolve:**
  - "Defensible" for a cost structure likely = completeness (nothing missing), no double-counting, defensible allocation logic, traceability to source docs. → Strong seed for the domain-specific adversarial-review criteria.
  - Source documents are likely spreadsheets/PDFs/exports → external-dependency + data-ingestion question (Phase 4).
  - Cost structuring may be **recurring** (reporting cycles) → memory/templating should make the next cycle faster.

### Live session — round 2: data, trust, cadence (2026-05-21)

**Data reality → incremental ingestion is core.**
- Sources: mixed — Excel, CSV, PDF, ERP/accounting exports, contracts, emails, "whatever the user brings." No cap on document count.
- Model: **sequential / incremental ingestion** — user feeds docs one at a time or in batches; the module builds a *growing structured picture* (analogy: BMad handling a single feature without the full codebase upfront).
- Hard requirement: the Diagnostic agent must **identify what each document is, extract what's useful, flag ambiguities, and ask clarifying questions** — never assume. Must handle mess and *drive the data conversation*. Cannot expect clean, pre-processed input.
- Implication: Diagnostic produces a persistent, growing **evidence base / data ledger** with provenance per datum.

**Trust bar → traceability is a first-class, end-to-end requirement.**
- Audience: **leadership** (above the executing team). They never see raw data → every number must trace to a source document.
- Challenge triggers: cost allocated to wrong cost center / expense class; budget overrun without explanation; unexplained month-over-month spikes; low OEE without root cause; any number not traceable to a source.
- **Domain adversarial-review checklist (minimum):** allocation correctness, no double-counting, variance explained, source traceability, consistency of classification criteria across periods.
- Implication: provenance / citation must flow through the *entire* artifact chain to the final report.

**Cadence → two operating modes.**
- **Structuring mode** — first-time, high effort; builds the taxonomy + baseline (full lifecycle).
- **Recurring analysis mode** — faster; reuses the existing structure; focuses on variance and trends (lighter loop).
- Memory **persists across cycles**: cost taxonomy, allocation criteria, org context → second run significantly lighter.
- Scope note: broader than cost — ongoing work includes performance analysis, accounting reports, operational KPI reviews (e.g., **OEE**). Cost-structuring is the flagship / first worked example; the lifecycle generalizes to operational analysis broadly.

### Live session — round 3: the "wow" capabilities (2026-05-21)

Scope confirmed: module is operationally broad (performance, KPI, accounting, OEE, variance — one-off or recurring). Cost-structuring is the proving ground; two-mode + traceability generalize to everything.

Five capability areas the user wants that nothing delivers well today:

1. **Data quality / inconsistencies**
   - Never silently accept questionable data — if a number looks wrong, ask.
   - Conflicting figures for the same item across documents → flag + drive resolution *before continuing*.
   - **Cross-document reconciliation is automatic**, not on-request.

2. **Context / memory**
   - Persist org vocabulary, KPIs, cost taxonomy, analytical conventions across sessions — never re-explain.
   - Carry history forward: "last quarter this was X, now it's Y — explain the variance before continuing?"
   - When analysis implies an action → proactively suggest capturing it as a **TASK / TODO** so follow-ups are traceable, not lost in chat.

3. **Conclusion integrity**
   - **Refuse to conclude on insufficient data** — a hard stop (not a warning), with an explicit statement of what's missing and why it matters.
   - Every conclusion carries a **confidence signal: fact / inference / assumption.**

4. **Delivery format**
   - Primary output: in-conversation or a structured **.md** file.
   - Includes **response fields** — user can add context, approve a finding, challenge an assumption, flag follow-up. The document is a **dialogue, not a verdict.**
   - Audience-specific formatting (CFO vs. ops director) = optional, not required.

5. **Collaboration / continuity**
   - **Auto-log every analytical decision** (why this allocation criterion, why a cost was excluded, what assumption was made on ambiguity).
   - **Decision log travels with the final artifact.**
   - Analysis must be transferable, auditable, repeatable by someone who wasn't in the original session.

**Synthesis (emergent):**
- Items 1, 3, 5 + traceability are not features of any single workflow — they are **module-wide behavioral invariants**: an *"Analytical Integrity Constitution"* every agent obeys.
- Items 2 + 5 → persistent shared memory + a portable **decision-log** artifact.
- Item 4 → a **living-dialogue document** (.md) with embedded response fields as the signature artifact format.
- New first-class artifacts emerging: **evidence base / data ledger** (provenance), **decision log**, **living-dialogue report**, plus the persistent **taxonomy + org profile** in memory.

## Build Roadmap

**Recommended build order** — follow the artifact chain; build shared workflows just before the agent that needs them; Owen last; setup generated by CM.

0. **Shared foundation** — author the **Analytical Integrity Constitution** reference file and fix the `_bmad/memory/oam/` structure convention. Every agent loads the Constitution on activation, so it must exist before agent builds. (`oam-setup` later just installs it.)
1. **`oam-agent-diana`** — start of the value chain and the most novel surface (incremental ingestion, reconciliation, evidence base). Her evidence base is everyone's input; build + test ingestion early.
2. **`oam-agent-helena`** — consumes the evidence base, produces `taxonomy.md`. Validates the structuring step.
3. **`oam-adversarial-review`** — build before Sofia (she invokes it). The signature quality gate.
4. **`oam-format-delivery`** — small; also invoked by Sofia. Build before her.
5. **`oam-agent-sofia`** — consumes Helena's framework, invokes review + format, assembles the living report. Full structuring pipeline now testable end-to-end.
6. **`oam-quick-analysis`** — build after the full pipeline exists so it can mirror a compressed, self-contained version.
7. **`oam-agent-owen`** — build last among agents: he routes to all the others and detects mode, so the specialists + quick track should exist first.
8. **`oam-setup`** — **generated by Create Module (CM)** at the end; scaffolds memory, installs the Constitution, collects config, registers help.

**Rationale:** building along the artifact chain (Diana → Helena → Sofia) means each new skill's inputs already exist, so every step is testable with real artifacts. Shared workflows land just before their first caller. Owen comes last — an orchestrator is only meaningful once there's something to orchestrate. Setup is generated, not hand-built.

**Validation:** after all skills are built, run **Validate Module (VM)**, then exercise the **flagship use case** (cost structuring from scattered docs) end-to-end in *structuring* mode, then re-run in *recurring* mode to confirm the second pass is genuinely lighter.

**Next steps:**

1. Build each skill **in the order above** using **Build an Agent (BA)** / **Build a Workflow (BW)** — pass this plan as context.
2. Run **Create Module (CM)** to generate `oam-setup` and scaffold module infrastructure.
3. Run **Validate Module (VM)**, then dogfood the flagship use case.
