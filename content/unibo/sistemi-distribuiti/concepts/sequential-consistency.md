---
title: "Sequential Consistency"
course: Sistemi Distribuiti
category: concept
topics: [consistency, data-centric, ordinamento, Lamport]
difficulty: avanzato
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Sequential Consistency

## Definizione

Un data store è **sequentially consistent** quando il risultato di **qualunque esecuzione** è lo stesso che si otterrebbe se:

- le operazioni (read e write) di **tutti** i processi sul data store fossero eseguite in **un qualche ordine sequenziale**, e
- le operazioni di **ciascun singolo processo** comparissero in tale sequenza **nell'ordine specificato dal suo programma** (program order).

## Intuizione

Idea-base: **tutte le operazioni di update sono viste da tutti i processi nello stesso ordine**.

Esiste cioè *un* ordine totale, globale, su cui tutti concordano; tale ordine deve solo **rispettare l'ordine di programma** di ogni processo, ma **non** è vincolato al tempo reale (non importa "quando" davvero un'operazione è avvenuta, purché tutti la vedano nello stesso punto della sequenza).

Questo modello nasce dal problema dell'**ordinamento consistente delle operazioni** tipico degli ambienti **paralleli e concorrenti**: quando più processi condividono risorse e vi accedono simultaneamente, nel committare uno stato per le repliche occorre **raggiungere un accordo sull'ordinamento globale degli update** (→ richiama la sincronizzazione globale della *tight consistency*, [[replicazione]]).

## Varianti e casi limite

- È **più forte** della [[causal-consistency]]: la sequential ordina *tutte* le write (anche quelle indipendenti), la causal solo quelle in relazione causale.
- Realizzarla richiede consenso sull'ordine totale → costosa, e qui si affaccia di nuovo il limite del [[teorema-cap]] e dei teoremi di impossibilità.
- Nota: è la nozione classica di Lamport; l'ordinamento totale concordato si appoggia (nei moduli futuri) ai **clock logici**.

## Connessioni

- [[consistency-model]] — famiglia data-centric
- [[causal-consistency]] — il suo indebolimento causale
- [[continuous-consistency]] — alternativa che quantifica la deviazione invece di imporre un ordine totale
- [[ordinamento-parziale-eventi]] — perché l'ordine *totale* è il punto difficile in un sistema concorrente
- [[consistency]] — la "C" atomica del CAP è ancora più stringente (operazioni in un singolo istante)
- [[lamport-scalar-clock]] — **(C5)** gli scalar clock costruiscono un **ordine totale** (con tie-break sui PID), il tipo di ordinamento che la sequential richiede

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 31–32)
