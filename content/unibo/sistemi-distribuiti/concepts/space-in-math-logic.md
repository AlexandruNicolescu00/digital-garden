---
title: "Lo Spazio in Matematica e Logica"
course: Sistemi Distribuiti
category: concept
topics: [geometria, euclidea, non-euclidea, Tarski, logiche modali, topologia, S4]
difficulty: avanzato
sources: [M7-Computing-with-Space.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Lo Spazio in Matematica e Logica

Come l'umanità ha imparato a **rappresentare, analizzare e ragionare** sullo spazio in modo formale — i fondamenti che rendono lo spazio **computabile** ([[computing-with-space]]).

## 1. Geometria

**Modellare lo spazio** astraendo dalla percezione della realtà: concetti geometrici di base (punto, retta, angolo, cerchio) e forme (triangolo, rettangolo…), dai Babilonesi (astronomia) agli Egizi ai Greci (geometria).

### Geometria euclidea (approccio assiomatico)
Gli *Elementi* di Euclide [Kline, 1972]: la geometria **non** più vista come "insieme di osservazioni empiriche e metodi pratici per misurare distanze/aree", ma come *"teoria matematica astratta che, pur radicata nella realtà percepita, ha un proprio diritto di esistenza e sviluppo"* [Aiello et al., 2012] → rappresentare lo spazio e ragionarci tramite **assiomi, teoremi, dimostrazioni**.

### Geometria non-euclidea
Lo spazio fisico **potrebbe non soddisfare** il **quinto postulato** di Euclide → geometrie non-euclidee: **Riemann** (ellittica), **Bolyai & Lobachevsky** (iperbolica). Rappresentano il vero spazio fisico **oltre** la percezione sensoriale diretta.
> *"Una manifestazione insuperata della superiorità dell'approccio astratto e logico della matematica su quello empirico delle scienze naturali, almeno per concetti fondamentali come spazio e tempo"* [Aiello et al., 2012].

## 2. Logica: da Euclide a Tarski

| Tappa | Contenuto |
|-------|-----------|
| **Fondazione assiomatica** | da Euclide a Peano; fondazione assiomatica *sound* della geometria di **Hilbert** [1950] |
| **Metodo analitico** | **coordinatizzazione** di Descartes (problemi geometrici via metodi algebrici); geometria come studio delle **trasformazioni** (programma di Erlangen di **Klein**); fondazione algebrica astratta |
| **Fondazione logica** | **Tarski [1959]**: geometria elementare come **teoria del prim'ordine** |

### Il risultato di Tarski
- la geometria elementare si sviluppa assiomaticamente con **due sole relazioni**: **betweenness** (essere-tra) ed **equidistance** (equidistanza);
- è come il **campo dei reali** via coordinatizzazione;
- **completezza e decidibilità** della teoria del prim'ordine dei reali ⟹ **procedura di decisione esplicita**: esiste un **algoritmo** che decide la verità di qualunque enunciato della geometria euclidea... **ma non efficiente**.

## 3. Logiche modali dello spazio

Le **logiche non-classiche** (modali) sono particolarmente interessanti per lo spazio: più **specifiche** e con **miglior comportamento computazionale** (spesso **decidibili**).

### Logiche modali della topologia
- problema base: la **distanza** → nozione di **metrica**;
- l'**intorno (neighbourhood)** generalizza la metrica → la **topologia** [Singer & Thorpe, 1967] (*"accanto ad algebra e geometria, una delle branche fondamentali della matematica contemporanea"*);
- **operatori modali reinterpretati**: □ (necessariamente) = operatore di **interno**, ◇ (possibilmente) = operatore di **chiusura** di uno spazio topologico;
- **S4** [Bull & Segerberg, 1984] è **sound & complete** rispetto alla semantica topologica [McKinsey & Tarski, 1944]: **S4 è la logica modale di qualunque spazio euclideo**.

### Morfologia matematica (MM)
Analizza **forma**, informazione spaziale, image processing; *modal morphologic* dalla somiglianza tra le proprietà algebriche degli operatori MM e degli operatori modali; **efficiente** per il ragionamento spaziale (esplorazione, focus of attention, riconoscimento/interpretazione).

> **Sintesi:** abbiamo strumenti matematici e logici per rappresentare, analizzare e ragionare sullo spazio, e possiamo **computare** sullo spazio e la sua organizzazione **con diversi livelli di efficienza**.

## 4. Connessioni

- [[computing-with-space]] — l'hub: perché lo spazio va rappresentato.
- [[spatial-computing]] — questi strumenti formali abilitano il computing spaziale.
- [[computer-science-foundations]] (M2) — l'approccio assiomatico/logico come modo di "spiegare e predire".
- [[spatial-computing-applications]] — geometria computazionale e GIS poggiano su questi fondamenti.

## 5. Sorgenti

- `raw-sources/M7-Computing-with-Space.pdf` (Space in Math & Logic — Geometry, Logics), slide 11–22.
- Riferimenti: Aiello et al. 2012; Kline 1972; Hilbert 1950; Tarski 1959; Balbiani et al. 2007; Bull & Segerberg 1984; McKinsey & Tarski 1944; Singer & Thorpe 1967.
