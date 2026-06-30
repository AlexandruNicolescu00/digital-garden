---
title: "Modello di Consistenza (Consistency Model)"
course: Sistemi Distribuiti
category: concept
topics: [consistency, replicazione, modelli, contratto]
difficulty: intermedio
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Modello di Consistenza (Consistency Model)

## Definizione

Un **modello di consistenza** è essenzialmente un **contratto** tra i processi e il data store, che garantisce la **correttezza dei dati** dato un insieme di **regole che i processi devono rispettare**.

Naturalmente, cosa sia "corretto" dipende anche da **cosa i processi si aspettano** — il che può essere problematico da definire in **assenza di una nozione globale di tempo di sistema** (eco del problema "tempo t" già visto in [[stato-sistema-distribuito]] e [[ordinamento-parziale-eventi]]).

## Intuizione

Invece di **un'unica** nozione di consistenza si definiscono **diversi tipi** di consistenza, ognuno adatto a uno scenario applicativo diverso. Così gli ingegneri possono esplorare il **trade-off costi/benefici** della consistenza, rilassandone i requisiti in base allo scenario specifico.

Punto-chiave: la definizione di modello come *contratto* **sposta il focus** dai dati replicati ai **processi che usano i dati** — cioè dalla natura della risorsa all'uso che se ne fa. Per questo esistono **molte** definizioni di consistenza: bisogni applicativi diversi → nozioni utili diverse. Ogni modello è poi suscettibile di **implementazioni diverse**, basate su protocolli diversi, le cui caratteristiche influenzano l'efficacia del modello.

## Le due grandi famiglie

| Famiglia | Riferimento della consistenza | Scenario tipico |
|----------|-------------------------------|-----------------|
| **Data-centric** | la **risorsa** (i dati e le loro repliche) | accessi concorrenti, update simultanei, ordinamento globale |
| **Client-centric** | la **vista del singolo client** sulla risorsa | mobile computing; un client si connette a repliche diverse nel tempo |

→ approfondite nel bridge [[data-centric-vs-client-centric]].

### Modelli data-centric
- **Continuous consistency** — limita la *deviazione* tra repliche → [[continuous-consistency]]
- **Sequential consistency** — tutti vedono gli update nello stesso ordine → [[sequential-consistency]]
- **Causal consistency** — ordina solo le operazioni in relazione causa/effetto → [[causal-consistency]]

### Modelli client-centric
- **Eventual consistency** → [[eventual-consistency]]
- **Monotonic reads, Monotonic writes, Read-your-writes, Writes-follow-reads** → [[client-centric-consistency]]

## Connessioni

- [[replicazione]] — il problema (copie divergenti) che i modelli regolano
- [[consistency]] — la nozione "forte/atomica" del CAP, un caso particolare (più stringente)
- [[teorema-cap]] — perché la consistenza forte non è gratis
- [[causalita]] — fondamento della causal consistency (C2)
- [[data-centric-vs-client-centric]] — confronto delle due famiglie

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 21, 25–26)
