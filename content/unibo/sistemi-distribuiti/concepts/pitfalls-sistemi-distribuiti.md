---
title: "Le Fallacie dei Sistemi Distribuiti (Pitfalls)"
course: Sistemi Distribuiti
category: concept
topics: [pitfalls, fallacie, assunzioni, rete, latenza, errori]
difficulty: base
sources: [M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Le Fallacie dei Sistemi Distribuiti (Pitfalls)

## 1. Definizione

Un sistema distribuito è **facile da costruire** — il che significa anche che è **facile da costruire male**. Gli errori tipici nasceno da un insieme di **false assunzioni** fatte dallo sviluppatore alle prime armi.

> **Le otto false assunzioni** (attribuite a **L. Peter Deutsch**; le "fallacies of distributed computing"):
> 1. la **rete è affidabile** (reliable);
> 2. la **rete è sicura** (secure);
> 3. la **rete è omogenea** (homogeneous);
> 4. la **topologia non cambia**;
> 5. la **latenza è zero**;
> 6. la **banda è infinita**;
> 7. il **costo di trasporto è zero**;
> 8. c'è **un solo amministratore**.

> Queste (false) assunzioni producono tipicamente **tutti** gli errori nell'ingegneria dei sistemi distribuiti.

## 2. Intuizione — perché sono insidiose

Ogni assunzione riguarda una proprietà **propria e specifica** dei sistemi distribuiti, che nei sistemi **non distribuiti** semplicemente **non si presenta**:

| Falsa assunzione | Proprietà reale ignorata |
|------------------|--------------------------|
| rete affidabile | **reliability** della rete (i messaggi si perdono) |
| rete sicura | **security** della rete |
| rete omogenea | **eterogeneità** della rete |
| topologia stabile | la **topologia** cambia (mobilità, guasti) |
| latenza zero | la **latenza** esiste ed è variabile |
| banda infinita | la **banda** è finita |
| trasporto gratis | i **costi di trasporto** contano |
| un amministratore | molti **domini amministrativi** |

> 🔗 Queste proprietà sono esattamente ciò che i [[obiettivi-sistemi-distribuiti|goal]] tentano di domare: la **latenza** → [[scalabilita|scalability]] (latency hiding); l'**eterogeneità** → [[trasparenza|access transparency]]; i **domini amministrativi** → [[scalabilita|administrative scalability]]; la **(in)affidabilità** → [[trasparenza|failure transparency]] e [[dependability]]; la mancanza di clock globale e la latenza → [[modello-sincrono-asincrono|modello asincrono]].

## 3. Connessioni

- [[obiettivi-sistemi-distribuiti]] — i goal come risposta alle pitfalls.
- [[scalabilita]] — latenza, banda, domini amministrativi.
- [[trasparenza]] — eterogeneità (access) e guasti (failure).
- [[dependability]] · [[modelli-di-fallimento]] (M1) — l'inaffidabilità della rete e i guasti.
- [[modello-sincrono-asincrono]] (C3) — latenza ≠ 0 → impossibile distinguere lento da morto.
- [[centralizzato-vs-distribuito]] (M0) — la voce "un amministratore" vs realtà multi-dominio.
- [[socket]] (M2) — il livello dove le fallacie diventano **codice**: UDP rende concreta "the network is reliable", il flow control TCP smentisce "bandwidth is infinite"/"latency is zero".
- [[udp-group-chat]] · [[tcp-echo-deadlock]] (M2) — esempi che *mostrano* le fallacie (perdita messaggi UDP; deadlock da buffer finiti).

## 4. Sorgenti

- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Definitions & Issues — Pitfalls), slide 16–17.
- Riferimento: L. Peter Deutsch (in Tanenbaum & van Steen 2017).
