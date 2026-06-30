---
title: "Consensus & Mining (Blockchain)"
course: Sistemi Distribuiti
category: concept
topics: [consenso, mining, permissioned, permissionless, BFT, throughput]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Consensus & Mining (Blockchain)

Il **consenso** è il collante che mantiene **in sync** le repliche del ledger ([[blockchain]]): è ciò che, allo step 4 del [[blocks-e-block-chain|ciclo di vita del blocco]], decide quale sarà il prossimo blocco. Le tecniche si dividono in due grandi famiglie a seconda del modello di identità.

## 1. Le due famiglie

### Permissioned BCT
Gli ID degli utenti sono **vincolati tramite CA** (Certification Authority). Si usano algoritmi di consenso **"classici" quorum/leader-based**:
- **BFT**: [[pbft|PBFT]] [Castro & Liskov, 2002], BFT-SMaRt, HoneyBadgerBFT;
- **non-BFT** (solo crash, non bizantini): [[paxos|Paxos]] [Lamport, 1998], [[paxos|Raft]] [Ongaro & Ousterhout, 2014], ZooKeeper, Google Chubby.

### Permissionless BCT
Accesso aperto a qualunque coppia `(K_pub, K_pr)`. Si usano approcci **"nuovi" competition-based**:
- **[[proof-of-work|Proof-of-Work]]** [Back, 2002], **Proof-of-Stake** [Kiayias et al., 2017], **Proof-of-Elapsed-Time** [Chen et al., 2017], **IOTA Tangle** [Popov et al., 2019].

## 2. Trade-off fondamentale

| | **Permissioned** (BFT/quorum) | **Permissionless** (competition) |
|---|---|---|
| Assunzioni su N | sì, N e gli ID noti; UB ~ 100/1000 repliche | nessuna assunzione su N; UB virtualmente ∞ |
| Throughput | **alto** (OoM ~ 1000 TX/s) | **basso** (OoM ~ 10 TX/s) |
| Consistenza | **"esatta"** | **probabilistica** ([[eventual-consistency]]) |
| Difesa Sybil | tramite **CA** | tramite **PoW** (potenza di calcolo) |
| Ideale per | organizzazioni chiuse multi-amministrative | **sistemi aperti** (es. criptovalute) |

*UB = Upper Bound, OoM = Order of Magnitude, CA = Certification Authority.*

> 🔑 È il [[teorema-cap|trade-off CAP]] declinato sul consenso: il ramo permissioned punta alla **consistenza forte** (come [[paxos|Paxos]]/PBFT, sacrificando scala/apertura), il ramo permissionless punta a **disponibilità + partition-tolerance + apertura** accettando consistenza solo **eventuale**. Vedi [[permissioned-vs-permissionless]].

## 3. Connessioni

- [[permissioned-vs-permissionless]] — bridge dedicato al confronto.
- [[pbft]] — il rappresentante BFT permissioned.
- [[proof-of-work]] — il rappresentante competition-based permissionless.
- [[byzantine-fault-tolerance]] — perché serve il consenso (fault bizantini, f<N/3).
- [[agreement-consensus]], [[consensus-problemi]] (C3) — il problema generale del consenso.
- [[paxos]] (C3) — Paxos/Raft come consenso non-BFT permissioned.
- [[eventual-consistency]], [[teorema-cap]] — il versante consistenza.

## 4. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — Consensus & Mining), slide 104–105.
- Riferimenti: Castro & Liskov 2002; Lamport 1998; Ongaro & Ousterhout 2014; Back 2002; Kiayias et al. 2017; Chen et al. 2017; Popov et al. 2019; Aublin et al. 2015; Cachin & Vukolić 2017.
