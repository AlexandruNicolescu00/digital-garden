---
title: "Feature che Impattano il Design dei Sistemi Distribuiti"
course: Sistemi Distribuiti
category: concept
topics: [ridondanza, failover, checkpoint, consensus, heartbeat, autenticazione, partitioning, fault-tolerance]
difficulty: intermedio
sources: [preliminaries_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Feature che Impattano il Design dei Sistemi Distribuiti

## Definizione

Sono **otto feature trasversali** che, quando presenti come requisito, impattano l'**infrastruttura**, gli **interaction pattern** o l'**architettura** di un sistema distribuito. Sono la versione *ingegneristica/operativa* (Module 2, Ciatto) di temi trattati teoricamente altrove nel corso: ciascuna è una risposta ricorrente alle **domande di Design** del [[se-workflow-distribuito|workflow SE]].

> Filo conduttore: quasi tutte servono la **[[dependability|dependability]]** (fault tolerance + availability), pagandola in **complessità**.

## Le otto feature

### 1. Redundancy (ridondanza)
**Cosa**: replicare **dati**, **servizi** o **hardware** su più nodi.
- **Why**: fault tolerance (anche evitando perdita dati), availability (distribuzione del carico), scalabilità.
- **How**: **Replication** (tutti i nodi hanno *tutti* i dati) vs **Sharding** (ogni nodo ha *una parte* dei dati).
- **Implicazioni**: la replica dei **dati** apre i problemi di [[consistency|consistenza]] → [[agreement-consensus|consensus]] *oppure* [[infrastruttura-e-componenti|master–slave]]; la replica dei **servizi** richiede load balancing + infrastruttura più complessa; meglio **server stateless + database stateful**.
- → pagina dedicata: [[replicazione]] (M3); *replication vs sharding*.

### 2. Failover
**Cosa**: quando un nodo/servizio primario fallisce, un **backup** subentra.
- **Active-passive**: il backup diventa attivo solo al fallimento del primario.
- **Active-active**: tutte le repliche sono attive, il traffico è rerouted in caso di guasto.
- **How**: un **[[infrastruttura-e-componenti|load balancer / proxy]]** ridirige il traffico ai backup; serve un meccanismo di **rilevamento guasti** (→ heartbeat, feature 5).
- **Implicazioni**: come per la replica dei servizi.

### 3. Checkpoints & Rollback Recovery
**Cosa**: salvare periodicamente lo stato di un processo (**checkpoint**), per poter **ripristinare** l'ultimo stato valido in caso di guasto (**rollback recovery**). Enfasi sull'essere **automatico**.
- **Why**: a volte è difficile *prevenire* una situazione negativa → la si **recupera** quando accade.
- **How**: (a) progettare il sistema per **snapshottare/ripristinare** lo stato automaticamente; (b) tracciare le **variazioni** anziché gli stati, e calcolare lo stato dalle variazioni — es. **CQRS** (Command Query Responsibility Segregation).
- **Implicazioni**: richiede scelte di modellazione/architettura/infrastruttura ad-hoc.
- → pagine dedicate: [[checkpointing-e-logging]], [[global-state-consistency]], algoritmi [[tamir-sequin-checkpointing]] / [[chandy-lamport-snapshot]] (C2).

### 4. Consensus
**Cosa**: un protocollo che fa **accordare** alcuni nodi (tipicamente server o database) su una **decisione comune** — *quale operazione eseguire*, *chi eleggere come leader*… — **anche in presenza di guasti**.
- Guasti considerati: **crash** (un nodo smette di rispondere) e **bizantini** (un nodo invia informazione errata, per errore o deliberatamente).
- **Why**: consistenza (stessa vista dei dati), fault tolerance, data redundancy.
- **How**: protocolli **Byzantine Fault Tolerant** (es. [[pbft|PBFT]]) e **Crash Fault Tolerant** (es. [[paxos|Paxos]], Raft).
- **Implicazioni**: design più complesso (i client devono sapere quale/i replica contattare); **latenza** agli occhi del client (il consenso avviene tra request e response).
- → pagine dedicate: [[agreement-consensus]], [[consensus-problemi]], [[byzantine-fault-tolerance]] (C3/C4).

### 5. Heart-beats, Timeout, Retries
**Cosa**: meccanismi base di **rilevamento dei guasti**.
- **Heart-beat**: segnale periodico tra nodi per accertare che siano *vivi e responsivi*; se un nodo smette di inviarli entro il periodo, è considerato **morto**. È un messaggio (quasi) vuoto: conta solo la sua **ricezione**.
- **Timeout**: tempo necessario per marcare *localmente* un'operazione remota come fallita (es. nodo irraggiungibile).
- **Retry**: un singolo fallimento spesso non basta (timeout corto, sfortuna) → conviene **ritentare**; parametri: *max retries* e *delay* (eventualmente variabile, cfr. **exponential back-off**).
- **How**: connessione aperta con invio periodico, oppure messaggi senza connessione; timeout + soglia di retry per marcare l'irraggiungibilità; decidere *cosa fare* in base a quali/quanti nodi sono irraggiungibili (prioritizzare consistenza? availability? → [[teorema-cap|CAP]]).
- **Nota**: utile **anche** con protocolli di rete affidabili (es. TCP).

### 6. Authorization & Authentication
**Cosa**:
- **Authentication**: permettere ai nodi (inclusi i client degli utenti) di **riconoscersi e distinguersi**; identificare gli utenti legittimi.
- **Authorization**: un server **concede o nega** l'accesso a risorse in base a identità/ruolo del client autenticato.
- **Why**: access control (chi può fare cosa), monitoring (chi sta facendo cosa), prerequisito per molti aspetti di cyber-security.
- **How**: registrare utenti/ruoli con credenziali; un server genera **session token** su richiesta; i nodi includono il token nelle interazioni successive, sanno **verificarlo** ed **enforcano** l'access control sul suo contenuto.
- **Implicazioni**: serve un **authentication server** (spesso con database), **crittografia** per i token, un meccanismo di access control (ACL, **RBAC**, ABAC…).
- → strumenti crittografici affini: [[strumenti-crittografici-blockchain]] (C4).

### 7. Data Partitioning
**Cosa**: cloni della stessa funzionalità, ciascuno a copertura di una **partizione geografica** (es. `amazon.uk` vs `amazon.it`, `google.fr` vs `google.com`). I cloni **non sono consistenti tra loro** perché i dati sono partizionati su base geografica; meccanismi indirizzano l'utente verso la partizione più vicina.
- **Why**: fault tolerance (un guasto in una regione non propaga), availability e load balancing (divide-and-conquer); a volte **obbligo normativo** (cfr. **GDPR**, art. 44).
- **How**: stessa infrastruttura deployata in luoghi diversi; eventuali meccanismi per connettere i cloni (nascondendo il partizionamento agli utenti).
- **Implicazioni**: procedure di deployment **riproducibili e parametriche** (possibilmente automatizzate); i cloni dovrebbero essere consapevoli dell'esistenza degli altri.
- ⚠️ Da non confondere con lo **sharding** (feature 1): il *data partitioning* qui è **geografico e a livello di sistema** (cloni quasi indipendenti); lo *sharding* è la suddivisione dei dati *dentro* un singolo store replicato.

## Intuizione

Queste feature sono il **ponte tra la teoria e la pratica** del corso: ogni voce ha una pagina teorica "a monte" (replicazione, consenso, checkpointing, dependability, CAP) e qui ne appare la **declinazione operativa** con i trade-off di design. Il messaggio ricorrente: ogni meccanismo di robustezza **aggiunge complessità** e spesso impone una scelta nel triangolo [[teorema-cap|CAP]].

## Connessioni

- [[se-workflow-distribuito]] — queste feature rispondono alle domande di Design/Deployment.
- [[infrastruttura-e-componenti]] — load balancer↔failover; master–slave↔replicazione; queue↔persistenza messaggi.
- [[dependability]] · [[mezzi-dependability]] — **(M1)** la cornice: fault avoidance/detection/removal/tolerance; ridondanza.
- [[replicazione]] · [[replica-management]] — **(M3)** replication vs sharding; consistenza delle copie.
- [[checkpointing-e-logging]] · [[protocolli-recovery]] — **(C2)** checkpoint & rollback recovery.
- [[agreement-consensus]] · [[paxos]] · [[pbft]] · [[byzantine-fault-tolerance]] — **(C3/C4)** consenso CFT/BFT.
- [[teorema-cap]] — **(C1)** la scelta consistency vs availability in caso di partizione.
- [[modelli-di-fallimento]] · [[classificazione-guasti]] — **(M1)** crash vs bizantini: cosa i meccanismi tollerano.
- [[distributed-pong]] · [[infrastrutture-sistema-distribuito]] — **(M2)** queste feature (heartbeat, timeout/retry, failover, replicazione) applicate al caso studio.

## Sorgenti

- `raw-sources/preliminaries_slides.pdf` (slide 69–76 — *Features Impacting DS Design*), G. Ciatto, *Preliminaries about Distributed Systems Engineering*, Distributed Systems — Module 2, A.Y. 2025/2026.
