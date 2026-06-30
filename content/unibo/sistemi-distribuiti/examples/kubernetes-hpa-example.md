---
title: "Hit Counter su Kubernetes con Horizontal Pod Autoscaler"
course: Sistemi Distribuiti
category: example
topics: [kubernetes, hpa, autoscaling, docker-compose, stress-test]
difficulty: intermedio
sources: [CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Hit Counter su Kubernetes con Horizontal Pod Autoscaler

## Problema

Una semplice web app che **conta le visite**, composta da:
- un **backend** (Python + Flask) che a ogni richiesta su `/` incrementa un contatore e mostra l'**hostname del Pod** che ha servito la richiesta;
- un **key-value store in-memory** (Redis) che conserva il contatore (`hits`).

Obiettivo: portare l'app dallo **sviluppo locale** alla **produzione su Kubernetes**, capace di **scalare automaticamente** sotto carico. (Repo: `github.com/Mala1180/kubernetes-hpa-example`.)

## Tecnica applicata

1. **Sviluppo con Docker Compose** — si containerizza il backend (Dockerfile, `python:3.12-slim`) e si definisce un `docker-compose.yml` con i servizi `backend` (porta 3000, env `REDIS_HOST/PORT`, `depends_on: redis`) e `redis` (`redis:7-alpine`). Avvio: `docker compose up -d --build`.
2. **Migrazione a Kubernetes** — per **ogni servizio** del compose si definisce un **[[kubernetes-objects|Deployment]]** + un **Service**; in più un **HorizontalPodAutoscaler** sul backend. (Il tool **Kompose** può convertire automaticamente il compose in manifest K8s, guidato da label `kompose.*`.)
3. **Monitoring** — Prometheus + Grafana per osservare le metriche (→ [[observability-monitoring]]).

## Soluzione (passo per passo)

- **`backend-deployment.yaml`** — `kind: Deployment`, `replicas: 1`, container `mala1180/kubernetes-example-backend`, `imagePullPolicy: Always`, env Redis, `resources.requests` (128Mi/100m) e `limits` (256Mi/200m).
- **`backend-service.yaml`** — `kind: Service`, `type: NodePort`, `port/targetPort: 3000`, `selector app: backend` (endpoint stabile verso Pod effimeri). Analogo per Redis.
- **`backend-hpa.yaml`** — `kind: HorizontalPodAutoscaler`, `scaleTargetRef` → Deployment `backend`, `minReplicas: 1`, `maxReplicas: 10`, trigger su **CPU > 50%** o **memoria > 70%** (`averageUtilization`).
- **Deploy locale con Minikube**:
  ```
  minikube start
  minikube addons enable metrics-server      # serve all'HPA
  kubectl apply -f k8s
  kubectl port-forward svc/backend 8080:3000
  ```
- **Stress test** — lo script `stress-test.sh` genera carico: la pagina diventa **non responsiva**, poi in pochi secondi l'**HPA crea nuove repliche**, e la pagina torna responsiva mostrando **Pod id diversi** (più Pod servono le richieste). Grafana mostra il picco di metriche.

## Errori comuni

- **Dimenticare il metrics-server** → l'HPA non ha metriche e **non scala** (`minikube addons enable metrics-server`).
- **`requests` non impostate** → l'HPA basato su utilizzo % CPU non può calcolare la percentuale (la utilization è relativa alle `requests`).
- **`selector` del Service ≠ label dei Pod** → il Service non trova endpoint e non instrada traffico.
- **Aspettarsi consistenza forte dallo stato in-memory**: Redis qui è **singola istanza**; replicare il *backend* non replica lo store. Lo stato condiviso (il contatore) vive in Redis, non nei Pod — i Pod sono **stateless** (coerente con il principio di immutabilità di [[kubernetes]]).
- **Affidarsi al `targetPort` sbagliato**: il `port` è client-facing, il `targetPort` è la porta nel container.

## Generalizzazione

È il pattern canonico **"da Compose a K8s production-ready"**: separare componenti **stateless replicabili** (il backend, scalato dall'HPA) dallo **stato condiviso** (Redis), esporre tutto dietro Service stabili, e delegare lo scaling a un controller che insegue il *desired state* (→ [[kubernetes-architettura]]). È l'incarnazione concreta del **replica management** (M3, [[replica-management]]): *content placement* via Deployment, *horizontal scaling* via HPA, fault tolerance via self-healing.

## Connessioni

- [[kubernetes-objects]] — Deployment, Service, HPA usati qui
- [[kubernetes-architettura]] — il reconciliation loop che fa convergere le repliche
- [[observability-monitoring]] — Prometheus/Grafana per lo stress test
- [[replica-management]] — la teoria della replicazione che questo esempio realizza
- [[quality-attributes]] — scalability/availability dimostrate dallo stress test

## Sorgenti

- `raw-sources/CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf` (slide 33–50; M. Matteini, 2025)
