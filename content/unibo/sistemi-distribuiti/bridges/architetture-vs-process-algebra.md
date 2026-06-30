---
title: "Architetture Software vs Process Algebra"
course: Sistemi Distribuiti
category: bridge
topics: [modellazione, architettura software, process-algebra, struttura, comportamento, formalismi]
difficulty: avanzato
sources: [M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf, M9-Modelling-Distributed-Systems-Process-Algebra.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Architetture Software vs Process Algebra

I **due modi di modellare** un sistema distribuito visti nel corso. M8 ([[software-architecture]]) fornisce la lente **strutturale**; M9 ([[process-algebra]]) quella **comportamentale e formale**. Questo bridge **chiude la domanda aperta** lasciata da M8: *gli stili architetturali bastano? si possono dimostrare teoremi?* — la risposta è la process algebra.

## 1. Confronto

| Aspetto | **Architetture software/di sistema** (M8) | **Process algebra** (M9) |
|---|---|---|
| Cosa modella | **struttura & organizzazione** dei componenti | **comportamento** dei processi |
| Natura | informale, **approssimativa**, pattern (stili) | **formale**, algebrico/assiomatico |
| Proprietà | comprese **qualitativamente** | **dimostrate** (provate) |
| Astrae da | (parzialmente) i dettagli implementativi | dettagli non necessari **+ collocazione fisica/distribuzione** |
| Output | configurazione di componenti/connettori/dati | termini dell'algebra → **LTS** (transition systems) |
| Usi | design, classificazione, confronto di sistemi | **specifica** + **verifica** di proprietà |
| Limite | non prova il comportamento | tratta solo **concorrenza**, non la distribuzione fisica |

## 2. Il nodo concettuale

> Le architetture software/di sistema modellano **struttura e organizzazione**, **non il comportamento**: le proprietà comportamentali si capiscono **qualitativamente**, non si **dimostrano**. Servono **modelli formali**: la process algebra cattura l'essenza dei sistemi **concorrenti** in modo **non ambiguo** e consente di **calcolare proprietà**.

Le due viste sono **complementari, non alternative**:
- l'architettura risponde a *com'è fatto e organizzato* il sistema (e *dove* sta — [[software-vs-system-architecture]]);
- la process algebra risponde a *come si comporta* e *quali proprietà* valgono, in modo provabile.

> 🔗 Entrambe **astraggono dalla distribuzione spaziale** ([[computing-with-space]], M7): l'architettura software lo fa informalmente, la process algebra **formalmente**. Nessuna delle due, da sola, copre la distribuzione fisica — il corso si ferma qui dichiaratamente.

## 3. La progressione del modelling (M8 → M9)

```
M2  ontologia: processi che behave + interact  ([[sistema-computazionale]])
        │
M8  STRUTTURA: software/system architecture, componenti & connettori, stili
        │   (modella organizzazione; proprietà solo qualitative)
        ▼
M9  COMPORTAMENTO: process algebra (operatori, leggi, transizioni)
            → proprietà DIMOSTRATE, specifica + verifica
```

Risolve anche il dubbio fondazionale di [[computer-science-foundations]] (M2): i risultati di impossibilità sono **teoremi**, e per provarli serve una rappresentazione formale — la process algebra è (parte del) rigore mancante.

## 4. Connessioni

- [[software-architecture]] (M8) · [[architectural-styles]] · [[componenti-connettori]] — la vista strutturale.
- [[process-algebra]] (M9) · [[algebra-processi-operatori-leggi]] — la vista comportamentale formale.
- [[software-vs-system-architecture]] (M8) — l'altra distinzione del modelling (logico/tempo vs spaziale): qui la coppia è **struttura vs comportamento**.
- [[computer-science-foundations]] (M2) — il bisogno di prove formali che la process algebra inizia a soddisfare.
- [[parallelo-concorrente-distribuito]] (M2) — la concorrenza, oggetto della process algebra.

## 5. Sorgenti

- `raw-sources/M8-Modelling-Distributed-Systems-Software-System-Architectures.pdf` (Prologue; Conclusion).
- `raw-sources/M9-Modelling-Distributed-Systems-Process-Algebra.pdf` (Prologue, slide 4–6; Conclusion, slide 27–28).
