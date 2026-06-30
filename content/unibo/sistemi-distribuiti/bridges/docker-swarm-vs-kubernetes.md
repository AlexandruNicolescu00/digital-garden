---
title: "Docker Swarm vs Kubernetes"
course: Sistemi Distribuiti
category: bridge
topics: [orchestrazione, kubernetes, docker-swarm, container]
difficulty: base
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Docker Swarm vs Kubernetes

Due strumenti di **orchestrazione di container** a confronto. Entrambi gestiscono cluster di container, ma a livelli diversi di complessità e automazione.

## Tabella di confronto

| Caratteristica | **Docker Swarm** | **Kubernetes** |
|----------------|------------------|----------------|
| Natura | tool nativo di clustering/orchestrazione di Docker | piattaforma open source di orchestrazione |
| Complessità | più semplice, più facile da configurare | più complesso e ricco di feature |
| Caso d'uso ideale | cluster piccoli, applicazioni meno complesse | cluster grandi, applicazioni complesse |
| Scaling / load balancing / failover | gestione **manuale** di allocazione risorse e scaling | gestiti **automaticamente** |
| Configurazione ambiente | file `docker-compose.yml` | file **YAML** (manifest, per ogni oggetto) |
| Access control | semplice, basato su **TLS** | avanzato, **RBAC** (Role-Based Access Control) |

## Sintesi: quando scegliere cosa

- **Docker Swarm** se il sistema è **piccolo/semplice** e si vuole partire in fretta con poco overhead, accettando di gestire a mano scaling e failover.
- **Kubernetes** ([[kubernetes]]) se servono **scala, automazione e controllo fine** (autoscaling, self-healing, RBAC) — al prezzo di una curva di apprendimento più ripida. È lo **standard de facto** in produzione.

## Connessioni

- [[kubernetes]] — la piattaforma approfondita nel modulo
- [[containerization]] — il livello comune sottostante (Docker)
- [[quality-attributes]] — l'automazione di K8s migliora scalability/availability/recoverability

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 8–9; M. Matteini, 2025)
