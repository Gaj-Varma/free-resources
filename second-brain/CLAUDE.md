# CLAUDE.md — Second Brain Vault

*This file is the "front door." Cowork loads it at the start of every session working in this vault. It defines what the vault is, how to behave in it, and what never to do. Keep it lightweight — it routes; it is not a knowledge dump.*

---

## Vault overview

This is my personal second brain: plain Markdown notes I own, on my disk. The vault is a **lightweight index** over external source material, and a **structured home** for the knowledge you distil. Some source material lives here; some lives in external folders that are only pointed to.

**Operating layer (root files):**
- **Front door** — this file. Read it first, then read `index.md` before doing real work.
- **Index** — `index.md`. A catalog of what exists and where, one line + a path per entry.
- **Log** — `log.md`. Append one line here for every ingest or change you make.

**Content folders:**
- **`inbox/`** — Staging only. Sources land here to be processed, then get filed and archived. Should tend back toward empty.
- **`raw/`** — Original source files (PDFs, articles, transcripts), archived as `raw/YYYY-MM/`. Immutable: read from it, never edit it.
- **`knowledge/`** — The distilled notes you write and maintain: summaries, concepts, decisions, how-tos. **This is what recall reads from.** Filed into topic subfolders (e.g. `knowledge/infrastructure/domains`) that grow organically — create a new subfolder when no existing topic fits.
- **`projects/`** — Active work with an outcome (the *doing*). Research is just a project; there is no separate research folder.
- **`areas/`** — Ongoing responsibilities and recurring topics.
- **`daily/`** — Journal entries and the scheduled daily recap.
- **`archive/`** — Completed projects and dormant material.

**Filing rule (projects vs. knowledge):** `projects/` holds active work; `knowledge/` holds durable learning. When a project or research thread produces something durable, distil it into `knowledge/<topic>/` — filed by **topic**, not by which project produced it — then archive the working material.

---

## First action every session

1. Read this file.
2. Read `index.md`.
3. Only then act on the request, following the index's pointers to deeper notes as needed. Do not scan the whole vault.

---

## Ingesting from the inbox (inbox → project or knowledge, + raw)

The `inbox/` is the single capture point. Anything can land here — a URL, some rough notes, a PDF, an article. When asked to process the inbox (or on the scheduled run), decide for each item whether it belongs to **active work** or is **durable reference**, then route it accordingly.

1. Read each item in `inbox/`.
2. **Quality gate.** If an item is too sparse to be useful (fewer than ~3 distinct ideas/facts), move it to `inbox/needs-clarification/` and log one line saying what's missing. Do not create a thin note.
3. **Decide the destination:**
   - **Active work** (research in progress, a live job, anything the person is currently *doing*) → create or update a folder under `projects/<name>/`. Put the person's own notes and any links there, and add your own clean summary of the source. If it's a brand-new topic of active work, start a new project folder. When a project later wraps, its durable lessons get promoted into `knowledge/` and the working material is archived.
   - **Durable reference** (something to keep and reuse, with no active work attached) → write a clean note into `knowledge/<topic>/` (create the topic subfolder if none fits): a 2–4 sentence summary, key facts/decisions, and links to related existing notes.
   - **If unsure which it is, ask** rather than guessing. A cue in the drop itself ("researching X, start a project" vs. "file this for reference") settles it; when there's no cue and the intent isn't obvious, ask before creating a project folder.
4. Add frontmatter (see schema below) to any note you create.
5. Move the original source from `inbox/` into `raw/YYYY-MM/`, then **delete the original from `inbox/`** so it is never reprocessed. This is a move, not a copy: the file must exist in `raw/` and no longer exist in `inbox/`. The note records the archived path so the source is always traceable. Never leave the original in two places.
6. Add or update the entry in `index.md`: one-line summary + path.
7. Append one line to `log.md`: `## [YYYY-MM-DD] ingest | <title> | → projects|knowledge`.

---

## Frontmatter schema (add to every note you create)

The `templates/` folder is the single source of truth for note structure. Each note type has a matching template: `knowledge-note.md` (for `knowledge/`), `daily.md`, `project.md`, `area.md`. **When you (the agent) create a note, open the template matching its type, copy the frontmatter block exactly, substitute any Templater token (e.g. `<% tp.date.now("YYYY-MM-DD") %>`) with its literal value, fill in the blank values (`summary`, `tags`, `source`), and add no fields that are not in the template.** This makes agent-written notes byte-identical in structure to the ones Templater stamps for hand-authored notes. The fields are `type`, `status`, `date`, `source`, `tags`, `summary`.

Do not invent extra frontmatter fields. If a note genuinely needs one, add it to the relevant template first (so both paths stay in sync), then use it — never one-off.

Keep it minimal and consistent. Consistency matters more than completeness.

**Authoring a knowledge note by hand (optional).** You (the human) can add your own note to `knowledge/<topic>/` directly — nothing is off-limits. Copy the frontmatter block from `templates/knowledge-note.md` (that file exists to be copied from, so you never touch this one), fill it in, and write your note. Or just ask the agent to write it, which applies the schema automatically. Templater is intentionally *not* mapped to `knowledge/` (it owns `daily/` and `projects/`) so it never double-stamps notes the agent writes there.

**Why `projects/` being a shared folder is safe (no duplicate frontmatter).** Both you and the agent write into `projects/` — you by hand in Obsidian, the agent when routing an inbox item to active work. These never collide on the same file. When *you* create a note inside Obsidian, Templater fires and stamps the frontmatter. When the *agent* writes a note, it writes directly to disk (outside Obsidian's UI), so Templater does **not** fire — the agent applies the frontmatter itself from the schema above. Each note is born from exactly one path and stamped once. (Edge case: if the agent writes into a Templater-mapped folder while Obsidian is open and watching, Obsidian can occasionally treat the external file as "new" and fire Templater on top of it — the same behaviour third-party sync engines trigger. It's intermittent, and a bad stamp is trivially reverted via Git.)

---

## Answering questions (recall)

1. Read `index.md` first; follow pointers to the relevant notes.
2. **Always cite the filename(s)** you drew the answer from. An answer with no traceable source is not acceptable.
3. Quote directly from a note when precision matters.
4. Synthesize across multiple notes when useful, but never invent. If the vault does not cover something, say so plainly rather than filling the gap from general knowledge.
5. If detail lives in an external folder that isn't currently granted, say what the summary knows and note that the source folder can be opened for the full detail.

---

## Conventions

- Use `[[wikilinks]]` for internal references; link related notes liberally.
- Prefer tables for comparisons.
- Write plainly. No emojis. No filler. Treat me as someone who knows the context.
- One idea per note where practical; small linked notes beat large ones.

---

## What NOT to do

- Do not create new files unless explicitly asked, or as part of a defined ingest/recap workflow. (Exception: if any template in `templates/` — `knowledge-note.md`, `daily.md`, `project.md`, `area.md` — is missing, recreate it from the frontmatter schema; the vault needs these as the single source of truth for note structure.)
- Do not restructure or rename existing folders.
- Do not edit anything in `raw/`.
- Do not delete notes in `knowledge/`, `projects/`, `areas/`, or `daily/`. If something seems stale, flag it `status: needs-review` and tell me. (Exception: during ingest you *do* remove the original source from `inbox/` after archiving it to `raw/` — that clears the staging copy, it does not delete knowledge.)
- Do not answer recall questions from general knowledge dressed up as if it came from my notes.
- Do not over-engineer a simple request.

---

## About me

*(Fill this in — who I am, what I'm working on this quarter, how I want to be spoken to. This is the profile the agent loads every session. Interview me to build it if it's blank.)*

