---
title: "Observability e Monitoring"
course: Sistemi Distribuiti
category: concept
topics: [observability, monitoring, prometheus, grafana, metriche]
difficulty: base
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Observability e Monitoring

## Definizione

L'**observability** è il quality attribute ([[quality-attributes]]) per cui un sistema permette di **capire cosa è successo / sta succedendo** al suo interno. Premessa realistica: *un sistema di produzione, prima o poi, andrà giù per qualche motivo* — un sistema **osservabile** consente di diagnosticarlo.

Il **monitoring** è l'insieme di strumenti e pratiche che **realizzano** l'observability. Un sistema di monitoring può:
- **raccogliere e mostrare metriche**;
- **allertare** l'amministratore di sistema;
- **loggare eventi**.

## Strumenti tipici (stack K8s)

- **Prometheus** — toolkit open source di monitoring e alerting; **raccoglie le metriche**.
- **Grafana** — interfaccia web open source per analytics e monitoring; **visualizza** le metriche raccolte (es. dashboard durante uno stress test).

> Kubernetes **non** è un tool di monitoring (→ [[kubernetes]]), ma offre integrazioni: Prometheus/Grafana sono lo stack di osservabilità comunemente affiancato.

## Connessioni

- [[quality-attributes]] — observability come QA, accanto ad availability/reliability/scalability
- [[mezzi-dependability]] — il monitoring abilita la **fault detection** (M1)
- [[kubernetes-hpa-example]] — Prometheus + Grafana usati nello stress test dell'esempio
- [[stato-sistema-distribuito]] — osservare lo stato esterno per inferire quello interno (M1)

## Da approfondire

- Logging distribuito, tracing e i "tre pilastri" dell'observability (metrics, logs, traces) — non trattati esplicitamente nella sorgente.

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 52–54; M. Matteini, 2025)
- [Prometheus] https://prometheus.io/ ; [Grafana] https://grafana.com/
