---
title: "Caso studio: Pokémon Go e il CAP"
course: Sistemi Distribuiti
category: example
topics: [CAP, case-study, location-based-games, cloud]
difficulty: intermedio
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Caso studio: Pokémon Go e il CAP

## Problema

Come si applicano concretamente le tre proprietà del [[teorema-cap]] a un **gioco location-based** reale (Pokémon Go, ma anche Ingress, Harry Potter: Wizards Unite, Minecraft Earth)? I *location-based games* usano la **posizione del giocatore** per far evolvere il gameplay.

## Tecnica applicata

Analisi delle tre proprietà CAP ([[partition-tolerance]], [[consistency]], [[availability]]) su un'architettura cloud reale.

## Soluzione: come si posiziona nel CAP

### Partition tolerance — obbligatoria
L'architettura è costruita attorno a **milioni di giocatori** che si muovono nello spazio fisico mondiale con dispositivi mobili. Rinunciare a P **non è un'opzione** perché:
- i dispositivi mobili sono **instabili** nella connettività;
- i giocatori sono **mobili** ed entrano/escono dalla copertura di rete;
- i giocatori si **concentrano** in alcune aree (eventi speciali);
- la scalabilità è **multi-livello**: numero totale di giocatori, numero di luoghi, numero di giocatori per luogo.
- Esempio di fallimento costoso: il **disastro al Chicago 2017 Pokémon Go Fest**.

### Consistency vs Availability — il trade-off
La consistenza dei dati di gioco (obiettivi, situazione, achievement) è essenziale. L'architettura è **logicamente centralizzata**, con **replicazione spaziale** per ridurre la latenza percepita e migliorare la scalabilità.

Quando serve **consistenza forte** (es. transazioni in-game), il giocatore può essere **costretto ad aspettare** la conferma dei server → la **prima vittima è l'availability** (la *Spinning Wheel of Death*). Conferma concreta del raffinamento di Brewer: durante criticità si sceglie C a scapito di A.

## Architettura su Google Cloud (flusso)

1. All'apertura dell'app, i **media statici** sono scaricati sul device (da **Cloud Storage**); **Cloud CDN** + **Cloud Load Balancing** servono i contenuti dall'edge network più vicino.
2. Le richieste passano da un reverse proxy **NGINX** al **Frontend** game service su **Google Kubernetes Engine (GKE)**.
3. Lo **Spatial Query Backend** gestisce le feature location-based con una **cache shardata per posizione** (quali Pokémon, gym, PokéStop, time zone).
4. Quando si cattura un Pokémon, il Frontend scrive su **Google Spanner** (**strongly consistent**); a write completata, risposta al device.
5. Le azioni sono registrate su **Bigtable** (NoSQL) come **Protobuf**, per logging/tracking, e inviate a un topic **Pub/Sub** per la pipeline di analisi.
6. Giocatori nella stessa regione sono sincronizzati dal **determinismo** delle mappe (stessi input → stesso stato).
7. Tutti i server sono in sync su impostazioni ed event timing → senso di "mondo condiviso".

## Errori comuni

- Pensare che si possa avere C, A e P insieme: il caso mostra che, sotto partizione, **si sceglie** (qui spesso C, sacrificando A).
- Confondere la replicazione spaziale (per latenza/scalabilità) con una garanzia di consistenza: la consistenza forte qui è demandata a **Spanner**, non alla cache.

## Generalizzazione

Esempio di sistema **CP "quando serve"** che combina componenti fortemente consistenti (Spanner) e componenti rilassati/cache (Bigtable, spatial cache): incarna il **CAP moderno** [Brewer, 2012] — massimizzare le combinazioni C/A sensate per ogni parte dell'applicazione. Vedi anche [[acid-vs-base]].

## Connessioni

- [[teorema-cap]] — la teoria applicata qui
- [[partition-tolerance]] · [[consistency]] · [[availability]] — le tre proprietà nel caso reale
- [[acid-vs-base]] — combinazione di consistenza forte e rilassata
- [[spatial-computing-applications]] (M7) — Pokémon Go come **LBS/AR**: lo spazio fisico che incontra lo spazio computazionale
- [[distributed-pong]] (M2) — altro gioco, stessa scelta CAP: **availability > consistency** nel real-time

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-...pdf` (slides 16–22)
- Google Cloud blog: *How Pokémon Go scales to millions of requests*.
