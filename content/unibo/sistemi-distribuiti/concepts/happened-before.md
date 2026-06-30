---
title: "Relazione Happens-Before"
course: Sistemi Distribuiti
category: concept
topics: [happens-before, causalità, ordine parziale, concorrenza, eventi]
difficulty: avanzato
sources: [C5-Logical-Clocks.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Relazione Happens-Before

## 1. Azioni, eventi, cause

Qualunque sistema computazionale può essere modellato come una **sequenza di azioni** [Baquero & Preguiça, 2016], dove un'azione è un **cambiamento di stato** del sistema (o del mondo): leggere dalla memoria, scrivere un file, accendere una lampada. In un sistema distribuito le azioni avvengono in **luoghi multipli** e si rappresentano come **eventi** (inviare/ricevere messaggi, modificare un record). La distribuzione fisica varia moltissimo (stessa macchina ↔ scala globale).

> **Non tutti gli eventi sono in relazione**, ma alcuni eventi **causano / influenzano** come altri (successivi) accadono. Rilevare le potenziali relazioni di **causa-effetto** tra eventi è **fondamentale** per progettare algoritmi distribuiti: consistenza di DB replicati, garbage collection, costruzione di checkpoint, analisi della concorrenza.

### Causalità interna vs esterna
- **Internal causality** — le relazioni causa-effetto **percepibili dentro** il sistema distribuito;
- **External causality** — le relazioni causa-effetto nel **mondo fisico esterno**. Esempio: Alice prenota cena per due e lo dice a Bob, poi Bob prenota due biglietti per il cinema; anche se i due sistemi di prenotazione fossero lo stesso (multiplex), **non sanno nulla** della relazione esterna "Alice lo dice a Bob".

> ⚠️ La causalità **esterna** in generale **non è rilevabile** dal sistema distribuito; può solo essere **approssimata dal tempo fisico** (se si trascura la conoscenza di dominio). → è il motivo per cui serve una nozione di **[[logical-clock|tempo logico]]**.

## 2. Definizione: happens-before `→` [Lamport, 1978]

Dati eventi generici `a, b`, la scrittura **`a → b`** ("a happens before b") significa che **tutti i processi concordano** che `a` accade prima, poi accade `b`. Si osserva **direttamente** in due situazioni:

1. **Stesso processo:** se `a` e `b` sono eventi dello stesso processo e `a` precede `b` localmente → `a → b` *(gli eventi locali sono ordinati dal tempo locale)*;
2. **Messaggio:** se `a` è l'evento di **invio** di un messaggio da un processo e `b` è l'evento di **ricezione** in un altro → `a → b` *(un messaggio impiega un tempo finito, positivo, non-nullo a propagarsi dal mittente al ricevente)*.

**Transitività:** `a → b` e `b → c` implicano `a → c`.

> **happens-before definisce un ordinamento parziale** sugli eventi del sistema distribuito → è la **formalizzazione** dell'[[ordinamento-parziale-eventi]] (M0) e della [[causalita]] (C2).

### Concorrenza `∥`
Quando **né** `a → b` **né** `b → a` sono osservabili, nulla si può dire sul loro ordinamento: `a` e `b` sono **concorrenti**, scritto **`a ∥ b`**. → è la *true concurrency* di [[parallelo-concorrente-distribuito]] (M2).

## 3. Causal paths & causal histories

- **Causal path:** un modo semplice per verificare se `c` può aver **causato** `e` è trovare almeno **un cammino diretto** da `c` a `e` basato sulla relazione happens-before (transitiva). Se esiste, `c` è una **possibile causa** di `e`: `c ↝ e`.
- **Causal history** [Baquero & Preguiça, 2016]: l'insieme degli eventi che precedono causalmente un dato evento. Ogni nodo accumula l'insieme degli eventi (propri + appresi via messaggi). Esempio: `{a1, a2, b1, b2, b3}` è la storia causale dell'evento `b3`. → i [[vector-clock|vector clock]] sono una codifica **compatta** delle causal histories.

> Esempio dalle slide: `a1 → a2`, `a1 → b2`, `b2 → c3`, `a1 → c3`; invece `a1 ∥ c2` (concorrenti: nessun causal path in nessuna direzione).

## 4. Connessioni

- [[ordinamento-parziale-eventi]] (M0) — happens-before **è** quell'ordine parziale, ora formalizzato.
- [[causalita]] (C2) — la relazione causa-effetto di cui happens-before è la cattura formale.
- [[logical-clock]] — come si **assegnano valori temporali** coerenti con happens-before.
- [[lamport-scalar-clock]] · [[vector-clock]] — gli algoritmi che la implementano.
- [[parallelo-concorrente-distribuito]] (M2) — la concorrenza `∥` come true concurrency.
- [[causal-consistency]] (M3) — l'ordine sulle write causalmente legate poggia su happens-before.
- [[chandy-lamport-snapshot]] (C2) — lo snapshot consistente rispetta happens-before (Lamport coautore).

## 5. Sorgenti

- `raw-sources/C5-Logical-Clocks.pdf` (Actions, Events, Causes; Logical Clocks — happens-before), slide 4–13.
- Riferimenti: Lamport 1978; Baquero & Preguiça 2016.
