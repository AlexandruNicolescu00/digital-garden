---
title: "Distributed Snapshot di Chandy & Lamport"
course: Sistemi Distribuiti
category: theorem
topics: [snapshot, checkpointing, nonblocking, marker, channel-state]
difficulty: avanzato
sources: [C2-Logging-Checkpointing.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Distributed Snapshot di Chandy & Lamport

## Cosa risolve

Produrre un **global checkpoint (snapshot) consistente** (→ [[global-state-consistency]]) **senza bloccare** l'esecuzione normale [Chandy and Lamport, 1985]. È il classico algoritmo per "determinare lo stato globale di un sistema distribuito" — il gancio lasciato aperto in [[stato-sistema-distribuito]] (il problema del "tempo t").

A differenza di [[tamir-sequin-checkpointing]]:
- è **nonblocking**: l'esecuzione normale **non** viene interrotta;
- si occupa **solo** di *come produrre* uno snapshot consistente: **non** prescrive come determinare la fine del round né come switchare atomicamente al nuovo checkpoint.

## Meccanismo: i Marker

I processi sono in stato **Normal** o **Checkpointing**.

1. **Inizio**: un processo qualsiasi può iniziare lo snapshot → prende il **checkpoint locale**, invia un **Marker** su **tutti i canali uscenti**, passa a *Checkpointing*.
2. **Ricezione di un Marker** (da Normal): il processo fa lo stesso (checkpoint locale + invio Marker su tutti i canali uscenti) e registra il **Marker Certificate** (da completare).
3. **Messaggi regolari durante Checkpointing**:
   - se sul canale **non** è ancora arrivato il Marker → il messaggio viene **aggiunto allo stato del canale** (channel state) ed eseguito;
   - se il Marker è **già** arrivato su quel canale → il messaggio viene solo eseguito.
4. **Fine**: quando il Marker Certificate è **completo** (Marker ricevuti da tutti i canali entranti), il processo torna in **Normal** (e riporta il completamento).

La regola dei Marker è esattamente ciò che permette di **catturare il channel state** in modo consistente: separa i messaggi "prima dello snapshot" da quelli "dopo".

## Proprietà

- **Nonblocking** → basso impatto sull'esecuzione, promuove l'**autonomia** dell'iniziatore (qualsiasi processo può iniziare).
- **Non atomico**: nessun meccanismo per la fine del round / switch atomico (a differenza del 2PC di Tamir-Sequin).
- Cattura **process state + channel state**, garantendo uno stato globale consistente.

## Confronto sintetico con Tamir-Sequin

Stesso system model, stesso uso di un **control message** speciale, **stesso** meccanismo di cattura del channel state, **stesso** overhead di comunicazione. Differenza: **blocking + coordinato + atomico** (Tamir-Sequin) vs **nonblocking + autonomo + non atomico** (Chandy-Lamport). Dettagli in [[protocolli-recovery]].

## Connessioni

- [[global-state-consistency]] — lo snapshot consistente + channel state
- [[stato-sistema-distribuito]] — risolve il gancio "catturare lo stato globale al tempo t"
- [[tamir-sequin-checkpointing]] — l'alternativa blocking
- [[causalita]] · [[ordinamento-parziale-eventi]] — l'algoritmo è di Lamport, padre della happened-before
- [[happened-before]] · [[lamport-scalar-clock]] — **(C5)** la relazione `→` di Lamport che fonda la consistenza dello snapshot (cut che rispetta la causalità)
- [[protocolli-recovery]] — la tassonomia completa

## Sorgenti

- `raw-sources/C2-Logging-Checkpointing.pdf` (slides 41–46)
- [Chandy and Lamport, 1985] *Distributed snapshots: determining global states of distributed systems*, ACM TOCS.
