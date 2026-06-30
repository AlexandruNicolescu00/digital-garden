---
title: "Tempo vs Spazio nel Computing (M6 vs M7)"
course: Sistemi Distribuiti
category: bridge
topics: [tempo, spazio, computing, situatedness, M6, M7]
difficulty: intermedio
sources: [M7-Computing-with-Space.pdf, M6-Computing-with-Time.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Tempo vs Spazio nel Computing (M6 vs M7)

I due moduli gemelli — **[[computing-needs-time|Computing with Time]]** (M6) e **[[computing-with-space|Computing with Space]]** (M7) — affrontano le **due dimensioni** del contesto di un processo computazionale ([[processo-computazionale-contesto]], M2): il **tempo** e lo **spazio**. Hanno una struttura parallela e si chiudono entrambi sul tema della situatedness/coordinazione.

## 1. Il parallelo

| Aspetto | **Tempo (M6)** | **Spazio (M7)** |
|---|---|---|
| Domanda di base | cos'è il tempo nel computing? | cos'è lo spazio nel computing? |
| Astrazione tradizionale | il tempo è **astratto via** (Turing/von Neumann) | lo spazio è **astratto via** (function over data) |
| Quadro filosofico | relatività: nessun tempo unico (Rovelli) | geometrie non-euclidee: nessuno spazio "unico" (Riemann…) |
| Fondamento formale | clock, ordini, causalità | geometria, topologia, **logiche modali** (S4) |
| Tassonomia di sistema | parallelo/concorrente (contesto **temporale**) | distribuito (contesto **spaziale**) — [[parallelo-concorrente-distribuito]] |
| Nozione "fisica" | [[physical-clock-synchronization\|orologi fisici]] (UTC/NTP) | [[physical-space-computational-systems\|spazio fisico/virtuale]], GIS, coordinate |
| Nozione "logica/relazionale" | [[logical-clock\|tempo logico]] (Lamport, vector) | topologia logica, [[middleware]] (logico-su-fisico) |
| Framework di sintesi | (verso la [[coordinazione]]) | [[spatial-computing\|spatial computing]] (3 classi) |
| Convergenza | **situatedness** = percepire/agire **al tempo giusto** | **situatedness** = computare **nel luogo giusto** |

## 2. La convergenza: situated computation

Entrambi i moduli convergono sulla **[[situatedness]]** (M4) e sui **[[distributed-pervasive-systems|sistemi pervasivi/IoT]]** (M5):
- M6: *"doing the right thing **at the right time**"* → la [[coordinazione]];
- M7: computazioni che occorrono *"localmente, **dove** percezione o azione hanno luogo"* → **space-awareness**.

> 🔑 Insieme, tempo e spazio costituiscono il **tessuto spazio-temporale** ([[distribuzione-spaziale-temporale]], M0) in cui un sistema distribuito situato deve sapere **quando** e **dove** sta operando per svolgere la sua funzione.

## 3. Connessioni

- [[computing-needs-time]] · [[computing-with-space]] — i due moduli messi a confronto.
- [[processo-computazionale-contesto]] (M2) — contesto temporale **e** spaziale del processo.
- [[parallelo-concorrente-distribuito]] (M2) — la tassonomia per asse tempo (parallelo/concorrente) e spazio (distribuito).
- [[situatedness]] (M4) · [[distributed-pervasive-systems]] (M5) — dove le due dimensioni si fondono.

## 4. Sorgenti

- `raw-sources/M6-Computing-with-Time.pdf` e `raw-sources/M7-Computing-with-Space.pdf`, A. Omicini, A.Y. 2025/2026.
