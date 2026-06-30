---
title: "Motivazioni dei Sistemi Distribuiti"
course: Sistemi Distribuiti
category: concept
topics: [motivazioni, vantaggi, fault-tolerance, scalabilità]
difficulty: base
sources: [M0-Why-Distributed-Systems.pdf, preliminaries_slides.pdf]
created: 2026-06-25
updated: 2026-06-27
---

# Motivazioni dei Sistemi Distribuiti

## Perché servono

Secondo [Ghosh, 2014], abbiamo bisogno di sistemi distribuiti per quattro ragioni principali:

1. **Ambienti geograficamente distribuiti** — il problema stesso è distribuito nello spazio (utenti, dati, risorse in luoghi diversi).
2. **Accelerazione del calcolo** (*computation speedup*) — più unità computazionali lavorano in parallelo.
3. **Condivisione di risorse** (*resource sharing*) — hardware, dati e servizi condivisi tra nodi.
4. **Tolleranza ai guasti** (*fault tolerance*) — il sistema continua a funzionare anche se alcuni componenti falliscono.

## La lista estesa (Module 2, Ciatto)

La prospettiva *ingegneristica* (M2) amplia le ragioni a **otto**, sottolineando che spesso **la funzionalità stessa da fornire *implica* la distribuzione** (non è una scelta, è una conseguenza):

1. **Scalability** — gestire sistemi su larga scala in modo efficiente.
2. **Fault Tolerance & Availability** — affidabilità nonostante i guasti.
3. **Low Latency & Geographical Distribution** — migliore esperienza utente in tutto il mondo.
4. **Resource Sharing** — uso efficiente di potenza di calcolo e storage.
5. **Handling Big Data** — elaborare i dati *localmente* anziché spostarli.
6. **Parallelism** — accelerare i task con esecuzione concorrente.
7. **Cost Efficiency** — ridurre i costi infrastrutturali con il *resource pooling*.
8. **Collaboration** — abilitare aggiornamenti e interazioni in tempo reale a distanza.

> Le ragioni di Ghosh sono un sottoinsieme di questa lista: geo-distribuzione (≈1,3), speedup (≈6), resource sharing (≈4), fault tolerance (≈2).

## Esempi (la distribuzione è spesso *inerente*)

| Esempio | Ragioni dominanti |
|---------|-------------------|
| **Google Search** | scalability (miliardi di ricerche/giorno), fault tolerance, low latency (migliaia di server nel mondo), parallelism |
| **Social / Messaging** (Instagram, WhatsApp) | scalability (milioni di utenti simultanei), availability, low latency, **handling big data** (dati nella regione dell'utente, *cfr. GDPR*), collaboration |
| **Online Shopping** (Amazon) | scalability, availability, geo-distribution (regolamenti/lingua/valuta locali), parallelism |
| **Online Gaming** (LoL, Fortnite) | scalability, low latency, geo-distribution, parallelism, collaboration |
| **Google Docs** | fault tolerance (autosave/backup), geo-distribution (sharing), **parallelism + collaboration** (editing concorrente) |
| **Federated Learning** (Gboard) | privacy (dati locali), handling big data, cost efficiency, resource sharing, parallelism (apprendimento on-device) |

> *Federated learning* è il caso emblematico: la distribuzione serve a **preservare la privacy** processando i dati dove si trovano, senza spostarli.

## Il rovescio della medaglia

Questi vantaggi hanno un costo. Più componenti ci sono, maggiore è il rischio che il fallimento di uno comprometta il resto del sistema, **a meno che non si adottino misure speciali**. Servono cioè **meccanismi appositi** per evitare che un guasto locale si propaghi — il che introduce complessità.

Questa tensione vantaggi/svantaggi è formalizzata nel confronto sistematico: → [[centralizzato-vs-distribuito]].

## Connessioni

- [[sistema-distribuito]] — il concetto centrale
- [[centralizzato-vs-distribuito]] — il confronto dei trade-off
- [[pervasivita-computazione-interazione]] — il contesto che rende queste motivazioni rilevanti
- [[obiettivi-sistemi-distribuiti]] — **(M4)** le motivazioni "a monte" diventano i goal di progetto (resource availability su tutti)
- [[se-workflow-distribuito]] — **(M2)** se la funzionalità *implica* distribuzione, l'intero workflow SE cambia
- [[features-design-distribuito]] — **(M2)** i meccanismi concreti (ridondanza, failover, consenso, partitioning) che realizzano fault tolerance e availability
- [[scalabilita]] — **(M4)** la scalability come obiettivo/dimensione

## Sorgenti

- `raw-sources/M0-Why-Distributed-Systems.pdf` (slide 10)
- `raw-sources/preliminaries_slides.pdf` (slide 8–14 — *Why distributed? / Examples*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
- [Ghosh, 2014] *Distributed Systems: An Algorithmic Approach*, CRC Press, 2nd ed.
