---
title: "Weak vs Strong Mobility"
course: Sistemi Distribuiti
category: bridge
topics: [code mobility, weak mobility, strong mobility, execution segment, middleware]
difficulty: intermedio
sources: [C6-Code-Mobility.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Weak vs Strong Mobility

La classificazione **centrale** della [[code-mobility|mobilità del codice]], basata su **quale segmento del processo** si muove insieme al codice [Fuggetta et al., 1998].

## 1. Confronto

| Caratteristica | **Weak mobility** | **Strong mobility** |
|---|---|---|
| Segmenti trasferiti | solo **code segment** (+ init data) | **code + execution segment** |
| Stato di esecuzione | **perso**: il codice esegue *ex novo* | **preservato**: stack, program counter, dati privati |
| Comportamento | si esegue ogni volta da capo; conta solo il contesto **target** | il processo si può **stop → move → restart** altrove |
| Requisiti | il target deve solo **poter eseguire** il codice | l'ambiente tecnologico (**middleware**) deve **supportarlo** |
| Complessità | **molto semplice**, nessuna restrizione particolare | **molto esigente** |
| Esempi | Java Applet, chunk JavaScript | migrazione di processo / mobile agent con stato |

## 2. Il nodo concettuale

> **Weak:** non ci interessa il contesto computazionale di partenza — il codice riparte da zero, e l'unico requisito è che la macchina target sappia eseguirlo. Semplice e portabile.
>
> **Strong:** vogliamo trasportare lo **stato di esecuzione** vivo (un processo "congelato" e ripreso altrove). Potente ma costoso, e raramente supportato di default → serve il [[middleware]].

## 3. Dimensioni ortogonali

La distinzione weak/strong si combina con altre scelte (vedi [[code-mobility]]):
- **sender- vs receiver-initiated** (chi avvia: mobile agent vs Applet/JS);
- **separate vs target process execution** (dove gira il codice mobile: sandbox vs processo target);
- **cloning vs migrating** (la strong mobility via clone = copia parallela ≈ [[replicazione]]);
- e, **ortogonale a tutto**, il problema del **[[migrazione-risorse|resource segment]]** (le risorse non si muovono come il codice).

## 4. Connessioni

- [[code-mobility]] — il quadro generale e le altre dimensioni.
- [[migrazione-risorse]] — il terzo segmento (risorse), problema indipendente.
- [[middleware]] — abilitatore della strong mobility.
- [[replicazione]] · [[trasparenza]] — il cloning come replica trasparente.
- [[scalabilita]] (M4) — la weak mobility (code shipping) per nascondere la latenza.

## 5. Sorgenti

- `raw-sources/C6-Code-Mobility.pdf` (Weak Mobility; Strong Mobility), slide 10–11.
- Riferimento: Fuggetta et al. 1998.
