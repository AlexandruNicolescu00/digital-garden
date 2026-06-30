---
title: "Scalabilità (Scalability)"
course: Sistemi Distribuiti
category: concept
topics: [scalability, decentralizzazione, latency, distribution, replication, caching, DNS]
difficulty: intermedio
sources: [M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Scalabilità (Scalability)

## 1. Definizione

A design time si possono fare **poche assunzioni realistiche** sulla "taglia" effettiva di un sistema distribuito. La **scalabilità** è la capacità di reggere la **crescita** lungo più dimensioni.

> **Dimensioni della scalabilità** [Neuman, 1994]. Un sistema *scala* quando cresce:
> 1. il **numero** di utenti e risorse (*size scalability*);
> 2. la **distribuzione geografica** di utenti e risorse (*geographical scalability*);
> 3. il **numero di domini amministrativi** distinti su cui si estende (*administrative scalability*).

> La scalabilità è un problema **ogni volta che una dimensione cambia ordine di grandezza**.

## 2. Problemi di scalabilità rispetto alla dimensione

Limitazioni tipiche [Tanenbaum & van Steen, 2017] — tutte **centralizzazioni**:
- **servizi centralizzati** — un singolo server per tutti gli utenti;
- **dati centralizzati** — un singolo database per tutti i componenti;
- **algoritmi centralizzati** — si assume informazione completa disponibile in **un solo posto**.

**La centralizzazione a volte è necessaria** (single server per requisiti di sicurezza/normativi; singola collezione dati se la replica è insicura; talvolta l'algoritmo teoricamente più efficiente è centralizzato), **ma ostacola la scalabilità** e va evitata quando possibile. → vedi [[centralizzato-vs-distribuito]].

## 3. Algoritmi decentralizzati vs centralizzati

I problemi degli algoritmi **centralizzati** [Raynal, 2013]: i dati devono fluire da/verso il punto centrale → rete **sovraccarica**; ogni problema di trasmissione compromette l'intero algoritmo. → **in un SD vanno usati solo algoritmi decentralizzati.**

> **Caratteristiche degli algoritmi decentralizzati** [Kshemkalyani & Singhal, 2011]:
> 1. **nessuna macchina** ha informazione completa sullo stato del sistema;
> 2. le macchine decidono solo su **informazione locale**;
> 3. il **guasto di una macchina** non rovina l'algoritmo;
> 4. **non** si assume l'esistenza di un **clock globale**.

> 🔗 Il punto 4 è esattamente l'ipotesi del [[modello-sincrono-asincrono|modello asincrono]] e la radice dell'[[ordinamento-parziale-eventi|ordinamento parziale]]; il punto 3 è la base della fault tolerance ([[mezzi-dependability]]); l'insieme prefigura il problema del [[agreement-consensus|consenso]].

## 4. Scalabilità geografica — tre tecniche [Neuman, 1994]

### (a) Hiding communication latency
Evitare di **attendere** risposte remote quando possibile, tramite **comunicazione asincrona**: l'applicazione invia la richiesta e **non si blocca**; all'arrivo della risposta viene interrotta e un *handler* completa la richiesta. *Problema:* a volte l'asincronia non è fattibile (es. utente Web in attesa) → tecniche alternative come **code shipping** (JavaScript, Java Applet: il client valida i form invece del server).

### (b) Distribution
Prendere un componente, **dividerlo in parti** e distribuirle nel sistema.
**Esempio — DNS:** organizzato gerarchicamente ad albero di domini; i domini sono divisi in **zone** non sovrapposte; i nomi di ciascuna zona sono gestiti da un **singolo server** (es. `apice.unibo.it`) → naming distribuito **senza** centralizzazione.

### (c) Replication
Quando la performance degrada, **replicare** componenti aumenta l'availability e risolve problemi di latenza, mettendo una copia **vicino** agli utenti.
- **Caching** = forma speciale di replicazione. Differenza chiave: il **caching** è una decisione del **client** della risorsa; la **replicazione** è decisa dall'**owner** della risorsa.

### Il problema della consistenza
Duplicare una risorsa (caching o replicazione) introduce **inconsistenza**, **tecnicamente inevitabile** in un setting distribuito. Il punto è **quanta** inconsistenza il sistema può tollerare e **come** nasconderla. → rimanda a [[consistency-model|modelli di consistenza]], [[replicazione]], [[eventual-consistency]] e al [[teorema-cap|CAP]].

## 5. Connessioni

- [[obiettivi-sistemi-distribuiti]] — la scalabilità è il 4° goal.
- [[centralizzato-vs-distribuito]] — la centralizzazione come anti-pattern di scalabilità.
- [[replicazione]] · [[eventual-consistency]] · [[consistency-model]] — replication/caching e il prezzo in consistenza.
- [[modello-sincrono-asincrono]] · [[ordinamento-parziale-eventi]] — niente clock globale; decisioni locali.
- [[agreement-consensus]] — la coordinazione decentralizzata senza stato globale.
- [[trasparenza]] — replication transparency nasconde la replicazione usata per scalare.
- [[partition-tolerance]] (CAP) — domini amministrativi e geografia → partizioni.
- [[distributed-computing-systems]] · [[cluster-vs-grid]] (M5) — i grid attraversano domini amministrativi; il trade-off rete/energia nelle [[distributed-pervasive-systems|sensor networks]].
- [[code-mobility]] (C6) — il **code shipping** (Applet/JS) come tecnica di latency hiding; muovere il codice per load balancing/scalabilità.

## 6. Sorgenti

- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Goals — Scalability), slide 43–55.
- Riferimenti: Neuman 1994; Tanenbaum & van Steen 2017; Raynal 2013; Kshemkalyani & Singhal 2011.
