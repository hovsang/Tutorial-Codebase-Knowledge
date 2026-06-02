# Deck Rebuild Wiki

A personal, LLM-maintained knowledge base for designing and rebuilding my backyard deck.

Built on the [LLM Wiki](https://github.com/) pattern: instead of re-deriving knowledge
from raw documents on every question (RAG), an LLM agent **incrementally builds and
maintains a persistent, interlinked markdown wiki** between me and my sources. The
knowledge is compiled once and kept current — cross-references, summaries, and flagged
contradictions accumulate over time.

## Layout

```
CLAUDE.md      # the schema — how the wiki is structured + the LLM's workflows
raw/           # immutable source documents (specs, notes, articles, photos) — read only
  assets/      # downloaded images referenced by sources
wiki/          # LLM-generated, interlinked knowledge base
  index.md     # catalog of every page
  log.md       # chronological activity log
  overview.md  # the project at a glance
  entities/    # vendors, materials, components, tools
  concepts/    # codes, span tables, flashing, finishes, load calcs
  decisions/   # design & material choices (ADR-style)
  sources/     # one summary per ingested source
```

## How to use it

1. Drop a source (spec PDF, contractor note, article, site photo) into `raw/`.
2. Tell the agent to **ingest** it — it reads, discusses, summarizes, and files the
   knowledge across the wiki.
3. **Query** the wiki with questions; good answers get filed back as new pages.
4. Periodically ask the agent to **lint** — find contradictions, gaps, and stale claims.

Open the folder as an [Obsidian](https://obsidian.md) vault to browse links and the graph
view. The agent's full operating manual is in [`CLAUDE.md`](./CLAUDE.md).
