---
title: "Sybil Attack"
course: Sistemi Distribuiti
category: concept
topics: [sybil attack, identità, P2P, maggioranza, sicurezza]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Sybil Attack

## 1. Definizione

> **Sybil attack** [Douceur, 2002]. Un attaccante sovverte un sistema P2P **creando un gran numero di identità pseudonime**, usandole per ottenere un'influenza **sproporzionatamente grande** nel sistema.

## 2. Quando un sistema è vulnerabile

I sistemi/protocolli P2P sono soggetti al Sybil attack quando **tutte** queste condizioni valgono:
- le **decisioni critiche** sono prese a **maggioranza**;
- le entità devono **votare** per stabilire una maggioranza;
- le entità sono identificate dai loro **ID**;
- è **facile o economicamente fattibile** per un attaccante creare molti ID.

→ È esattamente il rischio dei sistemi [[permissioned-vs-permissionless|permissionless]], in cui chiunque può generare coppie `(K_pub, K_pr)` e quindi molti [[blockchain-world-state|identificatori]].

## 3. Le due difese

| Approccio | Come neutralizza Sybil |
|-----------|------------------------|
| **Permissioned** (CA) | gli ID sono **certificati** da una Certification Authority → non si possono creare identità fittizie a costo zero |
| **Permissionless** (PoW) | il peso decisionale **non** è "una identità = un voto" ma **potenza di calcolo** ([[proof-of-work|Proof-of-Work]]): creare molte identità non aiuta, conta solo la CP → "1 CPU = 1 voto" |

> ⚠️ Nota la connessione con [[byzantine-fault-tolerance|BFT]]: i protocolli BFT classici (es. [[pbft|PBFT]]) decidono a maggioranza sulle identità → da soli sono vulnerabili a Sybil, e per questo i sistemi **permissioned** li abbinano alle CA.

## 4. Connessioni

- [[blockchain-world-state]] — pseudonimità vs identificatore certificato.
- [[proof-of-work]] — risolve Sybil legando il voto alla potenza di calcolo.
- [[permissioned-vs-permissionless]] — le due famiglie e le rispettive difese.
- [[byzantine-fault-tolerance]] — il voto a maggioranza che Sybil tenta di falsare.

## 5. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — The Sybil Attack), slide 107–108.
- Riferimento: Douceur 2002.
