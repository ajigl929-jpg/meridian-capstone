# Research Wiki — Design Spec

Date: 2026-09-11
Status: Approved by user (chat, 2026-09-11)

## Purpose

A persistent, compounding knowledge base for the full Meridian Markets
capstone (not just interview prep), following the LLM-wiki pattern
described at
https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f.
Starts with one ingest — `raw/client-brief.md` — and is designed to
grow across all four workshops as more sources (data extracts, meeting
notes) arrive.

## Architecture — three layers

- **Raw sources** (`raw/`) — immutable, human-curated. The wiki reads
  this layer but never edits it. Currently: `client-brief.md`. Future:
  POS/loyalty/labor extracts once Marcus delivers them, meeting notes,
  etc.
- **Wiki** (`docs/wiki/`) — LLM-generated and LLM-owned markdown:
  entity pages, concept pages, analysis pages, plus `index.md` and
  `log.md`.
- **Schema** (`docs/wiki/CLAUDE.md`) — tells the LLM how the wiki is
  organized, what conventions to follow, and what workflows
  (ingest/query/lint) to run. Links to
  `docs/data-handling-checklist.md` for what raw material is safe to
  ingest.

## Directory structure

```
docs/wiki/
  CLAUDE.md          # schema layer
  index.md           # catalog of every page
  log.md             # append-only activity log
  entities/
    meridian-markets.md
    dana-okafor.md
    marcus.md
  concepts/
    expansion-strategy.md
    loyalty-program.md
    data-sources.md
    engagement-timeline.md
  analysis/
    open-questions.md
    interview-questions.md
```

## Page conventions

- Every page opens with a short header: title, page type
  (entity/concept/analysis), last-updated date.
- A **Sources** line citing which raw file(s) the page draws from.
- Entity and concept pages include a **Related** list linking to other
  wiki pages (plain relative markdown links, e.g.
  `[Dana Okafor](../entities/dana-okafor.md)` — chosen over Obsidian
  `[[wikilinks]]` so pages render correctly on GitHub with no extra
  tooling).
- No fixed length — a page is as long as the source material
  supports. A one-paragraph stub with a Related list is a valid page
  at this stage.

## Workflows (defined in `docs/wiki/CLAUDE.md`)

- **Ingest** — when a new raw source is added, read it, then update
  every wiki page it touches: entities, concepts, `index.md`, and a
  new entry in `log.md`. Before ingesting, check the source against
  `docs/data-handling-checklist.md`'s classification gate — restricted
  files are never summarized into wiki pages.
- **Query** — answer a question by reading relevant wiki pages and
  citing them; a durable answer becomes (or updates) an analysis page.
- **Lint** — manual, on-request pass checking for orphan pages, stale
  claims, and contradictions between pages. Not run automatically.

## First ingest — deliverable for this session

Ingesting `client-brief.md` produces:

- **Entities**: `meridian-markets.md`, `dana-okafor.md`, `marcus.md`
- **Concepts**: `expansion-strategy.md`, `loyalty-program.md`,
  `data-sources.md`, `engagement-timeline.md`
- **Analysis**: `open-questions.md` (gaps/ambiguities in the brief),
  `interview-questions.md` (the primary deliverable — a draft,
  grouped question list for the Dana interview)
- `index.md` and `log.md`, initialized and populated with this first
  ingest.

## Data handling

The schema doc links to `docs/data-handling-checklist.md` rather than
re-implementing a hard gate. The ingest workflow description in
`docs/wiki/CLAUDE.md` will state that the classification check happens
before any file is added to `raw/`, and that restricted files are
never read into a wiki page — but enforcement is procedural (the
person/LLM doing the ingest follows the checklist), not a mechanical
block.

## Out of scope (for this spec)

- Ingesting anything beyond `client-brief.md` (POS/loyalty/labor
  extracts arrive later, as separate ingests).
- Workshop-per-page logs (superseded by `log.md` for now).
- Any hard/automated enforcement of the data-handling gate.
