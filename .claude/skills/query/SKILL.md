---
name: query
description: Answer a question against the wiki. Reads index.md first to find candidate pages, drills into relevant ones, synthesizes an answer with [[citations]], optionally files the answer back as a new wiki page in Explorations/ or Topics/.
---

# /query — answer a question against the wiki

You are executing the **Query** operation defined in `CLAUDE.md`. Follow this workflow.

## Workflow

### 1. Read `index.md` first

This is the catalog of all wiki pages. Use it to identify candidate pages relevant to the question. Faster than scanning all files.

### 2. Read [[Profile]] for context

The user's identity page provides context about who they are, their goals, and what they're optimizing for. This shapes how to answer.

### 3. Drill into candidate pages

Read the candidate pages from the index. Follow `[[links]]` between them to gather full context. Read raw sources in `Inbox/_processed/` or `Journal/` only if explicitly relevant.

### 4. Synthesize the answer

- Cite **every claim** with `[[page]]` format
- Be specific — name people, companies, dates, sources
- Distinguish **facts** (from wiki) from **inference** (your synthesis)
- If the wiki doesn't have the answer, say so explicitly — DO NOT hallucinate

Format depends on the question:
- Lookup question → short list with citations
- Comparative question → table
- Strategic question → structured analysis with recommendation

### 5. Propose filing the answer back

After answering, ask: *"Should this be filed as a new wiki page?"* Good candidates:
- Comparative analysis → `Explorations/<topic>.md` (with guiding question)
- Recurring theme → `Topics/<theme>.md`
- New insight about a person/company → update existing entity page

Karpathy's principle: **good answers should compound in the wiki, not disappear into chat history.**

### 6. If user accepts filing

- Create or update the target page using the appropriate template
- Add entry to `index.md`
- Add `# Sources` section citing the consulted pages

### 7. Append to `log.md`

```markdown
## [YYYY-MM-DD] query | <question, abbreviated to ~60 chars>
Read: <list of pages consulted>
Filed: <new/updated page if any, else "none">
```

---

## Rules

- **ALWAYS** read `index.md` first
- **ALWAYS** read `Profile.md` for user context
- **CITE every claim** with `[[page]]`
- If the wiki has no info on a topic, say so — don't fabricate
- Don't read `Inbox/_processed/` or `Journal/` unless the question explicitly requires raw sources

## What "good" looks like

A good query:
- Answers fully with citations
- Surfaces relevant connections the user might not have seen
- Either ends with a filed wiki page (knowledge compounds) or with a clear "no need to file" statement
