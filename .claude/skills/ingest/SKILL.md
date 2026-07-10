---
name: ingest
description: Process a raw source (Inbox/ file or pending Journal/ entry) into the wiki. Reads source, discusses takeaways with user, writes Library summary if needed, updates entity pages (People/Companies/Topics/Explorations/Projects), updates index.md, marks the source as processed (Inbox → _processed/ move, or Journal → ingested flag), appends log.md.
---

# /ingest — process a raw source into the wiki

You are executing the **Ingest** operation defined in `CLAUDE.md`. Follow this workflow exactly.

`/ingest` works on **two types of raw sources**:
- Files in `Inbox/` (excluding `Inbox/_processed/`) — typically clipped articles, PDFs, threads.
- Files in `Journal/` whose frontmatter has `ingested: false` or no `ingested:` key — daily notes that contain durable signal.

## Workflow

### 1. Identify the source

- If the user provided a path or title, use it.
- Otherwise, build the **pending sources list**:
  - `ls Inbox/` (excluding `_processed/`) — files in Inbox
  - `Journal/*.md` whose frontmatter does NOT have `ingested:` set to a date (i.e. `ingested: false` or key absent). Use Bash + grep: `grep -L "^ingested: 2" Journal/*.md` returns journals not yet ingested
- If the user has not specified one, present the pending list and ask which to ingest.
- If both lists are empty, tell the user and stop.

### 2. Read the source completely

Use the Read tool on the entire source file before doing anything else. Do NOT skim.

### 3. Discuss takeaways with the user

Present:
- **Summary**: 3-5 bullets of what the source says
- **Key entities mentioned**: people, companies, concepts (link with `[[]]` if they already exist in the wiki)
- **Relevance to you**: how it connects to your network / projects / interests (read [[Profile]] for context)
- **Questions**: ask what to emphasize, what's most relevant, what to skip

**Do NOT proceed to writing without this discussion.** Wait for user input.

### 3.5. Rule of 3 detection (content idea trigger)

During the discussion (step 3), as you identify the source's key concepts/stories,
run a lightweight check against the **raw source layer** to see if any concept
has now been said in **≥3 distinct sources** (the current one included → so
**≥2 prior hits** + current = trigger).

**Scope of the search — raw sources only**:
- `Inbox/_processed/*.md` — previously ingested clips/articles/PDFs
- `Journal/*.md` where `ingested:` is set to a date (already ingested journals)

Do **NOT** count Layer 2 derivatives (`Library/`, `Topics/`, `Explorations/`,
`Projects/`, `Profile.md`) — those are synthesis pages, not independent
occurrences. Counting them would inflate every recurring theme.

**Two-gate filter** — a concept is a candidate only if **BOTH** gates pass:

1. **Recurrence gate**: ≥3 distinct raw sources contain the concept (current + ≥2 priors).
2. **Content-shape gate**: the concept is **content-shape for you** — something
   you yourself say, or would want to share with your audience, on the themes you
   actually publish about. A recurring topic that is **not** content-shape for you
   (e.g. an internal-only operational detail, a private admin threshold, a purely
   personal logistics note) does **NOT** qualify even if it recurs across 3
   sources. Read [[Profile]] and your active positioning / channel pages to
   calibrate what "content-shape" means for you.

**Procedure**:

1. From the 3-5 key concepts you already identified in step 3, run targeted
   `grep` queries across the raw-source scope above. Use **multiple
   phrasings/synonyms** (the same concept rarely uses identical wording across
   languages, different authors, or your own voice vs. an article).
2. For each concept that passes **both gates**, present it to the user in chat as a
   **content idea candidate**, with:
   - the concept in one line
   - the 3+ raw source files where it appears
   - a one-line **angle candidate** (the contrarian/personal take that justifies
     why *you* should write it — not why the topic is valid)
3. **Ask the user which (if any) to save** — multi-select, default nothing. The user
   may also override gates (force-save a 2-hit concept; skip a 3-hit concept).
4. For each confirmed candidate:
   - Create `Content Ideas/<kebab-slug>.md` using `Templates/post-idea.md`
   - Fill `# Why this is a post` with the 3+ source citations (with short quotes)
   - Fill `# Hook` with the agreed angle
   - Set `trigger_count` in the frontmatter to the current hit count
   - Add the new idea file to `Content Ideas/_index.md` under `## Entries` → `### Idea`
   - Append the new page to the touched-pages list for steps 6 (index update) and 8 (log)

**This is a suggestion mechanism, not an autosave**: if the user declines all
candidates, write nothing. The trigger respects the same hypothesis-vs-decision
boundary as the rest of the wiki.

**Reinforcement of existing ideas**: if a concept already has a `Content Ideas/`
file and this source is a *new* reinforcement, do not create a duplicate — instead
append the new source to that file's `# Sources` and `# Why this is a post`
section and increment `trigger_count` in the frontmatter.

### 4. Plan the wiki updates

Show a concrete plan listing:
- New `Library/<title>.md` page (use `Templates/article.md` or `Templates/book.md`)
- Existing entity pages to update (People, Companies, Topics)
- New entity/concept pages to create if needed (e.g. a Topic page if the source establishes a recurring theme)

Wait for user confirmation before executing.

### 5. Execute the updates

For each page:
- Create or update with proper frontmatter
- Maintain bidirectional `[[links]]` between related pages
- Append to the `# Sources` section, with citation format depending on source type:
  - **Inbox source**: `- [[Inbox/_processed/<source>]] — <one-line context>`
  - **Journal source**: `- [[Journal/YYYY-MM-DD]] — <one-line context>`

A single source typically touches **5-15 pages**. Be thorough — every mention of a person, company, or concept is a potential cross-reference.

#### Action items → task hub (optional pattern)

If your vault has a recurring context that accumulates action items (a job, a client, a side project), you can route those items to a single **task hub** page in `Projects/` in addition to recording them on the source/derivative page. Add them in **Tasks plugin** format with a shared tag, e.g. `- [ ] <text> #<your-tag> 📅 YYYY-MM-DD`, and give the hub page an aggregator query on that tag so tasks scattered across the vault collect in one place. Watch for basename collisions: do not give a `Projects/` file the same name as a `Companies/` or `People/` file, or `[[Name]]` links become ambiguous. Delete this section if you don't use a task hub.

### 6. Update indexes — BOTH layers

This step has **two parts**. Do both — they serve different readers.

#### 6a. Root `index.md` (whole-vault catalog, read by Claude on `/query`)

- Add entries for any new pages with one-line summaries
- Update summaries of touched pages where the new source meaningfully changes them

#### 6b. Subfolder `_index.md` files (per-folder browse, read by user in Obsidian)

Every subfolder of the wiki (`People/`, `Companies/`, `Library/`, `Topics/`, `Explorations/`, `Projects/`) has its own `_index.md`. Each has a `## Entries` section with a **manual list** of all files in that folder (fallback for when Dataview is off, and for OS-level search).

**Rule**: any time you create a new page in a wiki folder, you MUST also add a line to that folder's `_index.md` under `## Entries`. Same for renames/removals.

For each folder touched in this ingest, open its `_index.md` and update the `## Entries` section to reflect current state.

> Skipping this step is the most common ingest mistake — the page exists but is invisible from the folder's index. Don't ship an ingest with mismatched `_index.md`.

### 7. Mark the source as processed

**Two different paths depending on source type** — pick the one that matches.

#### 7a. Inbox source — move to `_processed/`

```bash
mkdir -p Inbox/_processed
mv "Inbox/<source>" "Inbox/_processed/<source>"
```

This keeps `Inbox/` clean as a "to-process" queue while preserving the raw source.

#### 7b. Journal source — set the `ingested` flag

Do NOT move the file. Edit ONLY the `ingested:` key in the frontmatter:

- If the key exists (`ingested: false`), change it to `ingested: YYYY-MM-DD` (today's date — the ingest date, not the journal's date).
- If the key is absent, add it to the frontmatter.

**Body and all other frontmatter keys must remain untouched.** This is the single allowed modification to a Layer 1 file (see `CLAUDE.md` Layer 1 rule and its single exception).

### 8. Append to `log.md`

```markdown
## [YYYY-MM-DD] ingest | <source title>
Source: Inbox/_processed/<source-filename>
Touched: <comma-separated list of pages>
Notes: <1-2 lines on what was learned / what changed in the wiki>
```

---

## Rules

- **NEVER** modify the **body** of `Journal/` or `Inbox/` files — raw layer is immutable
- The **single allowed exception** is setting the `ingested:` frontmatter key on a `Journal/` file at end of ingest (Step 7b). Body untouched, other frontmatter keys untouched.
- **DEFAULT**: ingest one source at a time, stay engaged with the user
- **ALWAYS** cite sources in the `# Sources` section of every modified wiki page, using the right format for the source type (Inbox: `[[Inbox/_processed/...]]`; Journal: `[[Journal/YYYY-MM-DD]]`)
- **NEVER** fabricate facts not in the source or in the user's discussion
- If the user requests **batch ingest**, confirm explicitly before proceeding and skip the per-source discussion step

## What "good" looks like

A good ingest:
- Captures key takeaways in the Library summary
- Updates 5-15 related pages with new cross-references
- Surfaces 1-2 follow-up questions or opportunities for the user
- Leaves the wiki more connected than before (fewer orphans, more `[[links]]`)
- Touches **both** the root `index.md` and every relevant subfolder `_index.md` — no page is created without showing up in its folder's `_index.md` `## Entries` list
- Runs the **Rule of 3 detection** (step 3.5) and either proposes content idea candidates that pass both gates (recurrence + content-shape) or reports "no candidates" explicitly — the check is mandatory, the output is optional

## Pre-finish checklist

Before declaring the ingest done, verify:
- [ ] Every new page has a `# Sources` section citing the raw source
- [ ] Root `index.md` reflects new pages and updated summaries
- [ ] **Every subfolder `_index.md` touched lists all its current files** under `## Entries`
- [ ] **Rule of 3 detection (step 3.5) executed** — candidates proposed or "no candidates" reported explicitly. If any saved, `Content Ideas/_index.md` updated and the file appears in the log's Touched list
- [ ] Source marked as processed:
  - Inbox: file moved to `Inbox/_processed/`
  - Journal: `ingested: YYYY-MM-DD` set in the frontmatter (body untouched)
- [ ] `log.md` entry appended with format `## [YYYY-MM-DD] ingest | <title>`
