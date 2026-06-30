---
title: "Proof-of-Work (Mining)"
course: Sistemi Distribuiti
category: concept
topics: [proof-of-work, mining, branch, longest chain, incentivi, 51% attack]
difficulty: avanzato
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Proof-of-Work (Mining)

Il **Proof-of-Work (PoW)**, a.k.a. *mining*, è l'approccio di consenso **competition-based** dei sistemi [[permissioned-vs-permissionless|permissionless]] ([[consensus-mining]]). Originariamente concepito contro lo spam email [Back, 2002], oggi è ciò che **dona valore** alle criptovalute.

```
PoW = puzzle computazionale + strategia di risoluzione dei branch
```

## 1. Il puzzle computazionale

- le repliche (**miner**) **competono** per essere le prime a risolvere un puzzle computazionale, circa una volta ogni `Δt·ε` secondi;
- la **prova dell'effort** è facile da verificare e va inclusa nel blocco (solo i blocchi che la contengono sono validi);
- la prima replica che risolve il puzzle **mina** il prossimo blocco;
- la **difficoltà** deve essere variabile e controllabile.

**Esempio (Bitcoin):** modificare un campo del candidate block finché il suo hash inizia con un certo numero di zeri.
```
1. b_i = (ph_i, h_i, ts_i, m_i, txList), con m_i = 0
2. while H(b_i) ≥ h_threshold : incrementa m_i di 1
```
**Esempio (Ethereum):** si genera in modo riproducibile un grafo pseudo-random da ~1 GiB; il contenuto del candidate block seleziona parti del grafo, che vengono hashate insieme e incluse nel blocco (+ ASIC-resistance).

## 2. Branch e loro risoluzione

I blocchi minati sono **propagati** sulla rete P2P, ma la propagazione **richiede tempo**. Nel frattempo un'altra replica può aver minato un altro blocco (perché non ha ancora ricevuto il primo, o perché è corrotta). Questa situazione è un **branch** e rappresenta un'**inconsistenza** (più versioni possibili del ledger).

> I branch sono **inevitabili**: serve una **regola locale** per decidere quale branch è "quello vero". Finché la **maggioranza** dei miner segue la regola, un singolo branch verrà eventualmente selezionato da tutti → **[[eventual-consistency|eventual consistency]]**.

- **Bitcoin:** *Longest Chain Rule*;
- **Ethereum:** *GHOST* (Greedy Heaviest Observed SubTree) [Sompolinsky & Zohar, 2013].

### Longest Chain Rule
I nodi onesti considerano sempre la **catena più lunga** sentita per prima. Anche se avviene un branching, i due rami non crescono alla stessa velocità: uno diventerà più lungo più rapidamente, e l'altro sarà abbandonato (**orphan**). Non possiamo impedire alle repliche corrotte di generare branch, ma: *quanto è probabile che una singola replica produca un branch più lungo in meno tempo delle altre?* → in Bitcoin la probabilità di successo di un *double-spending attack* decade esponenzialmente col numero di blocchi di conferma `n` [Nakamoto, 2008]; `n_threshold = 6` (≈ 1h) dà 99.999% di sicurezza se l'attaccante controlla < ~13% della CP totale.

## 3. Proprietà del PoW

- approccio **competition-based, locale, eventually-consistent, stocastico**;
- **difficoltà auto-adattiva**, tale che `E[Δt] = costante` (compensa la variazione di potenza di calcolo dei miner: es. Bitcoin aggiorna periodicamente `h_threshold`);
- conta **solo la potenza di calcolo (CP)** → **resistente al [[sybil-attack|Sybil attack]]** (creare più identità non aiuta); ma vulnerabile al **51% attack** se un attaccante supera la maggioranza della CP [Eyal & Sirer, 2014];
- l'invariante `E[Δt] = cost` implica una **frequenza massima di aggiornamento** del sistema pari a `1/E[Δt]` (→ basso throughput).

## 4. Incentivi & disincentivi

Ipotesi: i miner sono **razionali (egoisti)**. Perché dovrebbero partecipare onestamente, dato che il PoW è costoso (risorse → denaro reale) e le TX spesso non sono loro?

- **Incentivo economico:** chi "vince" il puzzle è **ricompensato con denaro** creato *ex-nihilo* dal protocollo → incentivo a competere lealmente.
- **Disincentivo economico:** attacchi come il branching deliberato non sono strettamente *impediti*, ma resi **estremamente costosi**.

> Nelle blockchain a criptovaluta, **PoW, valore economico e sicurezza sono strettamente legati**: il PoW dà valore alla cripto → i miner vogliono compenso → il loro lavoro è il "carburante" del sistema → si comportano onestamente finché conviene → conviene finché la cripto ha valore.

## 5. Connessioni

- [[consensus-mining]], [[permissioned-vs-permissionless]] — PoW come consenso permissionless.
- [[sybil-attack]] — PoW lo neutralizza legando il voto alla CP.
- [[eventual-consistency]], [[teorema-cap]] — branch resolution = consistenza eventuale; scelta A+P.
- [[blocks-e-block-chain]] — consistenza probabilistica dei blocchi profondi.
- [[byzantine-fault-tolerance]] — alternativa "economica" ai BFT classici contro i fault bizantini.

## 6. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Blockchain Main Elements — The Proof-of-Work Mechanism), slide 113–122.
- Riferimenti: Back 2002; Nakamoto 2008; Sompolinsky & Zohar 2013 (GHOST); Eyal & Sirer 2014 (51%).
