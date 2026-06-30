---
title: "Strumenti crittografici della blockchain"
course: Sistemi Distribuiti
category: concept
topics: [crittografia, hash, firme digitali, hash chain, integrità]
difficulty: intermedio
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Strumenti crittografici della blockchain

I mattoni crittografici su cui poggia l'intera [[blockchain]]. Si dividono in **strumenti** (hash, crittografia asimmetrica) e **schemi** che li compongono (hash chain, firme digitali).

## 1. Funzioni hash 1-way

`digest = H(input)`. Proprietà (✓ = facile, ✗ = computazionalmente infattibile):

- ✓ calcolare `H(input)`; input grande → digest piccolo e di dimensione fissa;
- ✗ invertire: dato il digest, risalire all'input;
- ✓ **resistenza alle collisioni**: `P(H(x) = H(y) | x ≠ y) ≈ 0`.

> **Obiettivo:** rivelare i tentativi di **manomissione**. Una modifica minima dell'input cambia radicalmente il digest.
> Es. SHA-512: `hash("Alice->Bob: 1$")` e `hash("Allce->Bob: 1$")` sono completamente diversi.

## 2. Crittografia a chiave pubblica (asimmetrica)

Ogni utente possiede **due chiavi**: una **pubblica** (`K_pub`, disponibile a tutti) e una **privata** (`K_pr`, tenuta segreta). Proprietà:

- ✓ creare la coppia di chiavi; ✓ cifrare con una chiave; ✓ decifrare **con l'altra** chiave;
- ✗ decifrare con la chiave sbagliata; ✗ derivare una chiave dall'altra.

**Obiettivi:** confidenzialità, **autenticità**, **non-ripudio**. Use case tipico: *public-key authentication*. → nella blockchain gli utenti sono identificati da `f(K_pub)` ([[blockchain-world-state|user identifiers]]).

## 3. Schemi composti

### Hash chain
Le funzioni hash si compongono in **catene di hash**: si tiene traccia di **sequenze di eventi** (e del loro ordine relativo). Ogni evento *event_i* è in un blocco *b_i*; ogni blocco *b_{i+1}* registra l'hash del precedente `H(b_i)` → impedisce di **alterare/manomettere la sequenza** (cambiare un blocco passato cambierebbe tutti gli hash successivi). È lo scheletro di [[blocks-e-block-chain]].

### Firma digitale
Il mittente allega alla sua firma al messaggio:

```
Signature = Encrypt_Priv( H(Message) )
```

verificabile da chiunque con `K_pub`. Garantisce:
- **Autenticità** — mittente dichiarato = mittente reale;
- **Integrità** — il messaggio non è stato manomesso.

→ è ciò che rende valide le [[blockchain-transactions|transazioni]] (firma = prova di chi le ha emesse).

## 4. Connessioni

- [[notary-service]] — hash chain + timestamp + firme ⇒ servizio di timestamping/notary.
- [[blocks-e-block-chain]] — la hash chain è la struttura della catena di blocchi.
- [[blockchain-transactions]] — le firme certificano emittente e integrità delle TX.
- [[causalita]], [[checkpointing-e-logging]] (C2) — la hash chain ordina gli eventi come un log immutabile; parente del timestamping di Haber & Stornetta 1991.

## 5. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Background — Cryptographic Tools & Schemes), slide 40–44.
