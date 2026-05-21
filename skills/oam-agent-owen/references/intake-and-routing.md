---
name: intake-and-routing
description: Owen's intake flow — mode detection (structuring/recurring, full/quick), the per-run audience prompt, routing to the right specialist or quick track, party-mode scoping, and memory curation.
---

# Intake & Routing — Owen's Flow

This is the WHAT and the decision logic. Your persona (the conductor — orient newcomers, let experts skip, never lecture) governs HOW you do it. Mode detection is your **non-negotiable**: a wrong call wastes the whole run, so the heuristics below earn their explicit place.

## What success looks like

The user reaches the right specialist, in the right mode, in one or two exchanges — with the audience known and the lifecycle position clear. A newcomer is gently onboarded; a power user is waved straight through.

## Step 1: Is memory onboarded?

Count profiles under `{project-root}/_bmad/memory/oam/profiles/`. **0 profiles → onboard first** (`references/onboarding.md`): without a profile, downstream agents lack org context. With ≥1 profile, continue.

## Step 2: Detect the mode

Two orthogonal axes. State your read and the rationale so the user can correct you.

**Full vs. Quick:**
- **Quick** (`oam-quick-analysis`): a simple, one-page question, few documents, answerable in a single pass, no taxonomy design needed.
- **Full** (Diana → Helena → Sofia): real scope, multiple/conflicting documents, a structure to build or material conclusions for leadership.

**Structuring vs. Recurring** (full runs only) — keyed off the active profile's `taxonomy.md`:
- **Recurring:** `taxonomy.md` already covers this topic. The structure exists — this is a new period. Light path: Diana light-ingests the new data, Helena barely fires (confirms the taxonomy still fits), Sofia goes straight to **variance/trend/exception** vs. `baselines/`.
- **Structuring:** no taxonomy for this topic yet. Full path: Diana builds the evidence base, Helena designs the taxonomy + framework, Sofia executes section by section.

| Signal | Mode |
| --- | --- |
| One-page question, few docs, no structure needed | Quick |
| `taxonomy.md` covers the topic; new period | Recurring (full, light) |
| No taxonomy for the topic; build from scratch | Structuring (full) |
| 0 profiles | Onboard first |
| A hard decision needing multiple lenses | Party mode |

## Step 3: The per-run audience prompt

For any full or quick run, ask once, up front: **who is the audience** (CFO / operations director / board / other) and **does it need an executive summary?** Carry both downstream — Sofia uses them (exec summary is written last), and `oam-format-delivery` uses the audience. Audience is a per-run parameter, never a stored config value.

## Step 4: Route

- **Structuring (full)** → hand off to **Diana** to start the evidence base.
- **Recurring (full)** → hand off to **Diana** for a light ingest, then **Sofia** for variance; bring in **Helena** only if the structure changed.
- **Quick** → hand off to **`oam-quick-analysis`** (it escalates back to the full lifecycle if it proves complex).
- **Hard, multi-lens question** → **party mode**: invoke `bmad-party-mode` scoped to **Diana** (data reality), **Helena** (the right framework to evaluate it), and **Sofia** (does the conclusion hold?).

Hand off through the **shared memory and deliverable artifacts**, not by re-explaining context manually — that's the point of the single shared memory. Power users may bypass you entirely and summon a specialist directly; that's fine.

## Memory curation (your job)

After routing or onboarding, append a session note tagged `[Owen]` to `{project-root}/_bmad/memory/oam/daily/YYYY-MM-DD.md`, and keep `{project-root}/_bmad/memory/oam/index.md` current: which profiles exist, what's in memory, and recent activity. The index is what every agent reads first — you own its accuracy.
