# Research Wiki Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the first version of the Meridian capstone research wiki (`docs/wiki/`) by ingesting `raw/client-brief.md` — schema layer, entity/concept/analysis pages, and navigation files.

**Architecture:** Three-layer LLM-wiki pattern. `raw/` (source, untouched) → `docs/wiki/` (generated markdown pages, this plan's output) → `docs/wiki/CLAUDE.md` (schema/conventions, also this plan's output). No code, no build step — every task produces one or more markdown files reviewed by reading them.

**Tech Stack:** Markdown only. No frameworks, no test runner.

**Spec:** `docs/superpowers/specs/2026-09-11-research-wiki-design.md`

## Global Constraints

- Links are plain relative markdown links (`[text](path.md)`), never `[[wikilinks]]` — spec §Page conventions.
- Every page opens with: title, page type (entity/concept/analysis), last-updated date (`2026-09-11` for all pages in this plan), and a **Sources** line.
- Entity and concept pages include a **Related** list of links to other wiki pages.
- Only `raw/client-brief.md` is ingested in this plan — no other raw source exists yet (spec §Out of scope).
- No restricted data (per `docs/data-handling-checklist.md`) appears anywhere in the wiki — not applicable yet since the only source is the brief itself, but the loyalty-program page must flag that the *underlying* data (not the brief's mention of it) is restricted.
- No git commit step in this plan — user will review and commit manually (confirmed in chat 2026-09-11).

---

### Task 1: Schema layer

**Files:**
- Create: `docs/wiki/CLAUDE.md`

**Interfaces:**
- Consumes: nothing (first file written)
- Produces: the conventions every later task's pages must follow (page header format, link style, workflow names) — referenced informally by Tasks 2–6, not by code.

**Done looks like:** `docs/wiki/CLAUDE.md` exists and documents the three-layer architecture, the three workflows (ingest/query/lint), the page conventions, and links to `docs/data-handling-checklist.md` for what's safe to ingest.

**How to check it:** Open the file and confirm all four sections (Architecture, Page conventions, Workflows, Data handling) are present and each is at least a real paragraph — not a heading with no content under it.

- [ ] **Step 1: Write `docs/wiki/CLAUDE.md`**

```markdown
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
```

- [ ] **Step 2: Verify**

Run: `test -f "docs/wiki/CLAUDE.md" && echo OK`
Expected: `OK`

---

### Task 2: Entity pages

**Files:**
- Create: `docs/wiki/entities/meridian-markets.md`
- Create: `docs/wiki/entities/dana-okafor.md`
- Create: `docs/wiki/entities/marcus.md`

**Interfaces:**
- Consumes: `raw/client-brief.md` (source material)
- Produces: `entities/meridian-markets.md`, `entities/dana-okafor.md`, `entities/marcus.md` — linked from concept pages (Task 3) and analysis pages (Task 4) via their Related sections.

**Done looks like:** Three entity pages exist, each following the header/Sources/Related convention from Task 1, containing only facts traceable to `client-brief.md`.

**How to check it:** Read each page side by side with `raw/client-brief.md` — every factual claim (revenue, store count, employee count, dates, roles) should trace to a specific line in the brief. No invented details (e.g., no made-up last name for Marcus).

- [ ] **Step 1: Write `docs/wiki/entities/meridian-markets.md`**

```markdown
# Meridian Markets

Type: entity | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

Specialty grocery chain with 14 stores across Los Angeles, Orange, and
Ventura counties. Roughly $78M in annual revenue, about 620 employees.
Competes on prepared foods, local sourcing, and a smaller footprint
than national chains.

Grew from 6 stores to 14 in five years, mostly by taking over leases
from chains that pulled out of neighborhoods Meridian considered
underserved (see [Expansion Strategy](../concepts/expansion-strategy.md)).

Migrated to a new POS system in spring 2026, described in the brief as
an improvement.

## Related

- [Dana Okafor](dana-okafor.md) — VP of Operations, primary contact
- [Marcus](marcus.md) — IT contact
- [Expansion Strategy](../concepts/expansion-strategy.md)
- [Data Sources](../concepts/data-sources.md)
```

- [ ] **Step 2: Write `docs/wiki/entities/dana-okafor.md`**

```markdown
# Dana Okafor

Type: entity | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

VP of Operations at [Meridian Markets](meridian-markets.md). Wrote the
client brief (August 2026) and is the primary point of contact for the
engagement.

Email is the best way to reach her; she travels Tuesdays and
Wednesdays and is slow to reply. Her assistant can schedule time but
cannot answer analytics questions.

## Related

- [Meridian Markets](meridian-markets.md)
- [Marcus](marcus.md)
- [Interview Questions](../analysis/interview-questions.md)
```

- [ ] **Step 3: Write `docs/wiki/entities/marcus.md`**

```markdown
# Marcus

Type: entity | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

IT contact at [Meridian Markets](meridian-markets.md). Will pull a
data extract once the NDA is signed. The brief gives no last name or
further detail — this page is a stub pending more information.

## Related

- [Meridian Markets](meridian-markets.md)
- [Data Sources](../concepts/data-sources.md)
```

- [ ] **Step 4: Verify**

Run: `test -f "docs/wiki/entities/meridian-markets.md" && test -f "docs/wiki/entities/dana-okafor.md" && test -f "docs/wiki/entities/marcus.md" && echo OK`
Expected: `OK`

---

### Task 3: Concept pages

**Files:**
- Create: `docs/wiki/concepts/expansion-strategy.md`
- Create: `docs/wiki/concepts/loyalty-program.md`
- Create: `docs/wiki/concepts/data-sources.md`
- Create: `docs/wiki/concepts/engagement-timeline.md`

**Interfaces:**
- Consumes: `raw/client-brief.md`, and links into the entity pages from Task 2 (`../entities/meridian-markets.md` etc.)
- Produces: four concept pages — linked from analysis pages (Task 4) via their Related sections.

**Done looks like:** Four concept pages exist, each following the Task 1 conventions, and the loyalty-program page explicitly flags the underlying data as restricted (not just as a neutral fact).

**How to check it:** Read each page against the brief. Confirm `loyalty-program.md` contains an explicit restricted-data flag (not just "40,000 members") — grep for "restricted" in that file.

- [ ] **Step 1: Write `docs/wiki/concepts/expansion-strategy.md`**

```markdown
# Expansion Strategy

Type: concept | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

[Meridian Markets](../entities/meridian-markets.md) grew from 6 to 14
stores in five years by taking over leases from chains pulling out of
neighborhoods it considered underserved.

Leadership sees the Pasadena site as "the obvious next step" but wants
data to justify it before committing, rather than relying on instinct
and a spreadsheet as past expansion decisions have. The requested
dashboard — sales performance by store and by category — exists to
support this decision.

Open question: whether Pasadena is a signed lease, a letter of intent,
or just an internally favored target. See
[Open Questions](../analysis/open-questions.md).

## Related

- [Meridian Markets](../entities/meridian-markets.md)
- [Data Sources](data-sources.md)
- [Open Questions](../analysis/open-questions.md)
```

- [ ] **Step 2: Write `docs/wiki/concepts/loyalty-program.md`**

```markdown
# Loyalty Program

Type: concept | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

[Meridian Markets](../entities/meridian-markets.md) runs a loyalty
program with roughly 40,000 members. Per the brief, Dana Okafor
doesn't believe the company has meaningfully used this data before,
and understanding the customer better is a stated goal of the
engagement.

**🔴 Restricted.** Per
[docs/data-handling-checklist.md](../../data-handling-checklist.md),
loyalty program membership and purchase history — and any excerpt of
it — must never be pasted, uploaded, or otherwise read into an AI
tool, including this wiki. Any future analysis of this data happens
outside AI-assisted tooling; only sign-off aggregates may later be
brought in, case by case.

## Related

- [Meridian Markets](../entities/meridian-markets.md)
- [Data Sources](data-sources.md)
```

- [ ] **Step 3: Write `docs/wiki/concepts/data-sources.md`**

```markdown
# Data Sources

Type: concept | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

The brief names four data sources [Marcus](../entities/marcus.md) can
extract once the NDA is signed:

| Source | Classification | Notes |
|---|---|---|
| POS transactions (~3 years) | 🟢 Safe, conditional | Only if there's no customer/loyalty ID column — check before use |
| Loyalty program membership & purchase history (~40,000 members) | 🔴 Restricted | Never into an AI tool — see [Loyalty Program](loyalty-program.md) |
| Labor scheduling & hours | 🔴 Restricted | Never into an AI tool |
| Store attributes (sq ft, opening date, lease terms) | 🟢 Safe | Explicitly cleared in the brief |

Full rules: [docs/data-handling-checklist.md](../../data-handling-checklist.md).

## Related

- [Marcus](../entities/marcus.md)
- [Loyalty Program](loyalty-program.md)
- [Open Questions](../analysis/open-questions.md)
```

- [ ] **Step 4: Write `docs/wiki/concepts/engagement-timeline.md`**

```markdown
# Engagement Timeline

Type: concept | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

The brief estimates an eight-week engagement. Dana Okafor asked for
something to show the board when it meets "in three weeks" from the
brief's date (August 2026) — a preliminary deliverable, even if
incomplete.

An NDA follows the brief and covers all data listed in
[Data Sources](data-sources.md). [Marcus](../entities/marcus.md) can
only pull the data extract once that NDA is signed — exact signing
date not stated in the brief.

## Related

- [Meridian Markets](../entities/meridian-markets.md)
- [Dana Okafor](../entities/dana-okafor.md)
```

- [ ] **Step 5: Verify**

Run: `for f in expansion-strategy loyalty-program data-sources engagement-timeline; do test -f "docs/wiki/concepts/$f.md" || echo "MISSING $f"; done; grep -l "Restricted" docs/wiki/concepts/loyalty-program.md`
Expected: no `MISSING` lines printed, and the grep prints `docs/wiki/concepts/loyalty-program.md`

---

### Task 4: Analysis pages

**Files:**
- Create: `docs/wiki/analysis/open-questions.md`
- Create: `docs/wiki/analysis/interview-questions.md`

**Interfaces:**
- Consumes: entity pages (Task 2), concept pages (Task 3)
- Produces: `analysis/interview-questions.md` — this plan's primary deliverable — and `analysis/open-questions.md`, both linked from `index.md` (Task 5).

**Done looks like:** `open-questions.md` lists concrete gaps/ambiguities in the brief (not generic placeholders); `interview-questions.md` has a real, grouped question list ready to bring to the Dana interview.

**How to check it:** Read `interview-questions.md` and confirm every question is answerable-by-Dana and specific to something the brief left open (not a generic "tell me about your business" question). Cross-check each open question in `open-questions.md` against the brief to confirm it's genuinely unstated, not something the brief already answers.

- [ ] **Step 1: Write `docs/wiki/analysis/open-questions.md`**

```markdown
# Open Questions

Type: analysis | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

Gaps and ambiguities in the client brief, worth resolving before or
during the Dana interview.

- **Pasadena site status** — is it a signed lease, a letter of intent,
  or an internally favored target with no paper yet? The brief calls
  it "the obvious next step" but doesn't say how committed it is. See
  [Expansion Strategy](../concepts/expansion-strategy.md).
- **Category definitions** — the brief asks for "sales performance by
  store and by category" but never defines what the category taxonomy
  is (prepared foods vs. grocery vs. local-sourced, etc.).
- **What "better decisions" means concretely** — the brief lists
  increasing revenue, reducing costs, and improving customer
  experience as success criteria, but doesn't say how these will be
  measured or which one matters most if they trade off.
- **Competitor identity** — "national chains" is named as the
  competitive set, but no specific competitors are named.
- **Current reporting state** — the brief says decisions have been
  made "on instinct and a spreadsheet," but doesn't describe what that
  spreadsheet contains or who maintains it today.
- **Dashboard audience** — the brief says "leadership" will use the
  dashboard but only Dana is named. Unclear who else needs access or
  input.
- **NDA signing status** — unclear whether the NDA mentioned in the
  brief has been signed yet, which gates when
  [Marcus](../entities/marcus.md) can pull the data extract. See
  [Engagement Timeline](../concepts/engagement-timeline.md).
- **Board meeting date** — "three weeks" from the brief's date isn't
  an exact date, and the brief's own date is approximate ("August
  2026").
- **POS transaction schema** — whether the raw POS extract will
  include a customer/loyalty ID column is unknown until it arrives;
  this determines whether it's safe-to-use or restricted per
  [Data Sources](../concepts/data-sources.md).

## Related

- [Interview Questions](interview-questions.md)
- [Expansion Strategy](../concepts/expansion-strategy.md)
- [Data Sources](../concepts/data-sources.md)
```

- [ ] **Step 2: Write `docs/wiki/analysis/interview-questions.md`**

```markdown
# Interview Questions — Dana Okafor

Type: analysis | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md), [Open Questions](open-questions.md)

Draft question list for the stakeholder interview, grouped by topic.
Each question targets a gap identified in
[Open Questions](open-questions.md).

## Data & systems

1. Will the POS extract include any customer or loyalty ID column, or
   will it be fully anonymized transaction data?
2. What does the current "spreadsheet" process look like — what's in
   it, who updates it, how often?
3. Has the NDA been signed yet, and when should we expect Marcus's
   data extract?
4. Since the POS migration last spring, is historical (pre-migration)
   data in the same format, or will we be reconciling two schemas?

## Expansion & Pasadena

5. What's the actual status of the Pasadena site — signed lease,
   letter of intent, or still just a candidate?
6. What specifically would make Pasadena a "yes" versus a "no" — is
   there a target revenue, payback period, or sales-per-square-foot
   threshold in mind?
7. Beyond Pasadena, are there other candidate sites being considered,
   or is this a single-site decision right now?

## Goals & success metrics

8. Of "increase revenue, reduce operating costs, improve customer
   experience," is there a priority order, or are all three equally
   weighted for this engagement?
9. How is "category" defined for the sales-by-category dashboard —
   what's the taxonomy (prepared foods, local-sourced, grocery,
   etc.)?
10. What would make the board-meeting preview a success, even in
    preliminary form — what's the one number or chart they'd want to
    see first?

## Timeline & stakeholders

11. Who besides Dana will use the dashboard, and does anyone else need
    to review analytics questions before the board meeting?
12. What's the exact date of the board meeting in three weeks?
13. Is Dana the right person to sign off on scope changes, or does
    that go through someone else at Meridian?

## Related

- [Open Questions](open-questions.md)
- [Dana Okafor](../entities/dana-okafor.md)
```

- [ ] **Step 3: Verify**

Run: `test -f "docs/wiki/analysis/open-questions.md" && test -f "docs/wiki/analysis/interview-questions.md" && echo OK`
Expected: `OK`

---

### Task 5: Navigation files

**Files:**
- Create: `docs/wiki/index.md`
- Create: `docs/wiki/log.md`

**Interfaces:**
- Consumes: every page created in Tasks 1–4 (needs their titles/paths to catalog them)
- Produces: the navigation entry points a reader (or the next ingest) starts from.

**Done looks like:** `index.md` lists all 9 content pages (3 entity, 4 concept, 2 analysis) grouped by category, each with a one-line summary. `log.md` has one entry recording this ingest.

**How to check it:** Count the linked pages in `index.md` — should be 9. Confirm `log.md` has exactly one dated entry naming `client-brief.md` as the source and listing the pages it touched.

- [ ] **Step 1: Write `docs/wiki/index.md`**

```markdown
# Wiki Index

Catalog of every page in the research wiki. Updated on every ingest.

## Entities

- [Meridian Markets](entities/meridian-markets.md) — the client: 14-store specialty grocery chain in LA/Orange/Ventura counties
- [Dana Okafor](entities/dana-okafor.md) — VP of Operations, primary contact
- [Marcus](entities/marcus.md) — IT contact, will provide the data extract

## Concepts

- [Expansion Strategy](concepts/expansion-strategy.md) — how Meridian has grown, and the Pasadena decision
- [Loyalty Program](concepts/loyalty-program.md) — ~40,000-member program; underlying data is restricted
- [Data Sources](concepts/data-sources.md) — the four data sources named in the brief, and their classification
- [Engagement Timeline](concepts/engagement-timeline.md) — eight-week engagement, three-week board preview

## Analysis

- [Open Questions](analysis/open-questions.md) — gaps and ambiguities in the client brief
- [Interview Questions](analysis/interview-questions.md) — draft question list for the Dana interview

## Log

See [log.md](log.md) for the full ingest/query/lint history.
```

- [ ] **Step 2: Write `docs/wiki/log.md`**

```markdown
# Activity Log

Append-only. One entry per ingest, query, or lint pass.

---

**2026-09-11 — INGEST — raw/client-brief.md**

Pages created: entities/meridian-markets.md, entities/dana-okafor.md,
entities/marcus.md, concepts/expansion-strategy.md,
concepts/loyalty-program.md, concepts/data-sources.md,
concepts/engagement-timeline.md, analysis/open-questions.md,
analysis/interview-questions.md, index.md.

Notes: first ingest for the capstone wiki. Source contains no
restricted data itself (it's Dana's brief, not an extract), but
flags two future sources (loyalty, labor) as restricted per
docs/data-handling-checklist.md — captured in
concepts/loyalty-program.md and concepts/data-sources.md.
```

- [ ] **Step 3: Verify**

Run: `grep -c '\.md)' docs/wiki/index.md`
Expected: at least `9` (one per content page link; `log.md`'s own link is separate)

---

### Task 6: Cross-link validation

**Files:**
- Read-only: all files created in Tasks 1–5

**Interfaces:**
- Consumes: every file from Tasks 1–5
- Produces: nothing new — this is the plan's final check.

**Done looks like:** Every relative markdown link in `docs/wiki/` resolves to a real file. No broken links, no orphan pages (every content page appears in `index.md`).

**How to check it:** Run the link-check command below. It should print nothing (no output = no broken links). Then manually confirm all 9 content pages appear in `index.md` (already counted in Task 5, re-confirm here after all files exist).

- [ ] **Step 1: Check every relative link resolves**

Run this from the repo root:

```bash
cd docs/wiki
for f in $(find . -name '*.md'); do
  dir=$(dirname "$f")
  grep -oE '\]\([^)]+\)' "$f" | sed -E 's/^\]\((.*)\)$/\1/' | while read -r link; do
    target="$dir/$link"
    if [ ! -f "$target" ]; then
      echo "BROKEN LINK in $f -> $link"
    fi
  done
done
cd - > /dev/null
```

Expected: no output.

- [ ] **Step 2: Confirm no orphan pages**

Run: `for f in entities/meridian-markets entities/dana-okafor entities/marcus concepts/expansion-strategy concepts/loyalty-program concepts/data-sources concepts/engagement-timeline analysis/open-questions analysis/interview-questions; do grep -q "$f.md" docs/wiki/index.md || echo "ORPHAN: $f"; done`
Expected: no output.

- [ ] **Step 3: Report done**

If both checks pass with no output, the wiki build is complete. Tell the user: all 12 files created (1 schema + 9 content pages + 2 nav files), all links resolve, no orphans. Remind them the commit step is manual, per their earlier choice.
