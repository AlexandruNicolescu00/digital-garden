---
title: "Spatial Computing (il Framework)"
course: Sistemi Distribuiti
category: concept
topics: [spatial computing, spatial systems, situated systems, SCL, Beal]
difficulty: avanzato
sources: [M7-Computing-with-Space.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Spatial Computing (il Framework)

La **sintesi** del modulo M7: un "landscape framework" per le molte forme di computazione e sistemi in cui lo **spazio non può essere astratto via** [Shekhar et al., 2016].

## 1. Le tre classi di sistemi spaziali

Al workshop di Dagstuhl *"Computing Media and Languages for Space-Oriented Computation"* sono state identificate **tre classi** di sistemi spaziali, secondo il **ruolo dello spazio**:

| Classe | Ruolo dello spazio | Slogan |
|--------|--------------------|--------|
| **Distributed systems** | lo spazio è un **mezzo o una risorsa** | *intensive computing* / **coping with space** |
| **Situated systems** | la **location** nello spazio (e tempo) è **essenziale** per la computazione | **embedded in space** |
| **Spatial systems** | lo spazio è **fondamentale** al problema applicativo, **esplicitamente rappresentato e manipolato**, essenziale per **esprimere il risultato** | **representing space** |

> 🔗 Un gradiente: dal *coping* (distributed — lo spazio è un vincolo da gestire, → [[sistema-distribuito]]) all'*embedded* (situated — lo spazio è il contesto, → [[situatedness]], [[distributed-pervasive-systems]]) al *representing* (spatial — lo spazio è il contenuto del calcolo, → [[spatial-computing-applications|GIS, VR]]).

## 2. Spatial computer & spatial computing [Beal et al., 2011]

> - uno **spatial computer** (o *spatial computing system*) è un sistema computazionale dove **(la logica del)lo spazio è essenziale** per **rappresentare** il problema, **definire** la computazione ed **esprimere** il risultato;
> - lo **spatial computing** è qualunque forma di computazione in cui **(la logica del)lo spazio è rilevante** per esprimere ed eseguire la computazione.

## 3. Spatial Computing Languages (SCL)

I **linguaggi per lo spatial computing** nascono per **portare lo spazio nei linguaggi di programmazione**, permettendo al programmatore di trattare **esplicitamente** gli aspetti spaziali **a livello di linguaggio**.

> Lo spatial computing fa da **framework concettuale** ("landscape") per confrontare la già-lunga lista di SCL disponibili [Beal et al., 2013].

## 4. Connessioni

- [[computing-with-space]] — l'hub del modulo, di cui questa è la sintesi.
- [[space-in-math-logic]] — gli strumenti formali (geometria, topologia) che lo spatial computing sfrutta.
- [[spatial-computing-applications]] — GIS/VR/AR/LBS come istanze concrete.
- [[situatedness]] (M4) · [[distributed-pervasive-systems]] (M5) — i *situated systems* della classificazione.
- [[sistema-distribuito]] — i *distributed systems* come prima classe (spazio come risorsa/mezzo).
- [[tempo-vs-spazio-nel-computing]] — il parallelo con il tempo (M6).

## 5. Sorgenti

- `raw-sources/M7-Computing-with-Space.pdf` (Space in CS — Spatial Computing), slide 38–41.
- Riferimenti: Shekhar et al. 2016 (Dagstuhl); Beal et al. 2011/2013.
