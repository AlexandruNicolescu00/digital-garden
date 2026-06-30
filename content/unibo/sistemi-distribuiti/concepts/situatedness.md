---
title: "Situatedness (Immersione nell'Ambiente)"
course: Sistemi Distribuiti
category: concept
topics: [situatedness, context-awareness, ambiente, spazio-tempo, KIE, pervasive]
difficulty: avanzato
sources: [M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Situatedness (Immersione nell'Ambiente)

## 1. Definizione

> ⚠️ **Non è un goal classico** dei sistemi distribuiti: è il **quinto goal** aggiunto da Omicini.

> **Situatedness** [Suchman, 2007] = la proprietà dei sistemi di essere **immersi nel proprio ambiente**, cioè capaci di **percepire e produrre** (tempestivamente) cambiamenti dell'ambiente, trattando opportunamente gli **eventi** ambientali.

Sistemi *mobile*, *adaptive* e *pervasive* hanno reso evidente il ruolo-chiave della situatedness per i sistemi computazionali odierni [Zambonelli et al., 2015].

## 2. Situatedness & context-awareness

I sistemi computazionali sono sempre più influenzati dalla loro **natura fisica**. **Non** per mancanza di astrazione (anzi, i layer HW/SW sono ben separati), ma per i **requisiti sempre più complessi** che impongono una crescente **context-awareness** [Baldauf et al., 2007]. Definire la nozione di **contesto** è complesso [Omicini, 2002]: i suoi confini si confondono con quelli di **ambiente** (come emerge dai *multi-agent systems* [Weyns et al., 2007]).

### Spazio & tempo
Gli scenari di *mobile computing* mostrano che la situatedness richiede almeno consapevolezza del **tessuto spazio-temporale**: ogni sistema non banale deve sapere **dove** e **quando** sta operando per svolgere la sua funzione. Ogni ambiente è anzitutto fatto di **spazio e tempo** → il **contesto temporale e spaziale**.

> 🔗 Riconnette direttamente a M2: [[processo-computazionale-contesto]] (processo *timed/spatial/situated*) e [[distribuzione-spaziale-temporale]] (cosa è distribuito nello spazio e nel tempo).

### Ambiente e KIE
Più in generale, la situatedness richiede **consapevolezza dell'ambiente** in cui il sistema è deployato (natura, struttura, risorse offerte, possibili minacce). Caso rilevante: i **Knowledge-Intensive Environments (KIE)**, dove conoscenza abbondante e distribuita nell'ambiente è essenziale per le attività del sistema; accedere, comprendere e iniettare conoscenza interagendo **localmente** è cruciale.

## 3. Perché i sistemi distribuiti per la situatedness?

- la **distribuzione fisica** è essenziale per far fronte alla natura distribuita di molti ambienti di lavoro… e al bisogno di **computazioni situate**;
- quando i requisiti impongono computazioni situate in un ambiente fisico distribuito, i **sistemi distribuiti situati** sono **l'unica via d'uscita** (es. disaster recovery, monitoraggio ambientale, crowd steering, live events);
- **[[openness-sistemi-distribuiti|openness]]** e **[[scalabilita|scalability]]** sono essenziali per gestire rispettivamente l'**imprevedibilità** e la **complessità crescente** di tali ambienti.

## 4. Connessioni

- [[obiettivi-sistemi-distribuiti]] — la situatedness è il 5° goal (non classico).
- [[processo-computazionale-contesto]] (M2) — situated/timed/spatial: il contesto come parte del processo.
- [[distribuzione-spaziale-temporale]] (M0) — il tessuto spazio-temporale.
- [[pervasivita-computazione-interazione]] (M0) — pervasive computing e immersione.
- [[trasparenza]] — il rovescio della medaglia: la situatedness *vuole* consapevolezza del contesto, non occultamento.
- [[openness-sistemi-distribuiti]] · [[scalabilita]] — abilitatori della situatedness in ambienti complessi.
- [[smart-contract]] (C4) — di contro, esempio di sistema *disembodied* (lack of situatedness).
- [[distributed-pervasive-systems]] (M5) — la classe di SD che incarna la situatedness (embrace contextual changes).
- [[physical-space-computational-systems]] · [[spatial-computing]] (M7) — la situatedness vista dal lato **spazio**: computazione situata, *situated systems* (embedded in space).

## 5. Sorgenti

- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Goals — Situatedness), slide 57–63.
- Riferimenti: Suchman 2007; Zambonelli et al. 2015; Baldauf et al. 2007; Omicini 2002; Weyns et al. 2007.
