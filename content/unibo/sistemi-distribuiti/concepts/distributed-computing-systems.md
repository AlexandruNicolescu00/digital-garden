---
title: "Distributed Computing Systems (Cluster & Grid)"
course: Sistemi Distribuiti
category: concept
topics: [cluster, grid, parallel computing, virtual organisation, Beowulf, Foster]
difficulty: intermedio
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Distributed Computing Systems (Cluster & Grid)

## 1. Definizione

> **Distributed computing system** = sistema che usa una **molteplicità di computer distribuiti** per svolgere compiti ad **alte prestazioni**.

È la prima delle [[sorts-sistemi-distribuiti|tre classi]]. Si articola in due sotto-classi: **cluster** e **grid**.

## 2. Cluster Computing Systems

**Idea base:** una collezione di workstation/PC **simili**, con lo **stesso OS**, nella **stessa area**, interconnessi da una **LAN ad alta velocità**.

**Motivazione:** il rapporto prezzo/prestazioni sempre migliore rende più conveniente costruire un "supercomputer" mettendo insieme tanti computer semplici, anziché comprarne uno ad alte prestazioni. In più: maggiore **robustezza**, manutenzione e **aggiunta incrementale** di potenza di calcolo più facili.

**Uso:** **parallel computing** — tipicamente un singolo programma computazionalmente intensivo eseguito **in parallelo** su più macchine. → è il caso *parallelo* della tassonomia M2 ([[parallelo-concorrente-distribuito]]): stesso contesto temporale, memoria/rete strettamente accoppiate.

**Esempio — Beowulf clusters:** Linux-based; ogni cluster è una collezione di nodi di calcolo controllati e acceduti tramite un **singolo master node**.

## 3. Grid Computing Systems

**Idea base:** risorse di **organizzazioni diverse** vengono messe insieme per promuovere la **collaborazione** tra individui, gruppi o istituzioni, **attraversando i confini organizzativi**. La collaborazione prende la forma di una **virtual organisation**:
- una nuova entità organizzativa *virtuale* che include persone di organizzazioni esistenti;
- che accede a risorse messe a disposizione dalle organizzazioni partecipanti (server, database, dischi…).

> Per loro natura, i grid trattano **domini amministrativi diversi** → si lega all'*administrative scalability* di [[scalabilita|Neuman]].

### Architettura a layer [Foster et al., 2001]
| Layer | Ruolo |
|-------|-------|
| **fabric** | interfaccia alle risorse locali di un sito specifico |
| **resource** | gestione delle singole risorse (es. access control) |
| **connectivity** | protocolli di comunicazione per le transazioni grid (su più risorse) + protocolli di sicurezza/autenticazione |
| **collective** | accesso a risorse multiple — resource discovery, allocazione |
| **application** | applicazioni che operano nella virtual organisation |

> **Grid middleware layer** = il nucleo è dato dai layer **connectivity + resource + collective**: insieme forniscono **accesso uniforme** a risorse altrimenti disperse. → è il [[middleware]] applicato al grid; realizza l'*access transparency* ([[trasparenza]]).

## 4. Cluster vs Grid — omogeneità vs eterogeneità

| | **Cluster** | **Grid** |
|---|---|---|
| Hardware | simile/omogeneo | **eterogeneo** |
| OS | stesso OS | diversi |
| Rete | stessa LAN (locale) | geografica, tra organizzazioni |
| Domini amministrativi | uno | **molti** |
| Idea-chiave | potenza di calcolo aggregata | collaborazione / virtual organisation |

> In essenza: i **cluster sono omogenei**, i **grid sono eterogenei**.

## 5. Connessioni

- [[sorts-sistemi-distribuiti]] — la classe di cui questa è la prima.
- [[middleware]] — il grid middleware (connectivity+resource+collective) per accesso uniforme.
- [[scalabilita]] (M4) — i grid attraversano domini amministrativi (administrative scalability).
- [[parallelo-concorrente-distribuito]] (M2) — il cluster come parallel computing.
- [[motivazioni-sistemi-distribuiti]] · [[obiettivi-sistemi-distribuiti]] — resource sharing/availability come motore del grid; grid systems già citati [Foster et al., 2001].
- [[containerization]] · [[kubernetes]] (CX) — l'erede "moderno" del calcolo su molti nodi.

## 6. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Distributed Computing Systems), slide 9–17.
- Riferimenti: Tanenbaum & van Steen 2017; Foster et al. 2001 (anatomy of the grid).
