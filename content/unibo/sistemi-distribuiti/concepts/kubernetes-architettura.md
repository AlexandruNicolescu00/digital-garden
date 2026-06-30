---
title: "Architettura del Cluster Kubernetes e Reconciliation Loop"
course: Sistemi Distribuiti
category: concept
topics: [kubernetes, control-plane, etcd, desired-state, controller]
difficulty: intermedio
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Architettura del Cluster Kubernetes e Reconciliation Loop

## Definizione

Un **cluster Kubernetes** è composto da un **control plane** più un insieme di macchine worker, dette **nodi (nodes)**.

### Control Plane — gestisce il cluster
- **kube-apiserver** — espone l'**API HTTP** di Kubernetes; punto d'ingresso di ogni operazione.
- **etcd** — database **key-value** che memorizza i **metadati** del cluster (lo stato). (Lo stesso etcd citato in M3 come backend per la *strong consistency* via Raft.)
- **kube-scheduler** — osserva i servizi/Pod appena creati **senza nodo assegnato** e ne sceglie uno su cui eseguirli.
- **kube-controller-manager** — esegue i processi **controller** (i loop di riconciliazione, vedi sotto).
- **cloud-controller-manager** — incorpora la logica di controllo specifica del cloud provider.

### Worker Node — esegue le applicazioni containerizzate
- **kubelet** — garantisce che i container siano in esecuzione sul nodo.
- **kube-proxy** — mantiene le **regole di rete** sui nodi.
- **container runtime** — il software che esegue i container (es. Docker, containerd; via [[kubernetes|CRI]]).

## Intuizione: il reconciliation loop (desired vs current state)

È il cuore concettuale di Kubernetes. Quasi ogni **oggetto** ([[kubernetes-objects]]) ha due campi annidati:

- **Spec** — la descrizione delle caratteristiche che si **vogliono** (il *desired state*), fornita dall'utente.
- **Status** — lo stato **attuale** dell'oggetto, fornito e aggiornato da Kubernetes.

Un oggetto è un **"record of intent"**: una volta creato, il control plane **lavora costantemente** per far combaciare lo stato attuale (Status) con quello desiderato (Spec). I **controller** osservano la differenza e agiscono per annullarla.

> Questo è esattamente ciò che rende possibili **self-healing** e **autoscaling**: non si comanda *come* arrivare allo stato voluto (imperativo), si **dichiara** lo stato voluto e il sistema converge da solo (dichiarativo). Connessione teorica forte con [[stato-sistema-distribuito]] (M1) e con la nozione di stato che il sistema deve mantenere/recuperare.

## Connessioni

- [[kubernetes]] — overview e key features (la declarative config si fonda su questo loop)
- [[kubernetes-objects]] — gli oggetti su cui agisce il loop spec/status
- [[stato-sistema-distribuito]] — desired/current state ↔ stato interno e recovery (M1)
- [[mezzi-dependability]] — il reconciliation loop è un meccanismo di fault tolerance (detection + recovery automatici)
- [[teoria-vs-kubernetes]] — mappatura teoria↔meccanismi

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 15–18; M. Matteini, 2025)
