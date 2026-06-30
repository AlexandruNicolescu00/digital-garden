---
title: "Permissioned vs Permissionless (Consenso Blockchain)"
course: Sistemi Distribuiti
category: bridge
topics: [permissioned, permissionless, BFT, proof-of-work, CAP, throughput]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Permissioned vs Permissionless (Consenso Blockchain)

Le due grandi famiglie di [[blockchain]], distinte dal **modello di identità** e dal **tipo di consenso** ([[consensus-mining]]). È il [[teorema-cap|trade-off CAP]] e il [[sybil-attack|problema Sybil]] declinati sul consenso.

## 1. Confronto

| Caratteristica | **Permissioned** | **Permissionless** |
|---|---|---|
| Identità ([[blockchain-world-state]]) | ID certificati da **CA** | ID liberi `(K_pub, K_pr)`, pseudonimi |
| Consenso | **quorum/leader-based**: [[pbft|PBFT]], BFT-SMaRt, HoneyBadger; **non-BFT**: [[paxos|Paxos]], Raft, ZooKeeper, Chubby | **competition-based**: [[proof-of-work|PoW]], PoS, PoET, IOTA Tangle |
| Assunzioni su N | N e ID **noti**; UB ~ 100/1000 | **nessuna**; UB virtualmente ∞ |
| Throughput | **alto** (~1000 TX/s) | **basso** (~10 TX/s) |
| Consistenza | **"esatta"** (consistenza forte) | **probabilistica** ([[eventual-consistency]]) |
| Difesa Sybil | tramite **CA** | tramite **PoW** (potenza di calcolo) |
| Rischio identità | single point of trust (CA) | Sybil se non c'è PoW |
| Ideale per | organizzazioni **chiuse** multi-amministrative | **sistemi aperti** (criptovalute) |

## 2. Vantaggi / svantaggi

**Permissioned (+):** prestazioni elevate, finalità immediata e "esatta", governance nota.
**Permissioned (−):** richiede una CA fidata (centralizzazione della fiducia), non scala a reti aperte enormi.

**Permissionless (+):** apertura totale, nessuna autorità centrale, resistenza Sybil senza CA, ideale per ambienti adversariali.
**Permissionless (−):** basso throughput, alto costo energetico (PoW), consistenza solo eventuale/probabilistica, finalità solo statistica (≥ n blocchi).

## 3. Sintesi — quando scegliere cosa

- **Consorzio chiuso** (banche, supply chain tra aziende note) → **permissioned + BFT** (es. Hyperledger Fabric): conta velocità e finalità, l'identità è già governabile.
- **Sistema pubblico, senza autorità** (criptovaluta globale) → **permissionless + PoW/PoS** (es. Bitcoin, Ethereum): conta apertura e resistenza Sybil, si accetta throughput basso ed eventual consistency.

> 🔑 È lo stesso bivio del [[teorema-cap|CAP]] e dell'[[cap-vs-flp|FLP]]: il ramo permissioned privilegia **Consistency** (come [[paxos]]/PBFT), il ramo permissionless privilegia **Availability + Partition tolerance + apertura** accettando consistenza eventuale.

## 4. Connessioni

- [[consensus-mining]] — l'inquadramento generale.
- [[pbft]], [[proof-of-work]] — i due rappresentanti.
- [[sybil-attack]] — le due difese.
- [[teorema-cap]], [[cap-vs-flp]], [[eventual-consistency]] — la radice teorica del trade-off.

## 5. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Consensus & Mining I–II), slide 104–105.
