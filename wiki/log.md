# Log

Chronological, append-only record of wiki activity. Newest at the bottom.
Each entry uses a greppable header — `grep "^## \[" wiki/log.md | tail -5` shows recent
activity. Prefixes: `ingest | `, `query | `, `lint | `, `decision | `, `note | `.

## [2026-06-02] note | Wiki initialized
- Scaffolded the LLM Wiki for the deck rebuild project.
- Created schema [[CLAUDE]], [[overview]], [[index]], and category folders
  (entities, concepts, decisions, sources).
- Ready to ingest the first source.

## [2026-06-02] note | Restructured to standalone repo "deck-rebuild-wiki"
- Removed the unrelated Tutorial-Codebase-Knowledge project files.
- Promoted the wiki to the repo root (CLAUDE.md, raw/, wiki/ now at top level).
- Project name: deck-rebuild-wiki.
