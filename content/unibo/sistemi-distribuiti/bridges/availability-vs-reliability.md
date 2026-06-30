---
title: "Availability vs Reliability"
course: Sistemi Distribuiti
category: bridge
topics: [dependability, availability, reliability, metriche, MTTF]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Availability vs Reliability

Due attributi della [[dependability]] spesso confusi, ma **distinti**. Distinzione classica d'esame.

## La differenza in una riga

- **[[availability]]** — il sistema è pronto **in un dato istante** (probabilità puntuale).
- **[[reliability]]** — il sistema funziona **con continuità su un intervallo** di tempo Δt.

## L'esempio che chiarisce tutto

> Un sistema che **fallisce di frequente ma si ripristina velocissimamente** ha **alta availability** ma **bassa reliability**.

Ogni guasto dura pochissimo → in un istante casuale è quasi sempre "su" (availability alta). Ma non riesce a restare su con continuità per un intervallo lungo (reliability bassa).

## Le metriche affiancate

Fattori temporali:
- **MTTF** (Mean Time To Failure) — tempo medio fino al guasto
- **MTTR** (Mean Time To Repair) — tempo medio di riparazione
- **MTBF** (Mean Time Between Failures) — `MTBF = MTTF + MTTR`

| | Availability | Reliability |
|--|--------------|-------------|
| Definita su | un **istante** | un **intervallo** Δt |
| Formula | `A = MTTF / (MTTF + MTTR) = MTTF / MTBF` | `R(Δt) = e^(−λΔt)`, λ = failure rate |
| Migliora se | ↓ MTTR (ripari in fretta) **o** ↑ MTTF | ↓ λ ovvero ↑ MTTF |
| Notazione comune | "numero di 9" (es. five 9s = 99.999% ⇒ ≤ 5.256 min downtime/anno) | probabilità di sopravvivenza su Δt |

Nota: l'availability migliora **anche** riducendo l'MTTR; la reliability **no** — a lei interessa solo non guastarsi (λ basso). Per questo si possono dissociare.

## Collegamento con maintainability

La **maintainability** (facilità di riparazione) è strettamente legata all'availability: riparare in fretta abbassa l'MTTR e quindi alza A. Un sistema molto manutenibile tende ad essere molto disponibile.

## ⚠️ Nota cross-modulo: due sensi di "availability"

Attenzione: "availability" ha **due accezioni** nel corso, da non confondere:
1. **Senso CAP** ([[teorema-cap]]): proprietà *qualitativa* di liveness — *ogni richiesta a un nodo non-failing ottiene una risposta*.
2. **Senso dependability** (qui): metrica *quantitativa* — *probabilità che il sistema sia operativo in un istante* = MTTF/MTBF.

Sono correlate (entrambe "il sistema risponde quando serve") ma definite e misurate diversamente. Vedi [[availability]].

## Connessioni

- [[availability]] · [[reliability]] — i due concetti
- [[dependability]] — l'ombrello che li contiene
- [[teorema-cap]] — l'altro senso di "availability"
- [[mezzi-dependability]] — tecniche diverse per target diversi (es. checkpointing per availability)

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 33–41)
