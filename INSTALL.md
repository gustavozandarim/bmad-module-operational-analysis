# Installing the Operational Analysis Method (OAM)

A focused install + post-install guide. For what the module *is*, see [README.md](./README.md).

## Prerequisites

- **BMad Method** installed (the installer below pulls BMM; OAM rides alongside it).
- **Node.js 20.12+** (for `npx bmad-method`).
- **Python 3.11+** available as `python3` (used by the BMad customization resolver and the setup scripts — standard library only, no `pip install`).
- A supported agent tool — these instructions assume **Claude Code** (`--tools claude-code`).

## Install

Replace `gustavozandarim` with the account/org hosting this repository:

```bash
npx bmad-method install \
  --modules bmm \
  --custom-source https://github.com/gustavozandarim/bmad-module-operational-analysis \
  --tools claude-code \
  --yes
```

This installs BMM and the OAM custom module, placing the OAM skills where your tool discovers them and the OAM module data under `_bmad/`.

## Post-install

### 1. Windows users — apply the resolver UTF-8 fix

OAM's agents use emoji icons. On a Windows console using the legacy **cp1252** encoding, the core resolver script crashes when it serializes an emoji. The agents fall back to manual resolution (so they still work), but you'll see a traceback each launch until you fix it.

Edit `_bmad/scripts/resolve_customization.py` and add a self-contained UTF-8 guard at the **top of `main()`**, before any stdout write — no environment variable needed:

```python
def main():
    # Windows consoles default to cp1252, which cannot encode emoji icons.
    if sys.platform == "win32":
        sys.stdout.reconfigure(encoding="utf-8")
    ...
```

Or, without editing code, set the environment variable before each session:

```bash
# PowerShell
$env:PYTHONIOENCODING = "utf-8"
# bash
export PYTHONIOENCODING=utf-8
```

> `resolve_customization.py` is a **core BMad script** and is overwritten by reinstalls — re-apply the code fix afterward, or use the env-var approach.

### 2. Run module setup

Invoke **`oam-setup`** in your project. It:
- scaffolds the shared memory tree at `_bmad/memory/oam/`,
- installs the Analytical Integrity Constitution + conventions there,
- collects config — `oam_output_folder` (project-level, where deliverables are saved) and `default_profile` (personal),
- registers OAM's capabilities with `bmad-help`,
- and, on a fresh install with no profiles, hands off to Owen for onboarding.

## First run

1. Talk to **Owen** (🧭). With zero profiles he runs **first-run onboarding** — a few questions about your organization (vocabulary, strategic priorities, data sources) — and writes your first profile plus a seed taxonomy.
2. Then start an analysis: describe your job (*"structure our Q1 operating costs"*, *"monthly OEE review"*) and Owen detects the mode and routes you to Diana → Helena → Sofia, or to the quick track.
3. Power users can summon any specialist directly instead of going through Owen.

## Updating

Re-run the install command with the same `--custom-source` to pull a newer version of the module. Your **memory** (`_bmad/memory/oam/profiles/...`) and your **deliverables** (`oam_output_folder`) are not part of this repo and are preserved across updates. After any reinstall, re-apply the Windows resolver fix (see above).

## Uninstall / clean

Remove the OAM skill folders from your tool's skills directory and the `_bmad/memory/oam/` tree. Deliverables in your output folder are independent and can be kept or removed separately.
