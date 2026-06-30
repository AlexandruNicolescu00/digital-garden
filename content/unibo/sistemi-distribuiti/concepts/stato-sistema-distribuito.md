---
title: "Stato di un Sistema Distribuito"
course: Sistemi Distribuiti
category: concept
topics: [stato, recovery, boundary, modello-di-sistema]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Stato di un Sistema Distribuito

## Definizione

Un sistema (distribuito) è progettato per fornire **servizi** ai propri utenti (*client*). Ogni servizio ha un'**interfaccia** per richiederlo, e una **functional specification** definisce *cosa* dovrebbe fare. In ogni istante il sistema è in un dato **stato**.

Definizione generale: lo **stato di un sistema al tempo t** è la (minima) informazione che, insieme alla conoscenza della dinamica di un sistema (deterministico), permette a un osservatore di **descrivere completamente il comportamento futuro** da t in poi.

## Stato esterno vs interno

Lo stato di un SD è determinato **collettivamente** dallo stato di processi e thread (registri, stack, heap, file descriptor, kernel state…). Si divide in:

- **External / observable state** — la parte visibile all'utente tramite interazione; in pratica lo *stato astratto* definito dalla functional specification.
- **Internal state** — la parte **non** visibile agli utenti.

## Boundary e livello di astrazione

Per modellare un sistema serve definirne il **boundary** (confine): la linea fisica/logica che separa il sistema dall'**ambiente** (tutti gli altri sistemi che lo influenzano).
- dentro il confine: i **componenti**; fuori: l'**ambiente**.
- ⚠️ Cosa è "componente" e cosa è "sistema" dipende dal **livello di astrazione**: lo stesso sistema può essere visto come insieme di componenti o come componente di un sistema più grande (eco della natura ricorsiva in [[fault-error-failure]], e del confine "spazialmente sparso" in [[distribuzione-spaziale-temporale]]).

## Lo stato per il recovery

Lo stato serve per il **recovery** dopo un failure: un sistema può tornare al "punto" precedente al guasto **se** il suo stato era stato catturato (in modo consistente) e non perso — es. **serializzato su stable storage** prima del guasto e intatto dopo. È la base di logging/checkpointing/rollback (→ [[mezzi-dependability]]).

## ⚠️ Il problema aperto: "al tempo t" in un SD

Catturare lo stato *"al tempo t"* è problematico, perché **non sappiamo ancora cosa significhi "al tempo t"** in un sistema distribuito: è il problema complesso del **tempo nei sistemi distribuiti**, che il corso affronterà nei moduli successivi.

> Gancio diretto: questo è esattamente il nodo di [[ordinamento-parziale-eventi]] (niente clock globale, solo ordine parziale). La cattura di uno stato globale coerente richiederà strumenti come i clock logici e gli snapshot distribuiti.

**Risolto in C2**: la cattura di uno **stato globale consistente** è affrontata in [[global-state-consistency]] e realizzata dal [[chandy-lamport-snapshot|distributed snapshot di Chandy-Lamport]] (e da [[tamir-sequin-checkpointing]]).

## Connessioni

- [[dependability]] — perché serve modellare il sistema
- [[fault-error-failure]] — composizione ricorsiva e livelli di astrazione
- [[mezzi-dependability]] — checkpointing e rollback recovery
- [[ordinamento-parziale-eventi]] — il problema del "tempo t" senza clock globale
- [[distribuzione-spaziale-temporale]] — il confine sistema/ambiente
- [[global-state-consistency]] · [[chandy-lamport-snapshot]] — **(C2)** cattura dello stato globale consistente

## Da approfondire (moduli futuri)

- **Clock logici (Lamport), clock vettoriali** per catturare/ordinare gli eventi.
- ~~snapshot globali (Chandy-Lamport)~~ → coperto in C2: [[chandy-lamport-snapshot]].

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 12–16)
