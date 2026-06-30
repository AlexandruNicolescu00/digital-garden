---
title: "Logical Clock & Logical Time"
course: Sistemi Distribuiti
category: concept
topics: [tempo logico, clock consistency, strong consistency, monotonicità]
difficulty: avanzato
sources: [C5-Logical-Clocks.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Logical Clock & Logical Time

## 1. Perché il tempo logico

Nei sistemi distribuiti il **tempo** potrebbe fungere da **approssimazione della causalità** (qualcosa che accade prima *potrebbe* causare qualcosa che accade dopo). Ma così:
- si **perde la connessione esplicita** tra eventi (il **messaggio**);
- una nozione di **tempo comune** in un sistema distribuito **potrebbe non esistere affatto** (niente clock globale).

> ⭐ Serve una **nozione logica di tempo**, ancorata alla relazione [[happened-before|happens-before]] anziché all'orologio fisico.

## 2. Definizione

Un **time value** per un evento `a` è un valore `C(a)` su cui **ogni processo concorda** — il valore di un **logical clock**. Formalmente, se `H` è il dominio degli eventi e `T` quello del tempo (logico):

> **Logical clock** = una funzione `C : H → T` che soddisfa la **proprietà di monotonicità**:
>
> **`∀ a, b ∈ H :  a → b  ⟹  C(a) < C(b)`**   *(clock consistency condition)*

Quando `T` e `C` soddisfano **anche** la condizione inversa:

> **`∀ a, b ∈ H :  a → b  ⟺  C(a) < C(b)`**
>
> il sistema di clock si dice **strongly consistent** (fortemente consistente).

| Proprietà | Formula | Significato |
|-----------|---------|-------------|
| **Clock consistency** | `a→b ⟹ C(a)<C(b)` | il tempo rispetta la causalità (ma non viceversa) |
| **Strong consistency** | `a→b ⟺ C(a)<C(b)` | dal confronto dei tempi si **ricostruisce** la causalità |

> 🔑 Gli [[lamport-scalar-clock|scalar clock di Lamport]] garantiscono solo la **clock consistency**; i [[vector-clock|vector clock]] garantiscono la **strong consistency**.

## 3. Come si implementa

Implementare i logical clock richiede di affrontare **due problemi**:
1. **strutture dati locali** a ogni processo per rappresentare il tempo logico;
2. un **protocollo condiviso** per aggiornarle garantendo la consistenza.

Concretamente:
- ogni processo `pᵢ` mantiene un **local clock** (il proprio progresso nel tempo) e un **global clock** (la sua vista del tempo globale);
- **due regole** definiscono come si aggiornano local e global clock.

> I diversi **sistemi di logical clock** [Kshemkalyani & Singhal, 2011] differiscono per **rappresentazione** del tempo logico e per **protocollo** di aggiornamento, ma tutti implementano queste due regole e garantiscono la monotonicità legata alla causalità; ciascuno offre poi **proprietà specifiche** (es. *scalar time*, *vector time*, *matrix time*).

## 4. Connessioni

- [[happened-before]] — la relazione `→` che i logical clock devono rispettare.
- [[lamport-scalar-clock]] — l'implementazione scalare (clock consistency, non strong).
- [[vector-clock]] — l'implementazione vettoriale (strong consistency).
- [[ordinamento-parziale-eventi]] · [[causalita]] — l'ordine/causalità che il tempo logico codifica.
- [[modello-sincrono-asincrono]] (C3) — l'assenza di clock globale che motiva il tempo logico.
- [[scalar-vs-vector-clock]] — confronto delle due realizzazioni.
- [[tempo-nei-sistemi-distribuiti]] · [[physical-vs-logical-time]] (M6) — la cornice: tempo fisico vs logico; "conta l'ordine, non l'istante" (UNIX make/Gradle).
- [[physical-clock-synchronization]] (M6) — l'alternativa basata sugli orologi reali (UTC/NTP).

## 5. Sorgenti

- `raw-sources/C5-Logical-Clocks.pdf` (Logical Clocks — Time/Logical Time), slide 14–17.
- Riferimenti: Lamport 1978; Kshemkalyani & Singhal 2011.
