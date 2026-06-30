---
title: "Quality Attributes (Requisiti Non-Funzionali)"
course: Sistemi Distribuiti
category: concept
topics: [requisiti-non-funzionali, scalabilità, availability, observability]
difficulty: base
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Quality Attributes (Requisiti Non-Funzionali)

## Definizione

I **quality attributes (QA)** sono i **requisiti non-funzionali** di un sistema: non *cosa* il sistema fa (funzionalità), ma *come bene* lo fa. Nei sistemi distribuiti moderni — per lo più complessi — i QA **acquistano importanza centrale** nel design.

## Intuizione: la lente della lezione su Kubernetes

L'intera presentazione di [[kubernetes]] è organizzata mostrando come ogni feature **migliori uno o più QA**:

| Quality attribute | Significato | Realizzato in K8s da |
|-------------------|-------------|----------------------|
| **Scalability** | reggere carichi crescenti | autoscaling (HPA), repliche |
| **Availability** | il servizio risponde quando richiesto | autoscaling, ReplicaSet, self-healing |
| **Reliability** | il servizio funziona correttamente nel tempo | immutability, self-healing |
| **Recoverability** | tornare operativi dopo un guasto | self-healing (reconciliation) |
| **Observability** | capire cosa è successo / sta succedendo | monitoring (Prometheus, Grafana) → [[observability-monitoring]] |
| **Configurability** | facilità di configurare il sistema | declarative config (YAML, `kubectl`) |
| **Maintainability** | facilità di evolvere/manutenere | declarative config, Deployment (rollout/rollback) |

## Connessione con la dependability (M1)

Diversi QA coincidono o si sovrappongono con gli **attributi di dependability** ([[dependability]]):
- **availability**, **reliability**, **maintainability** sono **comuni** a entrambe le tassonomie (→ [[availability]], [[reliability]]);
- **recoverability** è la controparte "QA" del **recovery** studiato in C2 ([[checkpointing-e-logging]]) e dei *mezzi* di [[mezzi-dependability]];
- **observability** è il prerequisito pratico per la **fault detection** ([[mezzi-dependability]]): un sistema andrà *comunque* giù prima o poi — l'osservabilità permette di capire perché.

Differenza di prospettiva: la dependability (M1) è una tassonomia **teorica** delle minacce e dei mezzi; i QA sono il vocabolario **ingegneristico/di prodotto** con cui si valuta un sistema "production-ready".

## Connessioni

- [[kubernetes]] — lo strumento che la lezione usa per migliorare i QA
- [[dependability]] — la tassonomia teorica corrispondente (M1)
- [[availability]], [[reliability]] — attributi condivisi
- [[observability-monitoring]] — il QA "observability" e i suoi strumenti
- [[teoria-vs-kubernetes]] — ponte completo teoria↔pratica
- [[openness-sistemi-distribuiti]] — **(M4)** interoperabilità, portabilità, estensibilità: i QA non-funzionali dell'apertura
- [[obiettivi-sistemi-distribuiti]] — **(M4)** i goal di progetto come QA "a monte" (transparency, openness, scalability)
- [[software-architecture]] — **(M8)** le **proprietà architetturali** (facilità di evoluzione, riusabilità, efficienza, estensibilità) sono QA indotti dai vincoli dell'architettura

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 3, 10–13, 52; M. Matteini, 2025)
