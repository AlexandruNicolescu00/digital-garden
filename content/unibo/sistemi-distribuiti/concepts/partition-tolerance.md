---
title: "Partition Tolerance (Tolleranza alle Partizioni)"
course: Sistemi Distribuiti
category: concept
topics: [CAP, partition-tolerance, network, failure]
difficulty: intermedio
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Partition Tolerance (Tolleranza alle Partizioni)

## Definizione

La **partition tolerance** è la capacità del sistema di **continuare a fornire servizi** anche quando la rete si **partiziona** [Zhao, 2014].

Definizione di partizione [Gilbert and Lynch, 2002]:

> "Quando una rete è partizionata, tutti i messaggi inviati dai nodi di una componente verso i nodi di un'altra componente vengono **persi**."

Inoltre: *qualsiasi* pattern di perdita di messaggi può essere modellato come una **partizione temporanea** che separa i nodi nell'esatto istante in cui il messaggio è perso. Operativamente: se A invia un messaggio a B e questo **non arriva**, la rete è partizionata.

## Intuizione: perché P non è negoziabile

Nei sistemi reali — specie quelli **pervasivi nell'era IoT**, dove l'**instabilità della rete è la norma** [Grimm et al., 2004] — rinunciare alla partition tolerance **non è davvero un'opzione**: le partizioni *accadono* e basta.

Per questo, anche se in linea di principio si potrebbe sacrificare P (tenendo C+A), in pratica il [[teorema-cap]] costringe a scegliere tra **availability e consistency**.

## P è on/off, non uno spettro

Differenza concettuale importante:
- [[consistency]] e [[availability]] **variano su uno spettro** di gradi;
- la partition tolerance è più un **interruttore on/off**: o tolleri le partizioni o no.

## Le tre combinazioni "pick two"

| Si rinuncia a... | Si ottiene | Realismo |
|------------------|-----------|----------|
| Partition tolerance | **CA** (consistent + available) | poco realistico (le partizioni accadono) |
| Consistency | **AP** (available + partition-tolerant) | comune (es. BASE) |
| Availability | **CP** (consistent + partition-tolerant) | comune (es. attese su transazioni) |

## Connessioni

- [[teorema-cap]] — il teorema completo
- [[consistency]] · [[availability]] — le altre due proprietà
- [[pokemon-go-cap]] — perché in un gioco location-based P è obbligatoria
- [[distribuzione-spaziale-temporale]] — le partizioni come perdita di compresenza/canali

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-...pdf` (slides 11, 15, 28)
