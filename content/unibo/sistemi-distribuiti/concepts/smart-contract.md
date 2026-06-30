---
title: "Smart Contract"
course: Sistemi Distribuiti
category: concept
topics: [smart contract, gas, Turing-completeness, Order-Execute, Solidity, DAO]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Smart Contract

## 1. Definizione

> **Smart Contract (SC)** [Szabo, 1997]. Processi **stateful, user-defined, reattivi, immutabili** (e perciò **fidati**) e **deterministici**, che eseguono computazioni **replicate** ed **espressive** sulla [[blockchain]].

Attributi:
- **stateful** — incapsulano il proprio stato, come gli oggetti in OOP;
- **reattivi** — possono essere attivati solo da un client *off-chain*;
- **user-defined** — gli utenti deployano contratti che implementano qualunque business logic;
- **immutabili** — il codice non può essere alterato dopo il deploy;
- **replicati** — la blockchain agisce da **interprete replicato** ([[universal-smr|USMR]]);
- **espressivi** — espressi in un linguaggio **Turing-completo** (vedi §5: quasi-Turing-completezza).

## 2. Motivazione — la 3ª generazione

Finora la blockchain dà un modo fidato di **memorizzare/aggiornare dati** senza single-point-of-trust. Ma *come ci fidiamo degli utenti che producono quei dati?* Automatizzare i workflow via software richiederebbe fidarsi di sviluppatori/organizzazioni. Servono **entità computazionali fidate (agenti)** capaci di gestire automaticamente gli asset = gli **smart contract**.

`immutabilità + ispezionabilità + accountability + replicazione ⇒ fidabili nel gestire asset critici (es. denaro)`. Il codice è sempre "giusto", la vera storia è sulla blockchain → riduce dispute, rimuove il bisogno di arbitrato. **Lack of situatedness:** dati e computazione totalmente *disembodied* [Mariani & Omicini, 2013] — come il cloud, ma senza single point of trust. A ogni istante esistono **N copie** di un SC (una per replica), ma **computano come una sola**.

## 3. Run time — deploy & invoke

Almeno due operazioni offerte ai client:
- **deploy** — accetta il codice del SC, lo istanzia su **tutte** le repliche e genera un identificatore/indirizzo univoco;
- **invoke** — accetta l'indirizzo di un SC e degli argomenti, ed esegue quel SC con quegli argomenti.

**Invocazione (dettaglio):** l'utente pubblica una *invocation TX* con l'indirizzo del SC come destinatario e dati di input → ogni replica riceve ed esegue la TX → se la computazione **termina senza eccezioni**, gli effetti collaterali (sullo stato del SC) entrano nel nuovo stato intermedio (altrimenti vengono scartati) → il blocco contenitore è confermato dal consenso. È un **modello computazionale reattivo**.

## 4. Aggiornamento del modello BCT

```
EntityID  ::= UserID | SmartContractID
Asset     ::= ⟨UserID, Data⟩ | ⟨SmartContractID, SmartContractState⟩
SmartContractState ::= ⟨Storage, Code⟩      (Storage ≈ campi OOP, Code ≈ metodi OOP)
TX        ::= ⟨EntityID, Operation, Signature⟩
Operation ::= ⟨deploy, Code⟩
            | ⟨invoke, Arguments, SmartContractID⟩
            | ⟨transfer, EntityID, Value⟩
```

|  | User | Smart Contract |
|---|---|---|
| | Balance | Balance |
| | chiavi privata/pubblica | Code |
| | hash(Public Key) | Address, Storage |

La funzione `δ` ([[blockchain-transactions]]) ora crea un SC (deploy), aggiorna lo stato del SC invocato (invoke) o aggiorna i saldi (transfer); la funzione `ℓ` resta invariata (esegue le TX del blocco in sequenza). → è l'architettura **Order-Execute** [Androulaki et al., 2018].

## 5. Espressività vs terminazione

Cosa succede invocando un programma **non terminante** come SC? È **impossibile filtrarli** (problema della fermata), e nei sistemi aperti gli utenti non sono assunti benevoli → bisogna **scoraggiare** computazioni infinite/lunghe.

**Ethereum → Gas.** Gli utenti **pagano** per la computazione:
- la TX porta due campi: `gasLimit` e `gasPrice` (i miner possono ritardare TX con gasPrice basso; l'utente alza la priorità alzando gasPrice);
- a ogni istruzione bytecode un contatore `g` cresce secondo un listino [Wood, 2014];
- se `g > gasLimit` → eccezione e **rollback** degli effetti;
- in ogni caso, alla fine, il saldo dell'emittente cala di `ETH = gasPrice · g` (riscattato dal miner vincitore come compenso).
- **Conseguenza teorica:** i SC **non** sono davvero Turing-completi → **quasi-Turing-completezza** [Wood, 2014].

**Hyperledger Fabric → Execute-Order-Validate** (architettura alternativa all'Order-Execute).

## 6. Esempi e casi d'uso

- **Linguaggio:** Solidity (OOP, high-level) → vedi esempio [[solidity-counter]].
- **Use case:** gestione/pagamenti automatici di denaro (bollette, assicurazioni, dividendi); governance condivisa (**DAO** — Distributed Autonomous Organizations); tracciamento automatico di asset/supply-chain (es. con **IoT**: sensori rilevano movimenti, il SC paga/rimborsa automaticamente). Idea centrale: **delegare decisioni trust-critical ad agenti SW trasparenti e fidati**.

## 7. Issues (tecnologia immatura)

- **No privacy/segreti** — tutto ciò che è pubblicato è pubblico; lo stato "privato" di un SC non è segreto; la pseudo-anonimità si rompe con statistica/ML → *no voto segreto?*
- **Scarsa randomness** — il determinismo vieta la vera casualità (le repliche divergerebbero); le info dei blocchi sono manipolabili dai miner → *no lotteria?*
- **Inter-comunicazione & re-entrancy** — i SC sono oggetti che si chiamano con metodi **sincroni**; il flusso può attraversare più SC; codice non-reentrant può fallire o portare a frodi (es. The DAO) [Atzei et al., 2017; Luu et al., 2016].
- **Immutabilità** — benedizione e maledizione: contratti buggati/fraudolenti **non si possono correggere** → importanza cruciale del design corretto e della validazione formale.
- **Pura reattività** — i SC sono *time-aware ma non time-reactive*: non possono schedulare/posticipare computazioni (no pagamenti periodici autonomi).
- **Disembodiment & no concorrenza** — la computazione è ovunque ed è strettamente **sequenziale** → i SC non parallelizzano.
- **Granularità del costo** — il costo per-istruzione è la granularità migliore? (es. nel publish-subscribe paga di più il publisher [Ciatto et al., 2018]).

## 8. Connessioni

- [[universal-smr]] — la blockchain SC-enabled è una USMR.
- [[blockchain-transactions]], [[blockchain-world-state]] — il modello esteso con entità/asset SC.
- [[solidity-counter]] — esempio concreto in Solidity.
- [[state-machine-replication]] — i SC sono macchine a stati replicate; il determinismo è vincolante.
- [[byzantine-fault-tolerance]], [[consensus-mining]] — confermano l'esecuzione dei SC.

## 9. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Parte III — Smart Contracts, Examples & Use Cases, Issues), slide 132–165.
- Riferimenti: Szabo 1997; Wood 2014; Androulaki et al. 2018; Mariani & Omicini 2013; Atzei et al. 2017; Luu et al. 2016; Dupont 2017; Ciatto et al. 2018.
