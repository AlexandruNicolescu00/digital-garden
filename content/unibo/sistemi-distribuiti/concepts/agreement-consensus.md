---
title: "Agreement e Consensus"
course: Sistemi Distribuiti
category: concept
topics: [consensus, agreement, fault-tolerance, replicazione, recovery]
difficulty: avanzato
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Agreement e Consensus

## Definizione

Il **problema dell'agreement (accordo)** si pone quando i diversi componenti di un sistema hanno **viste potenzialmente divergenti** sullo stato del sistema e su ciò che vi accade. Un comportamento globale coerente si ottiene **solo** quando tutti i componenti **concordano** su tutto ciò che è rilevante (stato, eventi, …). Il punto chiave è che raggiungano la **stessa identica conclusione**, *indipendentemente da quale essa sia*.

Il **consensus** è:

> il processo con cui si raggiunge l'accordo sullo stato del sistema tra **macchine inaffidabili** connesse da reti **(possibilmente) asincrone**.

## Intuizione: perché serve il consenso

Quando dei processi falliscono, i processi **affidabili** devono accordarsi su:
- **cosa è accaduto** finora nel sistema, e
- **cosa accadrà** d'ora in poi,

così che **stato corrente ed evoluzione** siano entrambi **consistenti** (es. per la *system recovery*, → [[checkpointing-e-logging]]).

Il consenso è il cuore della **fault tolerance**: è essenziale per garantire la **consistenza delle repliche** ([[replicazione]]). Quando un crash impedisce la sincronizzazione delle repliche, il processo di recovery richiede consenso tra le repliche **sullo stato di ciascuna** e **sugli eventi** gestiti/da gestire.

> Faults sono allo stesso tempo **la sorgente dei problemi** dei sistemi distribuiti **e la ragione per cui** ne abbiamo bisogno (ridondanza). Il consenso è il modo più espressivo e generale di modellare problemi applicativi rilevanti in ambito distribuito.

## Esempi di problemi di agreement [Fischer, 1983]

- **Commit di transazioni**: i data manager di un DB distribuito devono accordarsi se **committare o abortire** una transazione distribuita (→ [[tamir-sequin-checkpointing]], 2PC).
- **File system replicato**: i nodi devono accordarsi su **dove risiedono le copie** dei file.
- **Controllo di volo**: il modulo motori e il modulo superfici di volo devono accordarsi se **continuare o abortire** un atterraggio in corso.

## Ontologia di base (il modello)

- **Componenti & connettori**: i sistemi non banali sono **concorrenti o distribuiti** (più computazioni *insieme*, logicamente o fisicamente); modellati come collezioni di **processi** (logici) o **processori** (fisici) interconnessi per interagire, via canali **one-to-one** (message-passing) o **condivisi** (shared-memory).
- **Concurrent / distributed / parallel**: la letteratura usa i termini in modo più o meno intercambiabile; quando il sistema è **indifferente alla natura fisica** del problema, la distinzione non è rilevante (eco di [[parallelo-concorrente-distribuito]]).
- **Modello di tempo**: sincrono vs asincrono → [[modello-sincrono-asincrono]] (decisivo per FLP).
- **Modello di fallimento**: processi/processori e link possono fallire in modi diversi (eco di [[modelli-di-fallimento]], [[classificazione-guasti]]); la fault tolerance è la proprietà di continuare a funzionare **oltre** i guasti.

## Connessioni

- [[consensus-problemi]] — le varianti formali (single-bit, interactive consistency, generals/broadcast) e le proprietà di correttezza
- [[modello-sincrono-asincrono]] — il modello temporale che rende il consenso (im)possibile
- [[flp-impossibility]] — perché il consenso è impossibile nel caso asincrono puro
- [[paxos]] — un protocollo che lo risolve rilassando le ipotesi
- [[state-machine-replication]] — dal consenso sullo stato al consenso sulle *azioni*
- [[replicazione]] · [[replica-management]] — il consenso garantisce la consistenza delle repliche (M3)
- [[tamir-sequin-checkpointing]] — il commit 2PC come istanza di consenso (C2)
- [[teorema-cap]] · [[cap-vs-flp]] — l'altro grande risultato di impossibilità
- [[componenti-connettori]] — **(M8)** l'ontologia componenti-connettori qui usata, formalizzata come elementi architetturali

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 4–18; A. Omicini, basato su Fischer 1983)
