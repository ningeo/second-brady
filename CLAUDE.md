# CLAUDE.md

This is a personal knowledge base. The LLM maintains a wiki in `notes/`; the human curates sources and writes journal entries.

## Repository structure

```
inbox/              # raw captures, messy allowed
journal/            # human-written daily entries (YYYY/YYYY-MM-DD.md)
journal/log.md      # LLM-written, append-only operation log
notes/              # THE WIKI — LLM-maintained, interlinked evergreen pages
notes/index.md      # flat catalog of all wiki pages, updated on every ingest
projects/           # active work notes (do not reorganize prematurely)
references/clipped/ # web clips and articles (immutable sources)
references/docs/    # longer documents and references (immutable sources)
references/templates/daily-journal-template.md
agents/librarian.md # the schema: conventions, operations, triage rules
```

## Key rules

- `references/` is immutable. Never modify source files — only read from them.
- `journal/` daily entries are human-written. Never edit them. `journal/log.md` is LLM-written.
- `notes/` is the wiki. The LLM creates, updates, and cross-links pages here.
- `notes/index.md` is a flat list of every wiki page with a one-line summary. Read it first when answering queries. Update it on every ingest.
- One concept per note in `notes/`. Use literal titles and simple filenames.
- Prefer standard markdown links. Minimal frontmatter. No invented taxonomy.
- Promote from `inbox/` to `notes/` only when something has been useful twice or would be painful to rediscover.

## Operations

### Triage inbox
When asked to triage an inbox item (or review the whole inbox):
1. Read the item
2. Recommend one of:
   - **Stay in inbox** — still raw, not yet proven useful
   - **Promote to `notes/`** — durable knowledge, useful twice or painful to rediscover
   - **Clip to `references/`** — contains source material worth ingesting (move to `references/clipped/` or `references/docs/`, then run ingest)
   - **Delete** — no longer relevant
3. Apply only with user approval

### Ingest a source
1. Read the source from `references/`
2. Discuss key takeaways with the user
3. Create or update relevant pages in `notes/`
4. Update `notes/index.md`
5. Append to `journal/log.md`: `## [YYYY-MM-DD] ingest | Source Title`

### Answer a query
1. Read `notes/index.md` to find relevant pages
2. Read those pages and any referenced sources
3. Synthesize an answer citing wiki pages and sources
4. If the answer is durable, offer to file it as a new page in `notes/`

### Lint the wiki
1. Scan for orphans, contradictions, stale claims, missing cross-references, concepts without pages
2. Report findings — apply fixes only with user approval
3. Append to `journal/log.md`: `## [YYYY-MM-DD] lint | Summary`

## What not to do

- Do not create files speculatively. No stubs, no empty templates, no placeholder pages.
- Do not reorganize `inbox/` or `projects/` without being asked.
- Do not add elaborate metadata, categories, or hierarchies.
- Do not modify source files in `references/`.
- Do not mix automated content into human journal entries.
