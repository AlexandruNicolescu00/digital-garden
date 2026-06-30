---
title: "Tassonomia dei Protocolli di Recovery"
course: Sistemi Distribuiti
category: bridge
topics: [recovery, checkpointing, logging, taxonomy, trade-off]
difficulty: avanzato
sources: [C2-Logging-Checkpointing.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Tassonomia dei Protocolli di Recovery

Panoramica e confronto di tutti i protocolli di recovery di C2 (→ [[checkpointing-e-logging]]).

## Il bivio fondamentale: stato vs eventi

|                | Checkpoint-based                                  | Log-based                                          |
| -------------- | ------------------------------------------------- | -------------------------------------------------- |
| Salva          | lo **stato**                                      | gli **eventi** (nondeterministici)                 |
| Recovery       | all'ultimo checkpoint consistente                 | fino al **punto esatto** prima del guasto (replay) |
| Assunzione PWD | **non** richiesta                                 | **richiesta** ([[checkpointing-e-logging]])        |
| Costo          | più semplice; **perdita di esecuzione** tollerata | più complesso; recovery più preciso                |

In pratica si **combinano** (checkpoint + log tra un checkpoint e il successivo): limita tempo di recovery e dimensione del log (GC).

## Checkpoint-based: uncoordinated vs coordinated

- **Uncoordinated checkpointing**: ogni processo decide **autonomamente** quando fare checkpoint. Problema: i checkpoint potrebbero **non** formare uno stato globale consistente; rischio di **domino effect**. Serve tracciare e registrare le **dipendenze** tra checkpoint → overhead e complessità.
- **Coordinated checkpointing**: i processi si coordinano per garantire la consistenza. Due protocolli classici:

| | [[tamir-sequin-checkpointing]] | [[chandy-lamport-snapshot]] |
|--|-------------------------------|------------------------------|
| Blocking? | **Blocking** (sospende l'esecuzione) | **Nonblocking** |
| Coordinazione | coordinator + participants, **2PC** | iniziatore autonomo, **Marker** |
| Atomicità del round | **sì** (atomico o abort) | **no** (nessun meccanismo di fine/switch) |
| Robustezza | più completo e robusto, conservativo | più leggero, promuove autonomia |
| Channel state | catturato | catturato (stesso meccanismo) |
| Overhead comunicazione | identico | identico |

## Log-based: pessimistic / optimistic / causal [Alvisi & Marzullo, 1998]

L'esecuzione è una sequenza di **state interval** tra eventi nondeterministici; loggando l'evento iniziale, l'intervallo si **replaya**.

| Tipo | Come logga | Trade-off |
|------|-----------|-----------|
| **Pessimistic** | messaggio loggato **sincronamente** *prima* di eseguirlo | semplice, recovery veloce; più latenza a runtime |
| **Optimistic** | eventi prima in **memoria volatile**, poi loggati **asincronamente** su stable storage | meno latenza; un guasto può **perdere** messaggi → rollback più indietro |
| **Causal** | gli eventi non ancora loggati sono **piggybacked** su ogni messaggio inviato | accesso a tutti gli eventi causalmente rilevanti → recovery consistente, ma complesso |

**Long story short**: optimistic e causal devono **tracciare le dipendenze** dei processi e piggybackare informazione → recovery più sofisticata/costosa e possibili **cascading recovery**. Il **pessimistic** è il più semplice e con recovery più veloce.

> 🔗 Il **causal logging** è un altro punto in cui la [[causalita]] entra operativamente (dipendenze causali piggybacked) — preludio ai clock vettoriali.

## Sintesi decisionale

- Vuoi semplicità e tolleri perdita di esecuzione → **checkpoint-based** (coordinato se serve consistenza garantita).
- Vuoi recuperare fino all'istante del guasto e vale la PWD → **log-based** (pessimistic se vuoi recovery veloce).
- Non puoi bloccare il sistema → **Chandy-Lamport**; vuoi atomicità/robustezza → **Tamir-Sequin**.

## Connessioni

- [[checkpointing-e-logging]] — le tecniche di base
- [[tamir-sequin-checkpointing]] · [[chandy-lamport-snapshot]] — i due protocolli coordinati
- [[global-state-consistency]] — perché serve la consistenza + channel state
- [[causalita]] — dipendenze causali nel causal logging
- [[mezzi-dependability]] — il quadro M1 (fault tolerance, ridondanza)

## Sorgenti

- `raw-sources/C2-Logging-Checkpointing.pdf` (slides 32–33, 45–51)
- [Alvisi and Marzullo, 1998] *Message logging: pessimistic, optimistic, causal, and optimal*.
