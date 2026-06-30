---
title: "Fondamenti: Scienza e Computer Science"
course: Sistemi Distribuiti
category: concept
topics: [fondamenti, scienza, computer-science, epistemologia]
difficulty: base
sources: [M2-Roots-of-Distributed-Systems-Computation-in-Space-Time.pdf]
created: 2026-06-25
updated: 2026-06-25
---

# Fondamenti: Scienza e Computer Science

## Perché partire da qui

Motivazione del modulo: i **risultati di impossibilità sono teoremi** (es. il [[teorema-cap]]). Sono essenziali perché *definiscono i campi*, delimitando lo spazio delle "mosse" ammissibili. Ma essendo teoremi, richiedono un'**ontologia scientifica ben definita**, una notazione condivisa e non ambigua, una nozione comune di *dimostrazione*.

Problema: la computer science **non ha** ancora questo, a differenza della matematica. Il teorema di Brewer fu enunciato senza prova; la prova arrivò dopo con limitazioni [Gilbert and Lynch, 2002]; tra le "centinaia di risultati di impossibilità" [Fich and Ruppert, 2003] si trovano notazioni eterogenee, ontologie diverse, nozioni di prova differenti.

## Cosa fa la scienza

Definizione (Science Council): *la scienza è la ricerca e applicazione di conoscenza del mondo naturale e sociale seguendo una metodologia sistematica basata sull'evidenza*.

Due funzioni fondamentali:

1. **Spiegazione (explanation)** — osserviamo **fenomeni** (fatti), e cerchiamo i **noumeni** (i modelli, le ragioni) che li spiegano.
   - **Fenomeno** = ciò che è percepito/osservato (oggetto dei sensi).
   - **Noumeno** (Kant) = la *cosa-in-sé* (*das Ding an sich*), opposta al fenomeno (la cosa come appare).
   - Più modelli possono spiegare gli stessi fatti, e non sono equivalenti: si preferiscono modelli con **meno assunzioni** e **più fatti spiegati** (più *espressivi*).
2. **Predizione (prediction)** — un modello scientifico funzionante non solo spiega, ma **predice** fenomeni non ancora osservati (es. redshift gravitazionale, bosone di Higgs predetto nel 1964 e osservato nel 2012). Il **potere predittivo** è parte essenziale di ciò che rende scientifica una teoria.

## Cos'è la computer science

*"Phenomena breed sciences"* [Newell et al., 1967]: ci sono i computer, ergo la CS è lo studio dei computer. Ma:
- il termine "computer" non è ben definito e cambia nel tempo;
- una scienza può avere un **oggetto di studio mobile** (anche la matematica era "scienza della quantità").

Risoluzione: un computer è una macchina che **computa**; i sistemi computazionali producono i fenomeni studiati dalla CS; quindi la **computazione** è l'oggetto di studio centrale, e l'*essenza della computazione* è il **noumeno** al cuore della CS (→ [[computazione]]).

Definizioni di CS nel tempo [Denning, 2008]: calcolo automatico (1940s) → information processing (1950s) → fenomeni attorno ai computer (1960s) → ciò che può essere automatizzato (1970s) → **computazione** (1980s) → processi informativi naturali e artificiali (2000s).

## Connessioni

- [[computazione]] — il noumeno centrale della CS
- [[teorema-cap]] — l'esempio di impossibility result che motiva il bisogno di rigore
- [[sistema-distribuito]] — l'oggetto finale, costruito su queste fondamenta
- [[computing-needs-time]] — **(M6)** la critica di Lee: le fondamenta (Turing/Church/von Neumann) trattano la trasformazione di dati ma **omettono il tempo** — un limite per i sistemi cyber-fisici
- [[software-architecture]] — **(M8)** la domanda aperta: gli stili architetturali bastano per una *scienza* dei SD? quale ruolo per i formalismi (process algebra)?
- [[process-algebra]] — **(M9)** il formalismo che inizia a fornire il **rigore** richiesto: rappresentazione non ambigua del comportamento e possibilità di **dimostrare** proprietà (i teoremi che le fondamenta richiedono).

## Sorgenti

- `raw-sources/M2-Roots-of-Distributed-Systems-...pdf` (slides 4–19, 63)
- [Newell et al., 1967]; [Denning, 2008/2010/2011]; [Fich and Ruppert, 2003].
