---
title: "PBFT — Practical Byzantine Fault Tolerance"
course: Sistemi Distribuiti
category: theorem
topics: [PBFT, BFT, consenso, leader, prepare, commit, view change]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# PBFT — Practical Byzantine Fault Tolerance

## 1. Enunciato / scopo

Il **PBFT** [Castro & Liskov, 2002] è un **protocollo di consenso BFT** ([[byzantine-fault-tolerance]]) leader-based: rende un sistema [[state-machine-replication|SMR]] resistente ai fallimenti bizantini, garantendo accordo sull'ordinamento delle richieste finché il numero di repliche faulty soddisfa `f < N/3` (cioè `N ≥ 3f+1`).

**Ipotesi:** rete (parzialmente) sincrona — il protocollo procede per **round**, come imposto dall'[[flp-impossibility|impossibilità FLP]]; al più `f` repliche bizantine su `N ≥ 3f+1`.

## 2. Funzionamento

1. tra le repliche è eletto un **leader** (primary);
2. i client inviano le richieste al leader e attendono **almeno `f+1` risposte** dalle repliche (`f` = massimo numero di repliche faulty) → garantisce che almeno una risposta venga da una replica onesta;
3. il leader fa **multicast** di ogni richiesta alle altre repliche;
4. le repliche si scambiano vari messaggi negoziando l'accordo, attraversando **due round**:
   - **prepare**
   - **commit**
5. infine le repliche inviano le risposte al client richiedente.

## 3. View change

Quando il leader è ritenuto **faulty** (es. *timeout*), se ne deve **eleggere uno nuovo**; anche questo implica che le repliche negozino il prossimo leader (*view change*).

## 4. Problemi / limiti

- **Complessità di messaggi:** quanti messaggi servono se `N = 1000`? → lo scambio all-to-all nei round prepare/commit cresce in modo **quadratico** (`O(N²)`), il che limita PBFT a reti **piccole/medie** ([[permissioned-vs-permissionless|permissioned]], UB ~ 100/1000 nodi).
- **Sybil:** cosa accadrebbe se si potessero creare ID di repliche fittizie? → PBFT decide a maggioranza sulle identità, quindi da solo è vulnerabile al [[sybil-attack|Sybil attack]] → richiede un contesto **permissioned** (CA).

## 5. Connessioni

- [[byzantine-fault-tolerance]] — PBFT è il rappresentante BFT classico; il bound `f < N/3`.
- [[consensus-mining]], [[permissioned-vs-permissionless]] — PBFT come consenso permissioned ad alto throughput.
- [[paxos]] (C3) — analogo leader-based ma solo crash-tolerant (non bizantino); confronto strutturale (prepare/commit ~ fasi Paxos).
- [[proof-of-work]] — l'alternativa permissionless/competition-based.
- [[flp-impossibility]] (C3) — perché PBFT procede per round e assume sincronia parziale.
- [[sybil-attack]] — la vulnerabilità che lo confina ai sistemi permissioned.

## 6. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — The PBFT Protocol), slide 110–111.
- Riferimento: Castro & Liskov 2002.
