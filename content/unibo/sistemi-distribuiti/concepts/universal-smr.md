---
title: "Universal State Machine Replication (USMR)"
course: Sistemi Distribuiti
category: concept
topics: [USMR, interprete, smart contract, SMR, universalità]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Universal State Machine Replication (USMR)

## 1. L'analogia universale

Come una **Universal Turing Machine** sta a una Turing Machine, e un **interprete** sta a un programma, così la **USMR** sta alla [[state-machine-replication|SMR]]:

```
UTM : TM  =  Interprete : Programma  =  USMR : SMR
```

- possiamo replicare un programma deterministico e stateful che implementa una **specifica business logic** (es. un ledger bancario) → SMR "ordinaria";
- **allo stesso modo**, possiamo replicare un programma deterministico che implementa un **interprete** (un programma che esegue altri programmi) → è ciò che chiamiamo **Universal State Machine Replication (USMR)**.

## 2. SC-enabled blockchain

Le operazioni ammissibili su tale interprete replicato dovrebbero permettere agli utenti di **deploy, undeploy e invoke** programmi.

> **Le blockchain abilitate agli [[smart-contract|smart contract]]** agiscono essenzialmente come **macchine a stati "universali" replicate** su cui si possono *deployare* smart contract.

Questo è il salto dalla 2ª alla **3ª generazione** di blockchain: non più solo asset custom, ma **codice arbitrario** eseguito in modo fidato e replicato.

## 3. Connessioni

- [[state-machine-replication]] (C3) — USMR è SMR il cui "programma replicato" è un interprete.
- [[smart-contract]] — i programmi deployati sull'interprete replicato.
- [[blockchain]] — la SC-enabled blockchain è una USMR.
- [[computazione]] (M2) — la distinzione macchina universale / programma è il cuore della computabilità (Turing).

## 4. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Universal SMR), slide 128.
