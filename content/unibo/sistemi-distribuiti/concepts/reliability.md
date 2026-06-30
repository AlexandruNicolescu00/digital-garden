---
title: "Reliability (Affidabilità)"
course: Sistemi Distribuiti
category: concept
topics: [dependability, reliability, MTTF, metriche]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Reliability (Affidabilità)

## Definizione

La **reliability** è la proprietà per cui un sistema **può funzionare con continuità senza guasti**. È una misura della capacità del sistema di fornire servizi corretti *con continuità per un periodo di tempo*.

Punto chiave: la reliability è definita **su un intervallo di tempo** Δt, a differenza dell'[[availability]] che è definita **su un istante**.

Un sistema *highly-reliable* è quello che molto probabilmente continuerà a funzionare per un lungo periodo.

## Definizione formale

$$\text{reliability} = R(\Delta t) = e^{-\lambda \Delta t}$$

dove **λ ∈ [0, ∞)** è il **failure rate** (tasso di guasto). Vale che λ è approssimativamente **proporzionale a 1/MTTF** (più basso il tasso di guasto, più alto il MTTF, più alta la reliability).

## ⚠️ Availability ≠ Reliability

Distinzione **critica per l'esame**:

> Un sistema che **fallisce di frequente ma si ripristina molto rapidamente** può avere **alta availability** e, allo stesso tempo, **bassissima reliability**.

- **Availability** = pronto in un *istante* (probabilità) → [[availability]]
- **Reliability** = funziona senza interruzioni su un *intervallo*

Confronto sistematico: → [[availability-vs-reliability]].

## Connessioni

- [[availability]] — l'attributo "gemello" da cui va distinta
- [[availability-vs-reliability]] — il confronto e le metriche affiancate
- [[dependability]] — l'attributo ombrello
- [[mezzi-dependability]] — tecniche per alta reliability (recovery-oriented, rollback)

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 34, 41)
