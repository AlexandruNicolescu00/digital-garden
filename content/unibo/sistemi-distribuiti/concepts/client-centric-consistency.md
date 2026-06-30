---
title: "Client-centric Consistency Models"
course: Sistemi Distribuiti
category: concept
topics: [consistency, client-centric, mobile, repliche]
difficulty: intermedio
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Client-centric Consistency Models

## Definizione

I **modelli di consistenza client-centric** garantiscono che, ogni volta che un client si connette a una **nuova replica**, quella replica sia **aggiornata coerentemente con i precedenti accessi del client** agli stessi dati su altre repliche/siti.

La consistenza **non** è più riferita direttamente alla risorsa (alla sua natura/dinamica), ma alla **vista che ciascun client ha** della risorsa.

## Intuizione: cambio di prospettiva

Scenario tipico — **mobile computing**:

- un client si connette a **repliche diverse nel tempo**;
- le differenze tra repliche dovrebbero essere rese **trasparenti** al client;
- **non** ci sono particolari problemi di update simultanei (a differenza del data-centric): l'attenzione è sulla coerenza *della sequenza di accessi di un singolo client*.

## Le quattro garanzie client-centric

Sia *x* un data item e si considerino operazioni **dello stesso processo/client**:

| Garanzia | Condizione | Idea | Esempio |
|----------|-----------|------|---------|
| **Monotonic Reads** | se un processo legge *x*, ogni read successiva su *x* restituirà **quello stesso valore o uno più recente** | non si "torna indietro" nelle letture | database e-mail distribuito |
| **Monotonic Writes** | una write su *x* è **completata prima** di qualunque operazione successiva su *x* dello stesso processo | l'**ordine degli update** è preservato sulle repliche | libreria software in sviluppo |
| **Read Your Writes** | l'effetto di una write su *x* sarà **sempre visto** da una read successiva su *x* dello stesso processo | evita l'effetto "aggiornamento di pagina web fallito" | aggiornamento password |
| **Writes Follow Reads** | una write su *x* che segue una read su *x* (stesso processo) avviene **sullo stesso valore letto o uno più recente** | le write agiscono solo su dati **aggiornati** | commenti ai post su Facebook |

## Eventual Consistency

L'**eventual consistency** è il modello client-centric "di base" per data store grandi e distribuiti con **quasi nessun conflitto di update** (vedi pagina dedicata [[eventual-consistency]]): se per un po' non avvengono update, gradualmente tutte le repliche diventano consistenti. Le quattro garanzie sopra **rafforzano** l'eventual consistency aggiungendo precise promesse *per-client*.

## Connessioni

- [[consistency-model]] — la cornice (contratto) e la dicotomia data-centric / client-centric
- [[eventual-consistency]] — il modello client-centric base, già introdotto in C1 (BASE)
- [[data-centric-vs-client-centric]] — confronto delle due famiglie
- [[causalita]] — writes-follow-reads e monotonic writes catturano dipendenze causali lato-client

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 35–40)
