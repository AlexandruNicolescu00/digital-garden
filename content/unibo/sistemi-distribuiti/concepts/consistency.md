---
title: "Consistency (Consistenza)"
course: Sistemi Distribuiti
category: concept
topics: [CAP, consistency, atomicità, safety]
difficulty: intermedio
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Consistency (Consistenza)

## Definizione

Un sistema è **consistente** quando si comporta *correttamente*, cioè dà risposte corrette quando interrogato. Nel contesto del [[teorema-cap]] [Gilbert and Lynch, 2002], un servizio consistente è modellato come un **atomic data object** in cui:

- le operazioni sono **totalmente ordinate**, e
- ogni operazione avviene in un **singolo istante di tempo**.

Conseguenza chiave: ogni operazione di **read** che avviene *dopo* il completamento di una **write** deve restituire il valore di *quella* write o di una successiva (mai un valore "vecchio").

Slogan: *"se il sistema è consistente, otteniamo risposte corrette"*.

## Intuizione

Tutti i client vedono la **stessa vista dei dati**, anche subito dopo un update o un delete. In una memoria condivisa distribuita, la consistenza forte significa che le repliche appaiono come un unico oggetto coerente.

È una proprietà di **safety** ("non accadono cose sbagliate") — contrapposta alla *liveness* dell'[[availability]]. Il CAP è un esempio del trade-off più generale tra **safety e liveness** in sistemi inaffidabili [Gilbert and Lynch, 2012].

## Tensione con l'ordinamento parziale

La definizione richiede operazioni **totalmente ordinate** — ma in un sistema distribuito gli eventi sono solo **parzialmente ordinati** (→ [[ordinamento-parziale-eventi]]), e non c'è un clock globale. Ricostruire un ordine totale coerente sulle repliche è proprio ciò che rende la consistenza *difficile* e costosa: questo è il cuore del [[teorema-cap]].

## Attenzione: consistency del CAP ≠ "C" di ACID

La consistency del CAP **non coincide** con la Consistency di ACID. Nel CAP il termine ingloba sia l'atomicità sia la consistenza. Vedi [[acid-vs-base]]. È un classico tranello d'esame.

## Varianti

La "C" atomica del CAP è solo il punto più forte di uno **spettro** di modelli (formalizzato in M3, → [[consistency-model]]):

- **Strong / atomic consistency** — quella del teorema CAP (es. Google Spanner: *strongly consistent*); operazioni totalmente ordinate, ognuna in un singolo istante.
- **Sequential consistency** — ordine totale concordato che rispetta il program order → [[sequential-consistency]].
- **Causal consistency** — ordina solo le write in relazione causa/effetto → [[causal-consistency]].
- **Continuous consistency** — limita le *deviazioni* (numeriche/staleness/ordering) via conit → [[continuous-consistency]].
- **Eventual consistency** — versione rilassata: le repliche convergono "alla fine" → [[eventual-consistency]].
- **Client-centric** (monotonic reads/writes, read-your-writes, writes-follow-reads) → [[client-centric-consistency]].
- **Strong read-after-write** — es. Amazon S3.

> Nota M3: in M3 "consistency" è la *correttezza vista dai processi* (contratto, [[consistency-model]]), nozione più ampia dell'**atomic object** del CAP. La consistency del CAP è il caso più stringente di questo spettro.

## Connessioni

- [[teorema-cap]] — dove questa definizione è usata nella prova
- [[availability]] — la proprietà in trade-off (liveness vs safety)
- [[partition-tolerance]] — la terza proprietà
- [[eventual-consistency]] — il rilassamento pratico
- [[ordinamento-parziale-eventi]] — perché la consistenza forte è difficile
- [[consistency-model]] — la cornice generale (data-centric vs client-centric)
- [[replicazione]] — il problema concreto (copie divergenti) che la consistenza regola

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-...pdf` (slides 6, 11, 27)
- `raw-sources/M3-Replication-Consistency-...pdf` (spettro dei modelli)
