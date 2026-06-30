---
title: "Processo Computazionale e Contesto"
course: Sistemi Distribuiti
category: concept
topics: [processo-computazionale, contesto, spazio, tempo, situated]
difficulty: intermedio
sources: [M2-Roots-of-Distributed-Systems-Computation-in-Space-Time.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Processo Computazionale e Contesto

Ontologia di base del corso (M2): i mattoni con cui si definiranno parallelo, concorrente e distribuito (→ [[parallelo-concorrente-distribuito]]).

## Processo computazionale

Assunzioni:
- il processo computazionale **elementare** è **sequenziale**;
- essendo l'espressione fenomenica della dinamica di una macchina computazionale, ha sia **input/output** sia **contesto**;
- di conseguenza, una **macchina computazionale è il luogo** dove avviene un processo computazionale.

Rappresentazione grafica del corso: il processo (computazione sequenziale) è il **cerchio blu**; il **contesto** è l'area **grigia** che lo avvolge.

## Contesto della computazione

Cos'è il contesto quando si parla di computazione? Quattro componenti:

1. **computing machine** (la macchina)
2. **resources** (risorse)
3. **time** (tempo)
4. **space** (spazio)

Si rappresenta il contesto **ogni volta** che una o più di queste componenti sono **rilevanti/essenziali** per modellare/capire la computazione — cioè per **capire la dinamica** del processo computazionale.

## Tipi di computazione secondo il contesto

- **timed computation** — quando il **tempo** della macchina è rilevante/essenziale.
- **spatial computation** — quando le **feature spaziali** della macchina sono rilevanti/essenziali.
- **situated computation** [Suchman, 1987] — più in generale, quando l'**ambiente** della macchina è rilevante/essenziale; dove l'ambiente è una combinazione significativa di feature **temporali e spaziali** con le **risorse** richieste dalla computazione.

## Perché è importante

Questa ontologia è lo strumento con cui M2 distingue i tipi di sistema: **la scelta del tipo di contesto definisce il tipo di sistema computazionale** (parallelo / concorrente / distribuito). In particolare, sarà il **contesto temporale** a distinguere parallelo da concorrente, e il **contesto spaziale** a definire distribuito. → [[parallelo-concorrente-distribuito]].

## Connessioni

- [[computazione]] — la nozione su cui si fonda
- [[sistema-computazionale]] — quando i processi diventano due o più e interagiscono
- [[parallelo-concorrente-distribuito]] — la tassonomia che ne deriva
- [[distribuzione-spaziale-temporale]] — l'eco in M0 di spazio/tempo come assi della distribuzione
- [[situatedness]] — **(M4)** la situatedness come goal: il processo *situated* immerso nel contesto spazio-temporale e nell'ambiente
- [[computing-needs-time]] (M6) · [[computing-with-space]] (M7) — i due assi del contesto (tempo e spazio) approfonditi; sintesi in [[tempo-vs-spazio-nel-computing]]
- [[code-mobility]] (C6) — il processo come azioni+stato+contesto ↔ i 3 segmenti (code/execution/resource) che si possono migrare

## Sorgenti

- `raw-sources/M2-Roots-of-Distributed-Systems-...pdf` (slides 31–39)
- [Suchman, 1987] *Plans and Situated Actions*.
