---
title: "Transazioni (Blockchain)"
course: Sistemi Distribuiti
category: concept
topics: [transazione, TX, validità, funzione di transizione, state machine]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Transazioni (Blockchain)

Le **transazioni** (TX, a.k.a. *inputs, messages*) sono gli **input** della macchina a stati replicata: rappresentano variazioni (potenziali, proposte) del [[blockchain-world-state|world state]].

## 1. Struttura

```
TX ::= ⟨ UserID, Operation, Arguments, Signature ⟩
```

- **UserID** — l'indirizzo `f(K_pub^U)` dell'utente che emette la TX;
- **Operation** — descrizione dell'operazione da applicare allo stato;
- **Arguments** — argomenti dell'operazione;
- **Signature** — firma crittografica della TX.

L'insieme delle operazioni possibili è **application-specific**; nel caso generale servono almeno un'operazione per **creare** asset e una per **aggiornarli**. Esempio criptovaluta:

```
TX ::= ⟨ UserID, deposit,  Value, Signature ⟩
     | ⟨ UserID, transfer, UserID, Value, Signature ⟩   (sender, receiver)
```

## 2. Validità

Una transazione è **valida** se:
1. è **ben formata**;
2. la firma **corrisponde** all'indirizzo dell'emittente;
3. la firma **certifica l'integrità** della transazione.

(Le tre condizioni poggiano su [[strumenti-crittografici-blockchain|firma digitale]].)

## 3. Esecuzione — la funzione di transizione δ

Una TX valida si applica a uno stato `s` tramite la funzione `δ`, producendo un nuovo stato `s'` (lo *stato risultante* di tx):

```
δ(s, tx) = s'
```

> ⭐ **`δ(·, ·)` è la macchina a stati (cioè il programma) che viene replicato** su tutti i nodi. È il cuore SMR della blockchain.

Può accadere che `s' = s` (la TX non cambia nulla), ad esempio se il mittente non ha fondi sufficienti in `s`, o per un errore a livello applicativo. La determinismo di `δ` garantisce che tutte le repliche, partendo dallo stesso stato e processando gli stessi input **nello stesso ordine**, attraversino la stessa successione di stati (vedi [[state-machine-replication]]).

## 4. Connessioni

- [[blockchain-world-state]] — le TX modificano il world state.
- [[blocks-e-block-chain]] — le TX sono raggruppate in blocchi, che applicano `δ` in sequenza tramite la funzione `ℓ`.
- [[state-machine-replication]] — `δ` è la macchina a stati deterministica replicata; l'ordine degli input è il problema da risolvere col consenso.
- [[byzantine-fault-tolerance]] — TX percepite in ordine diverso dalle repliche generano fault bizantini → serve il consenso sull'ordine.
- [[smart-contract]] — con gli SC le operazioni si estendono a `deploy / invoke / transfer`.

## 5. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — Transactions), slide 86–90.
