---
title: "Caso Studio: Distributed Pong (capstone Module 2)"
course: Sistemi Distribuiti
category: example
topics: [case-study, pong, pygame, event-based, udp, availability, coordinator-terminal, mvc]
difficulty: avanzato
sources: [pong_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Caso Studio: Distributed Pong (capstone Module 2)

> **Running example** che attraversa l'intero toolkit ingegneristico del Module 2 (Ciatto): partire dal gioco **Pong** (Python + PyGame) e renderlo **distribuito**, esibendo *in concreto* il [[se-workflow-distribuito|workflow SE]], la scelta dell'[[infrastrutture-sistema-distribuito|infrastruttura]], lo stile [[architectural-styles|event-based]], i [[socket|socket UDP]], le [[features-design-distribuito|feature di design]] e il trade-off [[teorema-cap|availability/consistency]]. È il **capstone del Module 2**, come la [[blockchain]] (C4) lo è del Module 1.
>
> Codice: `unibo-fc-isi-ds/dpongpy` (`pip install dpongpy`).

## Problema

Implementare una versione **distribuita** del classico **Pong**: fino a 4 *paddle* (uno per giocatore) e una *ball* che rimbalza su paddle e muri (*wall*), con giocatori che possono giocare **insieme da postazioni diverse**. Requisito di esperienza: **latenza bassa** (gioco fluido, niente lag su palla/paddle).

## Tecnica applicata

L'intero **workflow di ingegneria dei sistemi distribuiti** ([[se-workflow-distribuito]]) applicato passo per passo, sopra una base ben modellata col pattern **MVC**.

### Base non-distribuita: il game loop e l'MVC
Il cuore di ogni videogioco è il **game loop**: ciclo continuo che (1) **processa l'input**, (2) **aggiorna lo stato**, (3) **renderizza**, (4) **simula il passare del tempo** (gli oggetti si muovono anche senza input); un `wait` finale regola il **frame rate** (~30–60 fps). PyGame fornisce una **coda di eventi** (`pygame.event`, tipo + attributi). Per pulizia si separano (clean code → **MVC**):
- **Model** (`Pong`): `GameObject` (con casi `Paddle`, `Ball`), `Board`, `Wall`, ausiliari `Vector2`/`Rectangle`/`Direction`. Collisioni via **bounding box** (`overlaps`/`is_inside`/`intersection_with`/`hits`); **bouncing** = invertire la componente della velocità sull'asse di collisione + ricollocare la palla fuori dall'ostacolo.
- **Controller** = `InputHandler` (interpreta input → *control event*) + `EventHandler` (consuma i control event → aggiorna il model). Astrazioni: `PlayerAction`, `ActionMap` (keycode→azione), `ControlEvent` (`PLAYER_JOIN`/`PLAYER_LEAVE`/`GAME_START`/`GAME_OVER`/`PADDLE_MOVE`/`TIME_ELAPSED`).
- **View** (`PongView`→`ScreenPongView`/`ShowNothingPongView`): disegna lo stato sullo schermo.

> 💡 **Insight I/O:** gli **input** sono dati esterni che impattano lo stato; gli **output** sono rappresentazioni dello stato percepite dall'esterno. Il **time-passing** è un evento che **non corrisponde ad alcun input utente**: il sistema evolve *anche senza input* → trattato come un "input speciale" generato dal controllo stesso. Questa è la chiave che rende il design pronto alla distribuzione.

## Soluzione (rendere Pong distribuito)

### 1–2. Use case & requirements
Utenti davanti al proprio computer su Internet/LAN; interazione **sporadica nell'avvio** ma **fittissima durante** la partita; **nessun dato da memorizzare**, ma molta informazione da scambiare; ruoli: *player* (ed eventuali *spectator*). Requisiti: **coordinare input** da giocatori diversi; supportare **join/leave a runtime**; gestire **guasti** (player offline → pausa/rimozione/freeze del paddle; componente irraggiungibile → pausa); **latenza bassa**.

### 3. Design — scelta dell'infrastruttura
Quattro topologie a confronto (→ [[infrastrutture-sistema-distribuito]]): **local**, **centralized**, **brokered**, **replicated**. Scelta: **centralized** — un **Coordinator** (server) coordina N **Terminal** (client). Il broker aggiunge un 2° SPOF e un hop di latenza (decoupling temporale = *non-goal*); la replica con consenso è *overkill* per un gioco.

Mappatura dominio→infrastruttura (stile **master-slave**):
- `PongGame` aggiornato dal **Coordinator** e **replicato** su tutti i client;
- un `PongView` + un `InputHandler` **per client** (rendering locale, input locali → al server);
- un `EventHandler` sul **Coordinator** (riceve input remoti, aggiorna lo stato).

Interazione: i client conoscono l'**IP del server**; **fiducia reciproca**, niente autenticazione; lo stato va replicato **il più frequentemente possibile** per evitare inconsistenze → **priorità all'availability sulla consistency**.

### 4. Implementation
- **Protocollo**: **UDP** ([[stream-vs-datagram-sockets|datagram socket]]) — ideale per real-time/bassa latenza; la perdita di pacchetti in un gioco è tollerabile (ma duplicazione, disordine e ritardi danno grattacapi). TCP darebbe affidabilità ma latenza + gestione di molte connessioni simultanee lato server.
- **Serializzazione**: **JSON** (per scopi didattici), poi **BSON** (binario, meno overhead). Nessun DB. Comunicazione *trust-based*: ogni client comanda **uno e un solo** paddle (policy *first-come-first-served*).

### L'architettura distribuita (event-based)
- **Coordinator** = server centrale: esegue il **game loop**, aggiorna lo stato, **non** ascolta la tastiera né renderizza; **riceve input** dai client e **invia periodicamente lo stato** aggiornato.
- **Terminal** = client: ascolta la tastiera, **invia input** al coordinator, **riceve lo stato** e lo **renderizza**.
- È uno stile **[[architectural-styles|event-based]]**: il coordinator **pubblica** eventi *"game state update"* e **consuma** eventi *"input"*; i terminal fanno il viceversa. **2 tipi di processo**, **2 canali** (update / input), attività **concorrenti** in ciascun processo.

## Errori comuni / casi limite (protocollo Join/Leave)

**Join**: il terminal manda `PLAYER_JOIN` all'avvio (scelta del lato, *first-come-first-served*); il coordinator registra il paddle e resetta la palla. **Leave (graceful)**: alla pressione di ESC il terminal manda `PLAYER_LEAVE` *prima* di terminare; il coordinator de-registra il paddle (e termina se non restano paddle). I **casi limite** (con soluzioni) sono il cuore dell'esame → raccolti in [[distributed-pong-qa]]:

| # | Caso limite | Soluzione tipica |
|---|-------------|------------------|
| 1 | Coordinator non disponibile all'avvio del terminal | il terminal termina, **oppure** attende con **timeout + max retry** ([[features-design-distribuito|retry/back-off]]) |
| 2 | Terminal crasha **prima** di `PLAYER_LEAVE` | con UDP serve **heart-beating** custom; con TCP la connessione muore e il coordinator se ne accorge |
| 3 | Distinguere terminal crashato da terminal che non manda input | **timeout** lato coordinator |
| 4 | Coordinator crasha mentre i terminal girano | **timeout** lato terminal |
| 5 | Lato già occupato (`PLAYER_JOIN` doppio) | ignorare in silenzio **oppure** rifiutare → join diventa **request-response** |
| 6 | Input per il paddle "sbagliato" | ignorare/espellere → il coordinator deve **tracciare paddle↔terminal** |

> Sono esattamente le insidie delle [[pitfalls-sistemi-distribuiti|fallacie]] e dei [[modelli-di-fallimento|modelli di guasto]]: la rete perde messaggi, un nodo "lento" è indistinguibile da uno "morto" ([[modello-sincrono-asincrono]]).

## Availability vs Consistency (l'esperimento)

Con UDP, impostando `UDP_DROP_RATE` (es. `0.2` = 20% di pacchetti persi) si **simula una partizione di rete**. Osservando il gameplay:
- **fluido** → si sta privilegiando la **[[availability]]** (è il caso del design attuale);
- **laggoso** → si sta privilegiando la **[[consistency]]**.

> 🔑 Per i videogiochi conviene **availability > consistency** ([[teorema-cap|CAP]]): nessuno storage, nessun bisogno di consistenza forte. Stesso spirito del caso [[pokemon-go-cap]].

**Esercizio "Available Distributed Pong"** (deadline 31/12/2025, +1 punto): implementare **speculative execution** lato terminal — il game-loop locale **non si blocca mai** in attesa del coordinator, continua a simulare con gli input locali e, all'arrivo di un update remoto, **sovrascrive** lo stato locale con quello remoto.

## Generalizzazione

Distributed Pong è un'istanza del problema generale: *prendere un'applicazione e distribuirla*. Lo schema è riusabile: **(1)** modella bene in locale (MVC/separazione delle responsabilità), **(2)** scegli l'infrastruttura ([[infrastrutture-sistema-distribuito|local/centralized/brokered/replicated]]) bilanciando SPOF, latenza e costo, **(3)** scegli lo stile ([[architectural-styles|event-based]] per real-time interattivo), **(4)** scegli protocollo e serializzazione ([[socket|UDP+JSON/BSON]]) secondo i requisiti, **(5)** posiziona il trade-off [[teorema-cap|CAP]]. La separazione **model/control/view** è ciò che rende possibile sostituire il "control locale" con uno "distribuito" senza riscrivere il gioco.

## Connessioni

- [[se-workflow-distribuito]] — il workflow di cui questo è la *dimostrazione end-to-end*.
- [[infrastrutture-sistema-distribuito]] — la scelta local/centralized/brokered/replicated.
- [[architectural-styles]] — Pong distribuito è **event-based** (coordinator pub/sub).
- [[socket]] · [[stream-vs-datagram-sockets]] — **UDP** per il real-time; ne eredita l'inaffidabilità.
- [[features-design-distribuito]] — heartbeat, timeout/retry, failover, replicazione master-slave.
- [[teorema-cap]] · [[availability]] · [[consistency]] — availability > consistency nei giochi; esperimento `UDP_DROP_RATE`.
- [[infrastruttura-e-componenti]] — coordinator=server, terminal=client.
- [[interaction-patterns]] · [[protocolli-interazione-comuni]] — input/update come pub-sub; join request-response.
- [[replicazione]] · [[state-machine-replication]] — stato replicato master-slave sui client.
- [[pokemon-go-cap]] — altro gioco, stessa scelta CAP (availability).
- [[distributed-pong-qa]] — le domande d'esame sui casi limite.

## Sorgenti

- `raw-sources/pong_slides.pdf` (61 slide — *Distributed Pong*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
- Codice: https://github.com/unibo-fc-isi-ds/dpongpy ; libreria PyGame.
