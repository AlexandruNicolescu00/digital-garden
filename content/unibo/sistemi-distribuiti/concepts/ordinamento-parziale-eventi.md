---
title: "Ordinamento Parziale degli Eventi"
course: Sistemi Distribuiti
category: concept
topics: [eventi, tempo, causalità, ordinamento]
difficulty: intermedio
sources: [M0-Why-Distributed-Systems.pdf, C5-Logical-Clocks.pdf]
created: 2026-06-25
updated: 2026-06-26
---

# Ordinamento Parziale degli Eventi

## Definizione

In un sistema distribuito, gli eventi **non costituiscono più un insieme totalmente ordinato**. In generale, **l'ordinamento parziale è l'unica proprietà disponibile**: per molte coppie di eventi non è possibile dire quale sia avvenuto "prima" e quale "dopo".

## Intuizione

In un sistema centralizzato esiste un unico clock e un'unica linea temporale: ogni evento ha un istante, e due eventi qualsiasi sono confrontabili (totalmente ordinati). In un sistema distribuito questo crolla:

- non c'è un **tempo di sistema** condiviso (→ [[distribuzione-spaziale-temporale]])
- la relazione **before/after** non si applica a coppie di eventi che avvengono su nodi diversi senza comunicare tra loro

Quindi il massimo che si può affermare è un **ordine parziale**: alcune coppie di eventi sono confrontabili (uno causa o precede l'altro), molte altre no — sono *concorrenti*.

## Assunzioni che non valgono più

Con la distribuzione, alcune assunzioni comode sui sistemi cadono:

- gli eventi **non** formano un insieme totalmente ordinato → resta solo l'ordinamento parziale
- le interazioni ammissibili tra componenti **non dipendono più dalla compresenza**:
  - nello spazio / nel tempo
  - all'interno della stessa topologia fisica / virtuale

In altre parole, due componenti possono interagire pur non essendo "presenti insieme" nello stesso luogo o nello stesso istante.

## Perché è importante

Questo è il problema teorico fondante del corso: gran parte degli algoritmi distribuiti (sincronizzazione, mutua esclusione, consistenza, consensus) esistono proprio per **ricostruire un ordinamento utile** a partire da un ordine solo parziale. Il primo strumento formale per farlo sono i **clock logici**.

## Formalizzazione (C5): happens-before

In C5 questo ordine parziale riceve il suo nome formale: la relazione **[[happened-before|happens-before]]** `→` di [Lamport, 1978]. `a → b` quando (i) sono nello stesso processo e `a` precede `b`, oppure (ii) `a` è un *send* e `b` il corrispondente *receive*; è transitiva. Quando **né** `a → b` **né** `b → a`, gli eventi sono **concorrenti** (`a ∥ b`) — esattamente le coppie "non confrontabili" dell'ordine parziale. I [[logical-clock|clock logici]] ([[lamport-scalar-clock|scalari]] e [[vector-clock|vettoriali]]) assegnano valori temporali coerenti con `→`.

## Collegamento con la tassonomia dei sistemi (M2)

In M2 questo è esattamente ciò che distingue i **sistemi concorrenti** dai **paralleli**: in un sistema **parallelo** (stesso contesto temporale) gli eventi sono **totalmente ordinati**; in un sistema **concorrente** (contesti temporali diversi) sono **al più parzialmente ordinati**. Vedi [[parallelo-concorrente-distribuito]]. La "*true concurrency*" usa proprio gli ordini parziali per catturare le relazioni causali.

## Connessioni

- [[distribuzione-spaziale-temporale]] — la causa: perdita dell'unità spazio-temporale
- [[parallelo-concorrente-distribuito]] — parallelo (ordine totale) vs concorrente (ordine parziale)
- [[causalita]] — **(C2)** la causalità come fondamento dell'ordine parziale e del tempo distribuito
- [[sistema-distribuito]] — il contesto generale
- [[happened-before]] — **(C5)** la formalizzazione `→` di questo ordine parziale (gancio risolto)
- [[logical-clock]] · [[lamport-scalar-clock]] · [[vector-clock]] — **(C5)** gli strumenti che assegnano tempo logico coerente con l'ordine

## Ganci risolti (C5)

- ~~Relazione "happened-before" di Lamport e clock logici~~ → [[happened-before]], [[lamport-scalar-clock]].
- ~~Clock vettoriali per catturare la causalità~~ → [[vector-clock]] (strong consistency).
- ~~Eventi concorrenti vs causalmente correlati~~ → `a ∥ b` vs `a → b` in [[happened-before]].

## Sorgenti

- `raw-sources/M0-Why-Distributed-Systems.pdf` (slides 7–8)
- `raw-sources/C5-Logical-Clocks.pdf` (happens-before, slide 8–13)
