---
title: "Teorema CAP"
course: Sistemi Distribuiti
category: theorem
topics: [CAP, consistency, availability, partition-tolerance, impossibility]
difficulty: avanzato
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Teorema CAP

## Enunciato formale

**Teorema** [Gilbert and Lynch, 2002]:

> "È impossibile, nel modello di rete **asincrona**, implementare un oggetto dati read/write che garantisca le seguenti proprietà:
> - **availability**
> - **atomic consistency**
>
> in tutte le esecuzioni *fair* (incluse quelle in cui i messaggi vengono persi)."

Formulazione informale (congettura originale di [Brewer, 2000]): un sistema distribuito con dati condivisi (*shared-data*) può avere **al più due** delle tre proprietà desiderabili:

1. **C**onsistency
2. **A**vailability
3. tolerance towards network **P**artition

→ "pick two of three". Si veda anche la formulazione discorsiva: [[consistency]], [[availability]], [[partition-tolerance]].

## Ipotesi (definizioni precise usate nella prova)

Le definizioni formali sono il primo passo della prova [Gilbert and Lynch, 2002]:

- **Availability** — "ogni richiesta ricevuta da un nodo *non-failing* deve produrre una risposta". Se il sistema è available, otteniamo *risposte*.
- **(Atomic) Consistency** — il servizio è modellato come un **atomic data object**: le operazioni sono **totalmente ordinate** e ognuna avviene in un singolo istante. Conseguenza: ogni read che avviene *dopo* il completamento di una write deve restituire il valore di quella write o di una successiva. Se il sistema è consistent, otteniamo risposte *corrette*.
- **Network partition** — quando la rete è partizionata, tutti i messaggi inviati da una componente all'altra vengono **persi**. Qualsiasi pattern di perdita di messaggi è modellabile come partizione temporanea.

> ⚠️ **Attenzione terminologica:** la "consistency" del CAP **non** è la "C" di ACID. Qui *consistent* ingloba sia atomicità sia consistenza. Si veda [[acid-vs-base]]. Questo è un classico punto di confusione d'esame.

Riassunto operativo (nodi A, B):
- la rete è **partizionata** se un messaggio da A non arriva a B
- **availability** = B riceve il messaggio e risponde
- **consistency** = la risposta di B è *corretta*

## Tre modelli di rete

La prova è data per tre tipi di rete:
1. asincrona **con** perdita di messaggi
2. asincrona **senza** perdita di messaggi
3. parzialmente sincrona con clock locali

Per scopi didattici ci si concentra sul **modello asincrono**: nessun clock unico, i nodi agiscono solo in base a computazione locale e ai messaggi ricevuti. (Collegamento con M0: niente tempo globale → [[ordinamento-parziale-eventi]].)

## Dimostrazione (per assurdo)

**Assunzione (da contraddire):** atomicità, availability e partition tolerance sono *tutte* soddisfatte.

**Costruzione:**
- I nodi si partizionano in due insiemi disgiunti e non vuoti **G₁, G₂**.
- L'oggetto atomico *o* ha valore iniziale **v₀**, atteso consistente tra G₁ e G₂.
- **α₁**: una singola write `v₁ ≠ v₀` di *o* in G₁ (unica richiesta in quel periodo); durante α₁ nessun messaggio viene scambiato tra G₁ e G₂.
- Nessun messaggio da G₁ arriva in G₂.
- **α₂**: una singola read di *o* in G₂; anche durante α₂ nessun messaggio attraversa la partizione.

**Contraddizione (Q.E.D.):**
- Per **availability**, α₁ completa (v₀ → v₁) e α₂ completa.
- Eseguendo α₁ poi α₂, G₂ "vede" solo α₂ e **deve restituire v₀** (non ha ricevuto nulla da G₁).
- Ma α₂ avviene dopo il completamento di α₁: la consistenza richiederebbe la restituzione di v₁. Restituire v₀ **viola la consistency** (atomica). ∎

## Corollari e conseguenze

- Le tre proprietà non sono dello stesso tipo: **C e A sono spettri** (gradi), mentre **P è più on/off**.
- Nei sistemi reali (specie IoT/pervasivi, dove l'instabilità di rete domina) **rinunciare a P non è un'opzione** → in pratica il CAP costringe a **scegliere tra A e C**. (Vedi [[partition-tolerance]].)
- **Raffinamento di Brewer** [Brewer, 2012]:
  - (i) **in presenza** di partizione → scegliere un trade-off tra C e A;
  - (ii) **in assenza** di partizione → si possono avere *sia* C *sia* A.
  - Obiettivo moderno: massimizzare le combinazioni C/A sensate per la specifica applicazione, con piani per il funzionamento durante la partizione e per il recovery.

## Applicazioni

- Spiega perché la **"consistenza è difficile"** nei sistemi distribuiti (gancio da M0 → [[centralizzato-vs-distribuito]]).
- Motiva il passaggio da **ACID a BASE** e l'**eventual consistency** → [[acid-vs-base]], [[eventual-consistency]].
- Caso reale: [[pokemon-go-cap]].

## Lezioni apprese

- Quando la rete fallisce, tipicamente si sceglie tra un sistema **responsivo** (A) e uno **pienamente consistente** (C).
- Un **risultato di impossibilità** non è una resa: come Gödel per i sistemi assiomatici, fissa i confini e diventa una *leva* per metodi ingegneristici mirati.
- ACID va conosciuto, ma modelli meno stringenti (BASE) sono spesso utili nel mondo reale.

## Connessioni

- [[consistency]] · [[availability]] · [[partition-tolerance]] — le tre proprietà
- [[acid-vs-base]] — il trade-off applicato ai modelli di consistenza
- [[eventual-consistency]] — l'alternativa a valle del CAP
- [[pokemon-go-cap]] — caso studio
- [[ordinamento-parziale-eventi]] — perché serve il modello asincrono senza clock globale
- [[centralizzato-vs-distribuito]] — il CAP spiega la voce "consistency: difficult"
- [[flp-impossibility]] · [[cap-vs-flp]] — **(C3)** l'altro grande risultato di impossibilità ("not just CAP"); stessa coautrice (N. Lynch)
- [[agreement-consensus]] — **(C3)** la consistenza forte CP richiede consenso a quorum ([[paxos]])
- [[blockchain]] · [[eventual-consistency]] — **(C4)** la blockchain sceglie **A+P** ed eventual consistency (spesso *probabilistica*); il ramo [[permissioned-vs-permissionless|permissioned]] (BFT) sceglie invece **C**

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf` (slides 24–35)
- [Brewer, 2000] PODC '00; [Gilbert and Lynch, 2002] *ACM SIGACT News*; [Gilbert and Lynch, 2012]; [Brewer, 2012] *CAP twelve years later*.
