---
title: "Centralizzato vs Distribuito"
course: Sistemi Distribuiti
category: bridge
topics: [confronto, trade-off, architetture]
difficulty: base
sources: [M0-Why-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Centralizzato vs Distribuito

Confronto sistematico tra sistemi centralizzati e distribuiti, secondo [Puder et al., 2005].

## Tabella di confronto

| Criterio | Sistema centralizzato | Sistema distribuito |
|----------|----------------------|---------------------|
| **Economics** (economicità) | bassa | alta |
| **Availability** (disponibilità) | bassa | alta |
| **Complexity** (complessità) | bassa | alta |
| **Consistency** (consistenza) | semplice | difficile |
| **Scalability** (scalabilità) | scarsa | buona |
| **Technology** (tecnologia) | omogenea | eterogenea |
| **Security** (sicurezza) | alta | bassa |

## Come leggere la tabella

Non esiste un vincitore assoluto: il passaggio al distribuito **migliora** alcuni criteri e **peggiora** altri.

- **A favore del distribuito**: economicità, disponibilità, scalabilità. Sono le ragioni per cui si distribuisce (→ [[motivazioni-sistemi-distribuiti]]).
- **A favore del centralizzato**: semplicità della consistenza, omogeneità tecnologica, sicurezza, minore complessità.

Le voci "complessità alta" e "consistenza difficile" del distribuito sono il **prezzo** da pagare, e sono esattamente i problemi che il resto del corso affronta:

- la **consistenza difficile** nasce dalla perdita dell'ordinamento totale degli eventi → [[ordinamento-parziale-eventi]]; ed è formalizzata dal [[teorema-cap]] (in presenza di partizioni si deve scegliere tra [[consistency]] e [[availability]])
- la **disponibilità alta** dipende dalla tolleranza ai guasti → [[motivazioni-sistemi-distribuiti]]; vedi anche [[availability]] e [[partition-tolerance]]

## Sintesi: quando scegliere cosa

Si sceglie un sistema distribuito quando il problema **richiede** distribuzione geografica, scala, o resilienza ai guasti, ed è ragionevole sostenere il costo aggiuntivo di complessità e di gestione della consistenza. Si preferisce il centralizzato quando semplicità, sicurezza e consistenza forte sono prioritarie e i requisiti di scala/disponibilità sono modesti.

> Nota: la riga "Security: distribuito = bassa" è la valutazione di [Puder et al., 2005] e va intesa in senso relativo (più superfici d'attacco, più canali); non è un giudizio assoluto e sarà sfumata nei moduli su sicurezza e fault tolerance.

## Connessioni

- [[sistema-distribuito]] — il concetto centrale
- [[motivazioni-sistemi-distribuiti]] — perché si accettano gli svantaggi
- [[ordinamento-parziale-eventi]] — la radice teorica della "consistenza difficile"
- [[scalabilita]] — **(M4)** la centralizzazione (servizi/dati/algoritmi) **ostacola la scalabilità**: usare solo algoritmi decentralizzati
- [[pitfalls-sistemi-distribuiti]] — **(M4)** la fallacia "c'è un solo amministratore" vs realtà multi-dominio

## Sorgenti

- `raw-sources/M0-Why-Distributed-Systems.pdf` (slide 9)
- [Puder et al., 2005] *Distributed Systems Architecture: A Middleware Approach*, Morgan Kaufmann.
