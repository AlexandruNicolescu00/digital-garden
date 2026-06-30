---
title: "Workflow di Software Engineering per Sistemi Distribuiti"
course: Sistemi Distribuiti
category: concept
topics: [software-engineering, workflow, design, deployment, ingegneria]
difficulty: base
sources: [preliminaries_slides.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Workflow di Software Engineering per Sistemi Distribuiti

## Definizione

Il **workflow di software engineering** è la sequenza di attività con cui si concepisce, costruisce, rilascia e mantiene un prodotto software. Distribuire un sistema **non aggiunge una fase**: cambia (e complica) **ogni** fase del workflow standard, perché ogni passo deve ora rispondere a domande su *dove*, *quando*, *come* e *quanto spesso* i componenti interagiscono in rete.

> Questa è la lente **ingegneristica** (Module 2, Ciatto) sui sistemi distribuiti: complementare alla lente *teorica/ontologica* dei moduli M/C (Module 1, Omicini). Cfr. [[sistema-distribuito]] (le due facce: *modellazione* = computer science, *costruzione* = computer engineering).

## Il workflow standard (recap)

I 9 passi del ciclo di vita del software:

1. **Use case collection** — negoziare le aspettative con i clienti/stakeholder.
2. **Requirements analysis** — produrre la lista dei requisiti, ciascuno con i propri **acceptance criteria**.
3. **Design** — produrre il *blueprint*; in particolare **modelling**: quali entità del mondo reale sono rappresentate? come si comportano? come interagiscono?
4. **Implementation** — scrivere il codice che reifica il design.
5. **Verification** — verificare che il software soddisfi i requisiti (*automated testing*, *acceptance testing*).
6. **Release** — rendere disponibile una particolare versione ai clienti.
7. **Deployment** — installare e attivare il software.
8. **Documentation** — manuali e guide.
9. **Maintenance** — fixare bug, migliorare, adattare a nuovi requisiti.

## Cosa cambia con la distribuzione (passo per passo)

Solo i **concern aggiuntivi** introdotti dalla distribuzione:

| Passo | Domande/concern specifici dei SD |
|-------|----------------------------------|
| **1. Use case collection** | *Dove* sono gli utenti? Quando/con che frequenza interagiscono? Con quali **dispositivi**? Il sistema deve memorizzare dati utente — quali e *dove*? Tipicamente esistono **molti ruoli** diversi. |
| **2. Requirements analysis** | Le risposte sopra implicano **vincoli tecnici**: dove memorizzare i dati? con più data center, come tenerli **consistenti**? il sistema dovrà **scalare**? come gestire i guasti e fare recovery? Tutto ciò genera requisiti/criteri di accettazione aggiuntivi. |
| **3. Design** | Quali **componenti infrastrutturali** servono e quanti ([[infrastruttura-e-componenti|client, server, load balancer, cache, DB, broker, queue, worker, proxy, CDN…]])? Come si distribuiscono in rete? Come le entità di dominio mappano sui componenti? Come **comunicano** (quali [[interaction-patterns|interaction pattern]])? Quanti copie dei dati? **In caso di partizione, available o consistent?** ([[teorema-cap|CAP]]). Come si **trovano** (service discovery, naming, load balancing) e si **riconoscono** (autenticazione/autorizzazione)? Cosa fare quando un componente fallisce — *è davvero un guasto?* (retry, back-off, graceful degradation). |
| **4. Implementation** | Quali **protocolli di rete** (UDP, TCP, HTTP, WebSocket, gRPC, XMPP, AMQP, MQTT…)? Rappresentazione dei dati in transito (JSON, XML, YAML, Protocol Buffers…)? Storage persistente (relazionale, documento, key-value, graph…)? Query (SQL/NoSQL)? Autenticazione (OAuth, JWT) e autorizzazione (RBAC, ABAC)? |
| **5. Verification** | Come fare **unit-test** di componenti distribuiti? Il test di **integrazione** è cruciale. End-to-end test (ambiente di produzione vs test). La **deployment automation** serve per testare in ambienti production-like. |
| **6. Release** | I componenti hanno **cicli di rilascio e versioni propri**, e devono essere **resilienti alla coesistenza di più versioni**; preferire **rolling update** ai *big bang update*. |
| **7. Deployment** | *Dove* deployare (cloud, on-premises, hybrid)? *Come* (container, VM, bare metal)? Come **scalare** (orizzontale, verticale, auto-scaling)? Come **monitorare** (log, metriche, trace) e **mettere in sicurezza** (firewall, cifratura, certificati)? Tutto va **automatizzato** (esistono tool/aziende solo per questo). |
| **8. Documentation** | **Protocolli e formati dati** vanno documentati bene, così che terze parti creino componenti compatibili (es. la specifica di una Web API è pubblica). |
| **9. Maintenance** | **Monitoraggio continuo** di performance e availability; l'**issue tracking** è non banale (può richiedere sotto-sistemi ad-hoc); il *sunsetting* delle vecchie versioni è cruciale — l'**End-of-Life va pianificato, non brusco**. |

## Intuizione

In un sistema centralizzato molte assunzioni sono gratuite: un solo processo, una sola memoria, un solo orologio, fallimento "tutto-o-niente". La distribuzione **rompe** queste assunzioni (→ [[ordinamento-parziale-eventi]], [[pitfalls-sistemi-distribuiti|le 8 fallacie]]) e ogni rottura riappare come una **domanda di progetto** in qualche fase del workflow. Le risposte ricorrenti a queste domande sono i **componenti infrastrutturali**, gli **interaction pattern**, gli **stili architetturali** e le **feature di design** trattati nelle pagine collegate.

## Connessioni

- [[infrastruttura-e-componenti]] — i mattoni con cui si risponde al passo 3 (Design).
- [[interaction-patterns]] · [[protocolli-interazione-comuni]] — *come* i componenti comunicano (passi 3–4).
- [[socket]] · [[stream-vs-datagram-sockets]] — **(M2)** la scelta del protocollo di rete (UDP/TCP) e dei socket nel passo di Implementation.
- [[distributed-pong]] — **(M2)** il **capstone**: questo workflow applicato end-to-end per distribuire un videogioco.
- [[infrastrutture-sistema-distribuito]] — **(M2)** la decisione di Design su *quale infrastruttura* (local/centralized/brokered/replicated).
- [[features-design-distribuito]] — le feature trasversali (ridondanza, failover, consenso, heartbeat, auth, partizionamento) che impattano design e deployment.
- [[architectural-styles]] · [[software-architecture]] — la vista architetturale (passo 3, modelling) — **(M8)** la teoria di cui Module 2 dà la versione pratica.
- [[teorema-cap]] — la scelta available/consistent emerge esplicitamente nel design.
- [[obiettivi-sistemi-distribuiti]] · [[quality-attributes]] — **(M4/CX)** scalabilità, availability ecc. come requisiti non-funzionali.
- [[kubernetes]] · [[containerization]] — **(CX)** l'automazione di deployment/scaling/monitoring del passo 7.

## Sorgenti

- `raw-sources/preliminaries_slides.pdf` (slide 15–25), G. Ciatto, *Preliminaries about Distributed Systems Engineering*, Distributed Systems — Module 2, A.Y. 2025/2026.
