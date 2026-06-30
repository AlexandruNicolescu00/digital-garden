---
title: "Code Mobility (Mobilità del Codice)"
course: Sistemi Distribuiti
category: concept
topics: [code mobility, process migration, segmenti, mobile agents, cloning]
difficulty: intermedio
sources: [C6-Code-Mobility.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Code Mobility (Mobilità del Codice)

## 1. Perché muovere il codice

A volte **passare i dati non basta** [Fuggetta et al., 1998]:
- si vuole **cambiare il luogo** in cui il codice viene eseguito (load balancing, sicurezza, scalabilità);
- non si vuole **separare i dati dal codice** che opera su di essi (es. oggetti, **agenti**).

→ allora non basta passare i dati tra processi: **va passato il codice**. È la realizzazione concreta della *code mobility* citata in [[physical-space-computational-systems]] (M7) come capacità del [[middleware]].

### Reasons for migrating code
Muovere codice è tipicamente **muovere processi** (*process migration* [Milojicic et al., 2000]: tradizionalmente il codice si sposta col **contesto computazionale** intero). Motivazioni:
- **load balancing** · **minimizzare la comunicazione** · **ottimizzare la performance percepita**;
- **migliorare la scalabilità** ([[scalabilita]]) · **flessibilità** (configurabilità dinamica) · **fault tolerance** ([[mezzi-dependability]]).

## 2. L'essenza di un processo → il modello a 3 segmenti

C'è molto più del solo codice da muovere: execution status, pending signal, dati… Cos'è un processo? Ha un **insieme di azioni** da eseguire, uno **stato**, e opera in un **ambiente/contesto** — eco di [[processo-computazionale-contesto]] (M2).

> **Modello del processo per la migrazione** [Fuggetta et al., 1998] — tre **segmenti**:

| Segmento | Cosa contiene |
|----------|---------------|
| **code segment** | l'insieme delle **istruzioni eseguibili** del processo |
| **execution segment** | lo **stato di esecuzione**: dati privati, **stack**, **program counter** |
| **resource segment** | i **riferimenti alle risorse esterne** (file, stampanti, device, altri processi) |

> 🔑 **A seconda di quale porzione si muove insieme al codice, si classificano diversi tipi di mobilità** → [[weak-vs-strong-mobility|weak vs strong]].

## 3. Le dimensioni di classificazione

### Weak vs Strong mobility
- **Weak**: solo il **code segment** (+ eventuali dati di init); esecuzione *ex novo*; semplice.
- **Strong**: code **+ execution segment**; stop→move→restart; esigente, richiede supporto del [[middleware]].
→ confronto completo in [[weak-vs-strong-mobility]].

### Sender- vs Receiver-initiated
- **Sender-initiated**: la migrazione parte **dove il codice risiede/esegue**; es. **search-bot, mobile agent**; i server devono conoscere i client e proteggere le risorse → schema di interazione **più complesso**;
- **Receiver/Client-initiated**: la migrazione è iniziata dalla **macchina target** (che vuole un nuovo comportamento); es. **Java Applet, chunk JavaScript**; pochi resource da proteggere, client anche **anonimi** → schema **meno complesso**. (È il *code shipping* citato in [[scalabilita]] per nascondere la latenza.)

### Separate vs Target process execution
Nella weak mobility, il codice mobile si può eseguire nel **processo target** o in un **processo separato**. Es. gli **Applet** girano nello spazio di indirizzamento del browser (niente IPC sul target). Problema principale: **protezione** da codice malevolo o buggato → soluzione: assegnare l'esecuzione a un **processo separato** (sandboxing).

### Cloning vs Migrating
La strong mobility si può ottenere anche col **remote cloning**: il *clone* è una **copia esatta** del processo originale, eseguita sulla macchina target **in parallelo** all'originale (es. **fork** UNIX di un figlio su macchina remota). Il cloning è un'**alternativa** alla migrazione e **migliora la [[trasparenza|distribution transparency]]** (i processi sono replicati in modo trasparente) → parente concettuale della [[replicazione]].

## 4. Connessioni

- [[weak-vs-strong-mobility]] — la classificazione centrale (quale segmento si muove).
- [[migrazione-risorse]] — il problema del **resource segment** (binding alle risorse).
- [[physical-space-computational-systems]] (M7) — la code mobility come capacità del middleware (gancio risolto).
- [[middleware]] — la strong mobility richiede supporto dell'infrastruttura (es. JADE).
- [[scalabilita]] (M4) — code shipping (Applet/JS) per nascondere la latenza; load balancing.
- [[replicazione]] · [[trasparenza]] — il cloning come replica trasparente.
- [[processo-computazionale-contesto]] (M2) — processo = azioni + stato + contesto ↔ i 3 segmenti.

## 5. Sorgenti

- `raw-sources/C6-Code-Mobility.pdf` (Code Mobility), slide 6–16.
- Riferimenti: Fuggetta et al. 1998 (*Understanding code mobility*); Milojicic et al. 2000 (*Process migration*); Tanenbaum & van Steen 2017.
