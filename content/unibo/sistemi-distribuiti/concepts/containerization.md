---
title: "Containerization"
course: Sistemi Distribuiti
category: concept
topics: [container, virtualizzazione, docker, deployment]
difficulty: base
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Containerization

## Definizione

La **containerization** è una forma **leggera di virtualizzazione** che fornisce **isolamento di processo e di filesystem** sfruttando il **kernel Linux** (namespaces, cgroups). Ha sostituito le tradizionali **Virtual Machine** in molti scenari di deployment. Piattaforma popolare: **Docker**.

## Intuizione

Una VM virtualizza l'**intero hardware** ed esegue un OS guest completo: pesante e lenta da avviare. Un **container** condivide il kernel dell'host e isola solo ciò che serve (processi, filesystem, rete) → immagini piccole, avvio in secondi, alta densità per macchina. È questa leggerezza a rendere praticabile il deployment di sistemi distribuiti composti da **molte unità** replicabili e usa-e-getta.

I container sono pensati per essere **effimeri e stateless**: crearli e distruggerli è parte normale del loro ciclo di vita (premessa che [[kubernetes]] eleva a principio architetturale, → immutability).

## Connessioni

- [[kubernetes]] — orchestra container su un cluster; non è esso stesso una piattaforma di containerization
- [[docker-swarm-vs-kubernetes]] — due orchestratori a confronto
- [[processo-computazionale-contesto]] — il container è una realizzazione del "contesto" (macchina/risorse) di un processo (M2)
- [[quality-attributes]] — abilita scalabilità e deployment ripetibile

## Da approfondire

- Lezione di Giovanni Ciatto su Virtuale (rimando della sorgente per approfondire la containerizzazione).
- Container Runtime Interface (CRI): runtime alternativi a Docker (containerd, CRI-O) → vedi [[kubernetes]].

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 5; M. Matteini, 2025)
- [Docker] https://www.docker.com/
