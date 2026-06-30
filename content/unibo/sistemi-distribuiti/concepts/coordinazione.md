---
title: "Verso la Coordinazione (Beyond Synchronisation)"
course: Sistemi Distribuiti
category: concept
topics: [coordinazione, interazione, mutua esclusione, elezione, sincronizzazione]
difficulty: intermedio
sources: [M6-Computing-with-Time.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Verso la Coordinazione (Beyond Synchronisation)

## 1. Comunicazione < interazione

> La **comunicazione è solo metà della storia.** L'**interazione** è il problema più generale; **governare l'(inter)azione** è una questione fondamentale nei sistemi (distribuiti). *Fare la cosa giusta al momento giusto* è essenziale — e **"al momento giusto"** è il problema critico.

Il tempo ([[tempo-nei-sistemi-distribuiti|fisico]] o [[logical-clock|logico]]) risponde al **quando**; ma non basta.

## 2. Ordinare gli eventi non è sufficiente

A volte servono **politiche più articolate** del semplice ordinamento, per esempio per evitare che **accessi concorrenti** a una risorsa condivisa ne danneggino o corrompano la consistenza.

### Mutua esclusione
Esistono molti algoritmi di **mutual exclusion** — **centralizzati, decentralizzati, distribuiti** (es. **Token Ring**). M6 non li rivede in dettaglio; il punto chiave: alcuni sono basati su un **coordinatore**, e **tutti sono algoritmi di coordinazione**.

### Algoritmi di elezione
Molti algoritmi distribuiti richiedono che un **coordinatore venga eletto**: gli **election algorithm** sono (usati da) algoritmi di coordinazione.

## 3. Non è solo questione di tempo

> La **sincronizzazione** riguarda **quando** le cose accadono. Ma le **azioni** sono più che inviare messaggi: hanno una **natura**, e l'interazione significativa in un SD dipende tipicamente da tale natura. L'interazione **non** si riduce ad azioni distribuite "indifferenziate" opportunamente ordinate.

### Il problema della coordinazione [Omicini et al., 2001]
> **Coordinazione** = governare l'interazione basandosi **sia sul tempo, sia sulla natura delle azioni**, mirando al raggiungimento di un **obiettivo globale** per il sistema distribuito.

→ è il passo oltre la sincronizzazione: dal *quando* al *quando + cosa + perché*.

## 4. Connessioni

- [[tempo-nei-sistemi-distribuiti]] · [[logical-clock]] — il "quando" su cui la coordinazione si appoggia ma che non basta.
- [[agreement-consensus]] · [[state-machine-replication]] (C3) — il consenso come forma di coordinazione su uno stato/azioni.
- [[ordinamento-parziale-eventi]] · [[happened-before]] — ordinare gli eventi: necessario ma non sufficiente.
- [[situatedness]] (M4) — la "natura delle azioni" e l'ambiente richiamano la situatedness.
- [[componenti-connettori]] · [[architectural-styles]] (M8) — il **connettore** media la coordinazione; la **blackboard** (shared data-space) come *coordination medium* che guida i processi.

## 5. Da approfondire (tema "Coordinazione distribuita")

M6 apre il tema ma rimanda gli algoritmi: **mutua esclusione distribuita** (Token Ring, Ricart-Agrawala, Maekawa…) e **leader election** (Bully, Ring…) restano **da ingestare** in dettaglio.

## 6. Sorgenti

- `raw-sources/M6-Computing-with-Time.pdf` (Toward Coordination), slide 43–46.
- Riferimento: Omicini et al. 2001 (*Coordination of Internet Agents*).
