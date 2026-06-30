---
title: "Distributed Systems Nowadays: Cloud Native & CNCF"
course: Sistemi Distribuiti
category: concept
topics: [cloud native, CNCF, community, open source, microservices, Kubernetes]
difficulty: base
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Distributed Systems Nowadays: Cloud Native & CNCF

## 1. Complessità & componenti

> La crescente **complessità** dei sistemi distribuiti odierni è gestibile **solo da gruppi** di ingegneri e practitioner: gli **sforzi della community** sono essenziali, specie per i componenti di infrastruttura **critici** → **open software come default**.

Sono disponibili collezioni di componenti che fanno da fondamenta per progettare sistemi distribuiti complessi. È fondamentale: tenere d'occhio i progetti della community, **partecipare** al processo per conoscerne lo stato e contribuire, e **scegliere** la combinazione migliore di componenti.

**Foundation principali:** Apache Software Foundation (ASF), **Cloud Native Computing Foundation (CNCF)**, Free Software Foundation (FSF), Python Software Foundation (PSF).

## 2. La Cloud Native Computing Foundation (CNCF)

- fa parte della **Linux Foundation**;
- raccoglie e promuove progetti come fondamento del **cloud native computing**;
- classifica i progetti per stadio di maturità: **graduated / incubating / sandbox / archived**;
- include **Kubernetes, etcd, Prometheus** (i protagonisti del modulo [[kubernetes|CX]]).

## 3. Che cos'è il "cloud native" (CNCF Charter)

> Le tecnologie *cloud native* permettono di costruire ed eseguire **applicazioni scalabili** in ambienti moderni e dinamici (cloud pubblici, privati, ibridi). Le esemplificano: **container, service mesh, microservizi, infrastruttura immutabile, API dichiarative**.
> Queste tecniche abilitano sistemi **loosely coupled** che sono **resilienti, manageable e observable**. Combinate con una **robusta automazione**, permettono di fare cambiamenti ad alto impatto **frequentemente e in modo prevedibile** con minima fatica.

> 🔗 Ogni termine richiama il wiki: **container** → [[containerization]]; **API dichiarative / infrastruttura immutabile** → il [[kubernetes|paradigma dichiarativo]] e il reconciliation loop ([[kubernetes-architettura]]); **resilient/manageable/observable** → i [[quality-attributes]] e l'[[observability-monitoring]]; **microservizi loosely coupled** → la decomposizione scalabile ([[scalabilita]]).

## 4. Sintesi del modulo

- esistono **diverse classi** ([[sorts-sistemi-distribuiti]]) che raccontano l'evoluzione del campo;
- oggi i sistemi prendono in prestito caratteristiche da **tutte e tre**;
- la complessità ingegneristica **chiama gli sforzi della community**, con centinaia di componenti (critici) da selezionare e usare.

## 5. Connessioni

- [[sorts-sistemi-distribuiti]] — il quadro evolutivo che culmina nel cloud native.
- [[kubernetes]] · [[containerization]] · [[kubernetes-architettura]] (CX) — i progetti CNCF in azione (Kubernetes, etcd, Prometheus).
- [[quality-attributes]] · [[observability-monitoring]] (CX) — resilient/manageable/observable.
- [[scalabilita]] (M4) — applicazioni scalabili, microservizi loosely coupled.
- [[teoria-vs-kubernetes]] (CX) — il ponte teoria↔pratica del paradigma dichiarativo.

## 6. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Distributed Systems Nowadays), slide 40–44.
- Riferimenti: CNCF (linuxfoundation.org, cncf.io), Apache, FSF, PSF.
