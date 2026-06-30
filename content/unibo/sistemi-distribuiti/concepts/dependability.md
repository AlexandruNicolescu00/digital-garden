---
title: "Dependability"
course: Sistemi Distribuiti
category: concept
topics: [dependability, affidabilità, fault-tolerance, attributi]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Dependability

## Definizione

Nel contesto del *distributed computing*, la **dependability** è la capacità di un sistema distribuito di **fornire servizi corretti ai propri utenti nonostante le molteplici minacce** (*threats*): difetti software non rilevati, guasti hardware, attacchi malevoli, ecc.

Dal dizionario (Oxford): *"the quality of being able to be relied on to do what somebody wants or needs"* — sinonimo **reliability**. Attenzione però: in ingegneria dei sistemi dependability e [[reliability]] sono strettamente legate **ma non sono la stessa cosa**.

## Intuizione: perché conta

La dependability è oggi essenziale: i sistemi distribuiti reggono servizi quotidiani (banking, e-commerce, infrastrutture civili, cloud storage…). Due tensioni economiche:

- **Il costo dei fallimenti è enorme**: il downtime di un data center può costare da decine a centinaia di migliaia di euro l'ora.
- **Anche la dependability costa**: il costo cresce col livello di disponibilità richiesto (es. da 450$/sq ft per 99.671% a 1100$/sq ft per 99.995%). Da qui la domanda guida del modulo: *come ridurre il costo della dependability?* → formando esperti che sappiano progettarla.

## Per ragionare di dependability servono due modelli

1. un modo per **modellare il sistema** distribuito (→ [[stato-sistema-distribuito]])
2. un modo per **modellare le minacce** → in termini di **guasti** (faults), tramite la catena [[fault-error-failure]]

## I cinque attributi

Un sistema dependable ha attributi desiderabili [Tanenbaum & van Steen, 2017; Zhao, 2014]:

| Attributo | Significato | Note |
|-----------|-------------|------|
| [[availability\|Availability]] | pronto all'uso *in un istante* | metrica quantificabile; fondamentale |
| [[reliability\|Reliability]] | funziona senza guasti *su un intervallo* | metrica quantificabile; fondamentale |
| Integrity | protegge il proprio stato dalle minacce | fondamentale; ≈ consistenza delle repliche → [[consistency]] |
| Maintainability | facilità di riparazione/evoluzione (live upgrade) | difficile da quantificare; secondario |
| Safety | un fallimento non causa conseguenze catastrofiche | difficile da quantificare; secondario/non sempre applicabile |

- **Quantificabili come metriche**: availability, reliability.
- **Difficili da quantificare**: integrity, maintainability, safety.
- **Fondamentali per tutti i SD**: availability, reliability, integrity.
- **Secondari / non sempre applicabili**: maintainability, safety.

## Come si ottiene

Quattro approcci → [[mezzi-dependability]]: fault **avoidance**, **detection & diagnosis**, **removal**, **tolerance**.

## Lezioni apprese

- La dependability è una feature essenziale dei SD.
- Servono modelli di sistema e di minaccia per gestirla.
- Le tecniche di gestione dei guasti determinano il *livello* di dependability raggiunto.

## Connessioni

- [[fault-error-failure]] — il modello causale delle minacce
- [[classificazione-guasti]] — i tipi di guasto
- [[modelli-di-fallimento]] — tipi di fallimento e sistemi fail-*
- [[availability]] · [[reliability]] — i due attributi quantificabili
- [[mezzi-dependability]] — come ottenerla
- [[stato-sistema-distribuito]] — il modello di sistema
- [[motivazioni-sistemi-distribuiti]] — la fault tolerance era già una motivazione dei SD (M0)

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 5–11, 32, 37, 54)
- [Tanenbaum & van Steen, 2017] *Distributed Systems*, 3rd ed.; [Zhao, 2014] *Building Dependable Distributed Systems*.
