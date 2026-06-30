---
title: "ACID vs BASE"
course: Sistemi Distribuiti
category: bridge
topics: [consistency, transazioni, ACID, BASE, trade-off]
difficulty: intermedio
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# ACID vs BASE

Due filosofie opposte di gestione dei dati nei sistemi distribuiti, ai due estremi del trade-off del [[teorema-cap]].

## Cosa significano gli acronimi

| ACID | BASE |
|------|------|
| **A**tomicity | **BA** — Basically Available |
| **C**onsistency | **S** — Soft state |
| **I**solation | **E** — Eventual consistency |
| **D**urability | |

## Tabella di confronto

| Aspetto | ACID | BASE |
|---------|------|------|
| Priorità | correttezza / consistenza forte | disponibilità / performance |
| Consistenza | immediata, garantita | [[eventual-consistency\|eventuale]] |
| Stato | durevole | soft (rigenerabile, non durevole) |
| Risposte | esatte | anche approssimate, ma veloci |
| Posizione nel CAP | lato **C** (CP) | lato **A** (AP) |
| Esempi tipici | DBMS relazionali tradizionali | eBay, Amazon DynamoDB, molti NoSQL |

## La motivazione del passaggio a BASE

[Fox et al., 1997] osservano che la semantica ACID può essere **troppo forte**: lo spazio di progetto dei servizi di rete va partizionato secondo la semantica dei dati che ciascun servizio richiede. Per molti servizi Internet il valore primario per l'utente **non è** la consistenza forte o la durabilità, ma l'**alta disponibilità** dei dati.

Quindi: dove l'utente preferisce una risposta rapida (anche su dati un po' stantii) a una risposta esatta ma lenta, BASE batte ACID.

## ⚠️ Nota terminologica importante

La "**C**" di ACID (Consistency) **non** è la "consistency" del [[teorema-cap]]. Nel CAP, *consistent* (secondo [Gilbert and Lynch, 2002]) ingloba sia atomicità sia consistenza. Confondere i due significati è un errore classico. Vedi [[consistency]].

## Sintesi: quando scegliere cosa

- **ACID** quando la correttezza è critica e non negoziabile (transazioni finanziarie, in-game transactions).
- **BASE** quando disponibilità e scalabilità contano più della consistenza istantanea (feed, cataloghi, telemetria).
- I sistemi moderni spesso **mescolano** i due: es. DynamoDB con read eventualmente o fortemente consistenti configurabili (più costose). Vedi [[eventual-consistency]].

## Connessioni

- [[teorema-cap]] — il risultato che genera questo trade-off
- [[consistency]] · [[availability]] — le proprietà ai due estremi
- [[eventual-consistency]] — il modello su cui poggia BASE
- [[pokemon-go-cap]] — un sistema reale che combina forte (Spanner) e rilassato
- [[transazioni-distribuite]] — **(M5)** le proprietà ACID nelle transazioni distribuite (atomic/consistent/isolated/durable), nested transactions, TP monitor

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-...pdf` (slides 36–39, 41)
- [Fox et al., 1997] *Cluster-based scalable network services*.
