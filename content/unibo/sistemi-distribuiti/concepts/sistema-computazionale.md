---
title: "Sistema Computazionale"
course: Sistemi Distribuiti
category: concept
topics: [sistema-computazionale, interazione, contesto]
difficulty: intermedio
sources: [M2-Roots-of-Distributed-Systems-Computation-in-Space-Time.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Sistema Computazionale

## Definizione

**Sistema** (Oxford): un gruppo di cose/componenti connesse o che lavorano insieme.

**Sistema computazionale** [Goldin et al., 2006]: in un sistema computazionale, due o più **processi computazionali**:
- **si comportano** (*behave*) — computando, e
- **lavorano insieme** (*work together*) — **interagendo**.

Due le dimensioni costitutive, quindi: **computazione** + **interazione** (eco diretta della "pervasività di computazione e interazione" vista in M0 → [[pervasivita-computazione-interazione]]).

## Il contesto di un sistema computazionale

Come si rappresenta il contesto (→ [[processo-computazionale-contesto]]) quando i processi sono più di uno? Tre opzioni:

1. un contesto **separato e diverso per ogni** processo;
2. un **unico** contesto per l'intero sistema;
3. una **combinazione** delle due (un contesto per ogni processo *più* un contesto comune).

**Punto cruciale:** la **scelta del tipo di contesto definisce il tipo di sistema** computazionale:
- **parallelo**
- **concorrente**
- **distribuito**

→ Sviluppato in [[parallelo-concorrente-distribuito]].

## Connessioni

- [[processo-computazionale-contesto]] — il mattone (processo + contesto)
- [[parallelo-concorrente-distribuito]] — la tassonomia dei sistemi
- [[pervasivita-computazione-interazione]] — computazione + interazione già in M0
- [[sistema-distribuito]] — il caso particolare oggetto del corso
- [[componenti-connettori]] (M8) — i processi che *behave + interact* formalizzati come **componenti** (behave) e **connettori** (interact)
- [[process-algebra]] (M9) — *behave + interact* visto come **comportamento di sistemi interagenti**: la process algebra ne è lo studio algebrico (ciò che mancava agli automi I/O era *interazione + sistemi*)

## Sorgenti

- `raw-sources/M2-Roots-of-Distributed-Systems-...pdf` (slides 40–47)
- [Goldin et al., 2006] *Interactive Computation: The New Paradigm*.
