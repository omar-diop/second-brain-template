---
name: lint
description: Health-check the wiki. Find contradictions between pages, orphan pages, stale claims, missing concept/entity pages, broken cross-references, data gaps. Report as a checklist with suggested actions. Read-only — does not modify the wiki.
---

# /lint — health-check the wiki

You are executing the **Lint** operation defined in `CLAUDE.md`. This is read-only — report findings, do not modify pages.

## Workflow

Scan the wiki layer (`People/`, `Companies/`, `Library/`, `Topics/`, `Explorations/`, `Projects/`, `Profile.md`) and report on:

### 1. Contradictions
Pages claiming different facts about the same entity. E.g. one page says Person X works at Company A, another says Company B.

### 2. Orphan pages
Wiki pages with **no inbound `[[links]]`** from other wiki pages. Likely under-utilized or forgotten.

```bash
# Heuristic: grep for "[[Page Name]]" across the wiki excluding the page itself
```

### 3. Missing concept / entity pages
Concepts, people, or companies mentioned in many pages via `[[bracket]]` links that don't have their own page yet (dangling links).

### 4. Missing cross-references
Two pages that should clearly link to each other but don't. E.g. two people who attended the same event, where the event is on one page but not the other.

### 5. Stale claims
Old assertions in entity pages that may be outdated. Look at:
- `last_contact` dates older than 6 months on `status: active` people
- Frontmatter or content that contradicts more recent sources in `# Sources`

### 6. Data gaps
Areas where a quick web search could fill gaps. Suggest specific searches:
- A person's LinkedIn/Twitter is empty
- A company's `industry`, `hq`, `size` is empty
- A book has no `authors` or `rating` despite `status: done`

### 7. Index drift
Pages that exist in the vault but are missing from `index.md`, or `index.md` entries pointing to deleted pages.

## Output format

Produce a Markdown checklist grouped by category. For each finding, suggest a **specific actionable next step**:

```markdown
## Lint report — [YYYY-MM-DD]

### Orphan pages (3)
- [ ] `Companies/Northwind Labs.md` — no inbound links. Action: link from [[Maya Chen]] (already done?), check.
- [ ] ...

### Missing concept / entity pages (2)
- [ ] `[[O'Reilly]]` mentioned in [[Maya Chen]] and [[Jordan Blake]] but no page exists. Action: create `Companies/O'Reilly.md`.
- [ ] `[[DevConf 2026]]` mentioned in 2 people but no event page. Action: create `Topics/devconf-2026.md` (or similar).

### Stale claims (1)
- [ ] [[Maya Chen]] `last_contact: 2024-06-01` — over 2 years ago. Action: confirm status or mark `dormant`.

### Data gaps (4)
- [ ] [[Jordan Blake]] LinkedIn empty. Suggest: search "Jordan Blake Northwind Labs LinkedIn"
- [ ] ...
```

## After reporting

Append to `log.md`:

```markdown
## [YYYY-MM-DD] lint | wiki health check
Findings: <N orphans, M missing pages, K stale, L data gaps>
Suggested actions: <total count>
```

---

## Rules

- **READ-ONLY** — do not modify any page during lint
- If the user asks to fix a finding, that's a **separate operation** (could be a quick edit or another `/ingest`)
- On small vaults (< 30 pages), focus on the most impactful issues — don't be exhaustive
- Suggest concrete, scoped actions — not vague "improve this"

## What "good" looks like

A good lint:
- Catches at least one real issue the user wouldn't have noticed
- Suggests specific actions, not generic advice
- Keeps the wiki healthier and more connected over time
