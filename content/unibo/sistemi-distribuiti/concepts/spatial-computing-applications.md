---
title: "Computing con lo Spazio: Applicazioni (GIS, VR, AR, LBS)"
course: Sistemi Distribuiti
category: concept
topics: [geometria computazionale, GIS, virtual reality, augmented reality, LBS]
difficulty: base
sources: [M7-Computing-with-Space.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Computing con lo Spazio: Applicazioni (GIS, VR, AR, LBS)

Il panorama applicativo del **computing *con* lo spazio**: dall'uso della geometria per risolvere problemi all'integrazione di spazio fisico e virtuale.

## 1. Computing CON lo spazio

### Geometria computazionale [de Berg et al., 2000]
**Computare con la geometria** per risolvere problemi:
- usare lo **spazio per rappresentare** problemi (spaziali o **non** spaziali);
- usare la **geometria** per renderli computabili;
- usare **algoritmi** per calcolare le soluzioni.

> Esempio: trovare il *coffee shop più vicino* nel campus di Cesena, per **ogni** punto del campus → dividere il campus in **regioni** attorno a ciascun bar (l'insieme dei punti per cui quel bar è il più vicino). *Come si computa?* → è il problema dei **diagrammi di Voronoi**.

### Geographic Information Systems (GIS)
**Computare con la geografia**: rappresentare/catturare/memorizzare/visualizzare dati che rappresentano **posizioni sulla Terra**.
> Un GIS è un sistema computer-based che supporta lo **studio di fenomeni naturali e artificiali con una location esplicita nello spazio**, permettendo data entry, manipolazione e output interpretabile [Huisman & de By, 2009]. La nozione di spazio è **chiaramente definita**.

### Virtual Reality (VR)
Mondi **artificiali** con spazio artificiale, creati digitalmente come ambienti puramente computazionali, dove **persone reali** svolgono attività sensorimotoria e cognitiva.
> VR = dominio che usa computer science + interfacce comportamentali per **simulare in un mondo virtuale** il comportamento di entità 3D che interagiscono **in tempo reale** tra loro e con utenti in **immersione pseudo-naturale** via canali sensorimotori [Fuchs et al., 2011]. **Interazione** e **immersione** sono i concetti chiave.
- **Gaming**: l'applicazione più prominente della VR (es. Kinect); piattaforme per mondi virtuali (Unity3D, Unreal Engine).

## 2. Spazio fisico incontra spazio computazionale

### Location-Based Services (LBS)
Usano la **posizione** di utente/dispositivo per fornire informazione, intrattenimento, sicurezza: es. AroundMe, Mobike, **BotFighters/GeoZombie**, **Ingress, Pokémon Go**, Harry Potter: Wizards Unite, Minecraft Earth. *Mixed reality* è un vecchio termine che potrebbe tornare in uso.

### Augmented Reality (AR)
**Integra** spazio virtuale e fisico: fonde informazione del mondo reale con **contenuto digitale context-sensitive** in modo significativo [Furht, 2011].
> Il punto: i layer **virtuale e fisico si influenzano dinamicamente a vicenda**. La *gamification* porta facilmente ad applicazioni rilevanti — medicali, civiche, educative, turistiche (es. GeoZombie, un passo oltre Pokémon Go).

## 3. Connessioni

- [[computing-with-space]] — l'hub del modulo.
- [[space-in-math-logic]] — la geometria computazionale e i GIS poggiano sui fondamenti formali.
- [[spatial-computing]] — queste applicazioni si collocano nel framework dello spatial computing.
- [[pokemon-go-cap]] (C1) — Pokémon Go come **LBS/AR** (qui visto dal lato CAP/consistenza-disponibilità).
- [[physical-space-computational-systems]] — lo spazio fisico/virtuale che queste app abitano.

## 4. Sorgenti

- `raw-sources/M7-Computing-with-Space.pdf` (Computing with Space; Physical Space meets Computational Space), slide 30–36.
- Riferimenti: de Berg et al. 2000; Huisman & de By 2009 (GIS); Fuchs et al. 2011 (VR); Furht 2011 (AR); Prandi et al. 2016 (GeoZombie).
