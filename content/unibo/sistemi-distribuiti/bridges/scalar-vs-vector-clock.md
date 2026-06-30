---
title: "Scalar Clock vs Vector Clock"
course: Sistemi Distribuiti
category: bridge
topics: [scalar clock, vector clock, strong consistency, causalità, total order]
difficulty: avanzato
sources: [C5-Logical-Clocks.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Scalar Clock vs Vector Clock

Le due realizzazioni del [[logical-clock|tempo logico]] viste in C5, ai due livelli di potere espressivo rispetto alla [[happened-before|causalità]].

## 1. Confronto

| Caratteristica | **[[lamport-scalar-clock\|Scalar (Lamport)]]** | **[[vector-clock\|Vector]]** |
|---|---|---|
| Rappresentazione | un **intero** per processo | **array di `n` interi** (n = #processi, noto) |
| Aggiornamento ricezione | `lc ← max(lc, lc_msg)` | `∀k: vc[k] ← max(vc[k], ts(m)[k])` |
| Clock consistency `a→b ⟹ C(a)<C(b)` | ✅ | ✅ |
| **Strong consistency** `a→b ⟺ C(a)<C(b)` | ❌ (squashing local/global) | ✅ |
| Rileva la **concorrenza** `a ∥ b`? | ❌ (non distingue) | ✅ (vettori **non confrontabili**) |
| Ordine indotto | **totale** (con tie-break su PID) | **parziale** (per costruzione) |
| Costo per messaggio | O(1) | O(n) |
| Proprietà extra | event counting / **height** (se d=1) | causal histories compatte; "chain of events" |

## 2. Il nodo concettuale

> **Scalar:** `C(a) < C(b)` può valere **anche** quando `a ∥ b` → dal confronto **non** si ricostruisce la causalità. Adatto a stabilire un **ordine totale** (per liveness, event queue, mutua esclusione), assumendo che gli eventi concorrenti siano indipendenti.
>
> **Vector:** `vc(a) < vc(b) ⟺ a → b` → il confronto **ricostruisce esattamente** la causalità, e l'**incomparabilità** segnala la concorrenza. Costa di più (O(n)) ma è **strongly consistent**.

`scalar ⟶ (recupera causalità) ⟶ vector ⟶ (recupera ulteriore info) ⟶ matrix clock`

## 3. Quando usare cosa

- **Scalar (Lamport):** serve un **ordine totale** economico e non importa distinguere causale da concorrente (es. mutua esclusione distribuita, ordinamento di richieste, code di eventi).
- **Vector:** serve **rilevare la causalità/concorrenza** (es. [[causal-consistency|causal consistency]], causal delivery/multicast, riconciliazione di repliche, debugging di dipendenze, snapshot causali).

## 4. Connessioni

- [[logical-clock]] — clock consistency vs strong consistency: la distinzione che separa i due.
- [[lamport-scalar-clock]] · [[vector-clock]] — le due pagine di dettaglio.
- [[happened-before]] — la relazione `→`/`∥` che entrambi tentano di catturare.
- [[causal-consistency]] (M3) — applicazione tipica dei vector clock.
- [[sequential-consistency]] (M3) — applicazione tipica del total ordering scalare.

## 5. Sorgenti

- `raw-sources/C5-Logical-Clocks.pdf` (Scalar "No strong consistency" → Vector Clocks "Ordering"), slide 26, 28–32.
