---
title: "Blockchain ↔ Teoria del Corso (mappa capstone)"
course: Sistemi Distribuiti
category: bridge
topics: [blockchain, SMR, CAP, consenso, BFT, replicazione, middleware]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Blockchain ↔ Teoria del Corso (mappa capstone)

C4 è il **caso studio capstone**: la [[blockchain]] non introduce (quasi) teoria nuova, ma **incarna** quasi tutti i concetti del corso in un'unica architettura. Questa pagina mappa concetto teorico → meccanismo blockchain (l'analogo di [[teoria-vs-kubernetes]], ma per la DLT).

## 1. La mappa

| Concetto teorico | Meccanismo nella blockchain |
|---|---|
| [[middleware]] | la DLT **è** middleware: consensus-as-a-service per i processi |
| [[state-machine-replication]] (C3) | `δ(s, tx) = s'` è la macchina a stati replicata; i nodi sono repliche P2P |
| [[universal-smr]] | la SC-enabled blockchain è una SMR **universale** (interprete replicato) |
| [[replicazione]], [[replica-management]] (M3) | il ledger è replicato su ogni nodo; *process replication* del validatore |
| [[teorema-cap]] (C1) | la blockchain sceglie **A + P** → [[eventual-consistency]] (spesso probabilistica) |
| [[agreement-consensus]], [[consensus-problemi]] (C3) | il consenso conferma il prossimo blocco ([[consensus-mining]]) |
| [[flp-impossibility]] (C3) | impossibilità nell'asincrono → i protocolli procedono per **round** |
| [[classificazione-guasti]] (M1) — fault bizantini/arbitrari | [[byzantine-fault-tolerance|BFT]], bound `f < N/3`, [[pbft|PBFT]] |
| [[paxos]] (C3) | Paxos/Raft = consenso **non-BFT** permissioned (Chubby, ZooKeeper) |
| [[mezzi-dependability]] (M1) — ridondanza, fault tolerance | replicazione + consenso BFT come *means* di dependability |
| [[checkpointing-e-logging]], [[causalita]] (C2) | la [[blocks-e-block-chain|hash chain]] è un **log immutabile e ordinato** (timestamping, Haber & Stornetta) |
| [[centralizzato-vs-distribuito]] (M0) | rimozione del **single point of trust** (non solo single point of failure) |
| [[computazione]] (M2) — universalità di Turing | `USMR : SMR = UTM : TM = interprete : programma` |

## 2. La tesi unificante

> La blockchain prende il problema del [[notary-service|notary service]] (tracciare *chi-fa-cosa-quando* in modo non manomettibile) e lo rende **decentralizzato** rimuovendo il single point of trust, replicando il validatore ([[state-machine-replication|SMR]]) e tenendo le repliche in sync col **consenso** — il tutto offerto come [[middleware]].

L'arco è: *[[strumenti-crittografici-blockchain|crittografia]] → [[notary-service|notary]] → replicazione (SMR) → consenso (BFT/PoW) → [[smart-contract|smart contract]] (USMR)*.

## 3. Una tensione da segnalare

Il modello SMR di C4 sceglie esplicitamente **A+P + eventual consistency** (anche *probabilistica*, via [[proof-of-work|PoW]]), mentre il consenso "classico" del corso ([[paxos]], [[pbft|PBFT]]) privilegia la **safety/consistency**. Non è una contraddizione: sono i due rami del [[permissioned-vs-permissionless|trade-off permissioned/permissionless]], ai due lati del [[teorema-cap|CAP]] e dell'[[cap-vs-flp|FLP]].

## 4. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (intero modulo), Omicini & Ciatto, A.Y. 2025/2026.
