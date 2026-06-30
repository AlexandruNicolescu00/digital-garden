---
title: "Distribuzione Spaziale e Temporale"
course: Sistemi Distribuiti
category: concept
topics: [distribuzione, spazio, tempo, eventi]
difficulty: base
sources: [M0-Why-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Distribuzione Spaziale e Temporale

## Definizione

La **distribuzione** è la caratteristica che la natura fisica dei sistemi artificiali aggiunge ai componenti computazionali. Si declina su due dimensioni: **spaziale** e **temporale**.

## Distribuzione spaziale

In un sistema spazialmente distribuito sono distribuiti:

1. le **unità computazionali**
2. i **canali di comunicazione**
3. i **dati / informazione / conoscenza** (insieme alle loro *rappresentazioni*)
4. **sensori, attuatori**, ecc.

Conseguenza chiave: il **confine tra il sistema e l'ambiente circostante è spazialmente sparso** (*spatially sparse*) — non c'è una linea netta che separa "dentro" da "fuori".

## Distribuzione temporale

Ciò che è temporalmente distribuito sono fondamentalmente gli **eventi**:

- le cose accadono, ma **non più in una sequenza chiaramente ordinata**
- gli eventi sono *sparsi*, e la banale relazione temporale **before/after** semplicemente **non è applicabile a molte coppie di eventi**

## Perdita dell'unità spazio-temporale

La conseguenza più profonda è che **l'unità spazio-temporale del sistema si perde** (*spatio-temporal unity is lost*):

- non esiste più una nozione di **tempo di sistema** né di **posizione di sistema**
- i componenti del sistema, a diversi livelli di astrazione, sono solo **parzialmente correlati**, sia temporalmente sia spazialmente

Questo è il punto di partenza teorico dell'intero corso: poiché non c'è un tempo globale, l'ordinamento totale degli eventi va abbandonato in favore di un **ordinamento parziale** → [[ordinamento-parziale-eventi]].

## Connessioni

- [[sistema-distribuito]] — il concetto di cui questa è la caratterizzazione operativa
- [[ordinamento-parziale-eventi]] — la conseguenza diretta sulla nozione di tempo
- [[pervasivita-computazione-interazione]] — perché la distribuzione è oggi la norma
- [[computing-needs-time]] (M6) · [[computing-with-space]] (M7) — i due moduli che approfondiscono l'asse **tempo** e l'asse **spazio**; sintesi in [[tempo-vs-spazio-nel-computing]]

## Ganci risolti

- ~~Clock logici di Lamport e clock vettoriali~~ → [[logical-clock]], [[lamport-scalar-clock]], [[vector-clock]] (C5).
- L'asse **spaziale** (rappresentazione e computing dello spazio) → [[computing-with-space]], [[spatial-computing]] (M7).

## Sorgenti

- `raw-sources/M0-Why-Distributed-Systems.pdf` (slides 5–8)
- Asse spazio/tempo approfondito in M6 (`M6-Computing-with-Time.pdf`) e M7 (`M7-Computing-with-Space.pdf`).
