---
title: "Byzantine Fault Tolerance (BFT)"
course: Sistemi Distribuiti
category: concept
topics: [byzantine fault, BFT, consenso, f<N/3, FLP, eventual consistency]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Byzantine Fault Tolerance (BFT)

La **Byzantine Fault Tolerance** è la capacità di un sistema [[state-machine-replication|SMR]] di restare corretto (garantendo [[eventual-consistency|eventual consistency]]) anche quando alcune repliche **mentono** o si comportano arbitrariamente — i [[classificazione-guasti|fault bizantini]].

## 1. Da dove nascono i fault bizantini

Nel contesto blockchain, per il **valore** potenzialmente custodito nel ledger, i *bad actor* hanno **incentivi economici** a causare fault:

1. Alice e Bob, simultaneamente, aggiornano un sistema replicato contattando una replica;
2. il risultato finale dipende dall'**ordinamento delle richieste**;
3. tutte le repliche devono **concordare un ordinamento** perché le operazioni siano committate;
4. **fault bizantini (i):** inconsistenze possono sorgere e impedire l'accordo (anche solo perché le repliche percepiscono le TX in ordine diverso!);
5. **fault bizantini (ii):** i nodi possono **deliberatamente mentire**;
6. una *4ª replica* può rompere lo stallo…

> La prospettiva di una replica che produce fault bizantini è **terribile rispetto alla consistenza**: può impedire l'accordo *indefinitamente*. In assenza di BFT, una replica può alterare timing/ordinamento dei blocchi o escludere eventi.

## 2. BFT consensus algorithm

> **Definizione.** Un *BFT consensus algorithm* è un protocollo distribuito che rende un sistema SMR **resistente** ai fallimenti bizantini (cioè garantisce [[eventual-consistency|eventual consistency]]).

La blockchain, come i database distribuiti, sfrutta tipicamente algoritmi di consenso **BFT** per prevenire i fallimenti bizantini (es. [[pbft|PBFT]], BFT-SMaRt, HoneyBadgerBFT).

## 3. Le due impossibilità del consenso

Il consenso distribuito è **impossibile** sotto una qualunque di queste assunzioni:

1. **rete asincrona** [[flp-impossibility|[Fischer-Lynch-Paterson, 1985]]] — se i messaggi possono essere riordinati o ritardati arbitrariamente.
   → *è il motivo per cui tutti i protocolli di consenso procedono per **round periodici***.

2. **troppi bizantini:** se il numero `f` di repliche bizantine è tale che

   ```
   f ≥ N / 3        (N = numero totale di repliche)
   ```
   [Lamport, Shostak & Pease, 1982 — Byzantine Generals Problem].
   → *è il motivo per cui serviva la **4ª replica**: con f = 1 servono N ≥ 4 (cioè f < N/3).*

> 🔗 Questo **rafforza e generalizza** il messaggio del [[teorema-cap|CAP]] (vedi [[cap-vs-flp]]) e completa l'[[flp-impossibility|FLP]] aggiungendo il vincolo numerico sui guasti *bizantini* (FLP basta 1 crash *benigno*).

## 4. La risposta della blockchain: scegliere A+P

Il sistema SMR blockchain affronta le sfide dei sistemi aperti così:
- messaggi persi/corrotti/riordinati → trasporto robusto (TCP) + hash per integrità;
- mancanza di tempo globale → UTC via NTP;
- **[[teorema-cap|CAP]]** → **eventual consistency** (la blockchain sceglie **P e A**, come i DB distribuiti tipo MongoDB/NoSQL; spesso la consistenza è governata da regole **probabilistiche**);
- problema dei generali bizantini → **ridondanza + consenso BFT**.

## 5. Connessioni

- [[classificazione-guasti]] (M1) — i fault bizantini/arbitrari, qui operazionalizzati e con il bound f < N/3.
- [[flp-impossibility]] (C3) — l'impossibilità nel modello asincrono; qui affiancata al vincolo bizantino.
- [[pbft]] — un protocollo BFT concreto (leader, prepare/commit).
- [[proof-of-work]] — l'alternativa *competition-based* (permissionless) ai BFT classici.
- [[eventual-consistency]], [[teorema-cap]] — la consistenza che la BFT garantisce e la scelta A+P.
- [[consensus-mining]], [[permissioned-vs-permissionless]] — dove si collocano gli algoritmi BFT.
- [[physical-clock-synchronization]] (M6) — l'assunzione "macchine con accesso a UTC via NTP" usata per ordinare gli eventi nel modello SMR.

## 6. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (SMR — Issues, CAP, Byzantine Fault Tolerance), slide 63–74.
- Riferimenti: Fischer et al. 1985; Lamport et al. 1982; Brewer 2000; Castro & Liskov 2002.
