---
title: "Self-Assessment: Distributed Pong (design & casi limite)"
course: Sistemi Distribuiti
category: self-assessment
topics: [pong, design, availability, consistency, udp, event-based, fault-tolerance, esame]
difficulty: avanzato
sources: [pong_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Self-Assessment: Distributed Pong (design & casi limite)

Domande stile esame sul caso studio [[distributed-pong]]. Ogni domanda riporta una **risposta modello**, la **difficoltà** e gli **argomenti testati**.

---

## A. Scelte di design

### Q1. Perché tra le quattro infrastrutture (local, centralized, brokered, replicated) si sceglie la **centralized**? *(intermedio)*
**Risposta modello.** La *local* non offre distribuzione. La *brokered* aggiunge un **secondo SPOF** (broker oltre al server), un **hop di latenza** e un decoupling temporale che è un *non-goal* in un gioco real-time. La *replicated* (server + consenso) è **overkill**: per un videogioco non servono storage né consistenza forte, e il consenso costa latenza/complessità. La *centralized* offre il miglior compromesso: **un solo SPOF**, **latenza bassa**, deploy gestibile.
→ Testa: [[infrastrutture-sistema-distribuito]], [[teorema-cap]], [[features-design-distribuito]].

### Q2. Perché si sceglie **UDP** e non TCP? *(base)*
**Risposta modello.** Pong è **real-time**: conta la **bassa latenza**, e la perdita di qualche pacchetto è tollerabile (lo stato viene comunque rispedito di continuo). TCP darebbe affidabilità/ordine ma introduce latenza (ritrasmissioni, flow control) e complica il server, che dovrebbe gestire molte **connessioni simultanee**. Prezzo di UDP: duplicazioni, disordine e ritardi da gestire applicativamente.
→ Testa: [[stream-vs-datagram-sockets]], [[socket]].

### Q3. In che senso l'architettura di Distributed Pong è **event-based**? *(intermedio)*
**Risposta modello.** Ci sono **due tipi di evento** su **due canali**: il **Coordinator** *pubblica* gli eventi *"game state update"* e *consuma* gli eventi *"input"*; i **Terminal** fanno il viceversa (pubblicano input, consumano update). I componenti non si chiamano direttamente per nome ma reagiscono a/producono eventi → disaccoppiamento tipico dello stile.
→ Testa: [[architectural-styles]], [[interaction-patterns]].

### Q4. Perché il **time-passing** è modellato come un "input speciale"? *(avanzato)*
**Risposta modello.** Il gioco **evolve anche senza input dell'utente** (la palla si muove): serve un evento `TIME_ELAPSED` generato **internamente** dal controllo, non dall'utente. Trattarlo come input speciale gestito dall'`InputHandler` mantiene il design uniforme (un'unica pipeline input→control event→update) e separa pulito model/control/view (MVC), abilitando poi la distribuzione.
→ Testa: [[distributed-pong]], [[computing-needs-time|il tempo nel computing]].

---

## B. Casi limite del protocollo Join/Leave

### Q5. Cosa fare se il **coordinator non è disponibile** quando parte il terminal? *(base)*
**Risposta modello.** Due opzioni: il terminal **termina**; oppure **attende** con un **timeout** e un numero massimo di **retry** (eventualmente con back-off) che il coordinator diventi raggiungibile.
→ Testa: [[features-design-distribuito]] (timeout/retry).

### Q6. Cosa succede se un terminal **crasha prima** di inviare `PLAYER_LEAVE`? *(intermedio)*
**Risposta modello.** Con **UDP** serve un meccanismo di **heart-beat** custom per accorgersi dell'assenza; con **TCP** la connessione cade e il coordinator rileva il crash. In entrambi i casi il coordinator deve poi rimuovere il paddle.
→ Testa: [[features-design-distribuito]] (heartbeat), [[stream-vs-datagram-sockets]].

### Q7. Come distinguere un terminal **crashato** da uno che semplicemente **non manda input**? *(avanzato)*
**Risposta modello.** Non è distinguibile con certezza (è il cuore dell'asincronia: lento ≈ morto). Pragmaticamente: **timeout** lato coordinator + heart-beat → oltre la soglia, il terminal è considerato morto.
→ Testa: [[modello-sincrono-asincrono]], [[pitfalls-sistemi-distribuiti]].

### Q8. Cosa fare se due terminal scelgono lo **stesso lato**, o se un terminal manda input per il **paddle sbagliato**? *(intermedio)*
**Risposta modello.** Lato già preso: il coordinator **ignora** il `PLAYER_JOIN` o lo **rifiuta** (→ il join diventa **request-response**). Input per paddle altrui: il coordinator **ignora** o **espelle** il terminal — implica che tenga traccia dell'associazione **paddle↔terminal** (e, per l'espulsione, un leave **iniziabile dal coordinator**).
→ Testa: [[protocolli-interazione-comuni]] (request-response), [[features-design-distribuito]] (autorizzazione/trust).

---

## C. Availability vs Consistency

### Q9. Come si verifica **empiricamente** se il design privilegia availability o consistency? *(avanzato)*
**Risposta modello.** Si **simula una partizione** alzando il drop rate UDP (`UDP_DROP_RATE=0.2` → ~20% pacchetti persi) e si osserva il gameplay: **fluido** ⇒ si privilegia l'**[[availability]]** (il terminal continua a renderizzare nonostante i messaggi persi); **laggoso** ⇒ si privilegia la **[[consistency]]** (il terminal attende gli update). Per un gioco la scelta giusta è **availability**.
→ Testa: [[teorema-cap]], [[availability]], [[consistency]], [[pokemon-go-cap]].

### Q10. In cosa consiste l'esercizio "**Available Distributed Pong**"? *(avanzato)*
**Risposta modello.** Implementare **speculative execution** lato terminal: il game-loop locale **non si blocca mai** in attesa degli update del coordinator; continua a simulare con gli **input locali** e a renderizzare; quando arriva un update remoto, **sovrascrive** lo stato locale con quello remoto. Così il gioco appare **disponibile** anche con coordinator lento/irraggiungibile.
→ Testa: [[teorema-cap]], [[distributed-pong]].

---

## Connessioni

- [[distributed-pong]] — il caso studio completo.
- [[infrastrutture-sistema-distribuito]] — le quattro topologie (Q1).
- [[teorema-cap]] · [[availability]] · [[consistency]] — il trade-off (Q9–Q10).
- [[features-design-distribuito]] · [[stream-vs-datagram-sockets]] · [[architectural-styles]] — i meccanismi testati.

## Sorgenti

- `raw-sources/pong_slides.pdf` (slide 40–60 — *Towards Distributed Pong / Joining-Leaving / Analysis / Exercise*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
