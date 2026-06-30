---
title: "Transazioni Distribuite (ACID, Nested, TP Monitor)"
course: Sistemi Distribuiti
category: concept
topics: [transazioni, ACID, nested transactions, TP monitor, durabilità, commit]
difficulty: intermedio
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Transazioni Distribuite (ACID, Nested, TP Monitor)

## 1. Definizione

Le operazioni sui database si eseguono in termini di **transazioni**; quando i database sono **distribuiti**, le transazioni devono diventare distribuite, con primitive speciali fornite dal sistema distribuito o dal runtime. È il cuore dei [[distributed-information-systems|Transaction Processing Systems]].

## 2. Le proprietà ACID

> Una transazione deve esibire le proprietà **ACID** [Tanenbaum & van Steen, 2017]:

| Proprietà | Significato |
|-----------|-------------|
| **Atomicity** | i passi della transazione avvengono in modo **invisibile** al mondo esterno (tutto o niente) |
| **Consistency** | la transazione **non viola** gli invarianti del sistema |
| **Isolation** | transazioni concorrenti **non interferiscono** tra loro |
| **Durability** | una volta che la transazione **commit**, i suoi effetti sono **permanenti** |

> ⚠️ Attenzione terminologica: la **"C" di ACID** (non violare gli invarianti) è cosa **diversa** dalla *consistency* del [[teorema-cap|CAP]] (atomic data object, stessa vista per tutti) — flag già presente in [[acid-vs-base]] e [[consistency]].

## 3. Transazioni nested

Una **transazione nested** è composta da un numero di **subtransaction**; l'annidamento può essere **arbitrariamente profondo**.

### Il problema della durabilità
- l'intera transazione nested deve esibire le proprietà ACID;
- quindi, se una **subtransaction fallisce**, **tutte** le subtransaction fino a quel punto vanno **annullate**, *anche se avevano già committato*;
- gli effetti delle subtransaction **non possono essere davvero durabili** se l'intera transazione non ha successo.

> 🔑 La **durabilità qui si riferisce alla transazione top-level**, non alle singole subtransaction.

### Soluzione: private copy of the world
- tutte le transazioni operano su una **copia** dei dati → le subtransaction mantengono l'ACIDità nel "mondo locale";
- l'effetto di una transazione nested di successo è **propagato solo dopo** che ha avuto successo;
- se ha successo, la copia trasformata **diventa** il mondo; in ogni caso le transazioni restano ACID.

> 🔗 Questa idea ("lavora su una copia, commit solo alla fine") è imparentata con il **checkpoint/private working area** del recovery ([[checkpointing-e-logging]]) e con il *resulting state* delle [[blockchain-transactions|transazioni blockchain]].

## 4. Le transazioni nested come modo naturale di distribuire

> Le **transazioni nested sono il modo naturale** per distribuire le transazioni:
> - le subtransaction "foglia" sono normali transazioni su **singoli server**;
> - le **transazioni distribuite SONO transazioni nested**.

### TP monitor (soluzione storica)
Il **Transaction Processing monitor (TP monitor)** è una soluzione *early* che permette alle applicazioni di accedere a **più server DB** con una **semantica transazionale**, coordinando l'insieme come un'unica transazione.

## 5. Connessioni

- [[distributed-information-systems]] — la classe (TPS) in cui vivono.
- [[acid-vs-base]] — ACID vs il rilassamento BASE; le due filosofie ai due estremi del [[teorema-cap|CAP]].
- [[consensus-problemi]] (C3) — il **transaction commit** è una *variante del consenso* (Dolev-Strong): tutti i data manager devono decidere insieme commit/abort.
- [[tamir-sequin-checkpointing]] (C2) — il **2PC** (two-phase commit) realizza il commit atomico distribuito; bloccante, a differenza del consenso a quorum di [[paxos]].
- [[checkpointing-e-logging]] (C2) — "private copy of the world" ↔ private working area / rollback.
- [[blockchain-transactions]] (C4) — un altro modello di transazione con stato risultante e validità.

## 6. Da approfondire (gancio)

- **2PC / 3PC** in dettaglio: il tema "Transazioni distribuite: 2PC, 3PC" del programma. M5 introduce le transazioni distribuite e i TP monitor; i protocolli di commit atomico restano da approfondire (2PC accennato in [[tamir-sequin-checkpointing]]).

## 7. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Distributed Information Systems — Transactions), slide 20–26.
- Riferimento: Tanenbaum & van Steen 2017.
