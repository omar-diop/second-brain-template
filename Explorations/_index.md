---
type: index
folder: Explorations
---

# Explorations

Research / planning boards on a specific topic. Each exploration has a **guiding question** and a status (`active` | `paused` | `concluded`).

Use `Templates/exploration.md`.

Explorations are time-bounded by intent — once the guiding question is answered, the exploration concludes and the synthesis becomes either:
- A new Topic (if the theme is evergreen)
- A new Project (if it's actionable)
- Or simply `status: concluded` in frontmatter (the note stays where it is, queries filter it out)

## Useful queries

Active explorations:
```dataview
TABLE started, tags
FROM "Explorations"
WHERE status = "active"
SORT started DESC
```

Concluded explorations (for retrospection):
```dataview
LIST FROM "Explorations"
WHERE status = "concluded"
SORT file.mtime DESC
```

## Entries

Manual list — kept in sync on every `/ingest` (fallback if Dataview is off).

<!-- Fictional example page. Delete it once you start adding your own. -->

- [[should-i-start-a-newsletter]] — Guiding question: is a newsletter worth it for me right now? References [[knowledge-management]] and [[Jordan Blake]].
