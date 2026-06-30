---
title: "Software & System Architecture"
course: Sistemi Distribuiti
category: concept
topics: [architettura software, architettura di sistema, Fielding, stile, proprietà]
difficulty: intermedio
sources: [M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Software & System Architecture

## 1. Motivazione: modellare i sistemi distribuiti

L'ontologia di base ([[sistema-computazionale]], M2) dà il terreno; ora serve un **modo generale** di rappresentare i sistemi distribuiti, **indipendentemente** dalla loro natura eterogenea. Le domande:
- come rappresentare un SD a **design time**? a **run time**?
- come rendere conto della sua **evoluzione nel tempo** (→ [[computing-needs-time]]) e della sua **distribuzione nello spazio** (→ [[computing-with-space]])?

I SD sono **complessi**: per gestirne la complessità intrinseca vanno **organizzati**, e l'organizzazione si esprime in termini di **componenti software**.

## 2. Software architecture vs system architecture

| | **Software architecture** | **System architecture** |
|---|---|---|
| Cosa esprime | l'**organizzazione logica** dei componenti | la **collocazione (placement)** dei componenti |
| Asse | logico, possibilmente **nel tempo** | **distribuzione spaziale** |
| Relazione | i molti modi di organizzare i componenti | le molte **istanziazioni** di un'architettura software, dove i componenti hanno il loro posto reale |

→ vedi il confronto in [[software-vs-system-architecture]] (lega gli assi tempo/spazio di M6/M7).

## 3. Cos'è una software architecture [Fielding, 2000]

> Una **software architecture** è un'**astrazione degli elementi a run-time** di un sistema software durante una fase della sua operatività. Un sistema può comporsi di molti livelli di astrazione e molte fasi, ciascuna con la propria architettura software.

> È **definita da una configurazione di elementi architetturali** — **componenti, connettori e dati** — **vincolati** nelle loro relazioni per ottenere un insieme desiderato di **proprietà architetturali**.

(Gli elementi — componenti/connettori/dati — sono dettagliati in [[componenti-connettori]].)

## 4. Proprietà & vincoli architetturali

- le **proprietà architetturali** derivano dalla **selezione e disposizione** di componenti, connettori e dati:
  - **proprietà funzionali**;
  - **quality attributes** ([[quality-attributes]]): facilità di evoluzione, riusabilità dei componenti, efficienza, estensibilità dinamica;
- le proprietà sono **indotte dall'insieme dei vincoli** dell'architettura;
- i **vincoli architetturali** sono spesso motivati dall'applicazione di un **principio di ingegneria del software** a un aspetto degli elementi architetturali.

## 5. Architectural style

> Uno **stile architetturale** è formulato in termini di: **componenti**, **modo in cui sono connessi**, **dati** che fluiscono, e **configurazione** complessiva. Permette di **raggruppare/classificare** sistemi simili, **confrontarli** e fornire **pattern generali** di design.

Formalmente [Fielding, 2000]: *un insieme **coordinato di vincoli architetturali** che restringe ruoli/feature degli elementi e le relazioni ammesse tra essi, in qualunque architettura conforme a quello stile* → cattura l'**essenza di un pattern** di interazione, ignorando i dettagli accidentali. → i 5 stili principali in [[architectural-styles]].

> ⚠️ Nota critica (conclusione del modulo): gli stili sono modi **approssimativi e forse non-scientifici** di modellare i sistemi, ma **espressivi e astratti** abbastanza da aiutare l'ingegneria dei SD. *Bastano per una scienza dei SD? Si possono dimostrare teoremi? Che ruolo per i formalismi "math-like" (process algebra)?* → eco di [[computer-science-foundations]] (M2). **La risposta è M9:** le architetture modellano *struttura e organizzazione*, **non il comportamento**, che resta compreso solo *qualitativamente*; per **dimostrare** proprietà comportamentali serve un formalismo → [[process-algebra]]. Confronto in [[architetture-vs-process-algebra]].

## 6. Connessioni

- [[componenti-connettori]] — gli elementi architetturali (componenti, connettori, dati).
- [[architectural-styles]] — i 5 stili per i sistemi distribuiti.
- [[software-vs-system-architecture]] — logico (tempo) vs spaziale (placement).
- [[quality-attributes]] (CX) — le proprietà architetturali come QA.
- [[sistema-computazionale]] (M2) — componenti che *behave + interact* ↔ componenti & connettori.
- [[computer-science-foundations]] (M2) — il dubbio: è una vera scienza? process algebras.
- [[process-algebra]] (M9) — il **complemento formale**: modella il *comportamento* e ne **prova** le proprietà. → [[architetture-vs-process-algebra]].

## 7. Sorgenti

- `raw-sources/M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf` (Prologue; Software Architectures; Conclusion), slide 4–13, 30–31.
- Riferimenti: Fielding 2000 (*Architectural Styles and the Design of Network-based Software Architectures*); Tanenbaum & van Steen 2017.
