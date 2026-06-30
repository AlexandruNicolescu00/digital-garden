---
title: "Algoritmo di Lamport (Scalar Clock)"
course: Sistemi Distribuiti
category: theorem
topics: [Lamport, scalar clock, tempo logico, total ordering, event counting]
difficulty: avanzato
sources: [C5-Logical-Clocks.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Algoritmo di Lamport (Scalar Clock)

## 1. Enunciato / scopo

L'**algoritmo di Lamport** [Lamport, 1978] implementa un [[logical-clock|logical clock]] **scalare**: assegna a ogni evento un singolo intero `C(a)` che rispetta la [[happened-before|happens-before]] (`a → b ⟹ C(a) < C(b)`, **clock consistency**). È il più semplice sistema di tempo logico.

## 2. Algoritmo

Ogni processo `pᵢ` mantiene un **local clock** `lcᵢ` e un **incremento** `d`. Tre regole:

1. **Prima di ogni evento locale**, `pᵢ` incrementa il clock:
   ```
   lcᵢ ← lcᵢ + d        (d può valere 1)
   ```
2. **All'invio** di un messaggio `m` verso `pⱼ`, `pᵢ` **agguanta** (piggyback) `m` con `lcᵢ` **dopo** averlo aggiornato;
3. **Alla ricezione** di `m`, `pⱼ` aggiusta il proprio contatore per renderlo coerente col timestamp ricevuto:
   ```
   lcⱼ ← max(lcⱼ, lcᵢ)
   ```
   *(tipicamente seguito da +d per l'evento di ricezione).*

**Tempo globale:** `lcᵢ` è il tempo locale di `pᵢ`; per un evento `a` su `pᵢ`, `C(a) ≡ lcᵢ(a)` è il **tempo globale logico** del sistema.

> **Consistenza:** gli scalar clock sono **monotoni per costruzione**, quindi soddisfano `a → b ⟹ C(a) < C(b)`.

## 3. Proprietà notevoli [Kshemkalyani & Singhal, 2011]

### Total ordering
Gli scalar clock permettono un **ordinamento totale** degli eventi usando un **tie-break** per gli eventi con lo stesso timestamp (`C(a) = C(b)`): es. **identificatori di processo** linearmente ordinati.
> Assunzione: gli eventi **concorrenti sono indipendenti**, quindi ordinarli arbitrariamente **non viola** alcuna relazione causale. Le **proprietà di liveness** (event queue, mutua esclusione, ecc.) si appoggiano tipicamente al total ordering. → cfr. [[sequential-consistency]] (M3).

### Event counting (height)
Se l'incremento `d` è **sempre 1**, un evento con timestamp `h` ha la proprietà: `h − 1` = **durata logica minima** in numero di eventi, cioè il numero di eventi prodotti **sequenzialmente** prima di `a` (la **height** dell'evento), indipendentemente dai processi che li hanno prodotti — ovvero la lunghezza del **più lungo causal path** che termina in `a`.

### ⚠️ No strong consistency
> Il sistema di scalar clock **non è strongly consistent**:
> ```
> C(a) < C(b)  ⇏  a → b
> ```

Cioè da `C(a) < C(b)` **non** si può dedurre che `a` ha causato `b`: i time value possono risultare **totalmente ordinati anche quando gli eventi non lo sono** (concorrenti). Causa: il local clock e il global clock di un processo sono **schiacciati in uno solo** (*squashed*), perdendo informazione sulle dipendenze causali tra eventi di processi diversi.
> Esempio: quando `p₂` riceve il primo messaggio da `p₁` e aggiorna il clock a 3, **dimentica** che la dipendenza più recente da `p₁` era al timestamp 2.

→ il problema è risolto dai [[vector-clock|vector clock]].

## 4. Connessioni

- [[logical-clock]] — il framework generale che questo algoritmo realizza (clock consistency).
- [[happened-before]] — la relazione che i timestamp rispettano.
- [[vector-clock]] — l'estensione che recupera la **strong consistency**.
- [[scalar-vs-vector-clock]] — confronto diretto.
- [[sequential-consistency]] (M3) — il total ordering concordato sulle operazioni.
- [[chandy-lamport-snapshot]] · [[causalita]] (C2) — Lamport e la causalità nei sistemi distribuiti.

## 5. Sorgenti

- `raw-sources/C5-Logical-Clocks.pdf` (Logical Clocks — Lamport's Algorithm: Scalar Time I–IX), slide 18–26.
- Riferimenti: Lamport 1978; Kshemkalyani & Singhal 2011; Tanenbaum & van Steen 2017.
