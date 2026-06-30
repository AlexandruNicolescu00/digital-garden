---
title: "Topologie Infrastrutturali: Local vs Centralized vs Brokered vs Replicated"
course: Sistemi Distribuiti
category: bridge
topics: [infrastruttura, topologia, spof, broker, replicazione, consensus, design]
difficulty: intermedio
sources: [pong_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Topologie Infrastrutturali: Local vs Centralized vs Brokered vs Replicated

Quando si **distribuisce un'applicazione** ([[se-workflow-distribuito|passo di Design]]), una delle prime decisioni è **come organizzare l'infrastruttura**: dove sta la logica, quanti componenti, quali *single point of failure* (SPOF). Il caso studio [[distributed-pong]] espone **quattro topologie** in ordine crescente di robustezza e costo. La scelta è un bilanciamento tra **SPOF, latenza, complessità di deploy e bisogno di consistenza**.

## Le quattro topologie

### 1. Local (nessuna infrastruttura)
Tutto su una macchina: input → model → render in un unico processo. **Nessuna distribuzione** → i giocatori non possono giocare da postazioni diverse. È il punto di partenza (l'app non-distribuita ben modellata).

### 2. Centralized
Un **server centrale** coordina N client: riceve gli input, aggiorna lo stato, lo rispedisce ai client (che renderizzano). I client conoscono l'IP del server.
- **SPOF**: **uno** (il server).
- **Deploy**: chi avvia il server? (un giocatore → start-up più complesso; hosting online → access control, costi di deploy/manutenzione). Dove collocarlo?

### 3. Brokered
Come la centralized, ma un **[[infrastruttura-e-componenti|broker]]** fa da *relay* dei messaggi tra server e client.
- **SPOF**: **due** (broker **e** server) → ha *tutti* gli svantaggi della centralized **più** la complessità del broker.
- **Contro**: dove deployare il broker? **latenza maggiore** (hop aggiuntivo); introduce **decoupling temporale** server↔client — utile in generale, ma **non-goal** per un gioco real-time.

### 4. Replicated
Come la centralized, ma il **server è replicato** e un **[[agreement-consensus|protocollo di consenso]]** tiene le repliche sincronizzate.
- **Pro**: niente SPOF sul server; alta affidabilità/consistenza.
- **Contro**: **overkill** per un'app semplice; costo di consenso (latenza, complessità). Sensato quando consistenza forte e tolleranza ai guasti del server sono requisiti reali (es. servizi critici, [[blockchain|DLT]]).

## Tabella di confronto

| Topologia | Componenti | SPOF | Latenza | Consistenza/Robustezza | Quando |
|-----------|-----------|------|---------|------------------------|--------|
| **Local** | 1 processo | — (no distribuzione) | minima | — | sviluppo/single-player |
| **Centralized** | 1 server + N client | **1** (server) | bassa | media (stato sul server) | **default** per app interattive semplici |
| **Brokered** | broker + server + N client | **2** | più alta (hop) | media + decoupling temporale | quando serve disaccoppiare in tempo (pub/sub, MOM) |
| **Replicated** | M server (consenso) + N client | **0** sul server | alta (consenso) | alta | quando consistenza/fault-tolerance del server sono requisiti |

## Sintesi: quando scegliere cosa

- **Local** finché non serve davvero la distribuzione.
- **Centralized** è il punto di equilibrio per la maggior parte delle app interattive: un solo SPOF, latenza bassa, deploy gestibile → scelto in [[distributed-pong]].
- **Brokered** quando il **decoupling temporale** (produttori/consumatori non compresenti) è un *goal* — è lo stile [[architectural-styles|event-based]]/shared data-space con [[middleware|MOM]]; per un gioco real-time è invece un costo inutile.
- **Replicated** solo se la **tolleranza ai guasti del server** e la **consistenza** valgono il prezzo del [[agreement-consensus|consenso]] — per un videogioco si preferisce **availability** ([[teorema-cap|CAP]]).

> 🔑 La progressione local→centralized→brokered→replicated è un gradiente di **robustezza vs costo/latenza**: ogni passo rimuove o sposta uno SPOF ma aggiunge componenti, hop o protocolli. La scelta giusta dipende dai **requisiti non-funzionali** ([[quality-attributes]]) e dal trade-off [[teorema-cap|CAP]].

## Connessioni

- [[distributed-pong]] — il caso studio da cui nasce questo confronto.
- [[se-workflow-distribuito]] — la scelta dell'infrastruttura è il passo di Design.
- [[infrastruttura-e-componenti]] — i mattoni (server, client, broker) di ogni topologia.
- [[architectural-styles]] — brokered ↔ event-based; replicated ↔ consenso.
- [[features-design-distribuito]] — replicazione, failover, consenso come feature trasversali.
- [[teorema-cap]] — il trade-off availability/consistency che orienta la scelta.
- [[agreement-consensus]] · [[state-machine-replication]] — il cuore della topologia replicated.

## Sorgenti

- `raw-sources/pong_slides.pdf` (slide 43–47 — *About the Distributed Pong Infrastructure*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
