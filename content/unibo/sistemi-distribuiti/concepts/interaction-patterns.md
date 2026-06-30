---
title: "Interaction Patterns (Pattern di Interazione)"
course: Sistemi Distribuiti
category: concept
topics: [interazione, comunicazione, sequence-diagram, state-machine, message-flow, modellazione]
difficulty: base
sources: [preliminaries_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Interaction Patterns (Pattern di Interazione)

## Definizione

Un **interaction pattern** descrive **come** componenti diversi (nodi, processi, ecc.) **comunicano e coordinano** le proprie azioni per raggiungere un obiettivo comune. Un pattern definisce:

- il **flusso dei messaggi** tra i partecipanti;
- le **responsabilità** dei partecipanti;
- il **timing e la sequenza** delle comunicazioni.

Esempi: *request-response*, *publish-subscribe*, *auction (ContractNet)*, ecc. → catalogo concreto in [[protocolli-interazione-comuni]].

## I tre ingredienti di un pattern

1. **Participants** — si assume che vi siano *N* partecipanti all'interazione.
2. **Roles** — i partecipanti giocano **ruoli ben definiti**; i più comuni sono:
   - **initiator**: il partecipante che *inizia* l'interazione;
   - **responder**: il partecipante che *attende* che qualcun altro inizi.
3. **Messages** — l'informazione scambiata tra i partecipanti, che contiene tipicamente almeno:
   - **payload**: il contenuto effettivo del messaggio;
   - **metadata**: informazioni *sul* messaggio (source, destination, timestamp, conversation id, ecc.).

> I ruoli sono **relativi al pattern**, non al componente: lo stesso processo può essere initiator in un'interazione e responder in un'altra — esattamente come un componente può essere [[infrastruttura-e-componenti|server e client]] al tempo stesso.

## Intuizione

Definire *quali* [[infrastruttura-e-componenti|componenti infrastrutturali]] esistono non basta: bisogna specificare **come si parlano**. L'interaction pattern è il livello di modellazione che cattura proprio questo, ed è uno dei concern centrali del **Design** ([[se-workflow-distribuito|passo 3 del workflow]]: *how do components communicate? which interaction patterns do they enact?*). Gli [[architectural-styles|stili architetturali]] non sono che **combinazioni ricorrenti** di componenti + interaction pattern.

## Come rappresentare un interaction pattern

Le tre rappresentazioni sono **complementari**: vanno usate **insieme** per descrivere pienamente un pattern.

### 1. Sequence diagram (diagramma di sequenza)
Rappresentazione visuale del flusso di messaggi tra partecipanti:
- il **tempo è verticale**, i **partecipanti sono orizzontali**;
- le **frecce** rappresentano i messaggi inviati da un partecipante a un altro;
- le **lifeline** rappresentano la vita di un partecipante.

### 2. Message flow graph (grafo del flusso dei messaggi)
Rappresentazione visuale del flusso dei messaggi:
- ogni **nodo** rappresenta un **tipo di messaggio** (request, response, notification, ecc.);
- ogni **arco diretto** rappresenta una **risposta ammissibile** a un messaggio;
- il grafo può contenere **cicli** se il pattern ammette interazioni ripetute o reset;
- i nodi possono avere colori/forme diverse secondo il ruolo che invia/riceve.
- *Hint*: in OOP ogni tipo di messaggio può essere una **classe** → class diagram (PlantUML) o tool di graph-drawing (Graphviz, yEd).

### 3. State diagram / state machine (diagramma a stati)
Rappresentazione visuale delle **transizioni di stato interne** di un partecipante:
- ogni **stato** è una condizione del partecipante (es. prima/dopo l'invio/ricezione di un messaggio);
- ogni **transizione** è un **evento** che ne cambia lo stato (tipicamente la ricezione o l'invio di un messaggio);
- stati **iniziale** e **finale** sono speciali; tipicamente **uno state diagram per partecipante**.
- *Hint*: PlantUML può generare gli state diagram da descrizioni testuali.

> Le state machine qui usate per i partecipanti sono parenti delle **macchine a stati** della [[state-machine-replication|State Machine Replication]] e del modello di esecuzione di una [[blockchain-transactions|transazione blockchain]].

### Rappresentazioni ulteriori
- **FIPA AUML** (Agent UML) per sistemi ad agenti;
- **BPMN** (Business Process Model and Notation) per processi di business;
- **UML Activity Diagrams**.

## Connessioni

- [[protocolli-interazione-comuni]] — il **catalogo concreto** dei pattern (request-response, pub-sub, ContractNet, FIPA).
- [[infrastruttura-e-componenti]] — i componenti *tra cui* avviene l'interazione; ruoli initiator/responder ↔ client/server.
- [[se-workflow-distribuito]] — la scelta dei pattern è un concern del Design (passi 3–4).
- [[architectural-styles]] — **(M8/M2)** ogni stile prescrive *quali* pattern usare (layered→request-response, event-based→pub-sub…).
- [[componenti-connettori]] — **(M8)** la versione astratta: il **connettore** media l'interazione.
- [[coordinazione]] — **(M6)** l'interazione come problema di coordinazione (oltre la mera comunicazione).
- [[state-machine-replication]] — **(C3)** le macchine a stati come modello di un partecipante deterministico.
- [[socket]] · [[stream-vs-datagram-sockets]] — **(M2)** il livello *fisico* sotto i pattern: i messaggi viaggiano su socket TCP/UDP.

## Sorgenti

- `raw-sources/preliminaries_slides.pdf` (slide 38–42, 48 — *Interaction Patterns*), G. Ciatto, *Preliminaries about Distributed Systems Engineering*, Distributed Systems — Module 2, A.Y. 2025/2026.
- Riferimenti: FIPA (http://www.fipa.org), AUML, BPMN.
