# Repository Guidelines

## Project Structure & Module Organization

This repository is a Markdown-first personal knowledge system:

- `inbox/` for raw captures; messy material is allowed.
- `journal/` for human daily entries under `journal/YYYY/YYYY-MM-DD.md`.
- `journal/log.md` for LLM-maintained operation notes.
- `notes/` for durable, interlinked evergreen wiki pages.
- `notes/index.md` for the flat wiki catalog and one-line summaries.
- `projects/` for active work notes; do not reorganize prematurely.
- `references/` for source material, templates, and external docs.
- `agents/` for agent instructions and workflows.

## Build, Test, and Development Commands

There is no build system, package manager, or automated test suite:

- `rg --files` lists workspace files quickly.
- `rg -n "search term" notes references journal` finds relevant knowledge entries.
- `git diff --check` catches whitespace and patch formatting issues.
- `git status --short` reviews changed files.

## Coding Style & Naming Conventions

Use plain Markdown with clear headings, short paragraphs, and standard Markdown links. Keep AI-readable structure simple: no elaborate taxonomy, generated metadata, or placeholder files. Evergreen notes in `notes/` cover one concept per file and use literal filenames such as `codex-sandbox-setup.md`. Daily journal entries use `YYYY-MM-DD.md` inside the matching year folder. Keep frontmatter minimal.

## Testing Guidelines

Validate changes by reading rendered Markdown where practical and checking links manually. When updating `notes/`, also update `notes/index.md`. For ingest or lint operations, append a concise entry to `journal/log.md`. Do not modify daily journal entries as automated cleanup.

## Librarian Workflow

For wiki, ingest, query, lint, or inbox triage work, read `agents/librarian.md` before acting and follow it as the repo-specific operating procedure. Treat it as instructions, not wiki content. If it conflicts with direct user instructions, ask before changing protected areas such as `references/`, daily journal entries, `inbox/`, or `projects/`.

## Source Retention

After promoting an `inbox/` capture into a durable `notes/` page, remove the original inbox item once the note is self-contained, indexed, logged, and validated. Keep only durable links or source citations in the note. Do not delete or compact `references/` source material without explicit user approval, because references may be intentional provenance rather than disposable capture.

## Commit & Pull Request Guidelines

Recent history uses short, imperative commit messages, for example `Migrate to LLM Wiki pattern and add codex sandbox setup guide`. Keep commits focused on one logical change. Pull requests should describe affected areas, list wiki pages or sources changed, and note manual validation. Screenshots are only useful for rendered Markdown or Obsidian changes.

## Security & Agent-Specific Instructions

Treat `references/` as immutable source material: read it, but do not edit it. Do not create speculative stubs, reorganize `inbox/` or `projects/`, or promote captures without user approval. Do not add secrets, tokens, private keys, or local credentials.
