---
title: "Replicazione (Replication)"
course: Sistemi Distribuiti
category: concept
topics: [replicazione, scaling, consistency, performance, fault-tolerance]
difficulty: intermedio
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Replicazione (Replication)

## Definizione

La **replicazione** è il mantenimento di **più copie** della stessa risorsa (dato, servizio o processo) su nodi distinti di un sistema distribuito. Storicamente i primi oggetti a essere distribuiti — e quindi replicati — sono i **dati**, perciò il problema fondante è come garantire la **consistenza dei dati** tra copie distribuite.

## Intuizione

Si replica per tre grandi famiglie di ragioni:

1. **Affidabilità (reliability / fault tolerance).** Se una replica va in crash, il sistema continua a operare commutando su un'altra replica. Mantenere copie multiple protegge anche dalla corruzione dei dati: con 3 copie e ogni read/write eseguito su tutte, si può **mascherare una singola write fallita** prendendo per buono il valore restituito da almeno 2 copie (voting/majority).
2. **Prestazioni (performance).** Avvicinare una copia ai processi che la usano riduce il tempo di accesso.
3. **Scaling**, in due dimensioni:
   - **in numero** — un server replicato distribuisce il carico tra più copie quando aumentano i processi che accedono;
   - **in area geografica** — copie vicine ai client riducono la latenza percepita.

A prima vista i benefici (reliability, fault tolerance, accessibility, performance, scalability) sembrano *indiscutibili*. La domanda critica del modulo è: **basta replicare ed essere contenti?** No.

## Il rovescio: i costi della replicazione

Replicare introduce problemi:

- **Costi.** Servono nuove macchine (acquisto, spazi fisici, personale, manutenzione), spesso collocate vicino ai processi che accedono ai dati.
- **Computazione e banda.** Una replica è utile solo se le copie restano **consistenti** tra loro (e con l'originale): garantirlo richiede *computazione dentro le repliche* e *comunicazione tra repliche*, quindi banda.
- **Consistenza (il problema principale).** Appena una copia viene modificata diventa diversa dalle altre; le modifiche vanno propagate a tutte le copie. **Quando e come** propagarle determina il costo della replicazione.

### Esempio: caching delle pagine Web
Il browser memorizza localmente (cache) una pagina già scaricata e la restituisce subito → ottimo tempo di accesso percepito. Problema: se la pagina è cambiata sul server, la copia in cache è *stale*. Soluzioni, entrambe imperfette:
- **vietare il caching** → degrada le prestazioni percepite;
- **mettere il server in carico di invalidare/aggiornare le cache** → il server deve tracciare tutte le cache e mandare messaggi: carico pesante, **scarsa scalabilità**.

## La replicazione come tecnica di scaling, e il suo paradosso

La replicazione/caching è una tecnica di scaling standard, ma soffre di due tensioni:

- **Rapporto accesso/aggiornamento.** Se un processo P legge la replica N volte/s ma la replica è aggiornata M volte/s con N ≪ M, la maggior parte delle versioni aggiornate **non sarà mai letta** → comunicazione inutile. Vale la pena tenere la copia locale? O serve un'altra strategia di update?
- **Mantenere le repliche consistenti può diventare *esso stesso* un problema di scalabilità.** Intuitivamente una collezione di copie è consistente quando le copie sono *sempre uguali*, cosicché una read su qualunque copia restituisca lo stesso risultato. Per ottenerlo, ogni update su una copia *qualsiasi* deve essere propagato a **tutte** prima di operazioni successive → molta computazione e comunicazione.

> Questo è il regime della **replicazione sincrona**, il cui risultato è la cosiddetta **tight consistency** (consistenza "stretta"): l'update è eseguito su tutte le copie come **singola operazione atomica / transazione**.

Implementare l'atomicità su molte repliche **quando le operazioni devono anche essere veloci** è intrinsecamente difficile: tutte le repliche devono prima **accordarsi su quando** eseguire localmente l'update — ad es. accordarsi sull'**ordinamento globale** delle operazioni distribuite. La sincronizzazione globale costa molto tempo di comunicazione e — *è sempre possibile?* Oppure si sta sbattendo contro un altro insieme di **teoremi di impossibilità**? (Sì: → [[teorema-cap]] e, soprattutto, il **teorema [[flp-impossibility|FLP]]** sul consenso, C3. L'accordo sull'ordinamento globale **è** un problema di [[agreement-consensus|consenso]].)

## Il dilemma replicazione/consistenza

- da un lato: replicazione e caching **alleviano** i problemi di scalabilità migliorando le prestazioni;
- dall'altro: tenere tutte le copie consistenti richiede **sincronizzazione globale**, intrinsecamente costosa in prestazioni.

> *Is the cure worse than the disease?* Non esiste una risposta generale valida per ogni sistema distribuito — ma quasi sempre **una** soluzione esiste.

### La via d'uscita: rilassare la consistenza
Spesso l'unica soluzione reale è **rilassare i vincoli di consistenza**: non pretendere che gli update siano operazioni atomiche, evitando così la sincronizzazione globale (istantanea) e guadagnando prestazioni. Prezzo: le repliche **non sono sempre uguali ovunque**. Quanto si possa rilassare dipende dai **pattern di accesso e aggiornamento** dei dati e dallo **scopo d'uso**. Da qui la pluralità dei modelli di consistenza (→ [[consistency-model]]).

## Cosa si replica: non solo dati

- **Data replication** — il caso storico e di base.
- **Service replication** — si replicano *funzioni/servizi*, che possono o meno insistere sullo stesso data store: due livelli di replicazione, con due modelli di replica & consistenza (→ [[replica-management]]).
- **Process replication** — in contesti distribuiti **mobili** si replicano i *processi* (richiede *cloning* e meccanismi di più alto livello come *goal-passing*).

## Connessioni

- [[consistency-model]] — il contratto che governa quanto le repliche possono divergere
- [[teorema-cap]] — il "teorema di impossibilità" verso cui punta la tight consistency (C×A×P)
- [[eventual-consistency]] — l'esito tipico del rilassamento dei vincoli
- [[replica-management]] — dove/quando/da chi piazzare le repliche
- [[motivazioni-sistemi-distribuiti]] — fault tolerance e sharing come motori del distribuito (M0)
- [[acid-vs-base]] — tight/atomic vs rilassato è la stessa tensione ACID vs BASE
- [[agreement-consensus]] · [[paxos]] · [[state-machine-replication]] — **(C3)** il consenso come meccanismo per la consistenza delle repliche; SMR
- [[flp-impossibility]] — **(C3)** perché l'ordinamento globale atomico in asincrono non è gratis
- [[scalabilita]] — **(M4)** la replicazione (e il **caching**) come tecnica di scalabilità geografica; replication = decisione dell'owner, caching = decisione del client
- [[trasparenza]] — **(M4)** replication transparency: le repliche devono apparire "una sola cosa" (stesso nome, stesso stato)
- [[code-mobility]] — **(C6)** il **cloning** (strong mobility via copia parallela) come forma di replicazione trasparente dei processi

## Da approfondire (moduli futuri)

- Logical time / clock logici e vettoriali per l'ordinamento globale (ancora da ingestare nel wiki).
- Esempi concreti via Kubernetes (lezione CX di M. Matteini) — **coperti**: → [[kubernetes]], [[teoria-vs-kubernetes]], [[kubernetes-hpa-example]].
- Consensus per realizzare la tight consistency — **coperto** (C3): → [[agreement-consensus]], [[paxos]], [[state-machine-replication]].

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 6–19, 42–44; basato su Tanenbaum & van Steen, 2017)
