---
title: "Classificazione dei Guasti (Faults)"
course: Sistemi Distribuiti
category: concept
topics: [dependability, fault, byzantine, taxonomy]
difficulty: intermedio
sources: [M1-Dependability-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Classificazione dei Guasti (Faults)

## Definizione

Diversi tipi di **fault** (→ [[fault-error-failure]]) richiedono trattamenti diversi. I guasti si classificano secondo **sei criteri**.

## 1. Source (sorgente)

- **hardware faults** — guasti di componenti HW (blackout, dischi, chip difettosi)
- **software faults** — bug software (race condition, mancato boundary-check su array)
- **operator faults** — causati dall'operatore (misconfiguration, procedure di upgrade errate)

## 2. Intent (intenzione)

- **non-malicious faults** — non causati da intento doloso (guasto HW naturale, bug involontari)
- **malicious faults** — causati da chi vuole danneggiare il sistema (negare servizi, compromettere l'integrità) → vedi anche *commission faults* / **Byzantine faults**

## 3. Duration (durata)

- **transient faults** — attivato momentaneamente, poi torna dormiente (es. picco di tensione)
- **intermittent faults** — appare, sparisce da solo, riappare… (es. race condition tra due thread)
- **permanent faults** — una volta attivato resta finché il componente non è riparato/la causa rimossa (es. power outage; *crash fault* di processo)

## 4. Manifestation (manifestazione)

- **content faults** — i valori passati ad altri componenti sono sbagliati
  - se il componente passa **valori diversi a componenti diversi** → caso modellato come **Byzantine faults**
- **timing faults** — risposta troppo presto o troppo tardi; caso estremo: **non risponde più** (tempo infinito) → crash, hang, deadlock, loop infinito

## 5. Reproducibility (riproducibilità)

- **reproducible / deterministic faults** — si riproducono facilmente (es. null pointer); facili da identificare e riparare
- **nondeterministic faults** — difficili da riprodurre (es. interleaving specifico di thread su variabile condivisa); detti **Heisenbugs** per la loro incertezza

## 6. Relationship (relazione con altri guasti)

- **independent faults** — nessuna relazione causale tra A e B
- **correlated faults** — causalmente legati; se più componenti falliscono per una **causa comune** → **common mode failures**

## Concetti chiave da ricordare per l'esame

- **Byzantine faults** compaiono in *due* criteri: intent (malicious) e manifestation (valori diversi a componenti diversi). Sono il gancio verso il futuro modulo su **Byzantine Fault Tolerance** e consensus.
- **Heisenbug**, **common mode failure**, **crash fault** sono termini ricorrenti.

## Connessioni

- [[fault-error-failure]] — di cui questa è la tassonomia del "fault"
- [[modelli-di-fallimento]] — la classificazione *complementare* lato "failure" (Tanenbaum)
- [[mezzi-dependability]] — tipi diversi di fault → trattamenti diversi
- [[paxos]] — **(C3)** algoritmo di consenso che tollera **crash** ma **esclude** i guasti bizantini (n≥2f+1)
- [[agreement-consensus]] — **(C3)** raggiungere l'accordo tra processi alcuni dei quali faulty
- [[byzantine-fault-tolerance]] · [[pbft]] — **(C4)** i guasti bizantini *operazionalizzati*: bound `f < N/3`, protocolli BFT

## Byzantine Fault Tolerance — ora coperta (C4)

Il gancio "BFT da approfondire" è ora risolto da C4: i [[byzantine-fault-tolerance|fallimenti bizantini]] (nodi che mentono/danno valori diversi a componenti diversi) sono tollerabili sse il numero di repliche faulty soddisfa `f < N/3` (Byzantine Generals, Lamport et al. 1982), tramite consenso BFT come [[pbft|PBFT]]. Nei sistemi [[permissioned-vs-permissionless|permissionless]] si usa invece il [[proof-of-work|PoW]] (resistenza Sybil + maggioranza di potenza di calcolo).

## Sorgenti

- `raw-sources/M1-Dependability-in-Distributed-Systems.pdf` (slides 20–26)
- BFT ripresa in `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (slide 69–74, 110–111).
