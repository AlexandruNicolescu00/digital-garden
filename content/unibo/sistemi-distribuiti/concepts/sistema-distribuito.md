---
title: "Sistema Distribuito"
course: Sistemi Distribuiti
category: concept
topics: [introduzione, modelli, distribuzione, definizioni]
difficulty: base
sources: [M0-Why-Distributed-Systems.pdf, M4-Definitions-Goals-for-Distributed-Systems.pdf, preliminaries_slides.pdf]
created: 2026-06-25
updated: 2026-06-27
---

# Sistema Distribuito

## Definizione

Un **sistema distribuito** è un sistema artificiale il cui nucleo è costituito da componenti computazionali **distribuiti nello spazio e nel tempo**, che interagiscono tra loro, con gli esseri umani e con l'ambiente fisico. Concepire e costruire sistemi artificiali oggi significa, di fatto, avere a che fare con sistemi distribuiti.

> Nota: questa è la caratterizzazione introduttiva data nel modulo M0. La definizione più operativa è stata raffinata in M2.

**Definizione operativa** [Tanenbaum & van Steen, 2017]: *"a collection of independent computers that appears to its users as a single coherent system"* (una collezione di computer indipendenti che appare ai suoi utenti come un unico sistema coerente). Nell'ontologia di M2, un sistema distribuito è un [[sistema-computazionale]] in cui almeno due processi hanno un **contesto spaziale diverso** → [[parallelo-concorrente-distribuito]].

### Le definizioni classiche convergenti (M4)

M4 raccoglie **tre definizioni** classiche, *diverse ma convergenti*:

1. **Vista dell'utente** (*computer scientist*) — *"a collection of **independent computers** that appears to its **users** as a single coherent system"* [Tanenbaum & van Steen, 2017]. È una definizione **osservazionale**, *user-oriented*, dal punto di vista ingegneristico una sorta di definizione **a posteriori**.
2. **Vista dell'ingegnere** (*computer engineer*) — *"a collection of **autonomous computational entities** conceived as a single coherent system by its **designer**"*. È **costruttiva**, *design-oriented*, una definizione **a priori**.
3. **Definizione di Coulouris** [Coulouris et al., 2012] — *"one in which components located at networked computers communicate and coordinate their actions only by **passing messages**"*. Enfatizza: **distribuzione fisica** dei componenti, ruolo dell'**interazione** (comunicazione/coordinazione via rete), **disaccoppiamento** del controllo.

**Osservazioni:** nessuna definizione si concentra specificamente su *processi* o *dispositivi*, né assume nulla sulla loro natura → **eterogeneità**. In tutte, il sistema è visto come **single coherent system** (vista utente o ingegnere, o entrambe): serve **coerenza** sopra **molteplicità ed eterogeneità**.

### Due definizioni complementari (M2, Ciatto)

Il modulo ingegneristico (Module 2) ne aggiunge due, utili come "estremi" interpretativi:

- **L. Lamport (1987)** — *"A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable"*. Mette al centro le **interdipendenze nascoste** tra macchine: un guasto in una parte ha conseguenze **impreviste** altrove. È la lente della **[[dependability|fault tolerance]]** (cfr. anche le [[pitfalls-sistemi-distribuiti|fallacie]]).
- **Van Roy & Haridi** — *"A distributed system is a set of computers that are linked together by a network"*: definizione **molto astratta**, focalizzata sul *cosa* (computer + rete) più che sul *come*.

Insieme a Tanenbaum & van Steen e Coulouris, queste quattro definizioni coprono le caratteristiche essenziali: **componenti indipendenti**, **comunicazione via messaggi**, **sfida di presentare un sistema unificato** nonostante guasti e complessità interne.

### Working as one, looking as one (M4)
Realizzare il "sistema coerente unico" pone due sfide complementari:
- **collaboration** — molte entità autonome devono *collaborare* per ottenere **coerenza** (lavorare come un unico sistema);
- **amalgamation** — molte entità eterogenee devono *amalgamarsi* per ottenere **uniformità** (apparire come un unico sistema).

La soluzione di principio è il **[[middleware]]**, che per *separazione* abilita l'interazione e nasconde le differenze. → I bersagli ingegneristici sono gli [[obiettivi-sistemi-distribuiti|obiettivi (goals)]]; le trappole tipiche sono le [[pitfalls-sistemi-distribuiti|fallacie]].

## Intuizione

I sistemi computazionali sono diventati **pervasivi** (→ [[pervasivita-computazione-interazione]]): sono ovunque e interagiscono continuamente. La natura *fisica* dei sistemi artificiali aggiunge complessità rispetto ai sistemi puramente computazionali, e lo fa lungo due assi:

- **distribuzione** (spaziale e temporale) → [[distribuzione-spaziale-temporale]]
- **imprevedibilità dell'ambiente** in cui devono operare

Il punto cruciale è che, quando un sistema è distribuito, **una serie di assunzioni comode smettono di valere**: non esiste più un tempo di sistema né una posizione di sistema, e gli eventi non formano più una sequenza totalmente ordinata (→ [[ordinamento-parziale-eventi]]).

## Perché servono i sistemi distribuiti

Si veda la pagina dedicata: [[motivazioni-sistemi-distribuiti]] (ambienti geograficamente distribuiti, accelerazione del calcolo, condivisione di risorse, tolleranza ai guasti).

Per il confronto sistematico con l'alternativa centralizzata: [[centralizzato-vs-distribuito]].

## Le due facce dello studio

Modellare e costruire sistemi distribuiti generano problemi nuovi, su due piani complementari:

- **Modellazione** → nuovi framework teorici, modelli, astrazioni → oggetto della *computer science*
- **Costruzione** → nuove tecnologie, infrastrutture, metodi, metodologie → oggetto della *computer engineering*

Il corso mescola deliberatamente i due piani (teorico e metodologico/tecnologico) fin dall'inizio.

## Connessioni

- [[distribuzione-spaziale-temporale]] — cosa significa concretamente "distribuito"
- [[ordinamento-parziale-eventi]] — la conseguenza teorica più importante
- [[pervasivita-computazione-interazione]] — il contesto che motiva il campo
- [[motivazioni-sistemi-distribuiti]] — perché conviene distribuire
- [[centralizzato-vs-distribuito]] — trade-off rispetto all'approccio centralizzato
- [[obiettivi-sistemi-distribuiti]] — **(M4)** i goal di progetto (resource availability, transparency, openness, scalability, situatedness)
- [[middleware]] — **(M4)** la vista architetturale: il SD è organizzato attorno a un middleware
- [[pitfalls-sistemi-distribuiti]] — **(M4)** le 8 fallacie di chi sottovaluta la distribuzione
- [[sorts-sistemi-distribuiti]] — **(M5)** le tre classi (computing / information / pervasive) in cui si articolano i SD

## Sorgenti

- `raw-sources/M0-Why-Distributed-Systems.pdf` (slides 1–15)
- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Definitions & Issues, slide 7–14)
- `raw-sources/preliminaries_slides.pdf` (slide 3–7 — *What is a Distributed System?*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026 (definizioni Lamport 1987 e Van Roy & Haridi)
- [Ghosh, 2014] *Distributed Systems: An Algorithmic Approach*, CRC Press, 2nd ed.
- [Puder et al., 2005] *Distributed Systems Architecture: A Middleware Approach*, Morgan Kaufmann.
- [Coulouris et al., 2012] *Distributed Systems. Concepts and Design*, Pearson, 5th ed.
