---
title: "Cluster vs Grid Computing"
course: Sistemi Distribuiti
category: bridge
topics: [cluster, grid, omogeneità, eterogeneità, virtual organisation]
difficulty: intermedio
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Cluster vs Grid Computing

Le due sotto-classi dei [[distributed-computing-systems|distributed computing systems]], distinte dall'asse **omogeneità ↔ eterogeneità**.

## 1. Confronto

| Caratteristica | **Cluster** | **Grid** |
|---|---|---|
| Hardware | workstation/PC **simili** | **eterogeneo** |
| Sistema operativo | **stesso** OS | diversi |
| Rete | **stessa LAN** ad alta velocità | geografica, tra organizzazioni |
| Localizzazione | stessa area | siti dispersi |
| Domini amministrativi | **uno** | **molti** (cross-organisation) |
| Organizzazione | nodi + **master node** | **virtual organisation** |
| Scopo tipico | **parallel computing** (1 programma intensivo in parallelo) | **collaborazione** e condivisione risorse |
| Architettura | semplice (es. Beowulf) | a **layer** [Foster et al., 2001]: fabric/resource/connectivity/collective/application |
| Carattere | **omogeneo** | **eterogeneo** |

## 2. Vantaggi / svantaggi

**Cluster (+):** miglior rapporto prezzo/prestazioni vs un singolo supercomputer; robustezza; manutenzione e aggiunta incrementale di potenza facili. **(−):** scala mal oltre la singola area/organizzazione; richiede omogeneità.

**Grid (+):** aggrega risorse di organizzazioni diverse (virtual organisation); attraversa confini amministrativi; gestisce eterogeneità via grid middleware (accesso uniforme). **(−):** complessità di sicurezza/autenticazione, coordinamento tra domini, eterogeneità.

## 3. Sintesi — quando scegliere cosa

- **Cluster** → quando serve **potenza di calcolo aggregata** in un ambiente controllato e omogeneo (HPC, calcolo parallelo intensivo).
- **Grid** → quando serve **collaborare e condividere risorse** tra organizzazioni diverse, accettando eterogeneità e multi-dominio.

> 🔗 L'asse omogeneità/eterogeneità e i domini amministrativi richiamano direttamente la [[scalabilita|scalabilità]] (administrative & geographical, Neuman) e la [[trasparenza|access transparency]] (vista uniforme su risorse eterogenee). Confronta con [[docker-swarm-vs-kubernetes]] e [[centralizzato-vs-distribuito]] per altri assi di confronto.

## 4. Connessioni

- [[distributed-computing-systems]] — la classe che le contiene.
- [[scalabilita]] · [[trasparenza]] (M4) — domini amministrativi; vista uniforme su risorse eterogenee.
- [[middleware]] — il grid middleware come collante dell'eterogeneità.
- [[parallelo-concorrente-distribuito]] (M2) — il cluster come parallel computing.

## 5. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Cluster vs Grid), slide 9–17.
- Riferimenti: Tanenbaum & van Steen 2017; Foster et al. 2001.
