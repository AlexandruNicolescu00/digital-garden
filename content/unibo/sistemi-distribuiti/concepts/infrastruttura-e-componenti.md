---
title: "Infrastruttura e Componenti Infrastrutturali"
course: Sistemi Distribuiti
category: concept
topics: [infrastruttura, nomenclatura, client-server, proxy, load-balancer, broker, queue, mom, master-worker]
difficulty: base
sources: [preliminaries_slides.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Infrastruttura e Componenti Infrastrutturali

## Definizione

L'**infrastruttura** è *l'insieme di facilities hardware, software e di networking che permettono ai molti pezzi di un sistema distribuito di comunicare e inter-operare attraverso una rete*: la **backbone** (spina dorsale) dei componenti infrastrutturali del sistema.

Proprietà chiave:
- **Riusabilità trasversale** — SD con funzionalità diversissime possono poggiare su infrastrutture **simili** (lo stesso *kit* di mattoni).
- **Trasparenza** — l'infrastruttura è **trasparente agli utenti finali** (non la vedono) ma **essenziale** per il funzionamento. *Esempio:* il tuo social network preferito appare come una dashboard sul telefono, ma — dove sono memorizzati i dati? dove stanno i messaggi *dopo* l'invio e *prima* della consegna? dove avviene l'elaborazione? Computazioni diverse avvengono su componenti infrastrutturali diversi, secondo il loro **ruolo**.

Un **componente infrastrutturale** è un'**unità software** (un *processo* in senso OS) che gioca un **ruolo preciso** nel sistema; il ruolo dipende dallo *scopo* del componente e da *come* interagisce.

## Nomenclatura: il catalogo dei componenti

### Sinonimi neutri
- **Node** (nodo) — un componente infrastrutturale per cui il **ruolo non è rilevante**.
- **Peer** — un componente per cui il ruolo **non è specificato**, perché *tutti i componenti giocano lo stesso ruolo* (architetture peer-to-peer).

### Client e Server
- **Server** — componente con un **nome/indirizzo noto** che **risponde** alle richieste dei client; *ascolta (attende)* richieste remote ed espone tipicamente un'**interfaccia** (le richieste che sa servire).
- **Client** — componente che **invia richieste** ai server e ne attende le risposte; è chi **inizia** l'interazione, e può esporre un'interfaccia verso gli utenti.

### Proxy e Cache
- **Proxy** — un server che fa da **gateway** verso un altro server: intercetta le richieste dei client e le inoltra al server reale, e viceversa per le risposte; può **cachare** le risposte per ridurre il carico.
- **Cache server** (o *cache*) — un proxy che esegue caching.

### Load Balancer
- **Load Balancer** — un **proxy** che **distribuisce** le richieste in arrivo su **più server**, secondo una *policy* di distribuzione: **round-robin**, **least connections**, **least response time**, ecc. → meccanismo base di [[features-design-distribuito|failover e scalabilità]].

### Broker, Producer, Consumer
- **Broker** — un server che **media la comunicazione** tra **producer** e **consumer** di dati (messaggi): riceve messaggi dai producer e li inoltra a uno o più consumer. Assunzione comune: i consumer **dichiarano il proprio interesse** a ricevere messaggi.
- **Producer** / **Consumer** — chi invia / riceve messaggi (via broker). Lo **stesso componente** può essere simultaneamente producer e consumer.

### Queue (coda)
- **Queue** — struttura dati in cui i messaggi sono memorizzati in modo **FIFO**: i messaggi sono consumati **nell'ordine** di produzione e **non vanno persi** se i consumer non sono disponibili (storage).

### Message-Oriented Middleware (MOM)
- **MOM** — un broker con **più canali** per i messaggi: i messaggi sullo stesso **topic** vanno sullo stesso canale; i consumer si **iscrivono** ai canali.
- **Topic** — etichetta dei messaggi che permette ai producer di controllare quali consumer ricevono il messaggio, ai consumer di filtrare, al broker di **instradare**. La maggior parte delle tecnologie MOM implementa i canali con **queue**. → è il [[middleware]] dello stile [[architectural-styles|event-based / pub-sub]].

### Database
- **Database** — un server **specializzato** nello storage/retrieval di dati. Nelle architetture **three-tier** è il *terzo tier*, e fa da **server per il server** (che a sua volta agisce da client verso il DB).

### Master–Worker (Master–Slave / Leader–Follower)
- **Master** — un server che **coordina** il lavoro di più worker: distribuisce il lavoro e raccoglie i risultati.
- **Worker** — un server che **esegue** il lavoro assegnato dal master.
- Casi d'uso: **master–worker** per la *computazione parallela*; **master–slave** per la *replicazione dei dati*.

## Intuizione

Questi nomi non sono tecnologie ma **ruoli**: lo stesso processo può essere "server" rispetto a un client e "client" rispetto a un database. Riconoscere i ruoli è il primo passo del **Design** ([[se-workflow-distribuito|passo 3]]): scegliere *quali* componenti introdurre e *come* farli interagire ([[interaction-patterns]]). Gli [[architectural-styles|stili architetturali]] non sono che **combinazioni ricorrenti** di questi componenti e dei loro pattern di interazione.

## Connessioni

- [[se-workflow-distribuito]] — i componenti sono le risposte al passo di Design.
- [[interaction-patterns]] · [[protocolli-interazione-comuni]] — *come* questi componenti si parlano (request-response, pub-sub, ContractNet…).
- [[architectural-styles]] — **(M8)** ogni stile è caratterizzato da *quali* componenti e *quali* pattern usa.
- [[middleware]] — il MOM/broker è middleware: media l'interazione.
- [[features-design-distribuito]] — load balancer↔failover; master–slave↔replicazione; queue↔persistenza dei messaggi.
- [[replica-management]] · [[replicazione]] — **(M3)** master–slave = replicazione; *sharding* vs replication.
- [[componenti-connettori]] — **(M8)** la versione *astratta* (componente ⇄ connettore) di questa nomenclatura *concreta*.
- [[socket]] — **(M2)** sotto i ruoli client/server *logici* ci sono i client/server *socket*: gli endpoint `IP:porta` con cui i componenti si parlano davvero.

## Sorgenti

- `raw-sources/preliminaries_slides.pdf` (slide 26–37 — *Nomenclature*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
