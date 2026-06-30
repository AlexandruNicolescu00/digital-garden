---
title: "World State & User Identifiers (Blockchain)"
course: Sistemi Distribuiti
category: concept
topics: [world state, asset, user identifier, address, account, UTXO]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# World State & User Identifiers (Blockchain)

Il **modello dati** della [[blockchain]]: chi sono gli utenti e cosa rappresenta lo stato del sistema replicato.

## 1. User identifiers

Gli **utenti** possiedono (almeno) una coppia di chiavi `(K_pub, K_pr)` ([[strumenti-crittografici-blockchain|crittografia asimmetrica]]) e sono identificati da una funzione `f(K_pub)` della loro chiave pubblica:
- es. `f` = funzione hash 1-way;
- es. certificati digitali emessi da una **CA** (Certification Authority) fidata.

> ⚠️ In questo contesto gli identificatori si chiamano anche **indirizzi (addresses)**.

### Permissioned vs Permissionless
- **Permissionless** — ogni utente possiede *molti* identificatori non intelligibili → **pseudonimità**, decentralizzazione, ma vulnerabilità al [[sybil-attack|Sybil attack]];
- **Permissioned** — ogni utente possiede un *singolo* identificatore certificato (da una CA) → niente Sybil, ma **single point of failure/trust**.

→ questa dicotomia governa anche il tipo di consenso ([[permissioned-vs-permissionless]]).

## 2. The World State

Lo stato del sistema è concettualmente un **insieme di rappresentazioni di asset**, ciascuna con il proprietario:

```
SystemState ::= Asset | SystemState ∪ SystemState
Asset       ::= ⟨ UserID, AssetState ⟩       con UserID = f(K_pub^U)
```

L'**AssetState** ha campi che tracciano cosa una certa entità possiede attualmente; i campi dipendono dall'applicazione:
- `AssetState ::= Balance` — es. BCT con criptovaluta nativa tracciano (almeno) i saldi;
- `AssetState ::= CustomData` — altre BCT permettono dati custom.

### Implementazioni del world state
| Modello | Esempio |
|---------|---------|
| **account-based** | Ethereum [Wood, 2014] |
| **UTXO** (Unspent Transaction Output) | Bitcoin [Nakamoto, 2008] |
| **versioned key-value store** | Hyperledger Fabric [Androulaki et al., 2018] |

> Con gli [[smart-contract]] il modello si estende: un'entità può essere `UserID` **o** `SmartContractID`, e un asset può essere dati utente o lo stato (`Storage, Code`) di un contratto.

## 3. Connessioni

- [[blockchain-transactions]] — le transazioni propongono variazioni del world state via `δ(s, tx) = s'`.
- [[blocks-e-block-chain]] — ogni blocco registra lo stato risultante.
- [[sybil-attack]], [[permissioned-vs-permissionless]] — conseguenze del modello di identità.
- [[smart-contract]] — estende entità e asset con i contratti.
- [[state-machine-replication]] — il world state è lo **stato** della macchina replicata.

## 4. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — User Identifiers, The World State), slide 80–84.
