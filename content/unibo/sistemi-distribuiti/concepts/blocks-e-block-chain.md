---
title: "Blocks & Block Chains"
course: Sistemi Distribuiti
category: concept
topics: [block, hash chain, block validity, block lifecycle, eventual consistency]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Blocks & Block Chains

I **blocchi** sono liste di [[blockchain-transactions|transazioni]] timestampate e **collegate da hash** ([[strumenti-crittografici-blockchain|hash chain]]); la loro concatenazione è la [[blockchain]].

## 1. Struttura di un blocco

```
Block ::= ⟨ PrevHash, Height, Time, Metadata, TxList ⟩
```

Formalmente un blocco `b_i` è una 5-upla:

```
b_i = ( ph_i, h_i, ts_i, m_i, (tx_i1, r_i1, …, tx_in, r_in) )
```

- **PrevHash** `ph_i` — hash del blocco precedente (il link della catena);
- **Height** `h_i` — indice del blocco corrente;
- **Time** `ts_i` — timestamp del blocco;
- **Metadata** `m_i` — metadati application-specific;
- **TxList** — lista delle TX incluse e **(gli hash de)gli stati risultanti** `r_ij` (lo stato dopo aver applicato tutte le `tx_ik` per k ≤ j); `n_i` = numero di TX nel blocco.

## 2. Validità locale di un blocco

Sia `b_{i-1}` l'ultimo blocco noto alla replica; un nuovo `b_i` è **localmente valido** se:

- `ph_i = H(b_{i-1})` — il link punta davvero al precedente;
- `h_i = h_{i-1} + 1` — altezza incrementata di 1;
- `ts_i - ts_{i-1} ∈ [Δt⁻ ; Δt⁺]` — il tempo cade nella finestra attesa (`Δt`, ε sono parametri specifici della blockchain);
- gli stati intermedi sono coerenti: `r_i1 = δ(r_{i-1,n}, tx_i1)` e `r_ij = δ(r_{i,j-1}, tx_ij)` per j ≥ 2.

## 3. Esecuzione di un blocco — la funzione ℓ

Un blocco localmente valido `b_i` si applica allo stato `s_{i-1} = r_{i-1,n}` prodotto dal blocco precedente tramite la funzione `ℓ`, producendo `s_i`:

```
ℓ(s_{i-1}, b_i) = r_in ≡ s_i
```

applicando **in ordine** tutte le TX del blocco tramite `δ(·,·)` ([[blockchain-transactions]]).

## 4. Block features (recap)

- **replicazione + hash-chaining** ⇒ *untamperability* del passato;
- **hash-chain + time + ordering** ⇒ [[notary-service|timestamping/notary]] [Haber & Stornetta, 1991];
- **hash-chain + firma** ⇒ accountability + non-ripudio;
- pubblicati (quasi) periodicamente (parametro Δt);
- **consistenza probabilistica:** `lim(n→∞) P[inconsistent(B_i)] = 0`, dove *n* è il numero di blocchi **successori** di `B_i` e `inconsistent(B_i)` è vero se non tutti i nodi concordano sul contenuto di `B_i`. → più un blocco è "profondo", più è definitivo ([[eventual-consistency]], [[proof-of-work|longest chain]]).

## 5. Ciclo di vita

**Genesis block:** il primo blocco è assunto condiviso tra le repliche.

**Vita di una TX (parte I):** l'utente compila la TX con operazione e argomenti, la **firma** con `K_pr`, la diffonde a una o più repliche.

**Vita di un blocco** — ogni nodo, periodicamente con periodo Δt:
1. ascolta le TX pubblicate dai client;
2. le **valida ed esegue** (le invalide sono scartate);
3. compila il nuovo **candidate block** locale;
4. **partecipa al consenso** con le altre repliche (negozia il prossimo blocco).

⚠️ I [[byzantine-fault-tolerance|fault bizantini]] possono sorgere per repliche corrotte **o** semplicemente perché le repliche hanno percepito le TX **in ordine diverso** → è proprio per questo che serve lo step 4 (il consenso).

**Vita di una TX (parte II):** validata dai peer alla ricezione (scartata se invalida) → eseguita producendo uno stato intermedio → inclusa in un blocco → il blocco è **confermato** dal protocollo di consenso e appeso alla catena. (I cicli variano molto col [[consensus-mining|consenso]] adottato.)

## 6. Connessioni

- [[strumenti-crittografici-blockchain]] — la hash chain è lo scheletro.
- [[blockchain-transactions]], [[blockchain-world-state]] — contenuto e stato.
- [[consensus-mining]], [[proof-of-work]], [[byzantine-fault-tolerance]] — come si conferma il prossimo blocco.
- [[eventual-consistency]] — la consistenza probabilistica dei blocchi profondi.
- [[causalita]], [[checkpointing-e-logging]] (C2) — la catena è un log immutabile e ordinato di eventi.

## 7. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — Blocks & Block Chains), slide 92–102.
