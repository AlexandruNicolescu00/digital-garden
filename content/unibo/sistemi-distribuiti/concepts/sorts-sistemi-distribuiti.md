---
title: "Le Tre Classi di Sistemi Distribuiti (Sorts)"
course: Sistemi Distribuiti
category: concept
topics: [tassonomia, computing, information, pervasive, evoluzione]
difficulty: base
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Le Tre Classi di Sistemi Distribuiti (Sorts)

## 1. Definizione

I sistemi distribuiti **non sono una cosa nuova**: il *distributed computing* esisteva decenni fa, prima ancora che capissimo cosa sia davvero la [[computazione]]. Oggi le loro tipologie sono così diverse per natura, struttura, organizzazione e dinamica che **classificarle tutte** non è né facile né utile. Tuttavia, nei primi decenni di esistenza, i mutevoli bisogni e le risorse disponibili hanno promosso un'**evoluzione** che ha generato nel tempo **tre classi fondamentali riconoscibili** [Tanenbaum & van Steen, 2017]:

| Classe | Caratteristica principale |
|--------|---------------------------|
| **[[distributed-computing-systems]]** | usare una molteplicità di computer distribuiti per compiti ad **alte prestazioni** (cluster, grid) |
| **[[distributed-information-systems]]** | **integrare** molte applicazioni di rete separate (transazioni distribuite, EAI) |
| **[[distributed-pervasive-systems]]** | sistemi immersi nell'ambiente, dove l'**instabilità è la norma** (mobile, sensori) |

## 2. Intuizione — la storia evolutiva

> Scenari tecnologici e applicativi in evoluzione **hanno forzato** nuovi requisiti, strutture e obiettivi, facendo emergere classi diverse **nel tempo**.

L'ordine racconta grosso modo l'evoluzione: prima il *computing* (potenza di calcolo), poi l'*information* (integrazione di dati/applicazioni), infine il *pervasive* (immersione nell'ambiente). La classe dipende da:
- l'**ambiente** in cui il sistema è sviluppato;
- gli **obiettivi** ([[obiettivi-sistemi-distribuiti]]) da raggiungere;
- il **livello** delle tecnologie disponibili.

→ Modelli, metodologie e tecnologie **diverse** servono per progettare classi diverse.

## 3. Caso limite — oggi

> **La complessità odierna fa sì che i sistemi distribuiti prendano tipicamente in prestito caratteristiche da tutte e tre le classi base.**

La tassonomia resta un'utile lente concettuale, ma i sistemi reali sono ibridi. Inoltre, la complessità ingegneristica odierna richiede **sforzi della community** e centinaia di componenti software (critici) da selezionare e usare → vedi [[cloud-native-cncf]].

## 4. Connessioni

- [[sistema-distribuito]] — il concetto generale che questa tassonomia articola.
- [[distributed-computing-systems]], [[distributed-information-systems]], [[distributed-pervasive-systems]] — le tre classi.
- [[cloud-native-cncf]] — i sistemi distribuiti "di oggi" e l'ecosistema open.
- [[obiettivi-sistemi-distribuiti]] (M4) — i goal variano con la classe.
- [[parallelo-concorrente-distribuito]] (M2) — un'altra tassonomia, per *contesto* (spazio/tempo) invece che per *evoluzione/uso*.

## 5. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Prologue, Introduction, Conclusion), slide 4–7, 46–47.
- Riferimento: Tanenbaum & van Steen 2017.
