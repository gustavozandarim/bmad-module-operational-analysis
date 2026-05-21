---
name: onboarding
description: Owen's first-run onboarding — build the first profile (org-profile + seed taxonomy) for a new organization or client, so downstream agents have org context from the start.
---

# First-Run Onboarding — Owen's Flow

This is the WHAT and the schemas. Your persona (the conductor — warm, efficient, never a lecture) governs HOW. Onboarding is what makes you more than a router: it produces a tangible artifact — the first **profile** — that every downstream agent reads.

## When this runs

When `{project-root}/_bmad/memory/oam/profiles/` has **0 profiles** (community cold-start), or when the user is starting work for a **new organization or client** (a new profile in a consultant deployment). Each profile is fully isolated — client data never crosses profiles.

## What success looks like

A usable first profile exists: `org-profile.md` captures who the org is and where its data lives, and a seed `taxonomy.md` gives Helena a starting point. From here, the user can be routed into a real analysis with full context.

## The conversation

Ask a small, focused set of questions — enough to seed the profile, not an interrogation. Adapt to what the user volunteers:

- **Vocabulary:** terms, abbreviations, and names that mean something specific here.
- **Strategic priorities:** what leadership cares about; what "good" looks like for this org.
- **Data sources:** where operational data lives — systems, exports, spreadsheets, who owns each, and typical formats.
- **Analytical conventions:** period definitions, currency/units, reporting cadence, any existing classification norms.

## Write the profile

Pick a profile name (the org/client short name; default `{default_profile}`). Create `{project-root}/_bmad/memory/oam/profiles/{profile}/`.

**`org-profile.md`:**

```markdown
# Org Profile — {profile}

## Vocabulary
- {term} — {what it means here}

## Strategic Priorities
- {what leadership cares about; what "good" looks like}

## Data Sources
| Source | What it holds | Owner | Format |
| ------ | ------------- | ----- | ------ |

## Analytical Conventions
- Period: {definition}   ·   Currency/units: {...}   ·   Cadence: {...}
- {any existing classification norms}
```

**`taxonomy.md` (seed — Helena builds it out in the first structuring run):**

```markdown
# Taxonomy — {profile} (seed)

> Seeded at onboarding. Helena owns and expands this in the first structuring run.

## Category Taxonomy
- {any categories the org already uses}

## KPI Definitions
| KPI | Definition | Formula | Unit | Source basis |
| --- | ---------- | ------- | ---- | ------------ |

## Allocation & Classification Criteria
- {any existing rules}

## Methodology
- {defaults; to be confirmed by Helena}

## Change Log
- {date} — seeded at onboarding by Owen.
```

## Finish

Update `{project-root}/_bmad/memory/oam/index.md` to record the new profile, and append an `[Owen]` note to `{project-root}/_bmad/memory/oam/daily/YYYY-MM-DD.md`. Then route the user into their actual job (Step 4 of intake) now that the profile gives everyone context. Keep only *org structure* in memory — never put a specific analysis's data here; that travels with the deliverable.
