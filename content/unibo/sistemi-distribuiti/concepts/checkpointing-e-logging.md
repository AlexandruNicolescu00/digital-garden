---
title: "Checkpointing e Logging"
course: Sistemi Distribuiti
category: concept
topics: [checkpointing, logging, rollback-recovery, stable-storage, PWD]
difficulty: intermedio
sources: [C2-Logging-Checkpointing.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Checkpointing e Logging

## Le due tecniche fondamentali

**Checkpointing** e **logging** sono le tecniche **più fondamentali** per la dependability dei sistemi distribuiti: forniscono un percorso verso la **recovery** dopo un guasto. Sono una forma di **fault tolerance** facile da implementare e a basso overhead a runtime, usata a **tutti i livelli** dei meccanismi di dependability (→ [[mezzi-dependability]]). Limiti (se usate da sole): possibile perdita di informazione (solo checkpoint) e tempo di recovery più alto rispetto a tecniche più sofisticate.

### Checkpointing
Un **checkpoint** è una **copia dello stato del sistema**. Se disponibile dopo il guasto, riporta il sistema allo stato di quando fu preso. Tecnicamente = (periodicamente) copiare lo stato + **salvarlo su stable storage** che sopravvive ai guasti tollerati.

### Logging
Per recuperare il sistema **fino al punto esatto prima del guasto**, oltre ai checkpoint periodici si **loggano** informazioni: tutti i **messaggi in arrivo** e gli altri **eventi nondeterministici**.

## Rollback recovery (vs roll-forward)

Insieme, checkpointing e logging forniscono il **rollback recovery**: riportano il sistema a uno stato **precedente** al guasto (eco di [[mezzi-dependability]]). Esistono meccanismi di **roll-forward**, ma con overhead a runtime più alto e più risorse.

## Piecewise Deterministic Assumption (PWD)

I protocolli **log-based** funzionano sotto la **piecewise deterministic assumption**:
- ogni stato evolve **deterministicamente** finché non accade un **evento nondeterministico** (es. la ricezione di un messaggio);
- tutti gli eventi nondeterministici **possono essere identificati**;
- per ciascun evento si **logga abbastanza informazione**.

L'esecuzione è così modellata come **state interval** consecutivi (intervalli tra eventi nondeterministici): se l'evento iniziale è loggato, l'intero intervallo può essere **rieseguito (replay)**.

A differenza dei log-based, i protocolli **checkpoint-based** si concentrano sullo **stato** (non sugli eventi) e **non** richiedono la PWD → più semplici, ma tollerano perdita di esecuzione.

## Output commit problem

Un SD interagisce con l'esterno: quando emette un **output**, una porzione di stato diventa **osservabile** (una sorta di commitment). La recovery deve garantire una vista **observably-consistent**, ma in caso di guasto **non ci si può affidare all'esterno**: è l'**output commit problem**. → serve loggare abbastanza informazione di recovery **prima** di committare un output.

## Stable storage

Requisito essenziale: la **stable storage**, che sopravvive ai guasti e rende l'informazione disponibile al processo in recovery. Forme: **dischi locali** (solo guasti di processo), **dischi ridondanti** RAID-1/RAID-5 (guasti di disco), **file system replicati/cloud** (più robusti). Eco della **ridondanza** di [[mezzi-dependability]].

## Logging + checkpointing insieme

In pratica il **logging si usa sempre insieme al checkpointing**: gli stati sono salvati come checkpoint, gli eventi sono loggati da un checkpoint al successivo. Due benefici:
- **limita il tempo di recovery**: si riparte dall'ultimo checkpoint (non dallo stato iniziale) e si **replaya** fino al guasto;
- **limita la dimensione del log**: gli eventi prima del checkpoint si possono **garbage-collectare**.

## Connessioni

- [[mezzi-dependability]] — checkpointing/logging/rollback erano i ganci di M1, qui risolti
- [[global-state-consistency]] — il modello (stato globale + channel state)
- [[tamir-sequin-checkpointing]] · [[chandy-lamport-snapshot]] — protocolli checkpoint-based
- [[protocolli-recovery]] — la tassonomia completa (checkpoint vs log; varianti)
- [[stato-sistema-distribuito]] — stato e stable storage (M1)
- [[vector-clock]] · [[happened-before]] — **(C5)** il **causal logging** traccia le dipendenze causali tramite clock vettoriali/timestamp

## Sorgenti

- `raw-sources/C2-Logging-Checkpointing.pdf` (slides 14–16, 28–30, 48, 51)
