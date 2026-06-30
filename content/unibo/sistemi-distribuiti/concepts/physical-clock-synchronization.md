---
title: "Sincronizzazione degli Orologi Fisici"
course: Sistemi Distribuiti
category: concept
topics: [physical clock, sincronizzazione, UTC, NTP, Berkeley, skew, drift, offset]
difficulty: avanzato
sources: [M6-Computing-with-Time.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Sincronizzazione degli Orologi Fisici

## 1. Cos'è un orologio (fisico)

Un "clock" in un computer è in realtà un **timer** [Kshemkalyani & Singhal, 2011]: tipicamente un **quarzo oscillante** con un **counter** e un **holding register**. Quando il counter arriva a zero si genera un **interrupt** (un **clock tick**) e il counter viene ricaricato dal register.

## 2. Perché sincronizzare

- **Centralizzato:** **non serve** sincronizzazione — c'è un solo clock; un processo ottiene l'ora con una system call, e una richiesta successiva ottiene sempre un valore **maggiore** → **total ordering** degli eventi gratis.
- **Distribuito:** **niente clock globale, niente memoria comune**; ogni processo ha il proprio clock interno, che **deriva (drift)** di vari secondi al giorno; anche sincronizzati all'avvio, tick diversi portano a differenze → serve **clock synchronisation**... *o forse no?* (→ a volte basta il [[logical-clock|tempo logico]]).

> **Definizione.** La *physical clock synchronisation* è il processo che garantisce che processori **fisicamente distribuiti** abbiano una **nozione comune di tempo**.

**Perché serve** (per molte applicazioni/algoritmi):
- il **tempo del giorno** in cui un evento è accaduto su una macchina;
- l'**intervallo di tempo** tra due eventi su macchine diverse;
- l'**ordine relativo** di eventi su macchine diverse.

## 3. Terminologia [Kshemkalyani & Singhal, 2011]

Date due macchine `a, b` con clock `C_a, C_b`:

| Termine | Definizione |
|---------|-------------|
| **time** | `C_a(t)` = tempo del clock di `a`; per un **clock perfetto** `C_a(t) = t` |
| **frequency** | `C_a'(t)` = ritmo a cui il clock progredisce (derivata **prima** rispetto al tempo) |
| **offset** | `C_a(t) − t` = differenza tra clock e tempo reale; `C_a(t) − C_b(t)` = offset relativo |
| **skew** | differenza di **frequenza** tra clock e clock perfetto: `C_a'(t) − C_b'(t)` (skew relativo) |
| **drift** | `C_a''(t)` = derivata **seconda** rispetto al tempo; `C_a''(t) − C_b''(t)` = drift relativo |

### Inaccuratezza
Il produttore specifica la **massima skew rate** `ρ`; un clock è **entro le specifiche** se:

```
1 − ρ  ≤  dC/dt  ≤  1 + ρ
```

→ **fast clock** `dC/dt > 1`, **perfect clock** `= 1`, **slow clock** `< 1` (rispetto a UTC).

## 4. Global absolute time (UTC)

Per sincronizzarsi, i clock fisici hanno bisogno di uno **standard di tempo reale accurato**, es. **UTC (Universal Coordinated Time)** [ITU, 2002]:
- il tempo assoluto è gestito dal **BIPM** (Bureau International des Poids et Mesures, Sèvres): combina/media gli standard atomici ufficiali dei paesi membri → un'unica **UTC**;
- diffuso come **impulso radio (WWV)** dal **NIST** ogni secondo UTC, e via **satellite**;
- se **una** macchina ha accesso a UTC, un algoritmo sincronizza tutte le altre.

**Esempio — NTP (Network Time Protocol)** [RFC 5905, v4]: un time server ha il tempo assoluto globale e le altre macchine si sincronizzano.
> ⚠️ **I clock possono solo andare avanti:** le correzioni **non** possono riportare indietro il clock (si rallenta/accelera, mai si "torna indietro", per non violare la monotonicità degli eventi). → è l'UTC-via-NTP assunto in [[byzantine-fault-tolerance|SMR/blockchain]] (C4).

## 5. Absolute vs Relative time

Le esigenze applicative variano, e un time server potrebbe non essere disponibile in modo stabile (o per niente):
- a volte serve un tempo comune **legato al tempo fisico** → **global absolute time** (es. UTC);
- altre volte basta **condividere un tempo comune**, anche **slegato** dal tempo fisico → **global relative time**.

### Global relative time
Serve solo che esista un tempo **comune**, indipendentemente dall'assoluto: algoritmi in cui **server attivi interrogano (poll)** gli altri per trovare il tempo **medio** e le correzioni stimate; **nessuna** macchina deve avere UTC. Esempi:
- **Berkeley Algorithm** [Gusella & Zatti, 1989]: i *time daemon* di tutte le macchine si interrogano e rispondono, accordandosi su un tempo comune;
- **RBS (Reference Broadcast Synchronisation)** [Elson et al., 2002]: tempo globale relativo nelle reti **wireless**.

## 6. Connessioni

- [[tempo-nei-sistemi-distribuiti]] — l'hub: la risposta "tempo fisico" all'issue of time.
- [[physical-vs-logical-time]] — quando basta il tempo logico invece di sincronizzare gli orologi.
- [[logical-clock]] (C5) — l'alternativa relazionale (ordine, non istante).
- [[modello-sincrono-asincrono]] (C3) — sincronia/asincronia e bound sui delay; clock sync ≈ assunzione (parzialmente) sincrona.
- [[byzantine-fault-tolerance]] (C4) — l'assunzione "macchine con UTC via NTP" per ordinare gli eventi in SMR.
- [[distributed-pervasive-systems]] (M5) — RBS per le reti di sensori wireless.

## 7. Sorgenti

- `raw-sources/M6-Computing-with-Time.pdf` (Time in Distributed Systems — Physical Time), slide 25–36.
- Riferimenti: Kshemkalyani & Singhal 2011; ITU 2002 (UTC); Nelson et al. 2005 (WWV/NIST); Gusella & Zatti 1989 (Berkeley); Elson et al. 2002 (RBS).
