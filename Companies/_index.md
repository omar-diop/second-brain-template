---
type: index
folder: Companies
---

# Companies

Organisations, companies, and institutions as linkable entities.

**File naming**: `Company Name.md` (so `[[MetaCortex]]`, `[[Acme]]` work naturally).

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

<!-- Pagina d'esempio (fittizia). Cancellala quando cominci ad aggiungere le tue. -->

- [[MetaCortex]] — Piccolo studio dove lavora [[Trinity]]. Costruisce strumenti per la conoscenza.
