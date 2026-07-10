---
type: index
folder: Library
---

# Library

Books, articles, papers, videos, podcasts. Anything consumed for ideas.

**File naming**: `Title - Author.md` (or just `Title.md` for short titles).
**Templates**: use `Templates/book.md` for books, `Templates/article.md` for articles, papers, newsletters, threads.

## Useful queries

### Books

Currently reading:
```dataview
TABLE authors, started
FROM "Library"
WHERE type = "book" AND status = "reading"
SORT started DESC
```

Books done, 4★+:
```dataview
TABLE authors, rating, finished
FROM "Library"
WHERE type = "book" AND status = "done" AND rating >= 4
SORT finished DESC
```

### Articles

To read:
```dataview
TABLE authors, clipped
FROM "Library"
WHERE type = "article" AND status = "to-read"
SORT clipped DESC
```

Read:
```dataview
TABLE authors, clipped, rating
FROM "Library"
WHERE type = "article" AND status = "done"
SORT clipped DESC
```

## Entries

Manual list — kept in sync on every `/ingest` (fallback if Dataview is off).

<!-- Pagine d'esempio (fittizie). Cancellale quando cominci ad aggiungere le tue. -->

### Books
- [[Thinking in Systems - Donella Meadows]] — Come si comportano i sistemi; stock, flussi, cicli di feedback. Alimenta [[knowledge-management]].

### Articles
- [[The wiki-LLM approach - Andrej Karpathy]] — Tratta la conoscenza come un codebase mantenuto da un LLM. L'idea su cui è costruito tutto questo vault.
