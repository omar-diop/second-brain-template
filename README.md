# Second Brain — template

A personal "wiki LLM" second brain (Karpathy-style) in Obsidian markdown. Plain text so an LLM can read it, reason over it, and **actively maintain** it. You capture raw sources; an agent (Claude Code) distills them into connected wiki pages that compound over time.

This is a **starter template**. It ships with:
- the 3-layer schema ([CLAUDE.md](CLAUDE.md)),
- three Claude Code skills — `/ingest`, `/query`, `/lint` (in `.claude/skills/`),
- Obsidian templates (`Templates/`),
- a small set of **fictional example pages** so you can see how a populated vault looks.

> Don't copy the structure blindly — model it on your own constraints. The example pages exist to show the mechanics; delete them and make it yours.

## Architecture — 3 layers

| Layer | Folders | Owner |
|---|---|---|
| **Raw sources** (immutable) | `Inbox/`, `Journal/` | You write, Claude reads |
| **Wiki** (LLM-maintained) | `People/`, `Companies/`, `Library/`, `Topics/`, `Explorations/`, `Projects/`, `Content Ideas/`, `Profile.md` | Claude creates and updates |
| **Schema** | `CLAUDE.md`, `index.md`, `log.md` | Co-evolved |

You dump raw sources. Claude reads them and synthesizes the wiki.

## Quick start

1. **Clone or fork** this repo and open the folder in your editor.
2. **Fill in `Profile.md`** — it's a blank template. Claude reads it first on every query, so this is what makes answers feel like *yours*.
3. **Open the same folder as an Obsidian vault** and install the community plugins:
   - **Templater** → Settings → Templater → Template folder location = `Templates`
   - **Dataview**
   - **Tasks**
4. Configure the core **Daily Notes** plugin:
   - New file location: `Journal`
   - Date format: `YYYY-MM-DD`
   - Template file location: `Templates/daily`
5. (Recommended) Install the **Obsidian Web Clipper** browser extension:
   - Save location: `Inbox`
   - Template based on `Templates/article.md`
6. **Open Claude Code in this folder** and try `/ingest` on `Inbox/example-clipped-article.md` to see the workflow end to end.
7. **Delete the example pages** (everything marked as fictional) and start feeding your own.

## Daily workflow

### 1. Capture (you, low friction)

- Daily thoughts, calls, meetings → `Journal/YYYY-MM-DD.md` (open the daily note in Obsidian)
- Articles clipped from the web → `Inbox/` (via Web Clipper)
- Random link, idea, voice-note transcript → `Inbox/`

### 2. Process (Claude maintains the wiki)

Run `/ingest` in Claude Code. Claude reads a source from `Inbox/` (or an un-ingested `Journal/` note), discusses takeaways with you, writes any `Library/` summary, updates related entity pages, updates `index.md`, marks the source processed, and appends to `log.md`.

### 3. Query (Claude reads the wiki)

Run `/query <question>` in Claude Code. Claude reads `index.md` first, drills into relevant pages, answers with `[[citations]]`, and optionally files the answer back as a new page so insights compound.

Example questions:
- *"Who do I know working on knowledge tools?"*
- *"What have I read on systems thinking? Synthesise."*
- *"Before I decide on the newsletter, what relevant notes and people do I already have?"*

### 4. Maintain (periodic)

Run `/lint` in Claude Code to health-check the wiki: orphans, contradictions, stale claims, missing pages, data gaps. Read-only — reports a checklist.

## Skills

Located in `.claude/skills/`, committed with the vault:

- `/ingest` — process a raw source from `Inbox/` (or a `Journal/` note) into the wiki
- `/query <question>` — answer against the wiki with citations
- `/lint` — health-check the wiki

## Make it yours

- Rewrite `Profile.md` for who you are.
- Delete the fictional examples (People, Companies, Library, Topics, Explorations, Projects, Content Ideas, the example Journal and Inbox files), then reset `index.md` and `log.md`.
- Add or remove Layer-2 folders to fit your work — the schema is a starting point, not a cage. Update `CLAUDE.md` when you change the structure; it's co-evolved with you.

## Conventions (short)

Full version in [CLAUDE.md](CLAUDE.md).

- `First Last.md` for people, `Company Name.md` for orgs, `kebab-case.md` for topics
- ISO dates `YYYY-MM-DD`
- YAML frontmatter on every wiki page (`type`, `tags` minimum)
- `[[links]]` over tags whenever the target is an entity
- Every wiki page ends with a `# Sources` section citing raw sources
- Structure in English, content in whatever language is natural to you

---

*This template accompanies an article on building an AI-maintained second brain. The concept is based on [Andrej Karpathy's wiki-LLM approach](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).*
