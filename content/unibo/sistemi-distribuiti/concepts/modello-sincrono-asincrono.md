---
title: "Modello Temporale: Sincrono vs Asincrono"
course: Sistemi Distribuiti
category: concept
topics: [modello-temporale, sincronia, asincronia, failure-detection]
difficulty: intermedio
sources: [C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Modello Temporale: Sincrono vs Asincrono

## Definizione

Il **modello temporale** di un sistema distribuito riguarda le **assunzioni sui tempi** di comunicazione e di calcolo. Due estremi:

- **Sincrono** — esistono **due bound noti**:
  1. un limite **δ** sul **ritardo di trasmissione** dei messaggi, e
  2. un limite **φ** sulla **velocità relativa** dei processi.
  Questo permette il **failure detection accurato** dei processi (se non rispondi entro il bound, sei crashato).

- **Asincrono** — **nessun bound**. Modella tipicamente un sistema con **carico imprevedibile** su rete e CPU. Conseguenza cruciale: **non è mai possibile sapere se un processo è crashato oppure solo lento**.

## Intuizione: perché è il modello a fare la differenza

L'asincronia rende impossibile distinguere un processo **morto** da uno **lentissimo**: un messaggio che non arriva può significare "crash" o "arriverà tra un istante". È **questa indistinguibilità** a rendere il consenso impossibile nel caso asincrono puro → è l'ipotesi chiave del teorema [[flp-impossibility|FLP]].

> "Knowing how the models affect the properties of the systems is essential": cambiare il modello temporale **cambia quali problemi sono risolvibili**.

## Varianti e casi limite: come si "evade" da FLP

Nessun sistema reale è perfettamente sincrono né si accontenta dell'asincrono puro. Le vie di mezzo sono ciò che rende il consenso praticabile:

- **In pratica**: si accetta che a volte il sistema **non sia disponibile**, mitigando con **timer e backoff** (failure detector imperfetti basati su timeout).
- **In teoria**: si fanno **assunzioni di sincronia più deboli** (es. *partial synchrony*) — "i messaggi arrivano *entro un anno*": basta un bound, anche enorme e sconosciuto, per recuperare la terminazione.

## Connessioni

- [[flp-impossibility]] — l'asincronia pura è l'ipotesi che genera l'impossibilità
- [[agreement-consensus]] — il modello temporale è parte dell'ontologia del problema
- [[paxos]] — assume sincronia parziale (terminazione solo in intervalli "abbastanza lunghi")
- [[modelli-di-fallimento]] — il modello di fallimento si combina con quello temporale (M1)
- [[teorema-cap]] — il CAP usa un modello asincrono senza clock globale (parentela con FLP, → [[cap-vs-flp]])
- [[ordinamento-parziale-eventi]] — assenza di clock globale e ordine totale (M0)
- [[logical-clock]] — **(C5)** senza clock fisico globale si usa il **tempo logico** (ancorato alla causalità, non all'orologio)
- [[physical-clock-synchronization]] · [[tempo-nei-sistemi-distribuiti]] — **(M6)** l'altra via: sincronizzare gli orologi fisici (UTC/NTP); "synchronous" = stesso tempo

## Sorgenti

- `raw-sources/C3-The-Problem-of-Consensus-in-Distributed-Systems.pdf` (slide 8, 26; A. Omicini)
