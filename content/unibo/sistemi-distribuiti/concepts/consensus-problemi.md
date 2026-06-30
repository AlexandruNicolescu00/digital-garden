---
title: "Le Varianti del Problema del Consenso e le sue Proprietà"
course: Sistemi Distribuiti
category: concept
topics: [consensus, agreement, broadcast, proprietà, validità]
difficulty: avanzato
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Le Varianti del Problema del Consenso e le sue Proprietà

Il consenso non è un singolo problema ma una **famiglia** di problemi [Fischer, 1983]. Cambiando i requisiti, cambia il problema.

## Le varianti formali

### 1. Basic (single-bit) consensus
La forma più elementare: accordo su **un singolo bit**.
- un numero fisso *n* di processori, alcuni eventualmente **faulty**;
- ogni processore ha un **bit iniziale** *xᵢ*;
- i processi **non-faulty** devono accordarsi su un bit *y*, il **consensus value**.

La risolubilità dipende dall'esistenza di un **protocollo** in cui tutti i processi non-faulty **terminano con lo stesso valore** *y*.

### 2. Interactive consistency problem
Analogo al single-bit, ma l'obiettivo è accordarsi su un **vettore** *y*, il **consensus vector**: ogni componente rappresenta ciò che i processi pensano del valore iniziale di un dato processo. *"Consensus is about what any processor thinks about the initial value."*

### 3. Generals problem (= Reliable Broadcast)
Un processore specifico (il **general**, anche detto *transmitter*) prova a inviare il proprio bit iniziale *x* a **tutti** gli altri; i processi affidabili devono raggiungere consenso su *y*. È noto anche come **reliable broadcast problem**.

### 4. Transaction commit [Dolev and Strong, 1982]
Tutti i data manager che hanno partecipato a una transazione devono accordarsi se **installare i risultati** nel DB o **scartarli** (abort). Qualunque sia la decisione, **tutti devono prendere la stessa**, per preservare la **consistenza del DB**. → istanza concreta che lega il consenso al 2PC ([[tamir-sequin-checkpointing]]).

## Le proprietà / requisiti di correttezza

Requisiti diversi → problemi diversi. I principali:

| Proprietà | Significato |
|-----------|-------------|
| **Non-triviality** | *y* ∈ {0,1}; esistono un vettore *xᵢ* e un'esecuzione del protocollo che portano a *y* (l'output **non** è predeterminato/costante) |
| **Dependency** | *y* deve dipendere *in qualche modo* dagli input *x* |
| **Strong unanimity** | se **tutti** gli *xᵢ* = *x* ∈ {0,1}, allora *y* = *x* |
| **Weak unanimity** | come la strong, **ma solo se** non avvengono guasti a runtime |

Nella pratica algoritmica (es. [[paxos]]) le proprietà si enunciano spesso come il trio:
- **Agreement** (safety) — tutti i processi non-faulty decidono lo **stesso** valore;
- **Validity** (safety) — il valore deciso è uno dei valori **proposti** (≈ non-triviality/dependency);
- **Termination** (liveness) — ogni processo non-faulty prima o poi **decide**.

> ⚠️ È esattamente la **termination (liveness)** che il teorema [[flp-impossibility|FLP]] dimostra **non garantibile** nel modello asincrono puro: i protocolli reali preservano agreement+validity e **sacrificano** la garanzia di terminazione.

## Consenso bizantino (C4)

C4 aggiunge l'altro lato dell'impossibilità: oltre all'asincronia ([[flp-impossibility|FLP]]), il consenso è impossibile se le repliche **bizantine** sono `f ≥ N/3` ([[byzantine-fault-tolerance]], Byzantine Generals). I protocolli **BFT** ([[pbft|PBFT]]) e *competition-based* ([[proof-of-work|PoW]]) sono le risposte pratiche; nella [[blockchain]] il consenso è proprio ciò che ordina le [[blockchain-transactions|TX]] tra repliche non fidate.

## Connessioni

- [[agreement-consensus]] — il problema generale e la sua motivazione
- [[flp-impossibility]] — l'impossibilità che colpisce la *termination*
- [[paxos]] — garantisce agreement & validity, non la termination
- [[modello-sincrono-asincrono]] — il modello che decide la risolubilità
- [[tamir-sequin-checkpointing]] — il commit come istanza di consenso (C2)
- [[byzantine-fault-tolerance]] · [[pbft]] · [[consensus-mining]] — il consenso bizantino e le sue varianti permissioned/permissionless (C4)

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 13–16; Fischer 1983, Dolev & Strong 1982)
