---
title: "Oggetti Kubernetes (Pod, Deployment, Service, HPA)"
course: Sistemi Distribuiti
category: concept
topics: [kubernetes, pod, deployment, service, hpa, replicaset]
difficulty: intermedio
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Oggetti Kubernetes

## Definizione

Gli **oggetti** sono le **entità persistenti** con cui Kubernetes rappresenta lo **stato del cluster**: quali applicazioni girano (e su quali nodi), quali risorse hanno a disposizione, quali policy (restart, upgrade). Ogni oggetto è un **"record of intent"** (→ [[kubernetes-architettura]]).

### Il manifest
Creare un oggetto significa definirne la **spec** e alcune informazioni di base (es. il nome), di solito via un file di configurazione (**manifest**) in YAML passato a `kubectl`. Campi obbligatori:
- **apiVersion** — versione dell'API da usare;
- **kind** — il tipo di oggetto;
- **metadata** — dati identificativi (name, UID);
- **spec** — lo stato desiderato (formato specifico per ogni tipo di oggetto).

## La gerarchia degli oggetti di workload

```
HorizontalPodAutoscaler   →  regola le repliche di un Deployment
        │
   Deployment             →  update dichiarativi: rollout / rollback
        │
   ReplicaSet             →  mantiene N repliche identiche di un Pod
        │
     Pod                  →  unità minima deployabile (1+ container)
        │
  Container(s)            →  l'immagine effettiva in esecuzione
```

### Pod
**Unità minima deployabile**: un gruppo di **uno o più container** con **storage e rete condivisi**. Il contesto condiviso è un insieme di namespace/cgroup Linux. Due usi:
- **un singolo container** (caso più comune: il Pod fa da wrapper);
- **più container** co-locati e strettamente accoppiati, che condividono **IP e porte, hostname, volumi, PID** — utile per pattern come **Sidecar, Ambassador, Adapter** [Burns, 2024].

Nella spec si definiscono immagine, comando, porta, protocollo e le **risorse**: `requests` (CPU/memoria **garantite**) e `limits` (massimo usabile). CPU in **millicpu** (100m = 0.1 = 10%), memoria in byte (128Mi = 128MB).

### ReplicaSet
Gestisce il **numero di repliche** di un Pod (scale up/down); garantisce la **disponibilità** di N Pod identici creandoli dal proprio **Pod template**. Di norma **non** gestito direttamente, ma tramite un Deployment.

### Deployment
Oggetto di alto livello che gestisce un set di Pod (e ReplicaSet). Fornisce **update dichiarativi**: **Rollout** (dallo stato attuale al desiderato, es. cambio versione immagine **senza downtime**) e **Rollback**. È lo strumento per **rilasciare nuove versioni**. Campi chiave: `replicas`, `selector` (come trova i Pod da gestire), `template` (la spec dei Pod da creare).

### Altri oggetti di workload (alternativi al Deployment)
- **Job** — task one-off di breve durata.
- **CronJob** — azioni schedulate regolari (backup, report, …).
- **StatefulSet** — come un Deployment, ma mantiene una **identità sticky** per ogni Pod (utile per servizi *stateful*).
- **DaemonSet** — garantisce che **tutti (o alcuni) i nodi** eseguano una copia del Pod (aggiunti/rimossi con i nodi; GC automatica).

### Service
Astrazione che definisce un **insieme logico di Pod** e una **policy di accesso**; è l'**entry-point** dell'applicazione. Poiché i Pod sono **effimeri** (cambiano nodo e IP), il Service fornisce un **endpoint stabile** (IP + DNS) usando i **label selector**. Tipi:
- **ClusterIP** (default) — IP interno al cluster;
- **NodePort** — porta statica sull'IP di ogni nodo;
- **LoadBalancer** — esposizione esterna via load balancer del cloud (o MetalLB on-premise).

### HorizontalPodAutoscaler (HPA)
Scala automaticamente le repliche di un Deployment in base a metriche (es. CPU > 50%, memoria > 70%), tra `minReplicas` e `maxReplicas`. È l'oggetto che realizza l'**horizontal autoscaling** (→ [[kubernetes-hpa-example]]).

## Connessioni

- [[kubernetes-architettura]] — il reconciliation loop che mantiene vivi questi oggetti
- [[kubernetes]] — le key features realizzate da questi oggetti
- [[kubernetes-hpa-example]] — esempio completo con Deployment + Service + HPA
- [[replica-management]] — ReplicaSet/Deployment come *content placement* concreto (M3)
- [[teoria-vs-kubernetes]] — mappatura sui concetti del corso

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 17–31; M. Matteini, 2025)
- [Burns, 2024] *Designing Distributed Systems*, O'Reilly.
