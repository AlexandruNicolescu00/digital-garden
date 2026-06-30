---
title: "Il Tempo nei Sistemi Distribuiti (l'Issue of Time)"
course: Sistemi Distribuiti
category: concept
topics: [tempo, global time, sincronizzazione, fisico, logico, contesto temporale]
difficulty: intermedio
sources: [M6-Computing-with-Time.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Il Tempo nei Sistemi Distribuiti (l'Issue of Time)

Pagina-hub che inquadra **il problema del tempo** nei sistemi distribuiti e biforca verso le due risposte: tempo **fisico** ([[physical-clock-synchronization]]) e tempo **logico** ([[logical-clock]]).

## 1. Recap: contesti temporali e spaziali (M2)

M6 richiama la tassonomia di [[parallelo-concorrente-distribuito]] (M2):
- **parallel computing** — **stesso** contesto **temporale** `T` per tutti i processi;
- **concurrent computing** — almeno due processi hanno contesto **temporale diverso** (`T ≠ T'`);
- **distributed computing** — almeno due processi hanno contesto **spaziale diverso** (`S ≠ S'`);
- combinazioni: *distributed parallel* (`S ≠ S'`, stesso `T`) e **distributed concurrent** (`S ≠ S'`, `T ≠ T'`).

> Il **distributed concurrent computing** è il modello computazionale **più naturale e generale** per i sistemi distribuiti.

## 2. L'issue of time

> - in un sistema **centralizzato** il tempo è **non ambiguo** (un unico clock → [[lamport-scalar-clock|total ordering]] degli eventi via system call al kernel);
> - in un sistema **distribuito** **non c'è** una nozione naturale di tempo: niente clock globale, niente memoria comune.

Le **domande fondamentali**:
1. esiste una nozione di tempo **utile e ben fondata** in un sistema distribuito?
2. esiste una nozione **coerente di tempo globale**?
3. se no, cosa possiamo farci? **come sincronizziamo** le attività?

*Synchronous* = "esistente o che occorre **allo stesso tempo**" → ma "allo stesso tempo" è esattamente ciò che è problematico in un SD ([[modello-sincrono-asincrono]]).

## 3. Due domande chiave (e due risposte)

> *È **possibile** costruire una nozione di tempo globale in un SD? È **utile**?*

Le risposte biforcano in due famiglie:

| Risposta | Cos'è | Pagina |
|----------|-------|--------|
| **Tempo fisico** | sincronizzare gli orologi reali su uno standard (assoluto: UTC; o relativo: tempo comune condiviso) | [[physical-clock-synchronization]] |
| **Tempo logico** | rinunciare al tempo "vero" e usare un clock che cattura solo l'**ordine** degli eventi e la causalità | [[logical-clock]] · [[lamport-scalar-clock]] · [[vector-clock]] |

> 🔑 Spesso ciò che davvero serve **non è l'istante esatto** in cui gli eventi occorrono, **ma l'ordine** in cui occorrono — e a quel punto il tempo logico [Lamport, 1978] è sufficiente. Il confronto è in [[physical-vs-logical-time]].

## 4. Connessioni

- [[physical-clock-synchronization]] — la risposta "tempo fisico" (UTC/NTP/Berkeley/RBS).
- [[logical-clock]] · [[lamport-scalar-clock]] · [[vector-clock]] (C5) — la risposta "tempo logico".
- [[physical-vs-logical-time]] — il confronto e quando scegliere cosa.
- [[parallelo-concorrente-distribuito]] (M2) — i contesti temporale/spaziale richiamati nel recap.
- [[modello-sincrono-asincrono]] (C3) — il modello temporale che decide cosa è risolvibile.
- [[ordinamento-parziale-eventi]] (M0) — l'ordine parziale come unica struttura disponibile senza clock globale.
- [[computing-needs-time]] — la cornice concettuale (perché il tempo conta).

## 5. Sorgenti

- `raw-sources/M6-Computing-with-Time.pdf` (Time in Distributed Systems — recap & Issue of Time), slide 14–23, 35, 38.
- Riferimenti: Tanenbaum & van Steen 2017; Kshemkalyani & Singhal 2011; Lamport 1978.
