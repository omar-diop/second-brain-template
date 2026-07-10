# Second Brain, template

> Questo template accompagna l'articolo [Come uso l'AI come second brain](https://builderslog.substack.com/p/come-uso-lai-come-second-brain).

Un second brain personale in stile "wiki LLM" (à la Karpathy) in markdown Obsidian. Testo semplice, così un LLM può leggerlo, ragionarci sopra e **mantenerlo attivamente**. Tu catturi le fonti grezze; un agente (Claude Code) le distilla in pagine wiki collegate che compongono nel tempo.

Questo è un **template di partenza**. Include:
- lo schema a 3 livelli ([CLAUDE.md](CLAUDE.md)),
- tre skill di Claude Code: `/ingest`, `/query`, `/lint` (in `.claude/skills/`),
- i template Obsidian (`Templates/`),
- un piccolo set di **pagine d'esempio fittizie** per vedere com'è fatto un vault popolato.

> Non copiare la struttura alla cieca: modellala sui tuoi vincoli. Le pagine d'esempio servono a mostrare il meccanismo; cancellale e rendilo tuo.

## Architettura, 3 livelli

| Livello | Cartelle | Proprietario |
|---|---|---|
| **Fonti grezze** (immutabili) | `Inbox/`, `Journal/` | Le scrivi tu, Claude le legge |
| **Wiki** (mantenuto dall'LLM) | `People/`, `Companies/`, `Library/`, `Topics/`, `Explorations/`, `Projects/`, `Content Ideas/`, `Profile.md` | Claude crea e aggiorna |
| **Schema** | `CLAUDE.md`, `index.md`, `log.md` | Co-evoluto |

Tu butti dentro le fonti grezze. Claude le legge e sintetizza il wiki.

## Guida di setup

1. **Clona o forka** questo repo e apri la cartella nel tuo editor.
2. **Installa [Claude Code](https://claude.com/claude-code)** e aprilo dentro questa cartella (`claude` da terminale, o l'estensione IDE). Le tre skill vivono in `.claude/skills/` e sono già pronte: le richiami con `/ingest`, `/query`, `/lint`.
3. **Compila `Profile.md`**: è un template vuoto. Claude lo legge per primo a ogni query, quindi è ciò che rende le risposte *tue*.
4. **Apri la stessa cartella come vault Obsidian** e installa i plugin community:
   - **Templater** → Impostazioni → Templater → "Template folder location" = `Templates`
   - **Dataview**
   - **Tasks**
5. Configura il plugin core **Daily Notes**:
   - New file location: `Journal`
   - Date format: `YYYY-MM-DD`
   - Template file location: `Templates/daily`
6. (Consigliato) Installa l'estensione browser **Obsidian Web Clipper**:
   - Save location: `Inbox`
   - Template basato su `Templates/article.md`
7. **Prova il giro completo**: lancia `/ingest` su `Inbox/example-clipped-article.md` per vedere come una fonte grezza diventa pagine collegate.
8. **Cancella le pagine d'esempio** (tutto ciò marcato come fittizio) e comincia ad alimentare il tuo.

## Workflow quotidiano

### 1. Cattura (tu, a basso attrito)

- Pensieri, call, incontri → `Journal/YYYY-MM-DD.md` (apri la nota del giorno in Obsidian)
- Articoli clippati dal web → `Inbox/` (via Web Clipper)
- Link, idea, trascrizione di una nota vocale → `Inbox/`

### 2. Processa (Claude mantiene il wiki)

Lancia `/ingest` in Claude Code. Claude legge una fonte da `Inbox/` (o una nota `Journal/` non ancora ingerita), ne discute con te i punti chiave, scrive l'eventuale scheda `Library/`, aggiorna le pagine entità collegate, aggiorna `index.md`, marca la fonte come processata e aggiunge una riga a `log.md`.

### 3. Interroga (Claude legge il wiki)

Lancia `/query <domanda>` in Claude Code. Claude legge prima `index.md`, entra nelle pagine rilevanti, risponde con `[[citazioni]]` e propone di archiviare la risposta come nuova pagina, così la conoscenza compone.

Domande d'esempio:
- *"Chi conosco che lavora su strumenti per la conoscenza?"*
- *"Cosa ho letto sul systems thinking? Sintetizza."*
- *"Prima di decidere sulla newsletter, quali note e persone rilevanti ho già?"*

### 4. Manutieni (periodico)

Lancia `/lint` in Claude Code per un health-check del wiki: pagine orfane, contraddizioni, affermazioni superate, pagine mancanti, buchi di dati. È in sola lettura, restituisce una checklist.

## Skill

In `.claude/skills/`, versionate col vault:

- `/ingest` — processa una fonte grezza da `Inbox/` (o una nota `Journal/`) nel wiki
- `/query <domanda>` — risponde interrogando il wiki, con citazioni
- `/lint` — health-check del wiki

## Rendilo tuo

- Riscrivi `Profile.md` per chi sei tu.
- Cancella gli esempi fittizi (People, Companies, Library, Topics, Explorations, Projects, Content Ideas, il Journal e i file Inbox d'esempio), poi resetta `index.md` e `log.md`.
- Aggiungi o togli cartelle di Livello 2 in base al tuo lavoro: lo schema è un punto di partenza, non una gabbia. Aggiorna `CLAUDE.md` quando cambi la struttura, è co-evoluto con te.

## Convenzioni (in breve)

Versione completa in [CLAUDE.md](CLAUDE.md).

- `Nome Cognome.md` per le persone, `Nome Azienda.md` per le organizzazioni, `kebab-case.md` per i topic
- Date ISO `YYYY-MM-DD`
- Frontmatter YAML su ogni pagina wiki (`type`, `tags` minimo)
- `[[link]]` invece dei tag quando il target è un'entità
- Ogni pagina wiki finisce con una sezione `# Sources` che cita le fonti grezze
- Struttura in inglese, contenuto nella lingua che ti è più naturale

---

*Il concetto si basa sull'[approccio wiki-LLM di Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).*
