---
title: "Stato Globale e Consistenza"
course: Sistemi Distribuiti
category: concept
topics: [global-state, consistency, channel-state, recovery]
difficulty: avanzato
sources: [C2-Logging-Checkpointing.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Stato Globale e Consistenza

## System model e fault model

**System model** [Zhao, 2014]: **N processi** che interagiscono via **message exchange**, e con l'esterno via **input/output**.

**Fault model** (assunzioni di questo modulo):
- il fallimento avviene in un **processo**; quando fallisce, **si ferma e perde tutto lo stato volatile** → **fail-stop** (→ [[modelli-di-fallimento]]);
- la comunicazione è **affidabile** (es. TCP) e **FIFO** → ordinamento dei messaggi mantenuto, **nessuna partizione di rete**.

> ⚠️ Contrasto con il [[teorema-cap]]: qui si **assume via** la partizione di rete. Logging/checkpointing affrontano i guasti di *processo*, non le partizioni.

## Stato di processo e stato globale

- **Stato di processo**: nella forma più generale, l'intero **address space** nel SO (registri, stack, heap…); una libreria di checkpointing generica salva tutto l'address space (ma l'application semantics può definire uno stato più piccolo). Cfr. [[stato-sistema-distribuito]].
- **Stato globale**: lo stato di **tutti** i processi del sistema. Ma **l'aggregazione non basta**: gli stati dei processi sono **correlati** dagli scambi di messaggi (l'informazione scambiata cambia lo stato → dipendenze causali → [[causalita]]). **Le dipendenze non possono essere perse** nello stato globale.

## Consistenza dello stato globale (i 3 scenari)

Esempio: P₀ e P₁ sono due conti bancari A e B; m₀ è un deposito di $100 da A a B.

| Scenario | Checkpoint | Recuperabile? | Problema |
|----------|-----------|---------------|----------|
| **(a) inconsistente** | C₁ riflette la ricezione di m₀, C₀ no | ❌ | stato **non raggiungibile** dallo stato iniziale; i $100 "appaiono dal nulla" |
| **(b) consistente** | C₀ e C₁ coerenti su m₀ | ✅ | i $100 si spostano correttamente da A a B |
| **(c) consistente ma non recuperabile** | coerenti, ma m₀/m₁ erano **in transito** | ❌ | le **dipendenze sui messaggi in transito** si perdono → i $100 spariscono |

**Definizione**: uno stato globale è **inconsistente** quando *non è raggiungibile* dallo stato iniziale del sistema (es. un effetto registrato senza la sua causa).

## Channel state: la chiave dello scenario (c)

Per gestire lo scenario (c) serve un ulteriore tipo di stato: lo **stato del canale**.

**System model raffinato**:
- un **processo** = insieme di **stati** + insieme di **eventi** (uno stato è iniziale; gli eventi cambiano stato);
- un **canale** = comunicazione **unidirezionale affidabile** tra due processi (una connessione TCP = **due** canali);
- lo **stato di un canale** = l'insieme dei messaggi **in transito** (inviati ma non ancora ricevuti dal destinatario).

Salvando m₀ in C₀ e m₁ in C₁ **come channel state**, la recovery dallo scenario (c) diventa possibile. Catturare il channel state è ciò che rendono possibile i protocolli di snapshot → [[chandy-lamport-snapshot]], [[tamir-sequin-checkpointing]].

## Connessioni

- [[stato-sistema-distribuito]] — la nozione di stato e il problema del "tempo t" (M1)
- [[causalita]] — perché le dipendenze non vanno perse
- [[checkpointing-e-logging]] — le tecniche che usano questo modello
- [[chandy-lamport-snapshot]] · [[tamir-sequin-checkpointing]] — protocolli che catturano stato globale consistente + channel state
- [[teorema-cap]] — contrasto: qui niente partizioni
- [[modelli-di-fallimento]] — fail-stop

## Sorgenti

- `raw-sources/C2-Logging-Checkpointing.pdf` (slides 18–27)
