---
title: "Modelli di Fallimento e Sistemi Fail-*"
course: Sistemi Distribuiti
category: concept
topics: [dependability, failure, crash, byzantine, fail-stop, fail-safe]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Modelli di Fallimento e Sistemi Fail-*

## Classificazione dei failure [Tanenbaum & van Steen, 2017]

Classificazione complementare a quella dei fault (→ [[classificazione-guasti]]), centrata su *come* un server fallisce:

| Tipo di failure | Descrizione |
|-----------------|-------------|
| **Crash failure** | il server si ferma, ma funzionava correttamente fino allo stop |
| **Omission failure** | il server non risponde alle richieste |
| — *Receive omission* | non riceve i messaggi in arrivo |
| — *Send omission* | non invia i messaggi |
| **Timing failure** | la risposta arriva fuori dall'intervallo di tempo specificato |
| **Response failure** | la risposta del server è scorretta |
| — *Value failure* | il valore della risposta è sbagliato |
| — *State transition failure* | il server devia dal corretto flusso di controllo |
| **Arbitrary (Byzantine) failure** | il server produce risposte arbitrarie in momenti arbitrari |

Il caso **arbitrary/Byzantine** è il più severo: include qualsiasi comportamento, anche malevolo (→ [[classificazione-guasti]], Byzantine faults).

## Sistemi progettati per "fallire bene"

Quando il fallimento è inevitabile, lo si rende meno dannoso progettando il *comportamento* al fallimento:

### Fail-stop systems
Arricchiti con meccanismi tali che, quando falliscono, **smettono di rispondere** alle richieste. Evitano conseguenze catastrofiche rendendo il fallimento "pulito" e rilevabile (un crash è banale da rilevare via probing → [[mezzi-dependability]]).

### Fail-safe systems
Il fallimento **non causa grande danno** a persone o ambiente. Definiscono un insieme di **safe states**: quando non possono più operare secondo specifica, transitano in uno stato sicuro predefinito.
- es. il sistema di controllo di una centrale nucleare deve essere fail-safe (→ attributo [[dependability|safety]]).

### Fail-fast
Pratica di ingegneria del software: il sistema **si ferma immediatamente** appena entra in uno stato di errore o incontra una condizione inattesa. Abilita la **early detection** e la diagnosi dei fault: un fault propagato a molti componenti è molto più difficile da localizzare.

## Connessioni

- [[classificazione-guasti]] — la tassonomia lato fault
- [[fault-error-failure]] — la catena causale
- [[dependability]] — safety e l'attributo correlato
- [[mezzi-dependability]] — detection (crash facile da rilevare), fail-fast come pratica

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 27–30)
