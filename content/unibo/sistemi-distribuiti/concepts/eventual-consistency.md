---
title: "Eventual Consistency"
course: Sistemi Distribuiti
category: concept
topics: [consistency, BASE, replicazione, cloud, client-centric]
difficulty: intermedio
sources: [C1-The-CAP-Theorem-Availability-Consistency-Failure-in-Distributed-Systems.pdf, M3-Replication-Consistency-in-Distributed-Systems.pdf, C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf]
created: 2026-06-25
updated: 2026-06-26
---

# Eventual Consistency

## Definizione

La **eventual consistency** è un modello di consistenza rilassato in cui i dati "stale" (vecchi) sono **temporaneamente tollerati**, a patto che tutte le copie del dato raggiungano *prima o poi* (after a short time) uno stato consistente [Fox et al., 1997].

È la **"E" di BASE** (vedi [[acid-vs-base]]).

## Intuizione

A valle del [[teorema-cap]], se in presenza di partizione si vuole privilegiare l'[[availability]], occorre rinunciare alla [[consistency]] *forte/immediata*. L'eventual consistency è il compromesso: si risponde subito (anche con dati potenzialmente non aggiornati) e si lascia che le repliche **convergano** nel tempo.

Principi correlati [Fox et al., 1997]:
- **Soft state** — lo stato può essere rigenerato a costo di computazione/I/O aggiuntivo; il dato non è durevole.
- **Approximate answers** — risposte approssimate consegnate velocemente possono valere più di risposte esatte lente.

## Inquadramento M3: un modello *client-centric*

In M3 la eventual consistency è classificata come modello **client-centric** ([[client-centric-consistency]]). Scenario di riferimento:
- data store **grande e distribuito** con **quasi nessun conflitto di update**;
- tipicamente **una sola autorità che aggiorna**, e molti processi che **solo leggono**;
- l'unico conflitto è **read-write**: un processo vuole aggiornare mentre un altro legge concorrentemente lo stesso dato;
- esempi: **cambi DNS**, contenuti Web.

Issue: ai lettori possono essere forniti dati non aggiornati; nella maggior parte dei casi tale inconsistenza è **accettabile**. Se per un po' non avvengono update, **gradualmente tutte le repliche diventano consistenti** — da cui il nome *eventual*. Le quattro garanzie per-client (monotonic reads/writes, read-your-writes, writes-follow-reads) la **rafforzano** quando il client cambia replica.

## Nel mondo reale (Cloud)

La maggior parte dei servizi cloud oggi adotta BASE [Birman et al., 2012], es. eBay, Amazon DynamoDB. Dettagli:
- **Amazon S3** — consente **strong read-after-write** consistency.
- **Amazon DynamoDB** — write e read sono eventually consistent, ma si possono configurare **read fortemente consistenti** (più costose).
- I sistemi cercano comunque di **mascherare le inconsistenze** agli utenti.

## Caso blockchain (C4): consistenza *probabilistica*

La [[blockchain]] è un esempio paradigmatico di eventual consistency: scegliendo **A+P** sotto il [[teorema-cap|CAP]], le repliche convergono solo *prima o poi*. Nel [[proof-of-work|PoW]] i **branch** sono inevitabili e la convergenza è governata da una **regola locale** (longest chain / GHOST): finché la maggioranza la segue, un solo ramo viene selezionato da tutti. La consistenza è quindi **probabilistica**: `lim(n→∞) P[inconsistent(B_i)] = 0` al crescere dei blocchi successori ([[blocks-e-block-chain]]). I [[consensus-mining|consensi]] permissioned (BFT) ottengono invece consistenza "esatta" → vedi [[permissioned-vs-permissionless]].

## Connessioni

- [[acid-vs-base]] — il quadro ACID vs BASE in cui questo modello vive
- [[blockchain]] · [[proof-of-work]] · [[permissioned-vs-permissionless]] — eventual consistency (probabilistica) nella DLT (C4)
- [[consistency]] — la versione forte che qui viene rilassata
- [[teorema-cap]] — il risultato che motiva il rilassamento
- [[availability]] — ciò che si guadagna rinunciando alla consistenza forte
- [[consistency-model]] — la cornice generale dei modelli (M3)
- [[client-centric-consistency]] — la famiglia M3 a cui appartiene, con le 4 garanzie per-client
- [[replicazione]] — il rilassamento dei vincoli come via d'uscita dal dilemma replicazione/consistenza
- [[data-centric-vs-client-centric]] — collocazione rispetto ai modelli data-centric

## Modelli di consistenza correlati (ora coperti, M3)

- Data-centric: [[continuous-consistency]], [[sequential-consistency]], [[causal-consistency]]
- Client-centric: [[client-centric-consistency]] (monotonic reads/writes, read-your-writes, writes-follow-reads)

## Sorgenti

- `raw-sources/C1-The-CAP-Theorem-...pdf` (slides 36–38)
- `raw-sources/M3-Replication-Consistency-...pdf` (slide 36; inquadramento client-centric)
- [Fox et al., 1997]; [Birman et al., 2012] *Overcoming CAP with consistent soft-state replication*.
