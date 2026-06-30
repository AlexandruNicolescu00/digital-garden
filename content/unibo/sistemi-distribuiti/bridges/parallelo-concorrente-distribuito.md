---
title: "Parallelo vs Concorrente vs Distribuito"
course: Sistemi Distribuiti
category: bridge
topics: [parallelo, concorrente, distribuito, contesto, spazio, tempo, ordinamento]
difficulty: intermedio
sources: [M2-Roots-of-Distributed-Systems-Computation-in-Space-Time.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Parallelo vs Concorrente vs Distribuito

La distinzione centrale di M2, costruita sulla nozione di **contesto** (→ [[processo-computazionale-contesto]], [[sistema-computazionale]]). L'idea unificante: **il tipo di contesto che differisce tra i processi definisce il tipo di sistema**.

## Le tre definizioni di riferimento

Dato un sistema computazionale (≥ 2 processi che computano e interagiscono):

| Sistema | Definizione (quando…) | Contesto che cambia |
|---------|----------------------|---------------------|
| **Parallelo** | il contesto **temporale è lo stesso** per tutti i processi | tempo condiviso |
| **Concorrente** | almeno due processi hanno un contesto **temporale diverso** | tempo (T ≠ T′) |
| **Distribuito** | almeno due processi hanno un contesto **spaziale diverso** | spazio (S ≠ S′) |

- Il **contesto temporale** distingue **parallelo** da **concorrente**.
- Il **contesto spaziale** definisce il **distribuito**.

## Il punto chiave: ordinamento degli eventi

La differenza parallelo/concorrente si riflette nell'**ordinamento relativo degli eventi**:

- in un sistema **parallelo**, gli eventi sono **totalmente ordinati** (c'è un tempo comune T);
- in un sistema **concorrente**, gli eventi sono **al più parzialmente ordinati**.

> 🔗 Questo è *esattamente* il nodo di [[ordinamento-parziale-eventi]] (M0): senza un tempo di sistema condiviso, resta solo l'ordine parziale. M2 lo inquadra come la firma del concorrente.

Due accezioni di concorrenza [Degano & Montanari, 1987]:
- **interleaving** — gli eventi di processi diversi possono avvenire in *qualsiasi* ordine relativo; relazioni temporali/causali non rilevanti;
- **true concurrency** — si usano **ordini parziali** per catturare *esplicitamente* le relazioni temporali/causali tra eventi.

## Parallel computing: una precisazione

In letteratura "parallel computing" indica spesso processi non sequenziali che computano **nello stesso momento**, tipicamente su architetture **multi-core**, per problemi *computationally-intensive*. Nell'ontologia del corso ciò corrisponde al caso "stesso contesto temporale T".

## Distributed computing vs distributed systems

Ciò che conta è la **distribuzione fisica**:
- **distributed computing** — focus sulla distribuzione spaziale dei **processi**; "processi asincroni su dispositivi diversi che comunicano via **message passing** (no memoria condivisa)" [Kshemkalyani & Singhal, 2011]; *"attività eseguita su un sistema spazialmente distribuito"* [Lamport & Lynch, 1990].
- **distributed systems** — focus sulla distribuzione spaziale dei **dispositivi**; *"a collection of independent computers that appears to its users as a single coherent system"* [Tanenbaum & van Steen, 2017] (→ raffina [[sistema-distribuito]]).

Quindi: il **contesto spaziale** definisce entrambi.

## Le combinazioni (spazio × tempo)

Spazio e tempo sono assi indipendenti, quindi si combinano:

| Caso | Spazio | Tempo |
|------|--------|-------|
| Parallelo (non distribuito) | S comune | **T comune** |
| Concorrente (non distribuito) | S comune | **T ≠ T′** |
| **Distributed parallel** | **S ≠ S′** | T comune |
| **Distributed concurrent** | **S ≠ S′** | **T ≠ T′** |

## Sintesi

- **tempo** → parallelo (stesso T, ordine totale) vs concorrente (T diversi, ordine parziale);
- **spazio** → distribuito (S diversi);
- un sistema distribuito reale è tipicamente anche **concorrente** (niente clock globale → ordine parziale), il che riconduce al [[teorema-cap]] e all'intero corso.

## Connessioni

- [[sistema-computazionale]] · [[processo-computazionale-contesto]] — i mattoni dell'ontologia
- [[ordinamento-parziale-eventi]] — concorrente = eventi parzialmente ordinati (M0)
- [[sistema-distribuito]] — la definizione di Tanenbaum
- [[distribuzione-spaziale-temporale]] — gli assi spazio/tempo della distribuzione
- [[teorema-cap]] — perché un SD concorrente senza clock globale ha i limiti del CAP
- [[tempo-nei-sistemi-distribuiti]] — **(M6)** il recap dei contesti temporale/spaziale e l'issue of time (distributed concurrent = modello più generale)
- [[process-algebra]] — **(M9)** la **concorrenza** è la *prima fonte di complessità* nei SD; la process algebra è il suo **modello formale** (la parallel composition `∥`, interleaving vs interazione via comunicazione)

## Sorgenti

- `raw-sources/M2-Roots-of-Distributed-Systems-...pdf` (slides 49–61)
- [Degano & Montanari, 1987]; [Shonkwiler & Lefton, 2006]; [Kshemkalyani & Singhal, 2011]; [Lamport & Lynch, 1990]; [Tanenbaum & van Steen, 2017].
