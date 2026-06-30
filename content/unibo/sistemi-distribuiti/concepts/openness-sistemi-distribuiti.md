---
title: "Openness (Apertura dei Sistemi Distribuiti)"
course: Sistemi Distribuiti
category: concept
topics: [openness, standard, IDL, interoperabilità, portabilità, estensibilità]
difficulty: intermedio
sources: [M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Openness (Apertura dei Sistemi Distribuiti)

## 1. Definizione

> **Openness** = la proprietà di un sistema di lavorare con un **numero e tipo di componenti non fissati una volta per tutte** a design time.

- i sistemi aperti sono **progettati per essere aperti**;
- i sistemi aperti sono **fondamentalmente imprevedibili**.

## 2. Progettare sull'imprevedibilità

> **Designing over unpredictability requires predictable items.** Per progettare nonostante l'imprevedibilità, *qualcosa* deve essere **condiviso a priori** tra il sistema e i (potenziali) componenti: es. **regole standard** per sintassi e semantica dei servizi, per lo scambio di messaggi.

→ È il ruolo degli **standard** e del [[middleware]] (vedi *network effect* in [[middleware]]).

### Interfacce per i sistemi aperti
Le **interfacce** specificano la **sintassi** per accedere ai servizi:
- **IDL** (Interface Definition Languages) — definiscono *come* si specificano le interfacce;
- catturano la **sintassi**, non la **semantica**; spesso **non** specificano nemmeno il *protocollo*.

## 3. Issues non-funzionali dell'openness

Tre **requisiti non-funzionali** ([[quality-attributes|quality attributes]]) misurano l'apertura:

| Issue | Misura… |
|-------|---------|
| **Interoperability** | quanto è facile/difficile far lavorare un componente/sistema con altri **diversi**, basandosi su specifiche **standard** |
| **Portability** | quanto un'applicazione (o sua parte) può essere **spostata** su un diverso SD continuando a funzionare |
| **Extensibility** | quanto è facile/difficile **aggiungere** nuovi componenti e funzionalità a un SD esistente |

## 4. Connessioni

- [[obiettivi-sistemi-distribuiti]] — l'openness è il 3° goal.
- [[middleware]] — standard, IDL e interoperabilità sono il cuore del middleware (EAI, integrità concettuale, network effect).
- [[quality-attributes]] (CX) — interoperabilità/portabilità/estensibilità sono requisiti non-funzionali (QA).
- [[distributed-ledger-technology]] (C4) — i SD sono richiesti **aperti**; gli standard (ITU/ISO) servono proprio a questo.
- [[situatedness]] — l'openness è essenziale per gestire l'imprevedibilità di ambienti complessi.

## 5. Sorgenti

- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Goals — Openness), slide 39–41.
- Riferimento: Tanenbaum & van Steen 2017.
