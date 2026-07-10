---
type: index
folder: Templates
---

# Templates

Templater templates. Point Templater (Settings → Templater → "Template folder location") at this directory.

| Template | Use for |
|---|---|
| `person.md` | New entry in `People/` for `person` or `public-figure` |
| `company.md` | New entry in `Companies/` for any company or organisation |
| `book.md` | New entry in `Library/` for books (has started / finished / rating) |
| `article.md` | New entry in `Library/` for articles, papers, newsletters, threads |
| `exploration.md` | New entry in `Explorations/` |
| `daily.md` | Daily note in `Journal/YYYY-MM-DD.md` |

**Adding a template**: drop a new `.md` file here. Use Templater syntax: `<% tp.file.title %>`, `<% tp.date.now("YYYY-MM-DD") %>`, etc.
