---
title: "Tempo Fisico vs Tempo Logico"
course: Sistemi Distribuiti
category: bridge
topics: [physical time, logical time, sincronizzazione, UTC, causalità]
difficulty: intermedio
sources: [M6-Computing-with-Time.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Tempo Fisico vs Tempo Logico

Le due risposte all'[[tempo-nei-sistemi-distribuiti|issue of time]] nei sistemi distribuiti: sincronizzare gli **orologi reali** oppure rinunciare al tempo "vero" e catturare solo l'**ordine** degli eventi.

## 1. Confronto

| Caratteristica | **[[physical-clock-synchronization\|Tempo fisico]]** | **[[logical-clock\|Tempo logico]]** |
|---|---|---|
| Ancorato a | un orologio reale (oscillatore al quarzo) | gli **eventi** e la loro [[causalita\|causalità]] |
| Obiettivo | nozione comune di tempo **reale** | nozione comune di **ordine** |
| Riferimento | standard esterno (**UTC**/BIPM) o tempo comune | la relazione [[happened-before\|happens-before]] `→` |
| Tecniche | NTP, Berkeley Algorithm, RBS | [[lamport-scalar-clock\|scalar clock]], [[vector-clock\|vector clock]] |
| Problemi intrinseci | drift/skew/offset; clock solo in avanti; server può mancare | non dà il "wall-clock time"; n noto (vector) |
| Cosa misura | *quando* (istante/intervallo assoluto) | *prima/dopo/concorrente* (ordine relativo) |
| Causalità esterna | la **approssima** (unico modo) | **non** la cattura (solo l'interna) |

## 2. Il nodo concettuale

> Spesso **ciò che conta non è l'istante esatto** in cui gli eventi occorrono, **ma l'ordine** in cui occorrono [Lamport, 1978]. Inoltre: *se due processi non interagiscono, non serve sincronizzarli* — la mancanza di sincronizzazione **non sarebbe osservabile**. Esempi in cui basta l'ordine: **UNIX `make`**, **Gradle** (ricompilare in base a "più recente di").

Inoltre la **causalità esterna** (relazioni causa-effetto nel mondo fisico, fuori dal sistema) **non è rilevabile** dal sistema distribuito e può essere solo **approssimata dal tempo fisico** ([[happened-before]], C5) — è uno dei pochi casi in cui il tempo fisico aggiunge qualcosa che il logico non ha.

## 3. Absolute vs relative (dentro il tempo fisico)

Anche il tempo **fisico** si sdoppia:
- **global absolute time** — legato al tempo reale (UTC via NTP);
- **global relative time** — solo un tempo comune condiviso, anche slegato dal reale (Berkeley, RBS).

→ gradiente: `assoluto (UTC) ⟶ relativo (Berkeley/RBS) ⟶ logico (Lamport) ⟶ causale (vector clock)`, da "ancorato al reale" a "puramente relazionale".

## 4. Quando scegliere cosa

- **Tempo fisico assoluto (UTC/NTP):** servono timestamp reali, intervalli, audit, scadenze, correlazione con il mondo esterno; CPS e [[computing-needs-time|sistemi cyber-fisici]].
- **Tempo fisico relativo (Berkeley/RBS):** basta un tempo comune (es. reti di sensori [[distributed-pervasive-systems|wireless]]) senza accesso stabile a UTC.
- **Tempo logico (Lamport/vector):** serve solo **ordinare** eventi / rilevare causalità e concorrenza (build system, [[causal-consistency|causal consistency]], snapshot, mutua esclusione).

## 5. Connessioni

- [[tempo-nei-sistemi-distribuiti]] — l'hub che biforca nelle due famiglie.
- [[physical-clock-synchronization]] · [[logical-clock]] — i due rami in dettaglio.
- [[scalar-vs-vector-clock]] (C5) — il confronto interno al tempo logico.
- [[modello-sincrono-asincrono]] (C3) — sincronia/asincronia come modello di sistema.

## 6. Sorgenti

- `raw-sources/M6-Computing-with-Time.pdf` (Physical vs Logical Time; Absolute vs Relative), slide 35–41.
- Riferimenti: Lamport 1978; Kshemkalyani & Singhal 2011.
