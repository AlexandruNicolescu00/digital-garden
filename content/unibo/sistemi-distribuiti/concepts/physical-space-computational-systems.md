---
title: "Spazio Fisico nei Sistemi Computazionali"
course: Sistemi Distribuiti
category: concept
topics: [spazio virtuale, middleware, topologia, code mobility, situated, space-aware]
difficulty: intermedio
sources: [M7-Computing-with-Space.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Spazio Fisico nei Sistemi Computazionali

Come lo **spazio** entra nei sistemi computazionali reali: dalla distribuzione fisica agli spazi virtuali, al middleware come topologia, fino alla computazione situata.

## 1. Sistemi distribuiti come spazio (recap)

I sistemi distribuiti **originano dalla distribuzione fisica** di computazione, comunicazione e dati (unità computazionali, canali, dati distribuiti). Da qui una **scala di spazi virtuali**:
- le **reti locali** costruiscono i primi spazi virtuali;
- **Internet** e le location IP-based danno i primi spazi virtuali **"globali"**;
- il **WWW** è il primo spazio globale condiviso da agenti e umani — uno spazio **knowledge-intensive**.

→ è l'asse spaziale di [[distribuzione-spaziale-temporale]] (M0) reso concreto.

## 2. Middleware come topologia (recap)

> Il **[[middleware]]** è come un *sistema operativo per i sistemi distribuiti*: un'infrastruttura software che dà **struttura** agli ambienti distribuiti, eventualmente supportando la **mobilità** (**code mobility** [Fuggetta et al., 1998]).

Punto-chiave spaziale:
- l'**EAI** ([[distributed-information-systems|integrazione orizzontale]]) costruisce ambienti aggregati;
- il middleware **mappa la distribuzione logica su quella fisica**, fornendo **nozioni topologiche** per i sistemi distribuiti;
- es. **JADE** offre ai programmatori di agenti le nozioni di **container** e **platform** per rappresentare la *località* [Bellifemine et al., 2007].

> ⚠️ A volte, però, è la **distribuzione fisica** ciò che conta davvero (non basta la topologia logica) → la computazione **situata**.

## 3. Computazione situata e space-awareness

> La distribuzione fisica è essenziale per far fronte alla natura distribuita di molti ambienti di lavoro e al bisogno di **computazione situata**: computazioni che occorrono **localmente, dove avvengono la percezione o l'azione** (elaborando percezioni, guidando azioni, o entrambe).

Quando i requisiti impongono computazioni situate in un ambiente fisico distribuito, i **sistemi distribuiti situati** sono **l'unica via** (disaster recovery, monitoraggio ambientale, crowd steering). L'**IoT** rende il bisogno di computazione situata **ineludibile**.

> **Tesi:** oggi i sistemi computazionali non banali devono essere **space-aware**. → è la realizzazione spaziale della [[situatedness]] (M4) e dei [[distributed-pervasive-systems]] (M5).

## 4. Connessioni

- [[computing-with-space]] — l'hub del modulo.
- [[middleware]] — middleware come topologia logica-su-fisica + code mobility.
- [[code-mobility]] (C6) — la **code mobility** [Fuggetta et al. 1998] qui citata, sviluppata in dettaglio (gancio risolto).
- [[situatedness]] (M4) — la situatedness vista dal lato *spazio* (computazione situata).
- [[distributed-pervasive-systems]] (M5) — IoT e sensori come sistemi situati nello spazio.
- [[distribuzione-spaziale-temporale]] (M0) · [[processo-computazionale-contesto]] (M2) — il contesto spaziale del processo.
- [[spatial-computing]] — la sintesi (le 3 classi di sistemi spaziali).

## 5. Sorgenti

- `raw-sources/M7-Computing-with-Space.pdf` (Space in CS — Physical Space in Computational Systems), slide 25–28.
- Riferimenti: Fuggetta et al. 1998 (code mobility); Bellifemine et al. 2007 (JADE).
