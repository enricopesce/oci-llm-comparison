---
name: oci-genai-catalog-dev
description: "Maintain the OCI GenAI Catalog project at /home/opc/source/ocillms. Use when Codex needs to continue development on this static OCI model catalog: updating index.html, syncing models.json or imported-models.json, changing the guided-selection wizard or filter chips, reconciling hardcoded dates and counts with JSON data, or checking whether README.md and the local maintenance docs still reflect the current runtime architecture."
---

# OCI GenAI Catalog Dev

## Overview

Treat `models.json` and `imported-models.json` as the catalog data sources of truth, and treat `index.html` as the UI shell plus all client-side rendering logic. Start by running `scripts/catalog_audit.py --repo /home/opc/source/ocillms` unless the task is a very small copy-only change.

## Workflow

### Establish the current state

- Inspect `/home/opc/source/ocillms` directly; helper docs can drift, so confirm behavior from the code and JSON files.
- Read `references/project-map.md` when the task touches data sync, rendering, filters, wizard behavior, service-worker behavior, or stale docs.
- Check the worktree before editing so you do not overwrite user changes.

### Update the real source of truth first

- Edit `models.json` for native OCI chat, embedding, and rerank data.
- Edit `imported-models.json` for imported/open-weight families and models.
- Only edit the imported-model HTML rows in `index.html` when the page structure itself changes. Runtime rendering clears and replaces those rows from JSON on load.

### Keep `index.html` in sync with JSON-backed data

- Update hardcoded sync points after data changes:
  - header "Updated ..." copy
  - JSON-LD `dateModified`
  - stat-bar counts
  - imported-model intro counts
  - footnote "last updated ..." copy
- If a new native provider appears, update the provider-specific HTML sections and the hardcoded JS mappings in `renderAll()`, `PROVIDER_LABELS`, and `WIZ_PROVIDER`.
- If a new imported family appears, add a matching `.import-family-title[data-family="..."]` section so `renderImported()` has a mount point.

### Change interactive logic carefully

- Native tables render through `renderAll()` and a single `rowChat()` schema.
- Imported tables render through `renderImported()` using `family.id` and `data-family`.
- Filter chips are text-based; new filter controls only work if the row text or `data-tags` exposes matching terms.
- Wizard ranking depends on JSON fields such as `wizardTasks`, `wizardTier`, `wizardCtx`, `wizardWhy`, `regions`, `callType`, and `fineTunable`.
- The embedding wizard path is intentionally special-cased and ignores tier, deployment, and region filtering.
- The page clears all table bodies before fetching JSON; fetch failures leave empty tables. Be careful when changing fetch paths or startup order.

### Validate before finishing

- Run `scripts/catalog_audit.py --repo /home/opc/source/ocillms` after non-trivial changes.
- If you modify this skill, run `python3 /home/opc/.codex/skills/.system/skill-creator/scripts/quick_validate.py /home/opc/source/ocillms/.codex-skills/oci-genai-catalog-dev`.
- For UI or behavior changes, serve the repo locally with `python3 -m http.server 8080` from `/home/opc/source/ocillms` and exercise the reference view, wizard view, filters, and theme toggle.

### Keep supporting docs honest

- Update `README.md` and `references/project-map.md` when architecture or workflow changes materially.
- Treat the code and JSON files as the source of truth when helper docs drift.

## References

- Read `references/project-map.md` for the repo layout, hardcoded sync points, and recurring maintenance traps.

## Bundled helper

- Use `scripts/catalog_audit.py` to compare JSON counts and dates against the hardcoded values still living in `index.html`, and to flag stale project docs that commonly drift.
