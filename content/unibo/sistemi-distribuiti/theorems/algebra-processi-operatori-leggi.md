---
title: "Process Algebra: Operatori, Leggi, Transizioni"
course: Sistemi Distribuiti
category: theorem
topics: [process-algebra, operatori, assiomi, SOS, transizioni, branching-time]
difficulty: avanzato
sources: [M9-Modelling-Distributed-Systems-Process-Algebra.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Process Algebra: Operatori, Leggi, Transizioni

Il nucleo formale della [[process-algebra]]. Una process algebra è una **struttura matematica con tre operazioni binarie che soddisfano un insieme di leggi (assiomi)**; i processi si costruiscono a partire da **azioni atomiche** (comportamento indivisibile) combinandole con gli operatori [Baeten, 2005].

## 1. Operatori di base

Tre operatori per combinare processi in sistemi:

| Operatore | Notazione | Significato |
|---|---|---|
| **alternative composition** | `x + y` | il processo esegue **o** `x` **o** `y` (scelta) |
| **sequential composition** | `x ; y` (anche `x · y`) | esegue **prima** `x`, **poi** `y` |
| **parallel composition** | `x ∥ y` | esegue `x` **e** `y` concorrentemente / in parallelo |

Precedenza tipica: da `+` (più debole) a `;`. Gli operatori possono avere **elementi neutri** (qui ignorati).

## 2. Leggi strutturali (gli assiomi)

Le **sette leggi di base** [Baeten, 2005]:

| # | Legge | Nome |
|---|---|---|
| 1 | `x + y = y + x` | commutatività di `+` |
| 2 | `x + (y + z) = (x + y) + z` | associatività di `+` |
| 3 | `x + x = x` | **idempotenza** di `+` |
| 4 | `(x + y) ; z = x ; z + y ; z` | distributività **destra** di `+` su `;` |
| 5 | `(x ; y) ; z = x ; (y ; z)` | associatività di `;` |
| 6 | `x ∥ y = y ∥ x` | commutatività di `∥` |
| 7 | `(x ∥ y) ∥ z = x ∥ (y ∥ z)` | associatività di `∥` |

> **Enunciato (laws as axioms):** *qualunque struttura matematica con **tre operazioni binarie** che **soddisfa le leggi precedenti** è una process algebra.* Tipicamente tali strutture si esprimono come **automi**, detti **transition systems**.

Si noti: **`;` non è commutativa** (l'ordine conta) e **manca la distributività sinistra** (`x ; (y + z)` ≠ in generale `x ; y + x ; z`) — è una **scelta**, vedi §4.

## 3. Transizioni (Structural Operational Semantics)

Le regole SOS [Plotkin, 1981] danno la semantica **operazionale** single-step: ogni azione atomica `a` è una costante che **esegue se stessa** e poi **termina con successo** (`✓`):

```
a --a--> ✓
```

**Alternative composition** — può procedere in entrambi i modi (la scelta si risolve eseguendo un ramo):
```
x --a--> ✓            x --a--> x'
─────────────        ──────────────       (simmetricamente per y)
x+y --a--> ✓         x+y --a--> x'
```

**Sequential composition** — procede in un solo modo (prima `x`, poi `y`):
```
x --a--> ✓            x --a--> x'
─────────────        ───────────────
x;y --a--> y         x;y --a--> x';y
```

**Parallel composition** — può procedere per **interleaving** (semplicemente uno dei due avanza)…
```
x --a--> x'              y --a--> y'
──────────────         ──────────────
x∥y --a--> x'∥y        x∥y --a--> x∥y'
```
…**oppure** con i due processi che **interagiscono** tramite una **funzione di comunicazione** `γ`: se `x` può fare `a` e `y` può fare `b`, e `γ(a,b)` è definita, allora `x∥y` esegue l'azione **sincronizzata** `γ(a,b)`. È il punto in cui la process algebra modella la **comunicazione/interazione** (e non solo la mera coesistenza).

## 4. Scegliere le leggi: branching time vs linear time

Le sette leggi possono essere **estese** con la **distributività sinistra** di `+` su `;`:
```
x ; (y + z) = x ; y + x ; z      (left distributivity)
```

Effetto della scelta:

| Variante | Leggi su `+`/`;` | Conseguenza |
|---|---|---|
| **linear time** | distributività **sia destra che sinistra** | il **momento della scelta non conta** |
| **branching time** | solo distributività **destra** | il **momento della scelta conta** |

> Intuizione: in *linear time* un sistema è caratterizzato dai suoi **possibili tracciati** (sequenze di azioni); in *branching time* conta **quando** (in quale stato) si effettua la scelta — due sistemi con gli stessi tracciati ma rami di scelta diversi sono **distinti**. È una decisione di modellazione fondamentale (lega l'idea di [[ordinamento-parziale-eventi]]: cosa si osserva di un comportamento).

## 5. Operational vs axiomatic: il punto di sintesi

Roughly speaking [Baeten, 2005]:

- le **leggi strutturali** sono gli **assiomi** che permettono di **provare proprietà** del sistema (lato **verifica**);
- le **regole di transizione** danno una rappresentazione **operazionale**, adatta come **riferimento per l'implementazione**;
- quando le transizioni sono definite in modo da **rispettare** le leggi strutturali, **in linea di principio** si ottiene un'**implementazione con proprietà formali** (provate).

→ È esattamente il ponte **specifica ↔ verifica ↔ implementazione** che rende la [[process-algebra]] utile nell'ingegneria dei SD.

## 6. Connessioni

- [[process-algebra]] — la pagina-quadro (motivazione, ingredienti, storia, uso, bilancio).
- [[architetture-vs-process-algebra]] — perché questo formalismo completa l'architettura software (M8).
- [[parallelo-concorrente-distribuito]] (M2) — la parallel composition `∥` e la concorrenza; interleaving vs true concurrency.
- [[flp-impossibility]] · [[teorema-cap]] — esempi di *proprietà dimostrate* per via formale: la process algebra è lo strumento generale per provare proprietà comportamentali.

## 7. Sorgenti

- `raw-sources/M9-Modelling-Distributed-Systems-Process-Algebra.pdf` (Operators, Laws, Transitions), slide 16–25.
- Riferimenti: [Baeten, 2005] *A brief history of process algebra*; [Plotkin, 1981] *A structural approach to operational semantics*.
