# Second Brain — Schema for Claude

This is a personal knowledge management system inspired by Andrej Karpathy's "wiki LLM" approach. Knowledge lives in plain markdown so an LLM can read, reason over it, and **actively maintain it**.

**Owner:** you (see `Profile.md` for context — fill it in first)
**Primary tool:** Obsidian (vault root = repo root)
**Primary LLM access:** Claude Code, run from this directory

> This is a **template**. It ships with the schema, the three skills, and a handful of fictional example pages so you can see how a populated vault looks. Read `README.md` for setup, then delete the examples and make it yours. Anywhere this file says "you", it means the vault owner — edit `Profile.md` so Claude knows who that is.

---

## Architecture — 3 layers

The vault has three distinct layers with strict ownership rules.

### Layer 1 — Raw sources (human owns, immutable)

| Folder | Contents |
|---|---|
| `Inbox/` | Clipped articles, raw dumps, anything captured before processing |
| `Journal/YYYY-MM-DD.md` | Daily notes — meetings, ideas, captures during the day |

**Rule:** Claude **READS** these files but **NEVER modifies their body**. They are the source of truth — immutable historical record.

**Single exception**: Claude may write the `ingested:` key in the **frontmatter** of a `Journal/` file when distilling it into the wiki via `/ingest`. Value: `false` (or absent) = pending ingest; `YYYY-MM-DD` = processed on that date. Body and all other frontmatter keys remain untouched. `Inbox/` files have no equivalent flag — they are moved to `Inbox/_processed/` after ingest instead.

### Layer 2 — Wiki (Claude owns, actively maintains)

| Folder | Contents |
|---|---|
| `People/` | Humans — `type: person` or `type: public-figure` (use `Templates/person.md`) |
| `Companies/` | Organisations — `type: company` (use `Templates/company.md`) |
| `Library/` | Books (`Templates/book.md`) and articles/papers/threads (`Templates/article.md`) |
| `Topics/` | Evergreen theme indexes |
| `Explorations/` | Research/planning boards with a guiding question |
| `Projects/` | Active work with an outcome and deadline |
| `Content Ideas/` | Post idea pipeline surfaced by the **Rule of 3 trigger** during `/ingest`. Each file = one candidate post for a channel you publish on. Concept that appeared in ≥3 distinct raw sources **AND** is content-shape for you. Uses `Templates/post-idea.md`. States: `idea` → `drafting` → `published` / `dropped`. |
| `Profile.md` | Your own profile — read this for context on your roles, goals, positioning |

**Rule:** Claude **freely creates, updates, and maintains** these files. Claude is the wiki maintainer, not a passive reader. New pages, cross-references, and updates are expected.

### Layer 3 — Schema (co-evolved)

| File | Role |
|---|---|
| `CLAUDE.md` | This file — defines architecture, conventions, workflows |
| `index.md` | LLM-maintained catalog of every wiki page (poor man's RAG) |
| `log.md` | Chronological append-only record of operations |

**Rule:** `CLAUDE.md` is co-evolved with you — propose changes when patterns emerge. `index.md` and `log.md` are Claude's bookkeeping.

---

## Conventions

- **Frontmatter:** every wiki page has YAML frontmatter with at minimum `type` and `tags`
- **Links:** bidirectional, format `[[Note Name]]`. Prefer linking over tagging when the target is an entity
- **Tags:** lowercase kebab-case (`#fintech`, `#open-question`). Use sparingly — links > tags
- **Dates:** ISO `YYYY-MM-DD` always
- **File naming:**
  - People: `First Last.md`
  - Companies: `Company Name.md`
  - Books: `Title - Author.md`
  - Topics / Explorations / Projects / Content Ideas: `kebab-case.md`
- **Language:** structure & frontmatter in English; note content in whatever language is natural to you

### Citation convention

**Every wiki page must end with a `# Sources` section** listing the raw sources (or other wiki pages) it derives from:

```markdown
# Sources
- [[Journal/2026-01-15]] — call about the new CTO role
- [[Inbox/_processed/some-clipped-article]] — first meeting context
```

When Claude updates a wiki page during ingest, it appends new sources here. This makes provenance traceable.

---

## Edit protocol — page protection & decision states

Beyond the Layer 1 immutability rule (raw sources never modified), two operational rules apply to the wiki:

### 1. Canonical state pages require explicit confirmation

Before editing `Profile.md` — or making sensitive-theme changes to `Companies/` and `People/` pages (career, compensation, relationships, strategic positioning) — **show the proposed diff in chat and wait for explicit confirmation from the owner**. Do not apply directly even if the edit seems "obvious" from an ingest.

Exception: trivial mechanical updates (appending to `# Sources`, fixing a typo, updating a `last_contact` date) can be applied directly. Anything that touches narrative, framing, decisions, or status requires confirmation.

The reason: these pages are the canonical reference for who the owner is and what they're optimising for. An overreach here propagates to every future `/query` answer.

### 2. Hypothesis vs decision — respect the boundary between layers

Wiki pages serve different functions and use different language:

- **`Explorations/`** — where candidate directions, hypotheses, and open questions live. Hypothesis-driven language ("candidate for / hypothesis / to test / possible direction") is appropriate here.
- **`Profile.md`, `Companies/`, `People/`** — pages of **state**, not exploration. Reflect only **decided positions**, not candidate directions.

When an exploration produces a strong hypothesis (e.g. a candidate role title, a tentative pricing tier, an ICP under validation), **do not promote it** to a state page as if it were the chosen path. State pages use *"decide whether / figure out whether / explore / clarify"* — not *"renegotiate toward / target direction / commit toward"*.

A hypothesis is promoted to state only when:
- The exploration explicitly concludes (`# Conclusion` filled by the owner), or
- The owner explicitly says the hypothesis has become a decision

Until then, in state pages reference the exploration page itself (`see [[the-exploration]]`) rather than internalising its hypotheses.

---

## Operations

The vault is operated via three workflows. Skills in `.claude/skills/` automate these.

### Ingest (skill: `/ingest`)

Triggered when a new raw source lands in `Inbox/` **or** when a `Journal/YYYY-MM-DD.md` has `ingested: false` (or no `ingested:` key). Workflow:

1. **Read** the source — a file in `Inbox/` (excluding `_processed/`) or a journal in `Journal/` not yet flagged as ingested
2. **Discuss** key takeaways with the owner — ask what to emphasise, what's relevant
3. **Write** any new wiki page needed (for `Inbox/` sources this typically includes a `Library/` summary using `book.md` or `article.md` template; for `Journal/` sources, Library pages are unusual — the journal content is distilled into entity / exploration / topic updates)
4. **Update** related entity pages in `People/`, `Companies/`, `Topics/`, `Explorations/`, `Projects/` — add cross-references, insights, new interactions
5. **Update** `index.md` — add entry for any new page, update touched pages' summaries
6. **Append** to `log.md` an `ingest` entry with format: `## [YYYY-MM-DD] ingest | <source title>`, listing touched pages
7. **Mark the source as processed**:
   - **Inbox source**: move from `Inbox/` to `Inbox/_processed/` (keeps Inbox clean)
   - **Journal source**: set `ingested: YYYY-MM-DD` in the frontmatter. Do NOT move the file. Do NOT modify the body. This is the single allowed write to a Layer 1 file.

**Citation convention by source type**:
- Inbox: `- [[Inbox/_processed/<source>]] — <context>`
- Journal: `- [[Journal/YYYY-MM-DD]] — <context>`

Default: ingest one source at a time, stay engaged with the owner. Batch ingestion is opt-in.

### Query (skill: `/query`)

Triggered when the owner asks a question. Workflow:

1. **Read** `index.md` first to find candidate pages (faster than scanning all files)
2. **Drill** into relevant pages, follow `[[links]]`
3. **Synthesise** answer with inline citations using `[[link]]` format
4. **Propose** to the owner: should this answer be filed as a new wiki page (typically in `Explorations/` or `Topics/`)? Good analyses should compound in the wiki.
5. **Append** to `log.md` a `query` entry

### Lint (skill: `/lint`)

Triggered periodically (manually). Workflow:

1. **Scan** the wiki for:
   - Contradictions between pages
   - Stale claims superseded by newer sources
   - Orphan pages (no inbound `[[links]]`)
   - Concepts mentioned but lacking their own page
   - Missing cross-references between related pages
   - Data gaps that could be filled with a web search
2. **Report** as a checklist
3. **Suggest** new questions to investigate and new sources to search for
4. **Append** to `log.md` a `lint` entry

---

## Index & Log

### `index.md` — content catalog

Maintained by Claude. Lists every wiki page with link + one-line summary + key metadata. Organised by category (People, Companies, Library books, Library articles, Topics, Explorations, Projects, Content Ideas).

On every ingest, Claude:
- Adds entry for new pages
- Updates summary for touched pages

When answering queries, Claude reads this file **first** — it's the "search index" of the vault. This avoids embedding-based RAG and works well up to ~hundreds of pages.

### `log.md` — operation log

Append-only. Format for grep-ability:

```markdown
## [2026-01-15] ingest | Karpathy article on wiki LLM
Source: Inbox/_processed/karpathy-wiki-llm.md
Touched: Topics/wiki-llm.md, Library/karpathy-wiki-llm.md, index.md
Notes: Established the 3-layer architecture for this vault.

## [2026-01-16] query | Who do I know in AI adoption?
Read: People/{...}, Companies/{...}
Filed: Explorations/ai-adoption-network.md
```

Each entry starts with `## [YYYY-MM-DD] {operation} | {title}` so `grep "^## \[" log.md | tail -10` shows recent activity.

---

## When in doubt — where does this go?

- New raw thing? → `Inbox/`
- Met someone new? → `People/` (use `Templates/person.md`)
- New company / organisation? → `Companies/` (use `Templates/company.md`)
- Read / clipped something? → `Library/` (`book.md` or `article.md`)
- New research topic? → `Explorations/` (use template); promote to a `Topics/` note if it stabilises
- New project with outcome? → `Projects/`
- Evergreen theme with no specific project? → `Topics/`
- Concept appeared in ≥3 raw sources **and** is content-shape for you? → `Content Ideas/` (proposed during `/ingest` Rule of 3 step, use `Templates/post-idea.md`)

## Archiving

No `Archive/` folder — files stay where they are. To archive an item, update its frontmatter `status`:

- **Person / company**: `status: archived` (or `dormant` for "not active but not closed")
- **Book / article**: `status: done` / `status: abandoned` — once finished, finished
- **Exploration**: `status: concluded` once the guiding question is answered
- **Project**: `status: shipped` / `status: abandoned` once the outcome is reached or dropped
- **Content idea**: `idea` → `drafting` → `published` / `dropped`. *Dropped* often means "good idea but not coherent with my positioning" — keep the file (preserves the Rule of 3 trace), don't delete

Active Dataview queries filter on the appropriate `status` value. Files keep their original names and links — no breakage from folder moves. Git history is the audit log.

## Plugins assumed installed in Obsidian

- **Templater** — for `Templates/` to work (Template folder = `Templates/`)
- **Dataview** — for queries
- **Tasks** — for todos: `- [ ] task 📅 2026-01-20`

If a user query depends on these, suggest installing them rather than failing silently.

## Useful Dataview snippets

People I haven't talked to in a while:
```dataview
TABLE role, company, last_contact
FROM "People"
WHERE type = "person" AND status = "active"
SORT last_contact ASC
LIMIT 10
```

Currently reading:
```dataview
TABLE authors, started
FROM "Library"
WHERE type = "book" AND status = "reading"
```

Active explorations:
```dataview
LIST FROM "Explorations"
WHERE status = "active"
SORT started DESC
```
