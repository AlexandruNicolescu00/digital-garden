---
title: "Blockchain"
course: Sistemi Distribuiti
category: concept
topics: [blockchain, ledger, P2P, consensus, SMR, immutabilità]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Blockchain

## 1. Definizione

Una **blockchain** è un **ledger condiviso, trasparente, distribuito e append-only** — cioè un registro che traccia **ordine e tempistica** delle transazioni — con queste proprietà:

- **messo in sicurezza con schemi crittografici** ([[strumenti-crittografici-blockchain|hash 1-way, firme digitali, certificati, cifratura]]);
- **replicato su molti nodi di una rete peer-to-peer**, eliminando il bisogno di un intermediario centralizzato fidato e rendendo il ledger **immutabile**;
- le transazioni sono **approvate e propagate tramite [[agreement-consensus|consenso]]**, prevenendo perdita/corruzione di dati dovuta a macchine che crashano o **mentono** (robustezza ai [[byzantine-fault-tolerance|fallimenti bizantini]]).

**Use case ideale:** un *hub* sicuro per parti **mutuamente non fidate** che devono interagire.

> Più formalmente (top-down, ispirato a Ethereum [Wood, 2014]):
> **una blockchain è un'astuta implementazione di un sistema [[state-machine-replication|SMR]]** che tiene traccia di quali utenti possiedono certi *asset*, tramite un **ledger replicato** aggiornato per **consenso**.

## 2. Intuizione — blockchain ≠ Bitcoin

Errore comune: `blockchain = criptovalute = Bitcoin`. In realtà:

- la **blockchain** è *un modello* per computazione **distribuita & decentralizzata**, implementabile in molti modi, in domini eterogenei, con obiettivi disparati;
- **Bitcoin** [Nakamoto, 2008] è *una specifica implementazione* di blockchain, in un dominio specifico (criptovalute) con un obiettivo specifico (digitalizzare e decentralizzare la moneta fiat).

Le relazioni di inclusione: `criptovalute ⊂ smart contracts ⊂ blockchain`; Bitcoin, Ethereum, Hyperledger Fabric sono istanze diverse. C4 adotta un approccio **top-down** (modello generale prima delle tecnologie specifiche) per non perdere generalità.

## 3. I quattro elementi principali

Un BCT tiene traccia di chi possiede cosa, con:

| Elemento | Cos'è |
|----------|-------|
| **users** | identificati da chiavi pubbliche; firmano le proprie transazioni → [[blockchain-world-state]] |
| **assets** | qualunque cosa abbia valore, un proprietario ed eventualmente stato mutabile (denaro, documenti, atti notarili/di proprietà, contratti) → [[blockchain-world-state]] |
| **ledger** | un [[notary-service|servizio notarile replicato]] che conserva proprietà/evoluzione degli asset → [[blocks-e-block-chain]] |
| **consensus** | protocollo distribuito che mantiene e aggiorna **in sync** le repliche del ledger → [[consensus-mining]] |

**Architettura.** Le **repliche** R₁…R_N memorizzano una copia dell'**intero ledger** e raggiungono il consenso tra loro; i **client** C₁…C_M propongono update degli asset a una qualsiasi R_i e **non** hanno bisogno di memorizzare l'intero ledger. È una rete **P2P** di repliche con lo stesso comportamento osservabile ([[state-machine-replication|SMR]]).

## 4. Genealogia delle proprietà (recap)

- **replicazione + hash-chaining** ⇒ *untamperability* del passato;
- **hash-chain + tempo + ordering** ⇒ servizio di [[notary-service|timestamping/notary]] [Haber & Stornetta, 1991];
- **hash-chain + firma crittografica** ⇒ *accountability* + non-ripudio;
- i blocchi sono pubblicati (quasi) periodicamente (parametro Δt);
- **consistenza probabilistica**: `lim(n→∞) P[inconsistent(B_i)] = 0`, con *n* = numero di blocchi successori di B_i.

## 5. Connessioni

- [[distributed-ledger-technology]] — la blockchain è il caso particolare più diffuso di DLT.
- [[state-machine-replication]] — la blockchain **è** SMR (e [[universal-smr|USMR]] quando abilita gli [[smart-contract]]).
- [[strumenti-crittografici-blockchain]], [[notary-service]] — i mattoni da cui emerge.
- [[blockchain-world-state]], [[blockchain-transactions]], [[blocks-e-block-chain]] — il modello dati.
- [[consensus-mining]], [[proof-of-work]], [[byzantine-fault-tolerance]] — il collante.
- [[teorema-cap]] — la blockchain sceglie **A + P** ed [[eventual-consistency]] (spesso probabilistica).

## 6. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Parte II — Expectations, Background, Blockchain Main Elements), slide 35–98.
- Riferimenti: Nakamoto 2008, Wood 2014 (Ethereum), Androulaki et al. 2018 (Hyperledger Fabric), Schneider 1990, Haber & Stornetta 1991.
