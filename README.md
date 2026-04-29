# Goldsmith & Co — pitch site template

Template for new-business pitch sites. Each new repo created from this
template represents a single Goldsmith pitch (e.g., a search for one firm).

The Goldsmith portal's recruiter view can spawn one of these via the
`POST /api/generate-pitch` endpoint on `gco-query`, which:

1. Generates a `data.json` from a recruiter brief using Claude.
2. Creates a new repo under `Goldsmith-and-co` from this template.
3. Commits the generated `data.json`.
4. Enables GitHub Pages on `main` /.

The site renders 4 sections — KPIs, target talent pool, representative
hires, archetypal profiles — driven entirely by `data.json`. Methodology
dimensions and timeline are kept constant across pitches.

## Editing a generated pitch

Two paths:

- **Locally with Claude Code:**
  ```
  git clone Goldsmith-and-co/<firm-slug>-pitch
  cd <firm-slug>-pitch
  claude
  ```
  Edit `data.json`. Push to `main`. GitHub Pages rebuilds in ~30–60s.

- **In the portal** (Phase 2 — not yet built): inline editor for the
  `data.json` fields, with commit-back via the same GitHub App.

## Fields you'll usually replace

`data.json` ships with placeholder values; the LLM-generated version
replaces some of these but leaves any field that would require
fabricated data as the literal string `[TODO: verify]`. Replace these
before sharing the pitch URL externally:

- `meta.client`, `meta.client_full`, `meta.client_logo` (drop a PNG into
  the repo and reference it by filename), `meta.project_codename`,
  `meta.role`, `meta.prepared_date`
- `kpis[].value`, `universe[].value`
- `track_record[]` — every entry needs a real role + firm description
  + numbers
- `archetypes[].scores.*` — composite scores per dimension
- `archetypes[].facts[].value` — current firm, years of experience
- `talent_pool.creditHedge[]`, `altCredit[]`, `diversifiedAlts[]`,
  `desc{}` — list of relevant firms in each tier; `desc[firmName]`
  populates the hover tooltip

## Constants — don't edit per pitch

- `dimensions[]` — the 5 weighted assessment pillars (Institutional
  Capital Formation, Relationship Management, etc.). Goldsmith IP.
- `timeline[]` — the 13-week 5-phase search funnel.

If you need to change either, do it in this template repo, not in
generated pitches — otherwise the change won't propagate.

## How the templating works

`index.html` loads `data.json` on page load and binds three things:

- `[data-bind="meta.client"]` — element's text content is replaced by
  the resolved path
- `[data-show-when="meta.client_logo"]` — element is hidden when the
  resolved value is empty
- `[data-doc-title]` / `[data-client-logo]` — special-cased for
  document title and the per-firm masthead logo

Inside JS render functions (e.g. `renderPool`), values are interpolated
directly via template literals.
