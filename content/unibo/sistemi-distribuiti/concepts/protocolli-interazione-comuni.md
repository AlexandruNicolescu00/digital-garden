---
title: "Protocolli di Interazione Comuni"
course: Sistemi Distribuiti
category: concept
topics: [request-response, publish-subscribe, broker, multicast, contractnet, fipa, rpc]
difficulty: base
sources: [preliminaries_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Protocolli di Interazione Comuni

Catalogo dei **pattern di interazione** ricorrenti tra componenti di un sistema distribuito. È la controparte *concreta* della pagina [[interaction-patterns]] (che ne dà la teoria e le rappresentazioni).

## 1. Request—Response

Il pattern **più comune e basilare** di comunicazione tra due componenti.

- **2 ruoli**: client (initiator) e server (responder).
- **2 tipi di messaggio**: *request* e *response*.
- Ogni request è seguita da **esattamente una** response.
- Il client **invia** la request e **attende** la response; il server **attende** la request e **invia** la response.
- Spesso usato per realizzare:
  - **Remote Procedure Call (RPC)**;
  - **Remote Method Invocation (RMI)**;
  - **(Web) services**.

> È l'interazione tipica dello stile [[architectural-styles|layered]] (chiamate top-down) e [[architectural-styles|object-based]] (RMI).

## 2. Publish—Subscribe

Un pattern semplice per **diffondere informazione** a più destinatari.

- **2 ruoli**: *publisher* e *subscriber*.
- **2 tipi di messaggio**: *subscribe* e *notify*.
- **2 fasi**:
  1. **Subscription phase** — i subscriber dichiarano l'interesse a ricevere messaggi (qui *sono loro gli initiator*); i messaggi `subscribe` possono portare un **topic** (l'argomento di interesse). Nota: la subscription è essa stessa un request-response.
  2. **Notification phase** — i publisher inviano messaggi ai subscriber; i messaggi `notify` portano contenuti che tipicamente rappresentano **eventi**. I messaggi sono **broadcast** o **multicast** a seconda dell'implementazione.

### Publish—Subscribe con Broker
Nel pub-sub "puro" il publisher agisce *anche* da broker. Si può ridisegnare il pattern con un **[[infrastruttura-e-componenti|broker]] esplicito**:
- **disaccoppia** il publisher dai subscriber;
- il broker tipicamente **memorizza** i messaggi finché non sono consumati (→ [[infrastruttura-e-componenti|queue]]);
- i subscriber si iscrivono a **topic** (es. *TopicA*, *TopicB*) e il broker instrada di conseguenza (è il [[middleware|Message-Oriented Middleware]]).

> È l'interazione tipica dello stile [[architectural-styles|event-based]] (event bus) — disaccoppiamento *referenziale + spaziale*.

## 3. Unicast vs Broadcast vs Multicast

| Modalità | Significato |
|----------|-------------|
| **Unicast** | comunicazione **uno-a-uno** |
| **Broadcast** | comunicazione **uno-a-tutti** |
| **Multicast** | comunicazione **uno-a-molti** — implica un **criterio di selezione** dei "molti" |

## 4. ContractNet Protocol

Un protocollo semplice per **aste e negoziazioni**.

- **2 ruoli**: *initiator* e *contractor*.
- **5 tipi di messaggio**: CFP, proposal, award, accept, result.
- **5 fasi**:
  1. **Call for Proposals** — l'initiator broadcasta/multicasta una **CFP**, tipicamente con deadline + task.
  2. **Proposal Submission** — i contractor inviano proposte (tipicamente con costo stimato).
  3. **Proposal Evaluation** — l'initiator valuta le proposte e sceglie la migliore.
  4. **Award Contract** — l'initiator assegna il contratto al contractor scelto, che lo accetta.
  5. **Contract Execution** — il contractor esegue il contratto e restituisce il risultato.
- Casi non mostrati ma da gestire: **assenza di proposte** (nessun contractor), **nessuna proposta scelta**, **contractor che rifiuta** il contratto.

## 5. FIPA — Foundation for Intelligent Physical Agents

**FIPA** è un ente di **standardizzazione** per i sistemi ad agenti.

- Analogia (molto grossolana): **agente ≈ componente distribuito** in un sistema distribuito.
- FIPA ha standardizzato molti interaction protocol per agenti: *contract net*, *request-response*, *subscription*, *auction*, ecc.

## Intuizione

Questi protocolli sono i "mattoni di interazione" riusabili: scegliere il pattern giusto è parte del **Design** ([[se-workflow-distribuito|passo 3]]). La differenza chiave tra request-response e pub-sub è il **grado di disaccoppiamento** ([[architectural-styles|uncoupling]]): nel primo client e server si conoscono e sono compresenti; nel secondo (con broker) publisher e subscriber non si conoscono e possono non essere compresenti.

## Connessioni

- [[interaction-patterns]] — la teoria (ruoli, messaggi, rappresentazioni) di cui questa è il catalogo.
- [[infrastruttura-e-componenti]] — broker, queue, MOM, client/server: i componenti che realizzano questi protocolli.
- [[architectural-styles]] — **(M8/M2)** request-response↔layered/object-based; pub-sub↔event-based.
- [[middleware]] — **(C4)** il MOM/broker che media pub-sub.
- [[se-workflow-distribuito]] — la scelta dei protocolli è un concern di Design/Implementation.
- [[features-design-distribuito]] — heartbeat e retry sono interazioni "di sistema" sopra request-response.
- [[socket]] · [[stream-vs-datagram-sockets]] — **(M2)** il substrato: RPC/RMI su stream/TCP, broadcast/multicast su datagram/UDP.

## Sorgenti

- `raw-sources/preliminaries_slides.pdf` (slide 43–48 — *Common Interaction Protocols*), G. Ciatto, *Preliminaries about Distributed Systems Engineering*, Distributed Systems — Module 2, A.Y. 2025/2026.
- Riferimenti: FIPA Interaction Protocols (http://www.fipa.org/repository/ips.php3).
