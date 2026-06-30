---
title: "La Teoria del Corso vista in Kubernetes"
course: Sistemi Distribuiti
category: bridge
topics: [kubernetes, dependability, replicazione, fault-tolerance, stato]
difficulty: intermedio
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# La Teoria del Corso vista in Kubernetes

La lezione su [[kubernetes]] è un **caso studio** che *operazionalizza* i concetti teorici visti finora (M1 dependability, M3 replication & consistency, C2 recovery, M2 stato). Questa pagina mappa **concetto teorico → meccanismo K8s**.

## Tabella di mappatura

| Concetto del corso | Realizzazione in Kubernetes |
|--------------------|-----------------------------|
| **Replicazione** ([[replicazione]], M3) | **ReplicaSet** mantiene N copie identiche di un Pod; **Deployment** le gestisce con rollout/rollback |
| **Replica management** ([[replica-management]], M3): dove/quando/chi | **scheduler** (placement dei Pod sui nodi) + **Deployment template** (content placement); *server placement* = scelta dei nodi |
| **Fault tolerance / mezzi di dependability** ([[mezzi-dependability]], M1) | **self-healing**: detection + recovery automatici (ricrea container, riattacca volumi, ridirige traffico) |
| **Reliability / Availability** ([[reliability]], [[availability]], M1) | **autoscaling (HPA)** + ridondanza di repliche dietro un Service |
| **Recovery / rollback** ([[checkpointing-e-logging]], C2) | **Deployment rollback** a una versione precedente; container effimeri ricreati da template |
| **Stato del sistema, desired vs current** ([[stato-sistema-distribuito]], M1) | **reconciliation loop** spec↔status: il control plane insegue il *desired state* (→ [[kubernetes-architettura]]) |
| **Strong consistency dei metadati** ([[consistency]], CAP) | **etcd** (key-value store del control plane), consistente via **Raft** — un algoritmo di **consenso** (→ [[paxos]], [[agreement-consensus]], C3) |
| **Consenso / SMR** ([[agreement-consensus]], [[state-machine-replication]], C3) | **etcd**/Raft come modulo di consenso replicato; parenti: ZooKeeper/ZAB, Chubby |
| **Quality attributes / requisiti non-funzionali** | feature mappate 1:1 sui QA (→ [[quality-attributes]]) |
| **Observability / fault detection** ([[mezzi-dependability]]) | **Prometheus + Grafana** (→ [[observability-monitoring]]) |
| **Container come "contesto" del processo** ([[processo-computazionale-contesto]], M2) | il **Pod**/container fornisce macchina, risorse (CPU/mem requests-limits), rete |

## Sintesi

Kubernetes non introduce teoria nuova: **automatizza** in produzione le idee del corso. Il filo conduttore è il **paradigma dichiarativo** (desired state + reconciliation), che trasforma la fault tolerance da procedura imperativa esplicita a **proprietà emergente** del loop di controllo. È la risposta concreta al gancio lasciato aperto da M3 ("una classe a venire su Kubernetes darà esempi concreti").

## Connessioni

- [[kubernetes]], [[kubernetes-architettura]], [[kubernetes-objects]] — i meccanismi
- [[dependability]], [[mezzi-dependability]], [[reliability]], [[availability]] — M1
- [[replicazione]], [[replica-management]], [[consistency-model]] — M3
- [[checkpointing-e-logging]], [[stato-sistema-distribuito]] — C2/M1
- [[agreement-consensus]], [[paxos]], [[state-machine-replication]] — C3 (etcd/Raft)
- [[quality-attributes]] — la lente ingegneristica

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (intera; M. Matteini, 2025)
