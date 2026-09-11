# Wiki Schema — Meridian Capstone Research Wiki

This file tells the LLM (and any teammate) how this wiki is organized
and maintained. It is the schema layer of the three-layer pattern
described at
https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f.

## Architecture

- **Raw sources** (`../../raw/`) — immutable. Read, never edited by
  the wiki. Ingested sources are listed in `log.md`.
- **Wiki** (this directory) — LLM-generated and LLM-owned. Entity
  pages (`entities/`), concept pages (`concepts/`), analysis pages
  (`analysis/`), plus `index.md` (catalog) and `log.md` (activity
  log).
- **This file** — schema layer. Update it when conventions or
  workflows change.

## Page conventions

- Every page opens with: title (H1), a one-line type/date header, and
  a **Sources** line citing the raw file(s) it draws from.
- Links are plain relative markdown (`[text](../entities/foo.md)`),
  never Obsidian `[[wikilinks]]` — this wiki is read on GitHub.
- Entity and concept pages include a **Related** section linking to
  other wiki pages.
- No fixed page length. A one-paragraph stub with a Related list is a
  valid page when the source material is thin.

## Workflows

**Ingest** — when a new raw source is added:
1. Check it against `../data-handling-checklist.md`'s classification
   gate *before* reading it into this wiki. Restricted files (loyalty
   data, labor data, anything with a customer/employee identifier) are
   never summarized into a wiki page.
2. Read the source, then update every page it touches: create or
   revise entity/concept/analysis pages, add new pages to `index.md`,
   and append an entry to `log.md`.

**Query** — answer a question by reading the relevant wiki pages and
citing them. If the answer is worth keeping, write it as (or fold it
into) an analysis page.

**Lint** — manual, on-request pass: look for orphan pages (nothing
links to them, and they're not in `index.md`), stale claims, and
contradictions between pages. Not run automatically.

## Data handling

See `../data-handling-checklist.md` for the full classification rules.
Summary: sales totals by store/week and store attributes are safe;
loyalty and labor data (and anything with a customer/employee
identifier) are never ingested into this wiki; aggregates derived from
restricted data need sign-off first. This wiki links to the checklist
rather than mechanically enforcing it — the person running an ingest
is responsible for checking first.
