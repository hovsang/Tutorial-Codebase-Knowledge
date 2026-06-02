# Log

Chronological, append-only record of wiki activity. Newest at the bottom.
Each entry uses a greppable header — `grep "^## \[" wiki/log.md | tail -5` shows recent
activity. Prefixes: `ingest | `, `query | `, `lint | `, `decision | `, `note | `.

## [2026-06-02] note | Wiki initialized
- Scaffolded the LLM Wiki for the deck rebuild project.
- Created schema [[CLAUDE]], [[overview]], [[index]], and category folders
  (entities, concepts, decisions, sources).
- Ready to ingest the first source.
