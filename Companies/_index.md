---
type: index
folder: Companies
---

# Companies

Organisations, companies, and institutions as linkable entities.

**File naming**: `Company Name.md` (so `[[Northwind Labs]]`, `[[Acme]]` work naturally).

Use `Templates/company.md` for new entries.

## Useful queries

All active companies:
```dataview
TABLE industry, hq, size
FROM "Companies"
WHERE status = "active"
SORT file.name ASC
```

Companies I have contacts at:
```dataview
TABLE industry
FROM "Companies"
SORT file.name ASC
```

## Entries

Manual list — kept in sync on every `/ingest` (fallback if Dataview is off).

<!-- Fictional example page. Delete it once you start adding your own. -->

- [[Northwind Labs]] — Small product studio where [[Maya Chen]] works. Building knowledge tools.
