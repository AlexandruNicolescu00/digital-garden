---
title: "Availability (Disponibilità)"
course: Sistemi Distribuiti
category: concept
topics: [CAP, availability, liveness, fault-tolerance, dependability, MTTF]
difficulty: intermedio
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf, M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Availability (Disponibilità)

## Definizione

Un sistema è **available** quando *funziona*, fa accadere cose buone, è *live*. Nel [[teorema-cap]] [Gilbert and Lynch, 2002]:

> "Perché un sistema distribuito sia continuamente *available*, ogni richiesta ricevuta da un nodo **non-failing** deve produrre una **risposta**."

Slogan: *"se il sistema è available, otteniamo risposte"* (non necessariamente corrette — la correttezza è la [[consistency]]).

## Intuizione: nascondere i fallimenti

Uno dei principali vantaggi attesi dei sistemi distribuiti rispetto ai centralizzati è **continuare a fornire servizi nonostante i guasti** [Friedman and Birman, 1996]. L'assunzione intuitiva: se un componente fallisce o si **disconnette/partiziona**, altri lo sostituiscono così da **nascondere il guasto** al mondo esterno (o almeno ridurne l'impatto percepito).

Quando un sistema riesce a nascondere (la maggior parte de)i suoi guasti, è praticamente *sempre funzionante*: si dice **altamente disponibile** (*highly available*).

Domanda di progetto fondamentale: *quanto* guasto può sostenere un sistema prima che il guasto venga *notato*?

Tutti i client possono trovare una **replica** del dato, anche in caso di guasti parziali dei nodi.

## Safety vs liveness

L'availability è una proprietà di **liveness** ("prima o poi accade qualcosa di buono"), contrapposta alla **safety** della [[consistency]]. Il CAP è un caso particolare del trade-off generale safety/liveness in sistemi inaffidabili [Gilbert and Lynch, 2012].

## Spettro, non on/off

A differenza della [[partition-tolerance]], availability (come la consistency) **varia su uno spettro** di opzioni: si può essere "più o meno" disponibili. Esempio di perdita di availability: la *Spinning Wheel of Death* quando un client attende la conferma di una transazione fortemente consistente.

## ⚠️ Due sensi di "availability" nel corso

Il termine ha **due accezioni** distinte (da non confondere all'esame):

1. **Senso CAP** (questa pagina, [[teorema-cap]]) — proprietà **qualitativa** di *liveness*: ogni richiesta a un nodo non-failing ottiene una risposta.
2. **Senso dependability** (modulo M1) — **metrica quantitativa**: la probabilità che il sistema sia operativo in un dato istante.

### Availability come metrica di dependability (M1)

Definizione informale: misura della *prontezza* di un sistema dependable in un (qualsiasi) istante — la probabilità che il servizio sia lì quando il client lo richiede. Definita su un **istante** (≠ [[reliability]], definita su un intervallo).

Fattori temporali: **MTTF** (Mean Time To Failure), **MTTR** (Mean Time To Repair), **MTBF = MTTF + MTTR**.

$$\text{Availability} = \frac{MTTF}{MTTF + MTTR} = \frac{MTTF}{MTBF}$$

Si esprime in "numero di **9**": es. *five 9s* = 99.999% ⇒ al più ~5.256 minuti di downtime l'anno.

Confronto completo con la reliability: → [[availability-vs-reliability]].

## Connessioni

- [[teorema-cap]] — il trade-off formale (senso CAP)
- [[reliability]] — l'attributo gemello (senso dependability)
- [[availability-vs-reliability]] — il confronto e le metriche
- [[dependability]] — l'ombrello che la contiene come attributo
- [[consistency]] — la proprietà opposta nel trade-off pratico (CAP)
- [[partition-tolerance]] — la terza proprietà (on/off)
- [[pokemon-go-cap]] — availability come "prima vittima" quando serve consistenza forte
- [[motivazioni-sistemi-distribuiti]] — fault tolerance come motivazione dei SD
- [[kubernetes]] — **(CX)** autoscaling/self-healing come tecniche concrete per migliorare l'availability; vedi [[quality-attributes]]

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-...pdf` (slides 4–6, 26)
- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 33, 38–40)
