---
title: "Timestamping & Notary Service"
course: Sistemi Distribuiti
category: concept
topics: [timestamping, notary, hash chain, single point of trust, replicazione]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Timestamping & Notary Service

Sono gli **antenati concettuali** della [[blockchain]]: applicazioni delle [[strumenti-crittografici-blockchain|hash chain]] che, una volta replicate, diventano una blockchain. C4 le usa per costruire la blockchain *bottom-up*.

## 1. Timestamping service

Includendo **timestamp** nelle hash chain si ottiene un servizio di marcatura temporale [Haber & Stornetta, 1991]:

- i blocchi sono aggiunti (quasi) **periodicamente**;
- più eventi simultanei possono stare nello stesso blocco;
- tiene traccia di **cosa è successo e quando**.

## 2. Notary service

Facendo sì che il timestamping service **accetti solo eventi correttamente firmati**, si ottiene un **servizio notarile**:

- le firme devono essere valide, altrimenti l'evento non è notarizzato;
- tiene traccia di **chi ha fatto cosa e quando**;
- fornisce **non-ripudio** degli eventi e della loro tempistica.

**Stato risultante.** Dopo ogni blocco si può calcolare uno **stato risultante** tenendo conto di tutti gli eventi occorsi finora (es. un sistema di *punching in/out* dei dipendenti → lo stato è chi è dentro/fuori). È il germe della funzione di transizione `δ` di [[blockchain-transactions]].

**Applicazioni reali:** ledger di ordini/spedizioni/pagamenti in una supply chain; sistema di voto (a voto palese); registro di bandi pubbliche e candidature; registro di proprietà immobiliari; timbratura presenze; una criptovaluta.

> In generale: **qualunque caso in cui due o più parti abbiano interesse a mentire** su cosa hanno fatto o quando può trarre vantaggio da un servizio notarile.

## 3. Il problema: single point of trust

Un notary service si implementa facilmente come **sistema distribuito a architettura logicamente centralizzata**: un server centrale verifica la validità degli eventi, mantiene la hash chain e memorizza i dati; i client inviano nuovi eventi. Può essere ridondante, ma **è controllato da una singola parte/organizzazione**.

⚠️ **Single point of trust.** La parte che gestisce il server può **corrompersi** e alterare arbitrariamente ordine/tempistiche degli eventi, o impedire a un sistema di partecipare. *Nessuna difesa* contro questo. → è il problema del [[centralizzato-vs-distribuito|punto di controllo centrale]] portato sul piano della **fiducia**.

## 4. La via d'uscita: replicazione → SMR

1. Per evitare la centralizzazione della fiducia, si **replica** il notary service in modo che ogni replica sia controllata da una **parte/organizzazione diversa**;
2. quando un client manda un evento a una replica, esso viene **propagato** a tutte le altre;
3. ogni replica memorizza una copia della hash chain e degli eventi;
4. una replica può ancora essere compromessa, ma finché **la maggior parte** non lo è, i tentativi di manomissione si possono **rilevare** — *"la maggior parte"? quanti?* → la domanda che porta a [[byzantine-fault-tolerance|BFT]] (f < N/3);
5. in sostanza si replica il **software che valida** gli eventi → è l'obiettivo della [[state-machine-replication|State Machine Replication]] [Schneider, 1990].

> 🔗 `notary service + replicazione + classe Ledger ⇒ criptovaluta`.

## 5. Connessioni

- [[strumenti-crittografici-blockchain]] — hash chain, timestamp, firme: i mattoni.
- [[state-machine-replication]] — replicare il validatore *è* SMR; il ledger replicato è una macchina a stati.
- [[byzantine-fault-tolerance]] — "quanti onesti servono?" → consenso BFT.
- [[blockchain]] — un notary service replicato e tenuto in sync per consenso.
- [[centralizzato-vs-distribuito]] — il single point of trust è la versione "fiducia" del single point of failure.

## 6. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Background — Applications), slide 46–53.
- Riferimenti: Haber & Stornetta 1991; Schneider 1990; Charron-Bost et al. 2010.
