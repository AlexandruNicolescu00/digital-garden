---
title: "Componenti, Connettori e Dati (Elementi Architetturali)"
course: Sistemi Distribuiti
category: concept
topics: [componenti, connettori, dati, interfacce, interazione]
difficulty: intermedio
sources: [M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Componenti, Connettori e Dati (Elementi Architetturali)

I tre **elementi architetturali** di cui è fatta una [[software-architecture|software architecture]] [Fielding, 2000]. È l'**ontologia componenti-connettori** già richiamata in [[agreement-consensus]] (C3), qui resa esplicita.

## 1. Componente

> Un **componente** è un'**unità modulare con interfacce ben definite**, **rimpiazzabile** all'interno del proprio ambiente.

- le interfacce sono sia **required** sia **provided** — in **entrambe le direzioni** (ciò che il componente offre e ciò di cui ha bisogno);
- (Fielding) un componente è un'**unità astratta di istruzioni software e stato interno** che fornisce una **trasformazione di dati** tramite la sua interfaccia.

→ è la formalizzazione del *processo che behave & interact* di [[sistema-computazionale]] (M2).

## 2. Connettore

> Un **connettore** è un'**astrazione che media la comunicazione, la coordinazione e la cooperazione** tra componenti — cioè *qualunque cosa fornisca un meccanismo di interazione* tra componenti.

- (Fielding) un connettore è un **meccanismo astratto** che media comunicazione/coordinazione/cooperazione tra componenti;
- esempi concreti: un meccanismo **RPC**, un **event bus**, un **repository condiviso**, un **data-space** — gli stili architetturali differiscono proprio per il **tipo di connettore** ([[architectural-styles]]).

> 🔗 Il connettore è il luogo dell'**interazione**: media non solo *comunicazione* ma *coordinazione* ([[coordinazione]], M6) e cooperazione.

## 3. Dato

> Un **datum** è un **elemento di informazione** trasferito da un componente, o ricevuto da un componente, **tramite un connettore**.

## 4. Composizione

Mettendo insieme **componenti e connettori** si produce un'enorme gamma di organizzazioni e configurazioni possibili, che vengono poi **classificate** in termini di [[architectural-styles|stili architetturali]]. Le **proprietà** dell'architettura emergono dalla selezione e disposizione di questi elementi sotto **vincoli** ([[software-architecture]]).

## 5. Connessioni

- [[software-architecture]] — il quadro: architettura = configurazione vincolata di questi elementi.
- [[architectural-styles]] — gli stili si distinguono per il **tipo di connettore** e di interazione.
- [[agreement-consensus]] (C3) — l'ontologia componenti-connettori usata per inquadrare il consenso.
- [[sistema-computazionale]] (M2) — processi che si comportano e interagiscono.
- [[coordinazione]] (M6) — i connettori mediano coordinazione e cooperazione, non solo comunicazione.

## 6. Sorgenti

- `raw-sources/M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf` (Components & Connectors; Architectural Elements), slide 9–12.
- Riferimento: Fielding 2000.
