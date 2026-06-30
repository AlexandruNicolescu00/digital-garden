---
title: "Paxos (Algoritmo di Consenso Fault-Tolerant)"
course: Sistemi Distribuiti
category: theorem
topics: [paxos, consensus, quorum, Lamport, fault-tolerance]
difficulty: avanzato
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Paxos (Algoritmo di Consenso Fault-Tolerant)

## Enunciato / specifica

**Paxos** [Lamport, 1998 — *The Part-Time Parliament*] è un algoritmo per implementare il **consenso fault-tolerant** su un **singolo valore**.

**Ipotesi e garanzie:**
- gira su una rete **completamente connessa** di *n* processi e tollera fino a *f* fallimenti, con **n ≥ 2f + 1** (serve una **maggioranza** funzionante);
- i processi possono **crashare** e i messaggi possono **perdersi**, ma i fallimenti **bizantini sono esclusi** (eco di [[classificazione-guasti]]);
- garantisce **agreement** e **validity** (safety);
- la **terminazione** è assicurata **solo** se esiste un intervallo sufficientemente lungo in cui nessun processo riavvia il protocollo → **coerente con [[flp-impossibility|FLP]]**: rinuncia alla liveness garantita.

Storia: presentato nel 1989 (technical report), riformulato in [Lamport, 1998], generalizzato alla **State Machine Replication** [Lampson, 1996], provato corretto [De Prisco et al., 1997], spiegato in [Lamport, 2001 — *Paxos Made Simple*].

## Intuizione

Tre **ruoli** (un processo può ricoprirne più d'uno):
- **Proposer** — sottomette i valori proposti per conto dei client;
- **Acceptor** — decide i valori candidati per la decisione finale (sono **multipli**, in numero **dispari**);
- **Learner** — raccoglie le informazioni dagli acceptor, determina la decisione e la **riporta ai client**.

Idea centrale: protocollo a **due fasi** (*prepare/promise* + *accept/commit*), **accordo a maggioranza (quorum)**, e **numeri di sequenza monotonicamente crescenti** per etichettare le proposte. Una proposta è una coppia **(v, n)** con *v* valore e *n* sequence number. Una volta che un valore è **scelto**, ogni proposta futura **deve proporre lo stesso valore**.

## Le fasi (schema) [Ghosh, 2014]

**Fase 1 — Prepare (preparatoria)**
1. ogni proposer invia una proposta **(v, n)** a ogni acceptor;
2. se *n* è il **più grande** sequence number ricevuto da un acceptor, questo risponde con **ack(n, –, –)** = **promessa** di ignorare tutte le proposte numerate < *n*;
   - se però l'acceptor aveva **già accettato** una proposta (v′, n′) con n′ < n, risponde **ack(n, v′, n′)** — segnalando che un valore è già in gioco.

**Fase 2 — Accept (richiesta di accettazione)**
1. se il proposer riceve ack da una **maggioranza** di acceptor, invia **accept(v, n)** a tutti, chiedendo di accettare il valore;
   - ma se in fase 1 un acceptor aveva restituito un valore già accettato, il proposer **deve includere il valore con il sequence number più alto** tra quelli accettati (non può imporre il proprio);
2. un acceptor accetta (v, n) **a meno che** non abbia già promesso di considerare proposte con sequence number > *n*.

**Fase 3 — Decisione finale**
- quando una **maggioranza** di acceptor accetta un valore, questo diventa la **decisione finale**;
- gli acceptor fanno **multicast** del valore accettato ai **learner**, che determinano l'avvenuta accettazione a maggioranza e la comunicano ai **client**.

> Le fasi possono **iterare** (se nessun valore raggiunge la maggioranza), il che **teoricamente** può portare a **livelock** — niente progresso pur senza deadlock (di nuovo il fantasma di FLP sulla liveness).

## Corollari, varianti e applicazioni

- **Multi-Paxos** [Van Renesse & Altinbuken, 2015] — per la [[state-machine-replication|SMR]] serve consenso su un **array** di valori: un'istanza Paxos (indicizzata) per entry + **leader election** per ridurre i conflitti tra proposer.
- **Altri della famiglia**: **Fast Paxos** [Lamport, 2006]; **ZooKeeper**/**ZAB** [Hunt et al. 2010, Reed & Junqueira 2008]; **Egalitarian Paxos** [Moraru et al. 2013]; **Raft** [Ongaro & Ousterhout, 2014] (*"In search of an understandable consensus algorithm"*).
- **Applicazione reale — Chubby (Google)** [Chandra et al. 2007, Burrows 2006]: lock service affidabile basato su Paxos, usato in Google File System, Cloud Bigtable, …
- Connessione corso: è la base di **etcd**/Raft dietro [[kubernetes]] (CX) e dei sistemi citati nella tabella tecnologie di M3.

## Connessioni

- [[flp-impossibility]] — il vincolo che Paxos rispetta (safety sempre, liveness condizionata)
- [[agreement-consensus]] · [[consensus-problemi]] — il problema risolto e le proprietà
- [[state-machine-replication]] — Multi-Paxos come motore della SMR
- [[modello-sincrono-asincrono]] — assume sincronia parziale per terminare
- [[classificazione-guasti]] — i guasti bizantini sono esplicitamente esclusi
- [[tamir-sequin-checkpointing]] — confronto col 2PC (commit a fasi); Paxos a quorum tollera i crash che bloccano il 2PC
- [[pbft]] · [[byzantine-fault-tolerance]] — **(C4)** il "cugino" bizantino: PBFT estende l'idea leader/quorum ai fault arbitrari (`f<N/3`); Paxos/Raft sono il consenso **non-BFT** [[permissioned-vs-permissionless|permissioned]] delle blockchain (Chubby, ZooKeeper)
- [[kubernetes]] — etcd/Raft, ZooKeeper, Chubby come realizzazioni

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 27–43; Lamport 1998/2001, Ghosh 2014, Zhao 2014)
