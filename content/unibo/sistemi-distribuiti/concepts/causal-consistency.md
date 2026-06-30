---
title: "Causal Consistency"
course: Sistemi Distribuiti
category: concept
topics: [consistency, data-centric, causalità, ordinamento]
difficulty: avanzato
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf, C5-Logical-Clocks.pdf]
created: 2026-06-25
updated: 2026-06-26
---

# Causal Consistency

## Definizione

Un data store è **causally consistent** quando **tutti i processi vedono le operazioni di write che sono in relazione causa/effetto nello stesso ordine**.

Le operazioni **non correlate** (cioè *concorrenti*) possono invece essere viste in ordini diversi da processi diversi: l'ordinamento è **limitato** alle sole operazioni in relazione causale.

## Intuizione

È un **indebolimento** della [[sequential-consistency]]:

- la sequential impone un ordine totale concordato su **tutte** le write;
- la causal impone l'ordine **solo** dove c'è una relazione di **causa/effetto** ([[causalita]], C2), lasciando libere le write **concorrenti**.

Questo riduce il bisogno di sincronizzazione globale (si ordina solo ciò che è causalmente legato) pur preservando la coerenza dove conta: se una write B *dipende causalmente* da una write A, nessun processo vedrà B prima di A.

## Implementazione (C5): vector clock

Il modello si **realizza** tramite [[vector-clock|clock vettoriali]] (dependency tracking): poiché `vc(a) < vc(b) ⟺ a → b` (**strong consistency**), una replica può **ritardare** la consegna di una write finché non ha applicato tutte le write che la precedono causalmente (causal delivery), lasciando invece libere le write **concorrenti** (vettori non confrontabili). È la traduzione operativa di "rispettare l'ordine solo dove c'è [[happened-before|happens-before]]".

## Connessioni

- [[causalita]] — la relazione causa/effetto su cui il modello si fonda (happened-before, dipendenze)
- [[happened-before]] · [[vector-clock]] — **(C5)** la relazione `→` e il meccanismo (vector clock) che implementa la causal consistency
- [[sequential-consistency]] — il modello più forte che questo rilassa
- [[consistency-model]] — famiglia data-centric
- [[ordinamento-parziale-eventi]] — la causalità induce un **ordine parziale**, ed è esattamente questo che la causal consistency rispetta (a differenza dell'ordine totale della sequential)
- [[continuous-consistency]] — altro modo di rilassare (deviazione quantificata invece di ordine causale)

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 33)
- `raw-sources/C5-Logical-Clocks.pdf` (Vector Clocks, slide 28–33)
