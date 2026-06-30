---
title: "Fault, Error, Failure (Catena delle Minacce)"
course: Sistemi Distribuiti
category: concept
topics: [dependability, fault, error, failure, chain-of-threats]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Fault, Error, Failure (Catena delle Minacce)

## Definizione

Il modello causale fondamentale della [[dependability]] [Tanenbaum & van Steen, 2017]:

- **Failure** (fallimento) — un sistema *fallisce* quando **non si comporta come promesso**, cioè non è conforme alla sua *functional specification* (il servizio fornito all'interfaccia devia dalla specifica).
- **Error** (errore) — una *parte dello stato* del sistema che **potrebbe aver causato** un failure (stato con valori sbagliati).
- **Fault** (guasto) — la **causa** di un error.

Sintesi della catena causale: **fault → error → failure**.

## La dinamica: dormiente → attivato → propagato

1. Un fault può essere **dormiente** (*dormant*): non si manifesta finché non si verifica una condizione specifica.
   - es. un bug nel codice non eseguito; una variabile condivisa non protetta da lock finché due thread non la aggiornano insieme.
2. Quando la condizione è soddisfatta, il fault viene **attivato** (*activated*) → causa un **error** nel componente.
3. Quando il componente interagisce con altri, l'error si **propaga** (*propagates*) nel sistema.
4. Quando l'error raggiunge l'**interfaccia** e fa deviare il servizio dalla specifica → si verifica un **service failure**.

## Chain of threats (natura ricorsiva)

Per la natura **ricorsiva** della composizione dei sistemi, il *failure* di un sistema può diventare un *fault* in un sistema più grande di cui il primo è componente:

```
[Componente]  fault → error → failure
                                  │ (Activation)
[Sistema più grande]            fault → error → failure
                                                   │ (Propagation → Causation)
```

Questa relazione si chiama **chain of threats**. Per questo i termini "fault" e "failure" sono spesso usati in modo intercambiabile in letteratura — impropriamente.

> Collegamento con M0: che cosa sia "componente" vs "sistema" dipende dal **livello di astrazione** scelto (→ [[stato-sistema-distribuito]]), proprio come il confine sistema/ambiente è "spazialmente sparso" in [[distribuzione-spaziale-temporale]].

## Perché è importante

È il vocabolario di base di tutto il modulo: i diversi **tipi di guasto** (→ [[classificazione-guasti]]) e **modelli di fallimento** (→ [[modelli-di-fallimento]]) sono raffinamenti di questo schema, e i **mezzi** per la dependability (→ [[mezzi-dependability]]) intervengono a diversi punti della catena (evitare il fault, rilevarlo/diagnosticarlo, rimuoverlo, tollerarlo).

## Connessioni

- [[dependability]] — il concetto ombrello
- [[classificazione-guasti]] — come si classificano i fault
- [[modelli-di-fallimento]] — come si classificano i failure
- [[mezzi-dependability]] — dove si interviene sulla catena

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 17–19)
