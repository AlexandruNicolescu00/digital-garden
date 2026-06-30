---
title: "Protocollo di Checkpointing Globale di Tamir & Sequin"
course: Sistemi Distribuiti
category: theorem
topics: [checkpointing, coordinato, blocking, 2PC, recovery]
difficulty: avanzato
sources: [C2-Logging-Checkpointing.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Protocollo di Checkpointing Globale di Tamir & Sequin

## Cosa risolve

Produrre un **checkpoint globale consistente** (→ [[global-state-consistency]]) in modo **coordinato**, così da poter recuperare il sistema dopo un guasto. È un protocollo **coordinato** e **blocking** [Tamir and Sequin, 1984].

## Ruoli e meccanismo

- Un processo è il **coordinator** (conosce tutti gli altri); gli altri sono **participants**.
- Il coordinator usa un **two-phase commit (2PC)** per garantire che i checkpoint dei singoli processi siano **consistenti** tra loro.
- L'operazione di checkpointing globale è **atomica**: o **tutti** i processi creano il nuovo set di checkpoint, o si **aborta** e si torna al set precedente (transazione).
  - **fase 1**: creare un punto **quiescente** del sistema (tutti hanno fermato l'esecuzione normale);
  - **fase 2**: passare **atomicamente** dal vecchio checkpoint al nuovo.
- Il round si **aborta** se un participant non risponde (timeout).

## Messaggi di controllo

| Messaggio | Scopo |
|-----------|-------|
| **CHECKPOINT** | inizia il checkpoint globale; stabilisce il punto quiescente (tutti fermi) |
| **SAVED** | un participant informa il coordinator di aver fatto il checkpoint locale |
| **FAULT** | timeout occorso → abortire il round corrente |
| **RESUME** | il coordinator dice ai participant di riprendere l'esecuzione normale |

## Comportamento del coordinator (FSM)

1. ferma l'esecuzione normale e invia **CHECKPOINT** a ogni processo (fase 1);
2. attende **CHECKPOINT** da tutti (aborta se ne mancano);
3. fa il **checkpoint** del proprio stato;
4. attende **SAVED** da tutti (aborta inviando **FAULT** se ne mancano);
5. passa al nuovo checkpoint e invia **RESUME** a tutti;
6. riprende l'esecuzione normale.

## Comportamento del participant (FSM)

1. alla ricezione di **CHECKPOINT**, ferma l'esecuzione normale;
2. propaga **CHECKPOINT** su tutti i canali uscenti e attende **CHECKPOINT** da tutti gli entranti (aborta se mancano);
3. fa il **checkpoint** del proprio stato;
4. inoltra **SAVED** dal vicino downstream verso il vicino upstream;
5. propaga **RESUME** su tutti i canali uscenti (tranne quello da cui è arrivato);
6. riprende l'esecuzione normale.

> I messaggi regolari ricevuti durante il checkpointing vengono **loggati** (channel state).

## Proprietà

- **Blocking**: l'esecuzione normale è **sospesa** durante ogni round.
- **Coordinato**, **atomico** (2PC), più **completo e robusto** ma più conservativo.
- Cattura il **channel state** per garantire la recoverability.

## Connessioni

- [[global-state-consistency]] — produce uno stato globale consistente + channel state
- [[chandy-lamport-snapshot]] — l'alternativa **nonblocking** (confronto in [[protocolli-recovery]])
- [[checkpointing-e-logging]] — la tecnica di base
- 2PC → il **commit di transazioni** è un'istanza del problema del **consenso** (→ [[consensus-problemi]], [[agreement-consensus]], C3)
- [[paxos]] — **(C3)** consenso a quorum: tollera i crash che possono **bloccare** un 2PC; [[flp-impossibility]] spiega perché nessun commit asincrono garantisce la terminazione
- [[transazioni-distribuite]] — **(M5)** il 2PC come protocollo di commit atomico delle transazioni distribuite (ACID, nested, TP monitor)

## Sorgenti

- `raw-sources/C2-Logging-Checkpointing.pdf` (slides 34–40)
- [Tamir and Sequin, 1984] *Error recovery in multicomputers using global checkpoints*, ICPP '84.
