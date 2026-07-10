---
title: The wiki-LLM approach
source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
author: Andrej Karpathy
clipped: 2026-01-15
type: clip
---

# The wiki-LLM approach

> Cattura di esempio (fittizia). È com'è fatto un articolo clippato prima che `/ingest` lo trasformi in una pagina `Library/`. Questo è già stato processato (per questo vive in `_processed/`), producendo [[The wiki-LLM approach - Andrej Karpathy]].

Obsidian is the IDE, the LLM is the programmer, the wiki is the codebase.

L'idea: tieni la conoscenza come file markdown e punta un agente LLM alla cartella. L'agente legge i file, ne scrive di nuovi, li collega e tiene tutto in ordine. Tu catturi il materiale grezzo; l'agente fa la distillazione e i rimandi incrociati.

Il wiki cresce fino a centinaia di migliaia di parole, quasi tutte scritte e mantenute dall'agente. A mano ci metti le mani di rado: guidi, revisioni, e gli dai fonti nuove.

Il payoff non è l'archiviazione. È che emergono da sole connessioni che non avresti mai disegnato a mano, e la conoscenza compone: ogni fonte nuova viene confrontata con tutto ciò che c'è già.
