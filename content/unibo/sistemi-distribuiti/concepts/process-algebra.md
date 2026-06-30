---
title: "Process Algebra (Algebra dei Processi)"
course: Sistemi Distribuiti
category: concept
topics: [process-algebra, concorrenza, semantica, formalismi, comportamento]
difficulty: avanzato
sources: [M9-Modelling-Distributed-Systems-Process-Algebra.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Process Algebra (Algebra dei Processi)

## 1. Definizione

> Una **process algebra** è una **tecnica di descrizione formale** per sistemi informatici complessi, specialmente quelli con **componenti concorrenti che eseguono in parallelo e comunicano** [Bergstra et al., 2001].

Scomposizione dei termini:
- **process** — una serie di azioni o eventi;
- **algebra** — un calcolo di simboli che si combinano secondo **leggi (axioms) definite**;
- **calculus** — un sistema o metodo di calcolo.

Vista assiomatica [Baeten, 2005]: *una process algebra è una qualunque **struttura matematica che soddisfa gli assiomi** dati per gli operatori di base; un **processo** è un **elemento** di una process algebra*. Usando gli assiomi si possono **eseguire calcoli sui processi**.

> Sintesi storica [Baeten, 2005]: la process algebra è lo **studio del comportamento di sistemi concorrenti/distribuiti per via algebrica**.

## 2. Intuizione: perché esiste

È la **risposta diretta** alla domanda aperta di M8 ([[software-architecture]]):

- le **architetture software/di sistema** modellano **struttura e organizzazione**, **non il comportamento** → le proprietà *comportamentali* di un SD si capiscono **qualitativamente, non si dimostrano* davvero;
- le proprietà non sono realmente **provate** senza una **rappresentazione formale ben definita** (eco di [[computer-science-foundations]], M2: i risultati di impossibilità sono *teoremi*, servono ontologia e nozione di prova condivise).

La via scelta: **modelli formali per sistemi concorrenti**. La **concorrenza** è la *prima fonte di complessità* nei sistemi distribuiti (→ [[parallelo-concorrente-distribuito]]). Un modello formale:
- cattura l'**essenza** dei sistemi concorrenti/distribuiti, **astraendo** dai dettagli non necessari (e dalla **collocazione fisica/distribuzione** — come fa la software architecture, ma in modo **più formale**);
- fornisce una rappresentazione **non ambigua** delle entità computazionali e del loro comportamento;
- offre la possibilità di **calcolare proprietà**.

Tra i vari approcci possibili, il corso si concentra sulla **process algebra**.

## 3. Contesto storico

- **Prima della process algebra** [Baeten, 2005]: per ragionare sui sistemi concorrenti esistevano essenzialmente solo le **Petri Nets** [Peterson, 1977]. Il ragionamento formale era per lo più focalizzato sul dare **semantica (significato)** ai linguaggi di programmazione. Tre stili di semantica:

  | Semantica | Idea |
  |---|---|
  | **operational** | il programma è modellato come l'**esecuzione di una macchina astratta**; uno **stato** è una valutazione delle variabili, una **transizione** tra stati è un'istruzione elementare |
  | **denotational** | più astratta: il programma è modellato come una **funzione** che trasforma input in output |
  | **axiomatic** | enfasi sui **metodi di prova** di correttezza; nozioni centrali: **asserzioni** (triple precondizione / statement / postcondizione) e **invarianti** |

- Modelli iniziali: il comportamento come **funzione input/output**, il processo come **automa** (stati + transizioni). **Cosa manca?** → **interazione** e **sistemi**. Da qui la process algebra **tratta sistemi interagenti** e processi **concorrenti** (collega l'idea M2 di [[sistema-computazionale]]: processi che *behave + interact*).

- Le **process algebra** più note:
  - **ACP** — *Algebra of Communicating Processes* [Bergstra & Klop, 1984];
  - **CCS** — *Calculus of Communicating Systems* [Milner, 1980];
  - **CSP** — *Communicating Sequential Processes* [Brookes/Hoare/Roscoe, 1984].

## 4. Ingredienti comuni

Tutte le process algebra condividono tre ingredienti chiave [Bergstra et al., 2001]:

1. **compositional modelling** — un piccolo numero di **costrutti** per costruire sistemi grandi a partire da sistemi più piccoli (→ vedi gli operatori in [[algebra-processi-operatori-leggi]]);
2. **operational semantics** — sono equipaggiate di **Structural Operational Semantics (SOS)** [Plotkin, 1981], che descrive l'esecuzione **single-step** e permette di tradurre i termini dell'algebra in **Labelled Transition Systems (LTS)**;
3. **behavioural reasoning** — uso di **relazioni comportamentali** (behavioural relations) per **mettere in relazione** sistemi diversi espressi nell'algebra [Tribastone, 2014].

> Nota terminologica [Baeten, 2005]: *algebra* denota un approccio **algebrico/assiomatico** al comportamento dei sistemi; la process algebra usa metodi e tecniche dell'**algebra universale** [MacLane & Birkhoff, 1967]. ⚠️ Spesso, però, la process algebra **oltrepassa i confini** dell'algebra universale.

## 5. A cosa serve: specifica e verifica

Process algebra come strumento a doppia valenza [Baeten, 2005]:

- **specification** — rappresenta i sistemi concorrenti (struttura **e** comportamento) → si possono **specificare**;
- **verification** — permette di **calcolare il comportamento** dei sistemi → si possono **verificare**.

**Come si specifica** un modello per un sistema concorrente/distribuito *S*:
1. si sceglie il **framework algebrico** più adatto e le **leggi strutturali** specifiche;
2. si definiscono le **azioni atomiche** di *S*;
3. si definisce il **comportamento** in termini di **transizioni** conformi alle leggi strutturali.

**Come si verifica** una proprietà di *S*: si sfruttano **assiomi e transizioni** per **derivare** le proprietà.

→ Il nucleo formale (operatori, leggi, transizioni, branching/linear time) è in **[[algebra-processi-operatori-leggi]]**.

## 6. Bilancio (lessons learnt)

**Pro:**
- i **modelli formali sono essenziali** nei SD per **provare proprietà** e per **vincolare l'implementazione** di componenti e sistemi;
- la process algebra è tra le tecniche **più usate** per rappresentare i SD e ragionare sulle loro proprietà;
- non è troppo difficile usarla per rappresentare alcune proprietà di base; la **prova si può costruire sui sistemi**.

**Contro:**
- le **prove non sono intuitive** — non abbastanza da essere uno strumento quotidiano per gli ingegneri del software;
- si è trattata solo la **computazione concorrente**: **nessun accenno alla distribuzione fisica**. Quindi sappiamo trattare i sistemi **concorrenti** (almeno in linea di principio), **non** ancora i sistemi **distribuiti** veri e propri. *Esistono tecniche formali per la distribuzione, ma il corso si accontenta della process algebra.*

> ⚠️ Limite dichiarato: la process algebra astrae **via** la distribuzione spaziale (→ [[computing-with-space]], M7) — esattamente come l'architettura software, ma in modo formale. È il duale formale della distinzione [[software-vs-system-architecture]].

## 7. Connessioni

- [[algebra-processi-operatori-leggi]] — il nucleo formale: operatori (`+`, `;`, `∥`), 7 leggi, transizioni SOS, branching vs linear time.
- [[software-architecture]] (M8) — modella *struttura*; la process algebra ne è il **complemento** per il *comportamento*. → [[architetture-vs-process-algebra]].
- [[architetture-vs-process-algebra]] — bridge: informale/espressivo (M8) vs formale/dimostrabile (M9).
- [[computer-science-foundations]] (M2) — il bisogno di **rigore** e di una nozione di prova: la process algebra è una risposta parziale.
- [[parallelo-concorrente-distribuito]] (M2) — la **concorrenza** come prima fonte di complessità; la process algebra ne è il modello formale.
- [[sistema-computazionale]] (M2) — processi che *behave + interact*: la process algebra studia il **comportamento** di **sistemi interagenti**.

## 8. Sorgenti

- `raw-sources/M9-Modelling-Distributed-Systems-Process-Algebra.pdf` (Prologue; Basics; Conclusion), slide 4–14, 27–28.
- Riferimenti: [Baeten, 2005] *A brief history of process algebra*; [Bergstra et al., 2001] *Handbook of Process Algebra*; [Bergstra & Klop, 1984] (ACP); [Milner, 1980] (CCS); [Brookes et al., 1984] (CSP); [Plotkin, 1981] (SOS); [Peterson, 1977] (Petri Nets); [MacLane & Birkhoff, 1967]; [Tribastone, 2014].
