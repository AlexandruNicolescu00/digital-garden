---
title: "Stili Architetturali per Sistemi Distribuiti"
course: Sistemi Distribuiti
category: concept
topics: [layered, object-based, data-centred, event-based, shared data-space, pub-sub, blackboard]
difficulty: intermedio
sources: [M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf, preliminaries_slides.pdf]
created: 2026-06-26
updated: 2026-06-27
---

# Stili Architetturali per Sistemi Distribuiti

Gli stili ([[software-architecture|architectural style]]) sono **devised out** (individuati) più che inventati, come i *pattern*. Per i sistemi distribuiti si identificano **cinque** stili principali, distinti dal **tipo di connettore** e di interazione ([[componenti-connettori]]).

## 1. Layered

- i componenti sono organizzati **a livelli**: i componenti di un layer **chiamano solo** quelli del layer **sotto**, e sono **chiamati solo** da quelli del layer **sopra**;
- il flusso **request-response** è sempre **top-down / bottom-up**; il control flow segue lo stesso pattern dei dati.
- → es. lo stack di rete, il [[middleware]] (vista verticale), l'architettura a layer del [[distributed-computing-systems|grid]].

## 2. Object-based

- i componenti sono **oggetti**, connessi tramite un meccanismo **RPC**;
- le architetture **client-server** sono costruite con questo stile.

> **Layered + object-based** sono **oggi gli stili più importanti** per i sistemi distribuiti (con riserva sui futuri sviluppi).

## 3. Data-centred

- la comunicazione tra processi avviene tramite un **repository condiviso**;
- il repository può essere **passivo (reattivo)** o **(pro)attivo**;
- le feature dipendono dalla scelta del repository: come è rappresentata l'informazione, come si gestiscono gli eventi, come reagisce all'interazione, come i processi interagiscono attraverso di esso.
- → esempi **ovunque**: i sistemi **web-based** sono in gran parte data-centric; molte app condividono **file** in rete (richiama i [[distributed-information-systems|distributed information systems]], M5).

## 4. Event-based

- i processi comunicano tramite un **event bus** su cui si **propagano eventi** (che possono portare dati);
- esempio principale: **publish/subscribe** — i *publisher* pubblicano eventi tramite il [[middleware]], i *subscriber* ricevono gli eventi a cui si sono iscritti.

> **Feature chiave:** i processi comunicano **senza doversi riferire/conoscere** a vicenda (**referentially uncoupled**) e **senza condividere lo stesso spazio** (**uncoupled in space**).

## 5. Shared data-space

- **unione** di data-centred + event-based: il repository condiviso è un **data-space persistente** **ed** un **event bus** (i dati sono memorizzati e acceduti, insieme agli eventi correlati);
- esempio principale: i sistemi **blackboard** — i processi mettono dati nella *blackboard*, che **aggrega conoscenza, implementa policy e guida la coordinazione** dei processi.

> **Feature chiave:** i processi comunicano **senza compresenza** → **uncoupled in time** (oltre che in spazio).

## 6. La dimensione del disaccoppiamento (uncoupling)

| Stile | Connettore | Disaccoppiamento |
|-------|-----------|-------------------|
| Object-based | RPC | accoppiato (referenza + spazio + tempo) |
| Event-based | event bus | **referenziale + spazio** |
| Shared data-space | data-space + event bus | referenziale + **spazio + tempo** |

> 🔑 Il gradiente di **disaccoppiamento** è il cuore della *generative communication* (tuple space, stile Linda): più si sale, più i processi interagiscono **senza conoscersi, senza essere nello stesso luogo, senza essere compresenti** — eco dell'"uncoupling in terms of control" della definizione di [[sistema-distribuito|Coulouris]] e degli assi spazio/tempo ([[tempo-vs-spazio-nel-computing]]).

## 7. La vista ingegneristica (M2, Ciatto): componenti, pattern, pro/contro

Il Module 2 rilegge gli stili in chiave **pratica**, specificando per ciascuno *quali [[infrastruttura-e-componenti|componenti]]* e *quali [[protocolli-interazione-comuni|interaction pattern]]* usare, con pro/contro e l'opinione del docente. (M2 tratta **4** stili: *data-centred* non è separato ma confluisce in shared dataspace.)

### Layered
- **Componenti**: ogni layer è server/proxy per quelli sopra, client per quelli sotto.
- **Pattern**: Request—Response (≈ RPC) top-down/bottom-up; talvolta Publish—Subscribe.
- **Vincolo**: niente cicli tra layer (i layer inferiori non contattano i superiori).
- **Casi**: *Three-Tier* (Presentation / Application / Data Tier), *Hexagonal Architecture*. Esempio: Skyscanner sopra i Web service di compagnie/hotel.
- **Pro**: separation of concerns, modularità, riusabilità, scalabilità per-layer, manutenibilità, astrazione, interoperabilità.
- **Contro**: overhead di performance, complessità di design, struttura rigida (cross-cutting concern difficili), duplicazione, over-engineering nei sistemi piccoli.
- **Opinione del docente**: semplice → **default sensato**. Preferire *two-tier* per quick&dirty, *three-tier* se la flessibilità non è prioritaria, *hexagonal* se il sistema deve scalare in complessità.

### Object-based
- **Componenti**: ogni oggetto è simultaneamente client e server di altri oggetti.
- **Pattern**: Request—Response (≈ RMI). **Vincoli**: praticamente nessuno.
- **Esempi**: Microsoft COM, Java RMI, CORBA.
- **Pro**: encapsulation, riusabilità, modularità, interfacce chiare, flessibilità, language-agnosticism (CORBA), comportamento dinamico.
- **Contro**: overhead di comunicazione, gestione lifecycle/reference complessa, debugging difficile, scalabilità limitata, security, tight coupling via interfacce, state management.
- **Opinione del docente**: roba degli anni '90/2000, mai decollata, oggi **per lo più legacy**. L'OOP ha interazioni troppo *fine-grained*: mettere la rete in mezzo le rende ingestibili → **non usarlo per sistemi nuovi**.

### Event-based
- **Componenti**: un **event bus** (un broker per eventi, o un insieme di broker con routing); i server sono producer e consumer di eventi; i client interagiscono coi server come nel layered.
- **Pattern**: Request—Response (client↔server) + Publish—Subscribe (server↔event bus).
- **Vincolo**: i server non si conoscono — sanno solo *a quale evento reagire / quale produrre*.
- **Pro**: scalabilità, real-time processing, loose coupling, resilienza, comunicazione asincrona.
- **Contro**: complessità, debugging difficile, **event ordering**, latenza, data consistency, l'event hub come **single point of failure** se non fault-tolerant.
- **Opinione del docente**: molto popolare nell'industria ma **complesso, non entry-level**; capire il layered è un buon punto di partenza (i due stili spesso si combinano).

### Shared Dataspace
- **Componenti**: client e database (repository condiviso). **Vincolo**: i client fanno solo operazioni **CRUD**.
- **Pattern**: Request—Response (read/write) + Publish—Subscribe (notifiche asincrone di cambiamento, streaming di grandi query).
- **Esempi**: Oracle JavaSpaces, IBM TSpaces, GigaSpaces XAP.
- **Pro**: comunicazione disaccoppiata, coordinazione semplificata, scalabilità, fault tolerance, loose coupling, processing asincrono.
- **Contro**: overhead, **data consistency**, concorrenza/race condition, visibilità limitata (debug), house-keeping dello spazio, **single point of failure**.
- **Opinione del docente**: ha senso solo in **nicchie** — distributed data processing, stato condiviso tra (quasi) tutti i nodi con update concorrenti (database/file system distribuiti, multiplayer non real-time).

> Le **feature trasversali** che rendono robusti questi stili (ridondanza, failover, consenso, heartbeat…) sono trattate in [[features-design-distribuito]].

## 8. Connessioni

- [[software-architecture]] · [[componenti-connettori]] — il quadro e gli elementi.
- [[middleware]] — abilita event-based (pub/sub) e shared data-space.
- [[interaction-patterns]] · [[protocolli-interazione-comuni]] — **(M2)** i pattern (request-response, pub-sub) che ogni stile prescrive.
- [[infrastruttura-e-componenti]] — **(M2)** i componenti (layer, oggetti, event bus, database) di cui ogni stile è fatto.
- [[features-design-distribuito]] — **(M2)** le feature che mitigano i "contro" (SPOF, consistency, fault tolerance).
- [[distributed-pong]] — **(M2)** esempio completo di stile **event-based** (coordinator pub/sub di update e input).
- [[coordinazione]] (M6) — la blackboard come *coordination medium*; i connettori mediano la coordinazione.
- [[distributed-information-systems]] (M5) — il web data-centric, EAI.
- [[sistema-distribuito]] — l'uncoupling del controllo (Coulouris).

## 9. Sorgenti

- `raw-sources/M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf` (Architectural Styles), slide 16–28.
- `raw-sources/preliminaries_slides.pdf` (slide 49–68 — *Architecture and Architectural Styles in Practice*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026 (vista ingegneristica: componenti/pattern/pro-contro/opinione).
- Riferimenti: Fielding 2000; Tanenbaum & van Steen 2017.
