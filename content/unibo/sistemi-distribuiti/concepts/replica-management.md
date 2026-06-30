---
title: "Replica Management"
course: Sistemi Distribuiti
category: concept
topics: [replicazione, placement, servizi, processi]
difficulty: intermedio
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Replica Management

## Definizione

Supportare la replicazione in un sistema distribuito significa decidere **dove, quando e da chi** le repliche debbano essere collocate, e **quali meccanismi** adottare per mantenerle consistenti.

## I due sotto-problemi del placement

La gestione delle repliche si scompone in **due problemi distinti** (attenzione: *non* sono lo stesso problema):

1. **Placing replica servers** — dove collocare i **server** che ospiteranno repliche (decisione infrastrutturale, relativamente statica).
2. **Placing content** — quale **contenuto** replicare e su quali server piazzarlo (decisione legata ai pattern di accesso, più dinamica).

## Oltre i dati: cosa si può replicare

La replicazione **non** significa solo replicare dati. Esistono tre livelli (vedi anche [[replicazione]]):

- **Data replication** — il caso storico/base.
- **Service replication** — si replicano **servizi/funzioni**, che possono o meno insistere sullo **stesso data store**. Si hanno così **due layer di replicazione**, ciascuno con il proprio modello di replica & consistenza (uno per i dati, uno per i servizi).
- **Process replication** — in contesti distribuiti **mobili** si replicano i **processi**, per le stesse ragioni dei data store. Richiede **cloning** e meccanismi di più alto livello come il **goal-passing**.

## Realizzazione concreta: Kubernetes

Il "dove/quando/chi" del replica management è esattamente ciò che orchestra [[kubernetes]] (lezione CX di M. Matteini): il **ReplicaSet** mantiene N copie di un Pod, il **Deployment** ne gestisce rollout/rollback, lo **scheduler** decide il placement sui nodi e l'**HPA** scala orizzontalmente sotto carico. Vedi la mappatura completa in [[teoria-vs-kubernetes]] e l'esempio [[kubernetes-hpa-example]].

## Connessioni

- [[replicazione]] — perché e a che costo si replica
- [[consistency-model]] — i meccanismi per "mantenere consistenti" le repliche
- [[motivazioni-sistemi-distribuiti]] — geo-distribuzione e fault tolerance come driver del placement
- [[dependability]] — la replicazione come mezzo di fault tolerance (ridondanza fisica)
- [[kubernetes-objects]] — ReplicaSet/Deployment/Service come *content placement* concreto
- [[teoria-vs-kubernetes]] — mappatura teoria↔meccanismi
- [[state-machine-replication]] · [[paxos]] — **(C3)** il "come" della replicazione consistente via consenso

## Da approfondire (moduli futuri)

- Replicazione & consistenza per la **fault tolerance**: tabella tecnologie (strong→Raft/Paxos su etcd/Spanner; eventual→DynamoDB/Cassandra; causal→Gossip/Merkle Trees; tunable→Vector Clocks/CRDTs; transactional→Quorums; log-based→Consensus+TrueTime su Kafka). Il versante **consenso** (Raft/Paxos) è ora coperto da C3 ([[paxos]], [[state-machine-replication]]); restano da ingestare logical time e CRDT.

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 42–47)
