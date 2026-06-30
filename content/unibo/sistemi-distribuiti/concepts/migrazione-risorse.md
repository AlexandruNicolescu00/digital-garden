---
title: "Migrazione del Codice e Risorse Locali"
course: Sistemi Distribuiti
category: concept
topics: [code mobility, resource segment, binding, risorse, migrazione]
difficulty: avanzato
sources: [C6-Code-Mobility.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Migrazione del Codice e Risorse Locali

## 1. Il problema del resource segment

Finora ([[code-mobility]]) si è considerata la migrazione dei soli **code** ed **execution segment**. Ma il **resource segment** è il punto dolente:

> Le **risorse potrebbero non essere così facili da spostare** come codice e variabili. *Esempio:* un enorme database potrebbe **in teoria** essere spostato in rete, ma **in pratica** non lo sarà.

Due strade: **aggiornare i riferimenti**, oppure **spostare le risorse**. La scelta dipende da **due dimensioni di binding**:
1. **come il resource segment fa riferimento** alla risorsa (process-to-resource);
2. **come la risorsa è legata** alla macchina ospite (resource-to-machine).

## 2. Process-to-resource binding

*Come il processo necessita della risorsa:*

| Binding | Significato | Esempio |
|---------|-------------|---------|
| **by identifier** | serve **quella** risorsa, con un dato **nome** | una URL, un ID locale |
| **by value** | serve una risorsa in base al suo **valore** | librerie di codice |
| **by type** | serve una risorsa in base al suo **tipo** | device locali: stampanti, monitor |

> Forza del legame decrescente: *identifier* (la risorsa esatta) → *value* (un equivalente per valore) → *type* (un qualunque esemplare del tipo).

## 3. Resource-to-machine binding

*Quanto la risorsa è legata alla macchina:*

| Binding | Significato | Esempio |
|---------|-------------|---------|
| **unattached** | si sposta **facilmente** tra macchine | file associati al codice migrante |
| **fastened** | si sposta, **ma a un costo** | un database locale |
| **fixed** | **legata** a una macchina specifica | un monitor |

## 4. La matrice delle azioni

L'azione da intraprendere sui riferimenti alle risorse locali, quando si migra il codice, dipende dall'**incrocio** delle due dimensioni (3×3) [Tanenbaum & van Steen, 2017]: a seconda della combinazione si **rebinda** il riferimento (a una risorsa locale del target), si **muove** la risorsa, o si **stabilisce un riferimento globale** (rete). Es. una risorsa *fixed* legata *by type* → tipicamente si **rebinda** a un esemplare locale del target (una stampante locale); una risorsa *unattached* legata *by identifier* → conviene **muoverla** col codice.

> 🔗 Questo richiama il problema del **naming** e della [[trasparenza|location/access transparency]] (M4): un sistema di identificatori logici svincolati dalla posizione fisica facilita il rebinding.

## 5. Connessioni

- [[code-mobility]] — il quadro generale (i 3 segmenti); qui si dettaglia il **resource segment**.
- [[weak-vs-strong-mobility]] — la mobilità del codice/esecuzione vs il problema (ortogonale) delle risorse.
- [[trasparenza]] (M4) — naming e location transparency facilitano il rebinding dei riferimenti.
- [[middleware]] — il middleware media l'accesso alle risorse e i loro binding.

## 6. Sorgenti

- `raw-sources/C6-Code-Mobility.pdf` (Migration and Local Resources), slide 17–20.
- Riferimenti: Fuggetta et al. 1998; Tanenbaum & van Steen 2017.
