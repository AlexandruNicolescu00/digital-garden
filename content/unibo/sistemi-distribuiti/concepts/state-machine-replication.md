---
title: "State Machine Replication (SMR)"
course: Sistemi Distribuiti
category: concept
topics: [SMR, consensus, replicazione, fault-tolerance, ordinamento]
difficulty: avanzato
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# State Machine Replication (SMR)

## Definizione

La **State Machine Replication** [Schneider, 1990] è un **metodo generale per implementare servizi fault-tolerant** in sistemi distribuiti. Idea di base:

- la **stessa macchina a stati** è replicata su un sistema distribuito;
- ciascuna replica ha un **modulo di consenso**;
- **qualunque operazione** su qualunque replica è eseguita **solo se e quando tutte le macchine hanno concordato sullo stesso ordinamento di esecuzione**.

## Intuizione: dal consenso sullo stato al consenso sulle azioni

La replicazione si lega al consenso perché tutte le repliche di una risorsa devono **accordarsi sullo stesso stato nel tempo** ([[replicazione]]). La SMR opera uno **spostamento concettuale**:

> dal **consenso sullo stato distribuito** → al **consenso sulle azioni (operazioni) distribuite sullo stato**.

Se tutte le repliche partono dallo **stesso stato iniziale** ed eseguono la **stessa sequenza di operazioni nello stesso ordine**, finiscono nello **stesso stato**: la consistenza diventa una conseguenza dell'**ordinamento concordato** (eco della [[sequential-consistency]]).

## Varianti e casi limite

- **Assunzione di determinismo**: ogni replica, dato lo stesso stato e la stessa operazione, deve produrre lo **stesso** risultato — altrimenti l'ordinamento concordato non basta a garantire stati identici.
- **Da Paxos a Multi-Paxos**: [[paxos]] decide **un singolo valore**; la SMR ha bisogno di consenso su un **array** di valori (la sequenza di operazioni) → **Multi-Paxos** applica un'istanza di Paxos (indicizzata) a ogni entry dell'array, più **leader election** per ridurre i conflitti tra proposer.

## Applicazione concreta: la blockchain (C4)

La [[blockchain]] è *"un'astuta implementazione di un sistema SMR"*: la funzione di transizione `δ(s, tx) = s'` ([[blockchain-transactions]]) è la macchina a stati replicata su ogni nodo P2P; i [[blocks-e-block-chain|blocchi]] (hash chain) ordinano gli input; il [[consensus-mining|consenso]] ([[pbft|PBFT]] o [[proof-of-work|PoW]]) realizza l'ordinamento concordato anche in presenza di [[byzantine-fault-tolerance|fault bizantini]]. Replicando un **interprete** invece di una specifica business logic si ottiene la [[universal-smr|Universal SMR]] (blockchain con [[smart-contract|smart contract]]). Vedi la mappa [[blockchain-vs-teoria-del-corso]].

## Connessioni

- [[agreement-consensus]] · [[consensus-problemi]] — il consenso che la SMR usa come mattone
- [[paxos]] — l'algoritmo di consenso; Multi-Paxos per la sequenza di operazioni
- [[replicazione]] · [[replica-management]] — la SMR è il "come" della replicazione consistente (M3)
- [[sequential-consistency]] — stessa idea di ordine totale concordato sulle operazioni (M3)
- [[blockchain]] · [[universal-smr]] · [[byzantine-fault-tolerance]] — la SMR (anche bizantina) come fondamento della DLT (C4)
- [[kubernetes]] — etcd (store di K8s) usa consenso (Raft) in stile SMR; ZooKeeper/Chubby idem

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 39–41; Schneider 1990, Van Renesse & Altinbuken 2015)
- Ripreso in `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (SMR Overview, slide 56–61).
