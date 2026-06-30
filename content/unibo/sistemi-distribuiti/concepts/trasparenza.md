---
title: "Trasparenza (Distribution Transparency)"
course: Sistemi Distribuiti
category: concept
topics: [transparency, access, location, migration, replication, concurrency, failure]
difficulty: intermedio
sources: [M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Trasparenza (Distribution Transparency)

## 1. Definizione

> **Transparency** = nascondere all'utente le **proprietà non rilevanti** dei componenti e della struttura del sistema.

A volte la distribuzione fisica **non è una feature**: un buon motivo per costruire un SD è renderla **irrilevante** dal punto di vista dell'utente. Nascondendo le proprietà non rilevanti, il SD fornisce un **livello di astrazione più alto**. Esistono **diversi tipi** di trasparenza, a seconda della proprietà nascosta.

## 2. I sette tipi di trasparenza [Tanenbaum & van Steen, 2017]

| Tipo | Cosa nasconde |
|------|---------------|
| **Access** | differenze nella **rappresentazione dei dati** e nelle modalità di accesso alle risorse |
| **Location** | **dove** si trova una risorsa |
| **Migration** | il **cambiamento di posizione** di una risorsa |
| **Relocation** | lo **spostamento** di una risorsa *mentre è in uso* (versione "on-line" della migration) |
| **Replication** | che una risorsa è **replicata** |
| **Concurrency** | la **condivisione** di una risorsa tra più utenti |
| **Failure** | il **guasto** e il successivo **recovery** di una risorsa |

### Access transparency
Nasconde l'eterogeneità in rappresentazione dei dati, struttura dei componenti, interfacce/protocolli d'uso → vista **omogenea** su risorse eterogenee.

### Location transparency
La posizione fisica di una risorsa è spesso irrilevante per il suo uso (e viceversa). Richiede **naming**: un sistema di **identificatori logici** non legati alla posizione fisica (es. **URL**).

### Migration & Relocation
Le risorse (e gli **utenti**!) possono essere **mobili**; il sistema deve mantenere coerenza e funzionalità durante lo spostamento. *Relocation* = accesso garantito **mentre** lo spostamento avviene. La mobilità dell'utente è oggi un tema centrale.

### Replication transparency
La [[replicazione]] si fa per molte ragioni (copia locale → meno banda + accesso più veloce; ridondanza → fault tolerance), ma **non deve preoccupare l'utente**: tutte le repliche devono avere lo **stesso nome** ed essere **nello stesso stato**, così da apparire *una e una sola cosa*.

### Concurrency transparency
Utenti e risorse lavorano in modo **concorrente** e autonomo; due utenti possono usare la stessa risorsa simultaneamente. Il sistema definisce **politiche di accesso** (cooperativo o competitivo) trasparenti. → **Concurrency & consistency:** con accessi concorrenti la **[[consistency|consistenza]]** dello stato è a rischio e va garantita comunque (resource safety).

### Failure transparency
> *"You know you have a distributed system when the crash of a computer you've never heard of stops you from getting any work done."* — L. Lamport

Significa **mascherare i guasti** e nasconderli all'utente. È un **problema difficile**, per via della **latenza**: come distinguere una risorsa **morta** da una **molto lenta**? Il "silenzio" è dovuto a lentezza, scelta deliberata, guasto della risorsa o della rete? → è il cuore del [[modello-sincrono-asincrono|modello asincrono]] e dell'[[flp-impossibility|impossibilità FLP]]. La distribuzione consente però il **partial failure** (parte del sistema continua a funzionare), molto meglio del *total failure* dei sistemi centralizzati.

## 3. Il grado di trasparenza — un trade-off

⚠️ **Nascondere la distribuzione non è sempre la scelta migliore.** Esempi:
- utenti in **fusi orari** diversi → se nascosto, situazioni assurde (un download che "finisce prima di iniziare");
- a volte la **location-awareness è una feature** (sapere dove sta il file server prima di scaricare).

> **Trade-off trasparenza ↔ informazione:** riguarda tipicamente la *performance*, ma non solo (comprensibilità, ecc.). Ogni ingegnere deve individuare il **grado preciso** di trasparenza adatto al proprio sistema. → si collega alla [[situatedness]] (a volte serve *consapevolezza* del contesto, non occultamento).

## 4. Connessioni

- [[obiettivi-sistemi-distribuiti]] — la trasparenza è il 2° goal.
- [[replicazione]] · [[consistency]] — replication & concurrency transparency richiedono gestione della consistenza.
- [[modello-sincrono-asincrono]] · [[flp-impossibility]] — failure transparency e il problema "lento vs morto".
- [[partition-tolerance]] · [[availability]] — partial failure, mascheramento dei guasti.
- [[situatedness]] — il lato opposto: a volte la consapevolezza del contesto è desiderabile.
- [[distribuzione-spaziale-temporale]] (M0) — location/migration e i fusi orari.
- [[code-mobility]] (C6) — migration/relocation transparency in azione; il **cloning** migliora la distribution transparency.

## 5. Sorgenti

- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Goals — Transparency), slide 23–37.
- Riferimento: Tanenbaum & van Steen 2017.
