---
title: "Mezzi per Ottenere la Dependability"
course: Sistemi Distribuiti
category: concept
topics: [dependability, fault-tolerance, redundancy, checkpointing]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Mezzi per Ottenere la Dependability

## I quattro approcci [Zhao, 2014]

Per migliorare la [[dependability]] di un sistema distribuito si interviene sulla catena [[fault-error-failure]] in quattro modi:

### 1. Fault avoidance (evitare i guasti)
Costruire/usare componenti HW e SW meno soggetti a guasti, *prima* del deployment:
- **rigorous software design**, eventualmente con **metodi formali** per verificare le proprietà
- **rigorous software testing** per identificare e rimuovere i bug

### 2. Fault detection & diagnosis (rilevare e diagnosticare)
- I **crash fault** sono banali da rilevare: basta fare **probing** periodico dello stato dei componenti.
- Ma i componenti possono fallire in modi diversi dal crash → il probing non sempre basta. Se un fault non è rilevato, l'**integrità** non è garantita.
- Dopo il rilevamento serve la **diagnosi**: confermare il fault e **localizzare** il componente guasto (con modelli formali e strumenti statistici).
- Esempio concreto: l'**exception handling** nei linguaggi moderni.

### 3. Fault removal (rimuovere i guasti)
- Il componente guasto va **isolato e rimosso**, poi **riparato o sostituito** e reintrodotto (richiede riconfigurazione).
- In un SD serve una nozione di **membership**: il componente guasto è escluso dal sistema; quello riparato rientra nella membership.
- Caso speciale: **software update**.

### 4. Fault tolerance (tollerare i guasti)
Il software robusto da solo non basta (i guasti HW esistono). Se il sistema non è *stateless*, un semplice restart **non** ripristina lo stato precedente → servono tecniche apposite.
- Per alta **availability** ma non necessariamente alta **reliability**: **logging** e **checkpointing** possono bastare.
- Applicazioni più esigenti: **recovery-oriented computing**.
- Entrambe le classi si basano sul **rollback recovery**: tornare all'ultimo **stato corretto registrato** (→ [[stato-sistema-distribuito]]).

## Masking failure by redundancy

L'idea per **mascherare** i guasti (*hiding failures* da altri processi — eco di [[availability]] e M0) è la **ridondanza**, di tre tipi:

| Tipo di ridondanza | Esempio |
|--------------------|---------|
| **Information** | bit extra (es. codici a correzione d'errore) |
| **Time** | redo dopo abort di una transazione |
| **Physical** | repliche fisiche (tipica nei sistemi biologici) |

## Connessioni

- [[dependability]] — il concetto ombrello
- [[fault-error-failure]] — la catena su cui si interviene
- [[modelli-di-fallimento]] — crash facile da rilevare; fail-fast come pratica
- [[stato-sistema-distribuito]] — lo stato necessario per checkpointing/rollback
- [[availability]] · [[reliability]] — tecniche diverse per requisiti diversi
- [[consistency]] — l'integrità ≈ consistenza delle repliche ridondanti
- [[checkpointing-e-logging]] — **(C2)** logging/checkpointing/rollback recovery, qui sviluppati in dettaglio
- [[protocolli-recovery]] — **(C2)** la tassonomia completa dei protocolli di recovery
- [[kubernetes]] — **(CX)** self-healing = fault detection + recovery automatici; [[observability-monitoring]] come fault detection (Prometheus/Grafana)
- [[agreement-consensus]] · [[paxos]] — **(C3)** il consenso come mezzo di fault tolerance: garantire la consistenza delle repliche e la recovery dopo un crash
- [[byzantine-fault-tolerance]] · [[blockchain]] — **(C4)** ridondanza + consenso BFT come tolleranza ai fault *bizantini* (non solo crash); la DLT come fault tolerance offerta a livello di middleware

## Da approfondire (moduli futuri)

- Replicazione e protocolli di consistenza.
- ~~checkpointing coordinato / snapshot globali~~ → coperti in C2: [[tamir-sequin-checkpointing]], [[chandy-lamport-snapshot]].

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 46–52)
