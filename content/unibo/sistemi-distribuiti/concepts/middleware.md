---
title: "Middleware"
course: Sistemi Distribuiti
category: concept
topics: [middleware, interoperabilità, standard, integrazione, DLT]
difficulty: base
sources: [C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf, M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Middleware

## 1. Definizione

**Definizione tradizionale (verticale).** Il *middleware* è il software che **sta nel mezzo** tra il sistema operativo e le applicazioni:

```
        Applicazioni
   ─── top interface ───
         Middleware
  ─── bottom interface ──
     Sistema Operativo
```

La definizione tradizionale enfatizza i **layer verticali**: le applicazioni stanno sopra il middleware, che sta sopra l'OS; ci sono interfacce middleware-applicazione (top) e middleware-OS (bottom). [Tanenbaum & van Steen, 2017]

## 2. Intuizione — perché il middleware esiste

I problemi pratici dello sviluppo software:

- lo sviluppo software è difficile;
- i progettisti esperti sono rari (e costosi);
- le applicazioni diventano sempre più complesse.

Il middleware risponde **fattorizzando** lo sforzo:

- è sviluppato **una volta per molte applicazioni** → ci si può permettere progettisti migliori;
- **fornisce servizi** alle applicazioni (autenticazione, sicurezza, accesso a risorse condivise, …);
- **astrae dallo specifico OS** (portabilità).

> ⭐ La domanda-chiave di C4: tra i "servizi caldi" che vorremmo, c'è il **consenso**? Dovremmo riscrivere gli [[agreement-consensus|algoritmi di consenso]] da zero per ogni applicazione, o vorremmo averli **come parte del middleware**? → è esattamente ciò che offre la [[distributed-ledger-technology|DLT]].

## 3. Dimensioni concettuali

### Interoperabilità ed EAI
La feature-chiave del middleware è l'**interoperabilità**: applicazioni sullo stesso middleware interoperano (come accade su qualunque piattaforma comune, es. il file system dell'OS). Ma esistono molti middleware **incompatibili** tra loro: le app su A interoperano, quelle su B pure, ma A-app e B-app spesso no. Da qui il compito dell'**Enterprise Application Integration (EAI)**: enfasi sulla comunicazione **orizzontale** (application-to-application, middleware-to-middleware). → In M5 l'EAI è una sotto-classe dei [[distributed-information-systems]] e il middleware vi agisce da **communication facilitator** (integrazione anche a livello *applicativo*, non solo dei dati); il **grid middleware** ([[distributed-computing-systems]]) dà accesso uniforme a risorse disperse.

### Integrità concettuale
Lo sviluppo non avviene nel vuoto: quasi ogni progetto deve convivere con sistemi **legacy** (mai tempo/risorse per ripartire da zero). L'integrazione di sistemi è l'unica via d'uscita, ma la prima vittima è l'**integrità concettuale** — la proprietà di un sistema di essere comprensibile e spiegabile attraverso un insieme coerente e limitato di concetti. Il middleware, come **tecnologia di integrazione**, cerca di limitare il *conceptual drift* **gettando un modello** comune su applicazioni eterogenee.

### Astratto vs concreto
- **middleware astratto** = un *modello* comune (es. "oggetti distribuiti": CORBA, Java RMI, MS DCOM, OSGi condividono il modello degli oggetti raggiungibili in rete);
- **middleware concreto** = una *infrastruttura* comune, che punta all'interoperabilità reale e quindi deve gestire dettagli molto più fini (es. fino a CORBA 2.0 due implementazioni CORBA di vendor diversi non interoperavano).

### Standard e network effect
Trattandosi di infrastruttura, conta il **network effect**: il valore di una tecnologia cresce col numero di adottanti. Gli sforzi di **standardizzazione** diventano critici per costruire massa critica (consorzi: OMG CORBA, FIPA, OSGi, W3C). I grandi player spingono la propria tecnologia come *de facto standard*. → Lo stesso vale per la [[distributed-ledger-technology|DLT]] (ITU FG DLT, ISO/TC 307).

## 3-bis. La vista architetturale (M4)

In M4 [Tanenbaum & van Steen, 2017] il middleware è presentato come la **vista architetturale** del sistema distribuito: *un sistema distribuito è organizzato attorno a un middleware*. Il **layer middleware si estende su più macchine** e offre a ogni applicazione la **stessa interfaccia**.

> **Soluzione di principio a collaboration & amalgamation.** Tramite *separazione*, il middleware:
> - **abilita interazione significativa** tra componenti distribuiti autonomi (problemi di comunicazione: linguaggio dei messaggi e sua interpretazione semantica);
> - **nasconde le differenze** di tecnologia, struttura, comportamento, fornendo ad applicazioni e componenti un'**interfaccia condivisa comune** — *nello stesso modo in cui lo fa un sistema operativo*.

È ciò che permette di passare dalla vecchia interpretazione (non distribuita) dei sistemi a una nuova: estendere la vecchia interpretazione *preserva le buone abitudini ma frena le idee nuove* — e qui il middleware ha "ovviamente un ruolo da giocare". → realizza i due requisiti di [[sistema-distribuito|collaboration & amalgamation]] e abilita gli [[obiettivi-sistemi-distribuiti|obiettivi]] (specie [[openness-sistemi-distribuiti|openness]]).

## 4. Connessioni

- [[distributed-ledger-technology]] — la DLT è presentata come **istanza di middleware**: un layer di sincronizzazione che offre consenso ai processi (come il middleware offre servizi alle applicazioni).
- [[blockchain]] — *"blockchain as middleware"* è la tesi dell'intero modulo C4.
- [[sistema-distribuito]] · [[obiettivi-sistemi-distribuiti]] — **(M4)** il middleware come vista architetturale e mezzo per i goal (collaboration & amalgamation).
- [[openness-sistemi-distribuiti]] — **(M4)** standard, IDL e interoperabilità sono il versante "apertura" del middleware.
- [[centralizzato-vs-distribuito]] — il middleware è ciò che rende governabile l'eterogeneità del distribuito.
- [[kubernetes]] — anche K8s può essere letto come middleware/infrastruttura comune per il deploy.
- [[physical-space-computational-systems]] (M7) — il middleware come **topologia**: mappa la distribuzione **logica su quella fisica** e supporta la *code mobility*.
- [[code-mobility]] · [[weak-vs-strong-mobility]] (C6) — la **strong mobility** richiede il supporto del middleware (es. JADE).
- [[architectural-styles]] (M8) — il middleware abilita gli stili **event-based** (pub/sub) e **shared data-space**; è il connettore.
- [[socket]] (M2) — il **substrato di basso livello**: il middleware nasconde i socket offrendo modelli di comunicazione di più alto livello (RPC, oggetti distribuiti, MOM).

## 5. Sorgenti

- `raw-sources/C4-Distributed-Ledger-Technology-Blockchain-as-Middleware.pdf` (Parte I — Middleware), Omicini & Ciatto, A.Y. 2025/2026, slide 7–15.
- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Definitions & Issues — vista architetturale), slide 12–14.
- Riferimento: Tanenbaum & van Steen, 2017.
