# second-brady

Portable, markdown-first personal knowledge system with an LLM-maintained wiki layer.

## Principles

- Markdown is the source of truth.
- Tools are optional and replaceable.
- Git tracks history.
- Syncthing syncs working copies.
- The LLM maintains the wiki; the human curates sources and asks questions.

## Folders

- `inbox/` — quick capture, messy allowed
- `journal/` — daily logs (human-written) and `log.md` (LLM-written operation log)
- `notes/` — the wiki: durable, interlinked, LLM-maintained knowledge
- `projects/` — active work and working notes
- `references/` — templates, clipped articles, external documents (immutable sources)
- `agents/` — agent instructions and workflows

## Core workflow

1. Capture quickly into `inbox/`
2. Journal daily in `journal/`
3. Drop sources into `references/clipped/` or `references/docs/`
4. Ask the LLM to ingest sources — it updates `notes/`, `notes/index.md`, and `journal/log.md`
5. Query the wiki by asking questions — the LLM reads the index, finds pages, synthesizes answers
6. Periodically lint the wiki for orphans, contradictions, and gaps
7. Review `inbox/` weekly; promote what's proven useful

## Rules

- Do not over-organize capture.
- Distill only when something proves useful more than once.
- Prefer standard markdown links.
- Keep evergreen notes focused: one concept per note.
- Keep AI-readable structure simple and boring.
