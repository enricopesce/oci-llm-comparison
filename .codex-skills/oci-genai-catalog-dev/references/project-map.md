# Project Map

## Repo shape

- `index.html`: only app shell; contains all CSS, markup, wizard UI, filter UI, and inline JS.
- `models.json`: native OCI catalog source of truth.
- `imported-models.json`: imported/open-weight catalog source of truth.
- `sw.js`: small navigation-only service worker.
- `README.md`: public-facing project description.
- `.codex-skills/oci-genai-catalog-dev/`: local maintenance skill, references, and audit helper.

## Runtime flow

1. `index.html` clears all `tbody` elements inside `#referenceView`.
2. It fetches `models.json` and `imported-models.json` with `Promise.all(...)`.
3. `renderAll()` fills native chat/embed/rerank sections from `models.json`.
4. `renderImported()` fills imported family tables from `imported-models.json`.
5. `initFilters()` snapshots rendered rows and applies text-based filtering.
6. Wizard data is derived from JSON, not from the static recommendation cards.

## Fields that matter for behavior

- Native wizard behavior depends on `wizardTasks`, `wizardTier`, `wizardCtx`, `wizardWhy`, `regions`, `callType`, and `fineTunable`.
- Native row rendering normalizes `reasoning || thinking` and `toolUse || agentic`.
- Imported rows depend on `family.id`, `badge`, `label`, and each model's `type`, `params`, `activeParams`, `contextWindowLabel`, `contextClass`, `clusterShape`, and `clusterClass`.

## Hardcoded sync points in `index.html`

- Header updated date.
- JSON-LD `dateModified`.
- Stat-bar counts.
- Imported-section summary counts (`provider families` and `models`).
- Footnote data-source date.
- Provider/family mount points used by `renderAll()` and `renderImported()`.

## Known traps

- Fetch failure leaves the page with empty tables because the script clears table bodies before loading data.
- Imported-model HTML rows are duplicated fallback content. Runtime replaces them, so JSON edits are the real data changes.
- Filter chips use row text plus `data-tags`, with OR inside a chip group and AND across groups.
- Embedding recommendations ignore tier, deployment, and region filtering even though the wizard still asks those questions.
- The repo root contains a zero-byte `.codex` file; do not assume it is a directory for project-local Codex config.

## Practical commands

- Audit data sync points:
  - `python3 /home/opc/source/ocillms/.codex-skills/oci-genai-catalog-dev/scripts/catalog_audit.py --repo /home/opc/source/ocillms`
- Serve locally:
  - `python3 -m http.server 8080`
- Count current model totals quickly:
  - `jq '{chat: (.chatModels|length), embed: (.embeddingModels|length), rerank: (.rerankModels|length)}' models.json`
  - `jq '{families: (.families|length), imported: ([.families[].models[]] | length)}' imported-models.json`
