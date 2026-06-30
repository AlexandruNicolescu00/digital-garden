---
title: "Distributed Information Systems (TPS & EAI)"
course: Sistemi Distribuiti
category: concept
topics: [information systems, transaction processing, EAI, integrazione, middleware]
difficulty: intermedio
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Distributed Information Systems (TPS & EAI)

## 1. Definizione

> **Distributed information system** = sistema nato dall'esigenza di **integrare molte applicazioni di rete separate**, affrontando i **problemi strutturali di interoperabilità**.

È la seconda delle [[sorts-sistemi-distribuiti|tre classi]]. Due tipologie:

1. **Transaction Processing Systems** — diversi server non-interoperabili condivisi da più client → *distributed queries*, *distributed transactions* (→ [[transazioni-distribuite]]);
2. **Enterprise Application Integration (EAI)** — applicazioni sofisticate (non solo database, ma anche componenti di processo) che devono **comunicare direttamente** tra loro.

## 2. Transaction Processing Systems

Le operazioni sui database si fanno in termini di **transazioni**; quando i database sono distribuiti, anche le transazioni devono esserlo, con primitive speciali fornite dal sistema distribuito/runtime. La proprietà-cardine sono gli **[[acid-vs-base|ACID]]** (atomic, consistent, isolated, durable). Il cuore tecnico — transazioni **nested**, durabilità, TP monitor — è sviluppato nella pagina dedicata: → **[[transazioni-distribuite]]**.

## 3. Enterprise Application Integration (EAI)

> Non è solo questione di **accedere** a database distribuiti: l'integrazione deve avvenire **anche a livello applicativo**.

- oltre alla **data integration**, serve la **process integration**;
- le applicazioni devono **interagire e comunicare in modo significativo** tra loro;
- il **[[middleware]] come *communication facilitator*** è la soluzione: media la comunicazione applicazione-applicazione.

> 🔗 EAI è già citato in [[middleware]] (C4): enfasi sulla comunicazione **orizzontale** (application-to-application, middleware-to-middleware), in opposizione ai soli layer verticali OS/middleware/app.

## 4. Connessioni

- [[sorts-sistemi-distribuiti]] — la classe di cui questa è la seconda.
- [[transazioni-distribuite]] — il nucleo tecnico (ACID, nested, TP monitor).
- [[middleware]] — communication facilitator; EAI come task orizzontale del middleware.
- [[acid-vs-base]] — gli ACID delle transazioni vs il rilassamento BASE.
- [[consistency]] — l'integrazione dati e la consistenza tra sistemi.
- [[architectural-styles]] (M8) — i sistemi web-based come stile **data-centred**; l'EAI come integrazione orizzontale.

## 5. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Distributed Information Systems), slide 19, 27–28.
- Riferimento: Tanenbaum & van Steen 2017.
