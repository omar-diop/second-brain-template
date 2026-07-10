---
type: index
folder: People
---

# People

Everyone in the network — real contacts, public figures, companies. Distinguished via `type` in frontmatter:

- `person` — humans you've met or interacted with directly
- `public-figure` — authors, founders, experts you follow but haven't met

**File naming**: `First Last.md`

Companies and organisations live in `Companies/`.

## Useful queries

People I should reconnect with (sorted by oldest last contact):
```dataview
TABLE role, company, last_contact
FROM "People"
WHERE type = "person" AND status = "active"
SORT last_contact ASC
LIMIT 15
```

People met in 2026:
```dataview
LIST role + " @ " + company
FROM "People"
WHERE type = "person" AND contains(string(met_at), "2026")
```

People by tag (replace `fintech`):
```dataview
LIST FROM "People" AND #fintech
```

## Entries

Manual list — kept in sync on every `/ingest` (fallback if Dataview is off).

<!-- Pagine d'esempio (fittizie). Cancellale quando cominci ad aggiungere le tue. -->

### Person
- [[Trinity]] — Product designer e founder @ [[MetaCortex]]. Conosciuta a [[Zion Conf 2026]]. Interessata a [[knowledge-management]].

### Public figure
- [[Morpheus]] — Autore tech che seguo; scrive di systems thinking e dev tooling.
