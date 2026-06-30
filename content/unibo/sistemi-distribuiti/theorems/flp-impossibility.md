---
title: "Teorema FLP (Impossibilità del Consenso Asincrono)"
course: Sistemi Distribuiti
category: theorem
topics: [FLP, consensus, impossibilità, asincronia, liveness]
difficulty: avanzato
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Teorema FLP (Fischer–Lynch–Paterson)

## Enunciato formale

> *"Impossibility of distributed consensus with one faulty process"* [Fischer, Lynch, Paterson, 1985]
>
> In un sistema **completamente asincrono**, **nessun** protocollo di consenso può tollerare **anche un solo crash failure**, sotto il **solo** requisito di **non-trivialità**.

**Ipotesi (notevolmente deboli — è ciò che rende il risultato forte):**
- sistema **asincrono puro** (nessun bound su ritardi/velocità, → [[modello-sincrono-asincrono]]);
- **nessun** guasto bizantino considerato;
- il sistema di messaggi è **affidabile**: consegna tutti i messaggi correttamente ed **esattamente una volta**;
- basta **un singolo** processo che si ferma (crash), **al momento sbagliato**;
- si richiede solo la **non-trivialità** ([[consensus-problemi]]).

**Tesi:** nessun protocollo deterministico garantisce **insieme** safety (agreement/validity) e **terminazione**. Persino il caso più semplice (consenso su un bit) è irrisolvibile.

## Intuizione

In un sistema asincrono è **impossibile distinguere un processo crashato da uno solo molto lento** (→ [[modello-sincrono-asincrono]]). Di conseguenza esiste sempre una configurazione **"indecisa" (bivalente)** — da cui il sistema potrebbe ancora decidere 0 o 1 — e l'avversario (lo scheduling asincrono + un crash piazzato al momento opportuno) può **sempre rimandare** la decisione, mantenendo il sistema bivalente all'infinito. Lo **stop di un singolo processo al momento sbagliato** può far fallire il raggiungimento dell'accordo a qualunque protocollo di commit distribuito.

## Corollari e conseguenze

- *"That simple version of the consensus problem has no robust solution"* senza ulteriori assunzioni sull'ambiente o restrizioni sui guasti.
- **"Not just CAP"**: FLP è un risultato di impossibilità **distinto e complementare** al [[teorema-cap]] — vedi [[cap-vs-flp]]. Insieme delimitano lo spazio del fattibile.
- Gli impossibility results **definiscono lo spazio dei problemi che possiamo davvero risolvere**: conoscerli è essenziale per un'ingegneria seria (le proprietà sono difficili da provare, e i modelli decidono le proprietà).

## Applicazioni: come si "rende possibile l'impossibile"

Non si **viola** FLP, si **aggira** rilassando le ipotesi (cfr. [Howard, 2016] *"Distributed Consensus: Making Impossible Possible"*):
- **in pratica** — si accetta che il sistema **a volte non sia disponibile**, mitigando con **timer e backoff**;
- **in teoria** — assunzioni di **sincronia più debole** (partial synchrony: "i messaggi arrivano entro un anno").

I protocolli reali ([[paxos]], Raft) **sacrificano la garanzia di terminazione** (liveness) per preservare sempre **agreement e validity** (safety): terminano *solo* in periodi sufficientemente stabili.

## Connessioni

- [[modello-sincrono-asincrono]] — l'ipotesi di asincronia è il motore dell'impossibilità
- [[consensus-problemi]] — è la *termination* (liveness) a cadere
- [[paxos]] — il protocollo che convive con FLP rinunciando alla terminazione garantita
- [[agreement-consensus]] — il problema colpito
- [[teorema-cap]] · [[cap-vs-flp]] — l'altro grande risultato di impossibilità
- [[byzantine-fault-tolerance]] — **(C4)** la *seconda* condizione di impossibilità: oltre all'asincronia, `f ≥ N/3` repliche bizantine; è perché i protocolli procedono per **round**
- [[computer-science-foundations]] — i modelli formali come strumento di spiegazione/predizione (M2)

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 20–26; Fischer et al. 1985, Fich & Ruppert 2003, Howard 2016)
