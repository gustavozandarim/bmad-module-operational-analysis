# OAM Module Conventions

> Shared path + structure conventions for the Operational Analysis Method (OAM)
> module. Every OAM skill follows these. Authored as part of the shared
> foundation; `oam-setup` (generated later by Create Module) scaffolds and
> verifies this layout at install time.

## Shared law & memory (root of the persistent memory tree)

The Six Laws and these conventions are immutable module law. They ship in
`oam-setup/assets/` and `oam-setup` installs them to the root of the shared
memory tree — which is persistent (the install cleanup never touches
`{project-root}/_bmad/memory/`, only the per-module package dir
`{project-root}/_bmad/{code}/`). Every OAM
skill loads them on activation.

```
_bmad/memory/oam/
  analytical-integrity-constitution.md   # the Six Laws — loaded by every agent on activation (immutable law)
  module-conventions.md                  # this file (immutable)
  index.md                       # orientation — every agent reads first on activation
  lessons.md                     # cross-engagement METHOD lessons (no client data)
  daily/
    YYYY-MM-DD.md                # append-only session log, tagged by agent: [Diana] [Helena] [Sofia] [Owen]
  profiles/
    {profile}/                   # internal = 1 · consultant = many · community = 0 (built by onboarding)
      org-profile.md             # vocabulary, strategic priorities, data sources, conventions
      taxonomy.md                # cost taxonomy / KPI defs / allocation criteria — the durable STRUCTURE (Helena owns)
      baselines/
        {topic}-{period}.md      # lightweight historical results → powers "last quarter X, now Y"
```

**Memory-vs-deliverable split (core principle):**
- **In memory** (persists, makes recurring runs light): org profile, taxonomy /
  allocation criteria, historical baselines, method lessons.
- **Travels with the deliverable** (per-analysis, in the output folder): the
  evidence base (provenance ledger), the decision log, the living-dialogue
  report, and `actions.md`.

## Deliverables (per-analysis bundle — travels together, auditable)

Output root resolves from config `oam_output_folder`
(default `{output_folder}/oam/{profile}/{analysis}`):

```
{oam_output_folder}/
  evidence-base.md       # every datum + source + location + confidence (Diana builds; all append)
  analysis-design.md     # scope + KPIs + hypotheses + workplan for THIS run (Helena builds)
  decision-log.md        # every analytical decision + rationale (all agents append)
  adversarial-review.md  # the quality-gate findings: general pass + Constitution checklist (oam-adversarial-review writes)
  report.md              # the living-dialogue report with response fields (Sofia assembles)
  actions.md             # any action identified during the analysis (any agent appends)
```

> **Helena's split:** the *durable* structure (cost taxonomy, KPI definitions,
> allocation/classification criteria, methodology) lives in memory at
> `profiles/{profile}/taxonomy.md` and powers recurring mode; the *per-analysis*
> design (this run's scope, hypotheses, workplan) travels with the deliverable as
> `analysis-design.md`. Sofia reads both.

## Memory contract (who reads / writes what)

| File | Read by | Written by |
| --- | --- | --- |
| `index.md` | all agents on activation | Owen curates; any agent appends activity |
| `lessons.md` | all agents | Sofia (post-delivery distillation); any agent inline |
| `daily/YYYY-MM-DD.md` | Owen (orientation), any agent | all agents, every session |
| `profiles/{p}/org-profile.md` | all agents (active profile) | Owen (onboarding); Diana/Helena refine |
| `profiles/{p}/taxonomy.md` | all agents | **Helena** (builds/updates); Sofia references |
| `profiles/{p}/baselines/*` | Sofia (recurring), Owen | Sofia (post-delivery) |

## Config variables (collected by `oam-setup`)

| Variable | Default | Purpose |
| --- | --- | --- |
| `oam_output_folder` | `{output_folder}/oam/{profile}/{analysis}` | per-analysis deliverable bundle |
| `default_profile` | `default` (skip → triggers onboarding) | active profile under `profiles/` |

`communication_language` / `document_output_language` inherit from core BMad.
Audience is a per-run session parameter (Owen asks), **not** a config variable.

## Two operating modes (the spine)

- **Structuring mode** — first-time, high-effort, full lifecycle; *builds* the
  taxonomy + baseline into memory.
- **Recurring mode** — light, fast; *reuses* the structure from memory; focuses
  on variance, trends, exceptions.

Mode is inferred from memory state: taxonomy exists for the topic → recurring;
absent → structuring; small/simple scope → quick (`oam-quick-analysis`).
