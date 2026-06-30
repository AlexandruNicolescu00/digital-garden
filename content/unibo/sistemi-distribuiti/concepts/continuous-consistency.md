---
title: "Continuous Consistency (e Conit)"
course: Sistemi Distribuiti
category: concept
topics: [consistency, data-centric, conit, deviazione]
difficulty: avanzato
sources: [M3-Replication-Consistency-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Continuous Consistency (e Conit)

## Definizione

La **continuous consistency** è un modello **data-centric** il cui obiettivo è **imporre limiti alle deviazioni** tra repliche. Il livello di consistenza è definito su **tre assi indipendenti** di deviazione:

1. **Deviazioni numeriche** (assolute / relative)
   - es. due copie del prezzo di un'azione non devono differire di più di $0,01.
2. **Deviazioni di staleness** (anzianità)
   - es. i dati meteo non devono essere più vecchi di 4 ore.
3. **Deviazioni di ordinamento** (ordering)
   - es. in una bacheca, al massimo 6 messaggi possono restare fuori ordine.

Fissare limiti su questi tre assi definisce la nozione di **continuous consistency**.

## Intuizione: come si misura la deviazione — il conit

Nella prospettiva data-centric si usa la nozione tradizionale di **unità di consistenza**, chiamata **conit** (*con*sistency *unit*).

- La natura stessa di ogni data store, implicitamente o esplicitamente, **suggerisce** il proprio conit.
- Ma un modello di consistenza (e la replicazione) è definito attorno a un conit **progettato ad hoc**.
- La **deviazione si misura** come **differenza di conit**.
- **Non esiste una misura assoluta** della deviazione: il conit va scelto con cura, a seconda della risorsa e del problema.

## Varianti e casi limite: granularità del conit

La scelta della **granularità** del conit è un trade-off:

- **Conit più grande** — racchiude più dati: un singolo update locale "sporca" tutto il conit, aumentando il bisogno di propagazione e i conteggi di deviazione (rischio di falsi conflitti / over-counting).
- **Conit più piccolo** — **minor bisogno di propagazione**: gli aggiornamenti restano localizzati, ma cresce l'overhead di gestire molti conit.

(Le slide 29–30 illustrano graficamente i due casi con figure da Tanenbaum & van Steen, 2017.)

## Connessioni

- [[consistency-model]] — la famiglia (data-centric) a cui appartiene
- [[sequential-consistency]] — modello data-centric "tutto-o-niente" sull'ordine
- [[causal-consistency]] — rilassa l'ordinamento solo alle relazioni causali
- [[eventual-consistency]] — caso limite: deviazioni tollerate purché convergano
- [[data-centric-vs-client-centric]]

## Sorgenti

- `raw-sources/M3-Replication-Consistency-in-Distributed-Systems.pdf` (slide 27–30)
