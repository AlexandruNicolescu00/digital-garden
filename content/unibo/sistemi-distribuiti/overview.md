---
title: "Panoramica del Corso — Sistemi Distribuiti"
course: Sistemi Distribuiti
category: overview
topics: [introduzione, modelli, comunicazione, sincronizzazione, replicazione, consensus, fault-tolerance]
difficulty: base
sources: [M0-Why-Distributed-Systems.pdf, C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf, M1-Dependability-in-Distributed-Systems.pdf, M2-Roots-of-Distributed-Systems-Computation-in-Space-Time.pdf, C2-Logging-Checkpointing.pdf, M3-Replication-Consistency-in-Distributed-Systems.pdf, CX-Addressing-production-ready-distributed-systems-through-Kubernetes.pdf, C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf, C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf, M4-Definitions-Goals-for-Distributed-Systems.pdf, M5-Sorts-Distributed-Systems.pdf, C5-Logical-Clocks.pdf, M6-Computing-with-Time.pdf, M7-Computing-with-Space.pdf, C6-Code-Mobility.pdf, M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf, M9-Modelling-Distributed-Systems-Process-Algebra.pdf, preliminaries_slides.pdf, communication_slides.pdf, pong_slides.pdf]
created: 2026-06-25
updated: 2026-06-27
---

# Panoramica — Sistemi Distribuiti (UniBo)

## Obiettivi del corso

Un sistema distribuito è un insieme di processi indipendenti che **cooperano verso un obiettivo comune** attraverso una rete, pur apparendo all'utente come un sistema unico e coerente. Il corso risponde a tre domande fondamentali:

1. Come si coordinano processi che non condividono né memoria né clock?
2. Come si garantisce coerenza quando dati e computazione sono replicati?
3. Come si costruisce un sistema affidabile quando i suoi componenti possono fallire?

---

## Filo narrativo del programma

Il corso segue una progressione logica precisa:

```
Fondamenta
  └── Modelli e sfide dei sistemi distribuiti
        └── Comunicazione (come i processi si parlano)
              └── Tempo e causalità (come si ordina ciò che accade)
                    └── Coordinazione (mutex, elezione)
                          └── Coerenza e replicazione
                                └── Tolleranza ai guasti
                                      └── Consensus (Paxos, Raft)
                                            └── Sistemi reali
```

Ogni strato si appoggia al precedente: non si può capire il consensus senza aver capito la tolleranza ai guasti; non si può capire la replicazione senza aver capito la coerenza; non si può capire la coerenza senza aver capito come si ordina il tempo in un sistema senza clock globale.

---

## Temi principali

*Le pagine verranno create con i primi ingest. I link seguenti saranno attivi man mano che il wiki cresce.*

| Tema | Concetti chiave | Stato |
|------|----------------|-------|
| Introduzione e motivazioni | [[sistema-distribuito]] (4 definizioni: Tanenbaum, ingegnere, Coulouris, +Lamport/Van Roy), [[pervasivita-computazione-interazione]], [[motivazioni-sistemi-distribuiti]] (8 ragioni + esempi), [[centralizzato-vs-distribuito]] | M0 ingestato; definizioni arricchite in M4 e **preliminaries (M2-Ciatto)** |
| **Ingegneria dei SD (Module 2 — Ciatto)** | [[se-workflow-distribuito]], [[infrastruttura-e-componenti]], [[interaction-patterns]], [[protocolli-interazione-comuni]], [[architectural-styles|stili (vista pratica)]], [[features-design-distribuito]], [[socket]] | **preliminaries + communication_slides ingestati** (lente ingegneristica: come si *costruisce* un SD; socket TCP/UDP) |
| **Capstone Module 2: Distributed Pong** | [[distributed-pong]], [[infrastrutture-sistema-distribuito]], [[distributed-pong-qa]] | **pong_slides ingestato** — running example che unisce workflow+infrastruttura+event-based+UDP+CAP (il "blockchain" del Module 2) |
| Fondamenti e ontologia | [[computer-science-foundations]], [[computazione]], [[processo-computazionale-contesto]], [[sistema-computazionale]], [[parallelo-concorrente-distribuito]] | M2 ingestato |
| Definizioni, obiettivi e sfide | [[sistema-distribuito]] (3 definizioni), [[obiettivi-sistemi-distribuiti]], [[trasparenza]], [[openness-sistemi-distribuiti]], [[scalabilita]], [[situatedness]], [[pitfalls-sistemi-distribuiti]] | M4 ingestato |
| Classi (sorts) di sistemi distribuiti | [[sorts-sistemi-distribuiti]], [[distributed-computing-systems]], [[cluster-vs-grid]], [[distributed-information-systems]], [[distributed-pervasive-systems]], [[cloud-native-cncf]] | M5 ingestato |
| Comunicazione e middleware | [[socket]] (TCP/UDP, endpoint, full-duplex), [[stream-vs-datagram-sockets]], [[udp-group-chat]], [[tcp-echo-deadlock]]; [[middleware]] (interoperabilità, integrazione, standard); RPC, message passing | **communication_slides (M2) ingestato** (socket di basso livello); C4 (middleware); RPC da approfondire |
| Dependability e tolleranza ai guasti | [[dependability]], [[fault-error-failure]], [[classificazione-guasti]], [[modelli-di-fallimento]], [[reliability]], [[mezzi-dependability]], [[availability-vs-reliability]] | M1 ingestato |
| Recovery: checkpointing & logging | [[checkpointing-e-logging]], [[global-state-consistency]], [[causalita]], [[tamir-sequin-checkpointing]], [[chandy-lamport-snapshot]], [[protocolli-recovery]] | C2 ingestato |
| Tempo e causalità | [[tempo-nei-sistemi-distribuiti]], [[computing-needs-time]], [[ordinamento-parziale-eventi]], [[distribuzione-spaziale-temporale]], [[causalita]], [[happened-before]], [[logical-clock]], [[lamport-scalar-clock]], [[vector-clock]], [[physical-clock-synchronization]], [[scalar-vs-vector-clock]], [[physical-vs-logical-time]] | M0/M2/C2 + **C5 (clock logici)** + **M6 (clock fisici, CPS)** ingestati |
| Spazio e spatial computing | [[computing-with-space]], [[space-in-math-logic]], [[physical-space-computational-systems]], [[spatial-computing-applications]], [[spatial-computing]], [[tempo-vs-spazio-nel-computing]] | M7 ingestato |
| Mobilità del codice | [[code-mobility]], [[weak-vs-strong-mobility]], [[migrazione-risorse]] | C6 ingestato |
| Modellazione: architetture | [[software-architecture]], [[componenti-connettori]], [[architectural-styles]], [[software-vs-system-architecture]] | M8 ingestato |
| Modellazione: formalismi (comportamento) | [[process-algebra]], [[algebra-processi-operatori-leggi]], [[architetture-vs-process-algebra]] | M9 ingestato |
| Coordinazione distribuita | [[coordinazione]] (il problema della coordinazione); mutex (Token Ring, Ricart-Agrawala…), leader election (Bully, Ring…) | M6 (intro/problema); algoritmi da ingestare |
| Coerenza e replicazione | [[teorema-cap]], [[consistency]], [[availability]], [[partition-tolerance]], [[acid-vs-base]], [[replicazione]], [[consistency-model]], [[continuous-consistency]], [[sequential-consistency]], [[causal-consistency]], [[eventual-consistency]], [[client-centric-consistency]], [[replica-management]], [[data-centric-vs-client-centric]] | C1, M3 ingestati |
| Transazioni distribuite | [[transazioni-distribuite]] (ACID, nested, TP monitor), [[acid-vs-base]]; 2PC ([[tamir-sequin-checkpointing]]), 3PC | C1+M5 ingestati; 2PC/3PC in dettaglio da approfondire |
| Consensus | [[agreement-consensus]], [[consensus-problemi]], [[modello-sincrono-asincrono]], [[flp-impossibility]], [[paxos]], [[state-machine-replication]], [[cap-vs-flp]]; Raft/BFT da approfondire | C3 ingestato |
| Orchestrazione e produzione | [[containerization]], [[kubernetes]], [[kubernetes-architettura]], [[kubernetes-objects]], [[quality-attributes]], [[observability-monitoring]], [[docker-swarm-vs-kubernetes]], [[teoria-vs-kubernetes]], [[kubernetes-hpa-example]] | CX ingestato (guest lecture) |
| DLT, Blockchain & Middleware | [[middleware]], [[distributed-ledger-technology]], [[blockchain]], [[strumenti-crittografici-blockchain]], [[notary-service]], [[byzantine-fault-tolerance]], [[pbft]], [[blockchain-world-state]], [[blockchain-transactions]], [[blocks-e-block-chain]], [[consensus-mining]], [[proof-of-work]], [[sybil-attack]], [[smart-contract]], [[universal-smr]], [[solidity-counter]], [[permissioned-vs-permissionless]], [[blockchain-vs-teoria-del-corso]] | C4 ingestato (case study capstone) |
| Sistemi reali | ZooKeeper, Kafka, Cassandra; Bitcoin/Ethereum/Hyperledger (→ C4); Beowulf, grid, BAN, sensor networks, [[cloud-native-cncf|CNCF/cloud native]] (→ M5) | parz. (BCT in C4; classi+cloud native in M5); resto da ingestare |

---

## Tensioni fondamentali del campo

Tre trade-off ricorrono in quasi ogni argomento del corso:

- **Disponibilità vs Consistenza** ([[teorema-cap]]): non si può avere tutto in presenza di partizioni di rete
- **Safety vs Liveness nel consenso** ([[flp-impossibility]], [[cap-vs-flp]]): nel modello asincrono il consenso può preservare agreement/validity ma **non** garantire la terminazione → si rilassa la sincronia ([[paxos]])
- **Semplicità vs Tolleranza ai guasti**: i protocolli semplici assumono che i nodi non falliscano; la realtà è diversa (→ [[dependability]])
- **Availability istantanea vs Reliability nel tempo**: due facce dell'affidabilità che si possono dissociare (→ [[availability-vs-reliability]])
- **Prestazioni vs Correttezza**: sincronizzazione forte garantisce correttezza ma riduce il parallelismo
- **Replicazione vs Consistenza** ([[replicazione]]): replicare migliora scaling/performance, ma tenere le copie consistenti richiede sincronizzazione globale costosa — *is the cure worse than the disease?* La via d'uscita è scegliere un [[consistency-model]] adeguato ([[data-centric-vs-client-centric]])
- **Apertura vs Throughput/Consistenza** ([[permissioned-vs-permissionless]]): nelle [[blockchain]] il consenso *permissioned* (BFT, [[pbft]]) dà alta velocità e consistenza "esatta" ma richiede identità certificate; quello *permissionless* ([[proof-of-work]]) è aperto e Sybil-resistente ma lento e solo *eventually consistent* — è il [[teorema-cap|CAP]] e i [[byzantine-fault-tolerance|limiti bizantini]] (`f<N/3`) declinati sulla DLT
- **Fiducia centralizzata vs decentralizzata** ([[notary-service]], [[centralizzato-vs-distribuito]]): rimuovere il *single point of trust* (non solo il single point of failure) ha un costo — replicazione + consenso bizantino, ossia la [[distributed-ledger-technology|DLT come middleware]]
- **Trasparenza vs Consapevolezza del contesto** ([[trasparenza]], [[situatedness]]): nascondere la distribuzione alza l'astrazione, ma a volte la *location/context-awareness è una feature* (performance, situatedness) → ogni ingegnere sceglie il **grado** giusto di trasparenza
