---
title: "CAP vs FLP: due risultati di impossibilità"
course: Sistemi Distribuiti
category: bridge
topics: [CAP, FLP, impossibilità, consensus, consistenza]
difficulty: avanzato
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf, C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# CAP vs FLP: due risultati di impossibilità

La slide di C3 lo dice esplicitamente: **"Not just CAP, then"**. Il [[teorema-cap]] (C1) e il [[flp-impossibility|teorema FLP]] (C3) sono i due grandi risultati di impossibilità del corso. Sono **distinti ma imparentati**: entrambi nascono dall'asincronia e dai guasti, ed entrambi **delimitano lo spazio del fattibile** invece di chiuderlo.

## Tabella di confronto

| | **CAP** [Gilbert & Lynch, 2002] | **FLP** [Fischer, Lynch, Paterson, 1985] |
|---|---|---|
| Cosa afferma | non si possono avere **C, A e P** insieme: in presenza di **partizione**, scegli C **o** A | nel modello **asincrono puro**, nessun protocollo di consenso tollera **neppure 1 crash** garantendo la terminazione |
| Oggetto | proprietà di un **servizio/data store** replicato | risolubilità del **problema del consenso** |
| Ipotesi di guasto | **partizioni di rete** | **un singolo crash** (messaggi affidabili, no bizantini) |
| Modello di tempo | asincrono, no clock globale | asincrono puro (no bound su ritardi/velocità) |
| Cosa "cade" | C **oppure** A (durante la partizione) | la **terminazione** (liveness); safety preservabile |
| Via d'uscita | rilassare C (→ [[eventual-consistency]], BASE) o tollerare downtime | rilassare la sincronia (partial synchrony, timer/backoff) → [[paxos]], Raft |
| Nucleo comune | impossibile distinguere un peer **lento** da uno **partizionato/crashato** in assenza di tempo globale | |

## Relazione concettuale

Entrambi discendono dalla stessa radice: **senza un tempo globale e con guasti, un nodo non può sapere se un altro è morto o solo irraggiungibile/lento** (→ [[modello-sincrono-asincrono]], [[ordinamento-parziale-eventi]]). Il CAP lo legge come tensione **consistenza/disponibilità**; FLP come **impossibilità della terminazione** del consenso. Non a caso **Nancy Lynch** è coautrice di entrambi.

Inoltre **C, A, P e consenso si intrecciano**: garantire la consistenza forte delle repliche ([[consistency]], [[replicazione]]) richiede **consenso** ([[agreement-consensus]]); il consenso a quorum ([[paxos]], **n≥2f+1**) è proprio il meccanismo con cui i sistemi **CP** restano consistenti tollerando guasti, al prezzo della disponibilità durante le partizioni.

## Sintesi

- Gli impossibility results **non vietano l'ingegneria**: ne tracciano i **confini**. Ragionare sul rapporto tra modello formale e istanza pratica suggerisce la soluzione tecnicamente fattibile.
- CAP guida le **scelte di consistenza/disponibilità** (M3); FLP guida la **progettazione dei protocolli di consenso** (C3). Sono due lenti sullo stesso fenomeno: i **guasti** sono insieme il problema e la ragione d'essere dei sistemi distribuiti.

## Connessioni

- [[teorema-cap]] — il risultato CAP in dettaglio
- [[flp-impossibility]] — il risultato FLP in dettaglio
- [[paxos]] — come si convive con FLP
- [[eventual-consistency]] · [[acid-vs-base]] — come si convive con CAP
- [[agreement-consensus]] · [[replicazione]] — il filo consenso↔consistenza delle repliche
- [[modello-sincrono-asincrono]] — l'ipotesi condivisa (asincronia)

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 21; A. Omicini)
- `raw-sources/C1-The-CAP-Theorem-...pdf`
