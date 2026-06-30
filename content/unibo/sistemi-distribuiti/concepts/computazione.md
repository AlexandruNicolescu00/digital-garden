---
title: "Computazione"
course: Sistemi Distribuiti
category: concept
topics: [computazione, processo, macchina, simboli]
difficulty: base
sources: [M2-Roots-of-Distributed-Systems-Computation-in-Space-Time.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Computazione

## La domanda fondante

*"What is computation?"* è la domanda di base attorno a cui ruota tutta la computer science, e su cui ogni studio ben fondato dei sistemi distribuiti dovrebbe poggiare. (Cautela [Freeman, 2011]: uno "standard" rigido adottato troppo presto ostacola il progresso — serve comprensione comune, non irrigidimento.)

## Due caratterizzazioni complementari

### Computazione come manipolazione di simboli [Conery, 2010]
Una computazione è una **sequenza di passi semplici e ben definiti** che portano alla soluzione di un problema. Il problema e la soluzione devono essere **codificati in simboli**; un passo è una **manipolazione di simboli** che trasforma un insieme di simboli in un nuovo insieme.

### Computazione come processo [Frailey, 2010]
L'essenza della computazione si trova in **qualsiasi forma di processo** → la computazione è un processo, e ogni processo è anche una computazione.

> **Process** (Oxford): una serie di azioni/passi per raggiungere un fine; una serie naturale di cambiamenti; un'istanza di programma in esecuzione in un OS multitasking.

## Quando si definisce la computazione [Denning, 2011]

- il **modello computazionale conta**;
- molte computazioni importanti sono **naturali**;
- molte sono **non-terminanti**;
- molte sono **continue**;
- il *computational thinking* può essere definito.

## Macchina e macchina computazionale

**Macchina** (Britannica): dispositivo con uno scopo, che aumenta/sostituisce lo sforzo umano per compiti fisici; ha sempre **input, output** e un dispositivo **trasformante**.

**Input / output / stato** di una macchina:
- **input** = ciò che influenza la macchina dall'esterno
- **output** = come la macchina influenza l'esterno
- **stato al tempo t** = ciò che è necessario per capire l'evoluzione della macchina dopo t, dato un input (o, più in generale, dato il **contesto**). [Stessa nozione di stato vista in [[stato-sistema-distribuito]] (M1).]

Una **macchina computazionale** è diversa: il suo compito è **cognitivo** (non fisico), input/output sono **informazione**, e il suo *contesto* è… il punto chiave. Se non ci si lega a un modello specifico (Turing, von Neumann — artificiali, discreti, singolo dispositivo), conviene **astrarre dalla macchina** e concentrarsi sul **processo computazionale** (→ [[processo-computazionale-contesto]]).

## Connessioni

- [[computer-science-foundations]] — perché la computazione è il noumeno della CS
- [[processo-computazionale-contesto]] — l'ontologia costruita su questa nozione
- [[stato-sistema-distribuito]] — la nozione di stato "al tempo t"

## Sorgenti

- `raw-sources/M2-Roots-of-Distributed-Systems-...pdf` (slides 21–29)
- [Conery, 2010]; [Frailey, 2010]; [Denning, 2011]; [Turing, 1937]; [Burks et al., 1982].
