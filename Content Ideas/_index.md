---
type: index
folder: Content Ideas
---

# Content Ideas

Post idea pipeline fed by the **Rule of 3 trigger** during `/ingest`. Each file = one candidate post for a channel you publish on, surfaced because the same concept appeared in **≥3 distinct ingested sources** (raw layer: `Inbox/_processed/` + ingested `Journal/`) **AND** is content-shape for you (things you say / find worth sharing with your audience — not just any recurring topic).

**States**: `idea` → `drafting` → `published` | `dropped`. Filter on `status` to see what's hot.

**Template**: `Templates/post-idea.md`.

**Naming**: kebab-case, descriptive of the angle (e.g. `notes-are-the-discovery-engine.md`, not `idea-1.md`).

## Useful queries

Open ideas:
```dataview
TABLE status, trigger_count, target, created
FROM "Content Ideas"
WHERE type = "post-idea" AND status = "idea"
SORT created DESC
```

In draft:
```dataview
LIST FROM "Content Ideas"
WHERE type = "post-idea" AND status = "drafting"
```

## Entries

Manual list — kept in sync on every `/ingest` (fallback if Dataview is off).

<!-- Pagina d'esempio (fittizia). Cancellala quando cominci ad aggiungere le tue. -->

### Idea
- [[compounding-knowledge]] — *"la conoscenza compone quando la colleghi, non quando la archivi"*. Trigger: [[The wiki-LLM approach - Andrej Karpathy]] + [[Thinking in Systems - Donella Meadows]] + [[Journal/2026-01-15]]. trigger_count 3.
