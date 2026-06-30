---
title: "Data-centric vs Client-centric Consistency"
course: Sistemi Distribuiti
category: bridge
topics: [consistency, modelli, replicazione]
difficulty: avanzato
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Data-centric vs Client-centric Consistency

Le due grandi famiglie di **modelli di consistenza** ([[consistency-model]]) introdotte in M3. Differiscono per **a chi/cosa** è riferita la garanzia di consistenza.

## Tabella di confronto

| Caratteristica | **Data-centric** | **Client-centric** |
|----------------|------------------|--------------------|
| Riferimento della consistenza | la **risorsa** (dati e repliche) | la **vista del singolo client** |
| Domanda di fondo | "le repliche sono coerenti tra loro?" | "ciò che vede *questo* client è coerente coi suoi accessi passati?" |
| Update simultanei | problema centrale (serve accordo sull'ordine globale) | tipicamente **assenti/rari** |
| Scenario tipico | sistemi concorrenti, store condivisi | **mobile computing**, client che cambia replica nel tempo |
| Costo | alto (sincronizzazione/ordinamento globale) | basso (garanzie locali per-client) |
| Modelli | continuous, sequential, causal | eventual, monotonic reads/writes, read-your-writes, writes-follow-reads |
| Pagine | [[continuous-consistency]], [[sequential-consistency]], [[causal-consistency]] | [[eventual-consistency]], [[client-centric-consistency]] |

## Gradiente di "forza" (intra-famiglia)

- **Data-centric**, dal più forte al più debole: atomica/CAP ([[consistency]]) → sequential ([[sequential-consistency]]) → causal ([[causal-consistency]]) → continuous con deviazioni ([[continuous-consistency]]).
- **Client-centric**: eventual ([[eventual-consistency]]) come base, rafforzata dalle quattro garanzie per-client ([[client-centric-consistency]]).

## Sintesi: quando scegliere cosa

- Servono **accessi concorrenti coerenti** su uno store condiviso, con semantica forte → **data-centric** (e si paga in sincronizzazione, fino al limite del [[teorema-cap]]).
- Il sistema è **mobile/geo-distribuito**, con un'autorità che aggiorna e molti che leggono, e si tollera la *staleness* → **client-centric** (eventual + garanzie mirate per non sorprendere il client).
- In generale: la scelta dipende dai **pattern di accesso/aggiornamento** e dallo **scopo d'uso** dei dati ([[replicazione]]).

## Connessioni

- [[consistency-model]] — definizione di modello come contratto, da cui nasce la dicotomia
- [[replicazione]] — il dilemma replicazione/consistenza che motiva la pluralità di modelli
- [[teorema-cap]] — il muro che la consistenza forte (data-centric) incontra in presenza di partizioni
- [[acid-vs-base]] — analoga tensione "forte vs rilassato" sul versante transazionale

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 22–40)
