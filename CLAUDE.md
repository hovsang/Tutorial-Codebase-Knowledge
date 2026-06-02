# Deck Rebuild Wiki — Schema & Operating Manual

This repository is a **personal LLM-maintained wiki** for designing and building a new
backyard deck (the old one was demolished). It follows the "LLM Wiki" pattern: a
persistent, interlinked markdown knowledge base sits between me (the human curator) and
the raw source documents. **You (the LLM) own and maintain the wiki. I curate sources,
make the real-world decisions, and ask the questions.**

Your job is bookkeeping: read sources, extract what matters, file it into the right
pages, keep cross-references and summaries current, and flag contradictions. You do the
grunt work so the knowledge compounds instead of scattering across chat history.

---

## 1. The three layers

| Layer | Path | Who owns it | Rule |
|-------|------|-------------|------|
| **Raw sources** | `raw/` | The human | **Immutable.** Read only. Never edit or delete. Source of truth. |
| **The wiki** | `wiki/` | You (the LLM) | You create, update, cross-link, and maintain every page. |
| **The schema** | `CLAUDE.md` (this file) | Co-owned | Conventions + workflows. We evolve it together as we learn what works. |

- `raw/` holds spec sheets (PDF), contractor/site notes, clipped articles, and photos.
- `raw/assets/` holds downloaded images referenced by sources.
- `wiki/` holds everything you write. Never put generated pages in `raw/`.

---

## 2. Wiki structure & page taxonomy

```
wiki/
  index.md          # content catalog — every page, one-line summary, by category
  log.md            # chronological append-only record of ingests/queries/lints
  overview.md       # the project at a glance: goals, status, budget, key decisions
  entities/         # concrete nouns: vendors, materials/products, components, tools
  concepts/         # knowledge topics: codes, span tables, ledger flashing, finishes
  decisions/        # design/material decisions with rationale (ADR-style)
  sources/          # one summary page per ingested raw source
```

**Entity pages** (`wiki/entities/`) — concrete things in this project:
- *Vendors & people*: lumberyard, composite-decking supplier, contractor, building
  inspector, permit office. (e.g. `entities/trex-supplier.md`, `entities/inspector.md`)
- *Materials & products*: specific products under consideration or chosen
  (e.g. `entities/trex-transcend-decking.md`, `entities/grk-structural-screws.md`).
- *Components / zones*: physical parts of the deck — `entities/footings.md`,
  `entities/ledger-board.md`, `entities/joists.md`, `entities/railing.md`,
  `entities/stairs.md`, `entities/deck-surface.md`.

**Concept pages** (`wiki/concepts/`) — transferable knowledge, not project-specific objects:
- `concepts/building-codes-permits.md`, `concepts/footing-depth-frost-line.md`,
  `concepts/joist-span-tables.md`, `concepts/ledger-attachment-flashing.md`,
  `concepts/decking-material-comparison.md`, `concepts/fastener-systems.md`,
  `concepts/drainage-ventilation.md`, `concepts/finishes-sealing.md`,
  `concepts/load-calculations.md`.

**Decision pages** (`wiki/decisions/`) — when I make (or lean toward) a real choice,
record it ADR-style: the decision, the options considered, the tradeoffs, the rationale,
and the date. (e.g. `decisions/0001-composite-vs-pressure-treated.md`,
`decisions/0002-deck-size-and-shape.md`.) Link decisions to the concepts and entities
they touch.

**Source pages** (`wiki/sources/`) — one per ingested raw file. The durable summary of
that source plus what it changed in the wiki.

> Create new categories if the project needs them (e.g. `wiki/tasks/` for a build
> punch-list, `wiki/budget/` for cost tracking). Document any new category here in §2.

---

## 3. Page conventions

**Filenames**: lowercase, hyphenated, descriptive. `entities/ledger-board.md`, not
`Ledger Board.md`. Decisions are zero-padded numbered: `decisions/0003-railing-style.md`.

**Links**: use Obsidian wikilinks — `[[ledger-board]]`, `[[joist-span-tables]]`. Link
generously; the graph is the value. Every page should have at least one inbound and one
outbound link (no orphans).

**Frontmatter**: every wiki page starts with YAML so Obsidian Dataview can query it.

```yaml
---
title: Ledger Board
type: entity        # entity | concept | decision | source | overview
tags: [structural, attachment, critical-detail]
status: active      # active | decided | superseded | stub
sources: 2          # count of raw sources informing this page
created: 2026-06-02
updated: 2026-06-02
---
```

**Citations**: when a claim comes from a source, cite it inline with a wikilink to the
source page: `Frost line here is 42" per the local code amendment ([[src-county-deck-code]]).`
Keep claims traceable to `raw/`.

**Units & specifics**: this is a build — be precise. Always carry units (inches, feet,
lbs, °F), product model numbers, code section references, and dimensions. Vagueness is a
bug. When a measurement is mine-from-the-site vs. from-a-spec, say which.

**Safety / code criticality**: tag structurally-critical or code-critical details
(`tags: [critical-detail]`) — ledger attachment, footing depth, joist span, railing load,
guard height. Never soften or guess these; if a source is ambiguous, flag it and suggest
verifying with the local building department.

---

## 4. Operations

### Ingest — "I dropped a file in `raw/`, process it"

1. **Read** the source from `raw/`. For PDFs, read the text. For images
   (`raw/assets/`), view them — I may drop site photos, sketches, or chart screenshots;
   open them explicitly since you can't read inline-image markdown in one pass.
2. **Discuss** the key takeaways with me before writing much. Confirm what matters for
   *my* deck (dimensions, code jurisdiction, budget, the look I want).
3. **Write a source page** in `wiki/sources/` — `src-<short-name>.md` with frontmatter,
   a summary, key facts/specs extracted, and a "Wiki updates from this source" list.
4. **Integrate**, don't just append. Update the relevant entity/concept/decision pages.
   A single source may touch 5–15 pages. When new data **contradicts** an existing page,
   don't silently overwrite — note both, flag the conflict (`> ⚠️ Conflict:` callout),
   and ask me which to trust (newer code? local inspector? manufacturer spec?).
5. **Update `index.md`** — add/adjust catalog entries for any new or changed pages.
6. **Append to `log.md`** — one entry, parseable prefix (see §5).
7. **Update `overview.md`** if the project status, budget, or a key decision moved.

Default to **one source at a time, stay-involved**. Batch only if I ask.

### Query — "answer a question against the wiki"

1. Read `index.md` first to find relevant pages, then drill into them. Prefer the wiki
   over re-reading raw sources, but cite back to raw when precision matters.
2. Synthesize an answer **with citations** (wikilinks to the pages/sources used).
3. Choose the output form that fits: a markdown answer, a comparison table, a
   matplotlib chart (e.g. cost or span comparison), or a Marp slide for sharing with a
   contractor/spouse.
4. **File good answers back into the wiki.** A material comparison, a cost breakdown, a
   "what does code require for my ledger" synthesis — these are valuable. Offer to save
   them as a new concept/decision page so explorations compound. Don't let good analysis
   die in chat.
5. Log the query in `log.md`.

### Lint — "health-check the wiki"

Periodically (or on request), scan for and report:
- **Contradictions** between pages (esp. code/spec numbers).
- **Stale claims** a newer source has superseded (set `status: superseded`).
- **Orphan pages** with no inbound links; add cross-references.
- **Missing pages** — concepts mentioned repeatedly but lacking their own page.
- **Data gaps** I should fill — unknown frost-line depth, missing load rating, an
  un-verified code section. Suggest specific next sources or a web search.
- **Open decisions** that have enough info to now resolve.

Output a prioritized punch-list. Don't fix silently — propose, then act on what I confirm.
Append a `lint` entry to `log.md`.

---

## 5. index.md and log.md

**`index.md` — content-oriented catalog.** Every page listed under its category with a
wikilink, a one-line summary, and source count. Update on every ingest. This is your
table of contents and your first stop when answering a query.

**`log.md` — chronological, append-only.** Newest at the bottom. Each entry starts with a
consistent, greppable header so `grep "^## \[" wiki/log.md | tail -5` shows recent
activity:

```
## [2026-06-02] ingest | County Residential Deck Code (PDF)
- Extracted: frost line 42", guard height 36", ledger lag schedule.
- Updated: [[footing-depth-frost-line]], [[ledger-attachment-flashing]], [[railing]].
- Conflict flagged: code says 42" frost depth vs. contractor's verbal "36" — needs check.

## [2026-06-02] query | composite vs pressure-treated total cost
- Built cost table, filed as [[decisions/0001-composite-vs-pressure-treated]].
```

Prefixes: `ingest | `, `query | `, `lint | `, `decision | `, `note | `.

---

## 6. Working style

- **I curate and decide; you maintain.** I choose materials, set the budget, and make the
  call on tradeoffs. You keep the knowledge organized and surfaced.
- **Be precise and cautious on anything structural or code-related.** When in doubt, say
  "verify with your local building department / inspector" rather than asserting.
- **Touch many pages per pass.** That's the point — the maintenance cost is near zero for
  you and prohibitive for me. Keep cross-references and summaries current every time.
- **Stay involved by default.** Discuss before large rewrites. Surface conflicts instead
  of resolving them silently.
- **Everything is git.** Suggest a commit after a meaningful ingest/lint so the wiki has
  version history.

---

## 7. Tooling notes (optional / as-needed)

- **Obsidian**: open this folder as a vault. Use graph view to spot hubs and orphans;
  Dataview to query frontmatter (status, tags, source counts).
- **Images**: keep downloaded source images in `raw/assets/`. Read a source's text first,
  then open referenced images for detail.
- **Search**: at this scale `index.md` is enough. If the wiki outgrows it, we can add a
  local markdown search (e.g. `qmd`) and document the workflow here.
- **Marp**: for slides to share a plan/quote comparison with a contractor or partner.

---

*This schema is a living document. When we discover a convention that works better,
update this file — future sessions start by reading it.*
