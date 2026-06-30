---
title: "Esempio: Smart Contract Counter in Solidity (Ethereum)"
course: Sistemi Distribuiti
category: example
topics: [smart contract, Solidity, Ethereum, evento, stato]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Esempio: Smart Contract `Counter` in Solidity (Ethereum)

## 1. Problema

Mostrare concretamente come uno [[smart-contract]] sia un **oggetto stateful replicato**: un contatore che chiunque può incrementare, mantenendo stato (`value`, `owner`) ed emettendo eventi.

## 2. Tecnica applicata

**Solidity** — linguaggio object-oriented, high-level per smart contract Ethereum (`solidity.readthedocs.io`). Il contratto è una classe con campi (≈ Storage) e metodi (≈ Code).

```solidity
contract Counter {
    event Increased(uint oldValue, address cause);   // definizione evento

    address owner;
    uint value;

    function Counter() public { owner = msg.sender; } // costruttore

    function inc(uint times) public {                 // API
        for (uint i = 0; i < times; i++) {
            emit Increased(value++, msg.sender);
        }
    }
}
```

## 3. Soluzione — esecuzione passo per passo

L'esecuzione è una sequenza di **stati** ancorati alla [[blocks-e-block-chain|catena di blocchi]]; ogni invocazione è una [[blockchain-transactions|TX]] che applica `δ`:

- **State 0:** Alice 40, poi `Alice → Bob 20` ⇒ Alice 20, Bob 20.
- **State 1:** `Bob → Dan 3`; Alice deploya `"Counter.sol"` ⇒ `Counter.owner = Alice, Counter.value = 0`; Dan 3.
- **State 2:** `Counter.inc(1)`, `Counter.inc(2)` ⇒ `Counter.value = 3`; emessi eventi `Increased(0,Bob)`, `Increased(1,Dan)`, `Increased(2,Dan)`; i saldi calano per il **gas** (Bob 16.89, Dan 2.83, Eve 5.28).
- **State 3:** `Counter.inc(5)`, `Counter.inc(2)` ⇒ `Counter.value = 5` (rispetto al risultato atteso si vede l'effetto del costo); saldi aggiornati per il gas.

Ogni `inc(times)` cicla `times` volte → ogni iterazione costa **gas** ([[smart-contract|Ethereum & Gas]]): più alto `times`, più alto il costo, fino al `gasLimit`.

## 4. Errori comuni

- Pensare che `value++` sia "gratis": ogni istruzione bytecode incrementa il contatore di gas; un `inc(times)` con `times` grande può eccedere il `gasLimit` → **eccezione e rollback** di tutti gli effetti.
- Credere che esista **una** copia del contratto: ne esistono **N** (una per replica), ma computano come una sola ([[smart-contract|SC instances]]).
- Aspettarsi **randomness** o computazione **proattiva/periodica**: impossibili (determinismo + pura reattività).
- Trattare `owner`/storage come **segreti**: sono pubblici sulla blockchain.

## 5. Generalizzazione

Il `Counter` è il "Hello World" degli smart contract: un caso minimo di **stato incapsulato + API reattiva + eventi**. Generalizza alla classe dei contratti che fanno da **intermediari fidati** in workflow multi-parte (pagamenti automatici, DAO, tracciamento supply-chain via IoT) → vedi [[smart-contract]] §6.

## 6. Connessioni

- [[smart-contract]] — definizione, gas, run-time, issues.
- [[universal-smr]] — il contratto gira su una macchina a stati universale replicata.
- [[blockchain-transactions]], [[blocks-e-block-chain]] — deploy/invoke come TX dentro i blocchi.

## 7. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Examples & Use Cases — An Example: Solidity), slide 151–152.
