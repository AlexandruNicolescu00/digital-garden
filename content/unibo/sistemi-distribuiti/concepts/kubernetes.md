---
title: "Kubernetes (Orchestrazione di Container)"
course: Sistemi Distribuiti
category: concept
topics: [kubernetes, orchestrazione, container, declarative, autoscaling]
difficulty: intermedio
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Kubernetes (Orchestrazione di Container)

## Definizione

**Kubernetes** (K8s) è una piattaforma **portabile, estensibile e open source** per **gestire workload e servizi containerizzati**. È lo **standard de facto** per l'**orchestrazione di container**.

### Cosa Kubernetes *non* è
- **non** è una piattaforma di containerization (come [[containerization|Docker]]);
- **non** è un PaaS (come Heroku);
- **non** è un cloud provider (ma ci lavora insieme);
- **non** è un tool di deployment (la CI/CD la definisce l'organizzazione);
- **non** è un tool di logging/monitoring (ma offre integrazioni, → [[observability-monitoring]]).

## Intuizione: perché esiste

I sistemi software oggi sono per lo più **distribuiti e complessi**, e i **requisiti non-funzionali** ([[quality-attributes]]) acquistano importanza. La containerization ha aperto nuovi modi di sviluppare e deployare, ma gestire **a mano** molti container su molte macchine (scaling, load balancing, failover, networking) non scala. Kubernetes **automatizza** tutto questo su un cluster.

## Le cinque key features

| Feature | In cosa consiste | Quality attribute |
|---------|------------------|-------------------|
| **Immutability** | le risorse non si modificano dopo la creazione: per cambiare si **distrugge e si ricrea**; container effimeri e stateless; configurazione **dichiarativa** (un cambiamento produce un nuovo *desired state*, non muta l'esistente) | consistency, reliability |
| **Declarative configuration** | "everything is an object"; oggetti descritti in **YAML** (o JSON) e gestiti col tool dichiarativo **`kubectl`** (es. `kubectl apply -f file.yaml`) | configurability, maintainability |
| **Autoscaling** | scaling automatico in base a metriche: **horizontal** (più/meno repliche) e **vertical** (più/meno CPU/memoria per container) | availability, scalability |
| **Self-healing** | sostituisce automaticamente i container falliti; riattacca i volumi su un altro nodo se il nodo cade; rimuove dalla rotta un Pod malato e ridirige il traffico verso istanze sane | recoverability, reliability |
| **CRI (Container Runtime Interface)** | interfaccia a plugin che svincola K8s da Docker: supporta containerd, CRI-O, Mirantis | estensibilità |

## Connessioni

- [[kubernetes-architettura]] — control plane + worker nodes + reconciliation loop
- [[kubernetes-objects]] — Pod, ReplicaSet, Deployment, Service, HPA, …
- [[containerization]] — il livello sottostante che K8s orchestra
- [[quality-attributes]] — la lente con cui valutare ogni feature
- [[docker-swarm-vs-kubernetes]] — confronto con l'orchestratore nativo di Docker
- [[teoria-vs-kubernetes]] — come K8s realizza i concetti teorici del corso
- [[replica-management]] — K8s come risposta concreta al "dove/quando/chi" della replicazione (M3)
- [[cloud-native-cncf]] — **(M5)** Kubernetes/etcd/Prometheus come progetti **CNCF**; il cloud native come stato dell'arte dei SD

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 3, 7, 10–14; M. Matteini, 2025)
- [Kubernetes] https://kubernetes.io/docs/concepts/overview/
