---
type: index
folder: Projects
---

# Projects

Active work with a **defined outcome** and (usually) a deadline. Once shipped or abandoned → set `status: shipped` or `status: abandoned` in frontmatter (no folder move).

Each project = one note at first; promote to a folder when it grows (e.g. `Projects/project-name/index.md` + sub-notes).

A project note should pull together:
- Outcome & success criteria
- People involved (`[[Person]]` links)
- References (`[[Library note]]`, `[[Exploration]]`, `[[Topic]]`)
- Todos (`- [ ] task` — picked up by Tasks plugin)
- Decisions log

**Difference vs Topics**: Projects have an outcome and end. Topics are evergreen reference indexes.

## Useful queries

Active projects by deadline:
```dataview
TABLE status, deadline
FROM "Projects"
WHERE type = "project" AND status = "active"
SORT deadline ASC
```

Open todos across all projects:
```tasks
not done
path includes Projects
```

## Entries

Manual list — kept in sync on every `/ingest` (fallback if Dataview is off).

<!-- Fictional example page. Delete it once you start adding your own. -->

- [[personal-site-redesign]] — Outcome: ship a new personal site by end of quarter. Pulls from [[knowledge-management]].
