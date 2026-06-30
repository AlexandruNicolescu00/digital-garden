---
title: "Software Architecture vs System Architecture"
course: Sistemi Distribuiti
category: bridge
topics: [architettura software, architettura di sistema, logico, spaziale, tempo, spazio]
difficulty: intermedio
sources: [M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Software Architecture vs System Architecture

Le due facce del modellare un sistema distribuito ([[software-architecture]]): l'**organizzazione logica** dei componenti contro la loro **collocazione fisica**. La distinzione mappa esattamente i due assi del corso — **tempo** (M6) e **spazio** (M7).

## 1. Confronto

| Aspetto | **Software architecture** | **System architecture** |
|---|---|---|
| Si occupa di | **organizzazione logica** dei componenti | **placement** (collocazione) dei componenti |
| Asse | logico, possibilmente **nel tempo** | **distribuzione spaziale** |
| Domanda | *come* sono organizzati i componenti? | *dove* vengono messi nel sistema distribuito? |
| Relazione | lo **stile** (pattern di organizzazione) | un'**istanziazione** concreta dello stile su macchine reali |
| Modulo affine | [[computing-needs-time]] (M6) — evoluzione nel tempo | [[computing-with-space]] (M7) — distribuzione nello spazio |

## 2. Il nodo concettuale

> Una **software architecture** descrive *come* i componenti sono organizzati (lo **stile**: layered, object-based, event-based…), possibilmente lungo la sua **evoluzione temporale**. Una **system architecture** ne è un'**istanziazione**: assegna ai componenti il loro **posto reale** nel sistema distribuito — cioè affronta la **distribuzione spaziale**.

Una stessa software architecture (es. client-server [[architectural-styles|object-based]]) ammette **molte** system architecture (dove stanno client e server, su quali nodi/data center).

> 🔗 È la coppia **tempo/spazio** ([[tempo-vs-spazio-nel-computing]], [[distribuzione-spaziale-temporale]]) applicata al *modelling*: il *quando/come logico* (software) e il *dove fisico* (system).

## 3. Limite e questione aperta

Gli stili architetturali sono modi **approssimativi e forse non-scientifici** di modellare, ma **espressivi** abbastanza da aiutare l'ingegneria dei SD. Domanda aperta del modulo: bastano per una **scienza** dei sistemi distribuiti? Si possono **dimostrare teoremi**? Che ruolo per i formalismi *math-like* (**process algebra**)? → [[computer-science-foundations]] (M2). **Risposta in M9:** no, le architetture modellano la *struttura* non il *comportamento*; per provarne le proprietà serve la [[process-algebra]] → confronto in [[architetture-vs-process-algebra]].

## 4. Connessioni

- [[software-architecture]] — la pagina-quadro.
- [[architectural-styles]] — gli stili (livello software).
- [[computing-needs-time]] (M6) · [[computing-with-space]] (M7) — i due assi che questa distinzione riflette.
- [[distribuzione-spaziale-temporale]] (M0) — la perdita dell'unità spazio-temporale che rende necessarie entrambe le viste.

## 5. Sorgenti

- `raw-sources/M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf` (Software Architectures; Conclusion — Software vs system architectures), slide 7, 30.
