---
title: "Distributed Ledger Technology (DLT)"
course: Sistemi Distribuiti
category: concept
topics: [DLT, ledger, consensus, middleware, standard, blockchain]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Distributed Ledger Technology (DLT)

## 1. Definizione

**DLT [ITU FG DLT, 2017].** La *Distributed Ledger Technology* si riferisce ai processi e alle tecnologie correlate che permettono ai nodi di una rete di **proporre, validare e registrare in modo sicuro** cambiamenti di stato (update) su un **ledger sincronizzato** distribuito tra i nodi della rete.

**DLT [ISO/TC 307, 2024 — ISO 22739].**
- **ledger** — un *information store* che mantiene record di transazioni destinate a essere **finali, definitive e immutabili**;
- **distributed ledger** — un ledger condiviso tra un insieme di nodi DLT e **sincronizzato tra essi tramite un meccanismo di consenso**;
- **distributed ledger technology** — la tecnologia che abilita l'operatività e l'uso dei distributed ledger.

> 🔑 **Dov'è la sincronizzazione?** Il *ledger* è sincronizzato, **mentre i nodi e la rete non sono tenuti a essere sincroni**. La DLT funziona quindi come **layer di sincronizzazione per i processi** — esattamente ciò che ci si aspetta da un [[middleware]] in un sistema distribuito. Il collante è il [[agreement-consensus|consenso]].

## 2. Intuizione

Un *ledger* è un registro che traccia **ordine e tempistica** delle transazioni. Renderlo "distribuito" significa replicarlo su molti nodi che non si fidano l'uno dell'altro, e tenerlo coerente **senza un intermediario centrale fidato**, usando il consenso. La DLT è la generalizzazione di cui la [[blockchain]] è il caso più noto.

## 3. DLT vs BCT (Blockchain Technology)

⚠️ **DLT ⊋ BCT.** La *blockchain technology* (BCT) è chiaramente una DLT, ma:

> una blockchain è **un modo** di implementare un distributed ledger — **non tutti** i distributed ledger usano blockchain.

Esempi di DLT **non-blockchain**:
- **IOTA** (Tangle — un DAG, non una catena);
- **Hedera Hashgraph**;
- **CORDA**.

→ vedi [[blockchain]] per il caso blockchain.

## 4. Standard, benefici, use case

### Standardizzazione
I sistemi distribuiti sono tipicamente richiesti di essere **aperti** (oltre che sicuri/affidabili/disponibili); gli standard condivisi sono il cuore di ogni sistema aperto, e il middleware è il luogo naturale degli standard. Enti:
- **ITU** (International Telecommunication Union) → **ITU FG DLT** (Focus Group on Application of DLT);
- **ISO** → **ISO/TC 307** (blockchain and DLT).

Dopo il 2018 la blockchain esce dal Gartner Hype Cycle; ITU (2019) e ISO (2020) concludono con successo gli sforzi di standardizzazione → segnale di maturazione verso l'exploitation industriale.

### Benefici [FG DLT, 2019c]
- tecnologia sicura e cost-effective per servizi globalmente scalabili;
- **tamper-resistant**, auditabile, resistente a fallimenti sistemici;
- strumento per rilevare/mitigare frodi;
- come istanza di middleware, è una **General Purpose Technology (GPT)**: non vale per sé, porta guadagni di produttività ad altre tecnologie e settori (adozione lenta ma trasversale).
- Benefici specifici: **trasparenza & fiducia** (risorsa condivisa fidata per interazioni tra utenti *non fidati*), **sicurezza** (cifratura, access control, identity management, fault tolerance), **incentivi** (riduzione costi via disintermediazione).

### Use case [FG DLT, 2019c]
Finanza (lettere di credito, assicurazioni), pharma (supply di vaccini, distribuzione farmaci), medicina (registri sanitari, organi/sangue), ICT (number portability), energia (P2P energy trading via smart contract), …

## 5. Connessioni

- [[middleware]] — la DLT **è** middleware (consensus-as-a-service).
- [[blockchain]] — il caso particolare più diffuso di DLT.
- [[agreement-consensus]], [[consensus-problemi]] — il meccanismo di sincronizzazione del ledger.
- [[state-machine-replication]] — il ledger replicato è una macchina a stati replicata.
- [[teorema-cap]] — un ledger distribuito vive sotto i vincoli CAP (→ scelta A+P + [[eventual-consistency]]).

## 6. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Parte I — DLT), slide 17–28.
- Riferimenti: ITU FG DLT 2017/2019a-c; ISO/TC 307 2020/2024.
