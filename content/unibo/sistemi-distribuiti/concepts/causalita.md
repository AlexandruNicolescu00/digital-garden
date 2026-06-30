---
title: "Causalità, Dipendenze e Tempo"
course: Sistemi Distribuiti
category: concept
topics: [causalità, dipendenze, tempo, interazione]
difficulty: intermedio
sources: [C2-Logging-Checkpointing.pdf, C5-Logical-Clocks.pdf]
created: 2026-06-25
updated: 2026-06-26
---

# Causalità, Dipendenze e Tempo

## Dal problema: i componenti non lavorano in isolamento

I componenti di un sistema svolgono ruoli diversi (funzioni, servizi, task, goal) ma **non operano in isolamento**: **interagiscono** e **dipendono** l'uno dall'altro per il completamento delle proprie operazioni. Le **inter-dipendenze** attraversano contesti di processo diversi (eco di [[parallelo-concorrente-distribuito]]):
- **nel tempo** nei sistemi **concorrenti**;
- **nello spazio** nei sistemi **distribuiti**.

> Domanda: come si **modellano** le dipendenze? Risposta (ingannevolmente semplice): con la **causalità**.

## Causalità: dal senso comune alla scienza

- **Causa come strumento cognitivo**: il legame "causa-effetto" è uno strumento umano per capire la dinamica della realtà, per **spiegare e predire** (eco di [[computer-science-foundations]]).
- **Causa nella scienza**: non è banale. Nei paper di fisica dell'800 la parola "causa" compare spesso nei *titoli*, quasi mai nel *corpo*.
- **Correlazione ≠ causazione**: l'associazione di effetti può essere un *confounding factor* (es. ipotetico "genotipo cancerogeno" che causa sia tumore sia predisposizione a fumare → la correlazione fumo/tumore non basta a provare la causa; argomento usato dall'industria del tabacco).
- Una nozione di causa **scientificamente valida** esiste, ma serve una definizione matematica precisa: in [Pearl, 2009] la correlazione è un concetto **statistico**, la causazione è definita in termini **probabilistici**.

## La causalità che useremo nel corso

Per i sistemi distribuiti **non serve** l'intero apparato matematico/filosofico. Si usa la causa legata all'astrazione di **system model**, per:
- definire **dipendenze tra componenti** (tipicamente processi sequenziali);
- **approssimare una nozione utile di tempo (distribuito)**.

Idea chiave: **un legame causale determina una relazione temporale** tra due eventi — secondo l'intuizione, **la causa precede temporalmente i suoi effetti**. Nozione semplificata, ma utile nel nostro contesto.

> 🔗 È il ponte verso la relazione **"happened-before"** di Lamport e i clock logici: la causalità è ciò che permette di ricostruire un ordine parziale utile senza clock globale.

## Formalizzazione (C5): da causalità a tempo logico

C5 chiude questo ponte. La causalità tra eventi diventa la relazione **[[happened-before|happens-before]]** `→` (causa-effetto **interna** al sistema), distinta dalla **causalità esterna** (nel mondo fisico, non rilevabile dal sistema — es. "Alice lo dice a Bob" — solo approssimabile col tempo fisico). I **[[logical-clock|clock logici]]** assegnano tempo coerente con `→`: gli [[lamport-scalar-clock|scalar clock]] danno clock-consistency, i [[vector-clock|vector clock]] la **strong consistency** (`vc(a)<vc(b) ⟺ a→b`), codificando le **causal histories** e permettendo di **rilevare la concorrenza**.

## Connessioni

- [[ordinamento-parziale-eventi]] — la causalità fonda l'ordine parziale degli eventi
- [[parallelo-concorrente-distribuito]] — dipendenze nel tempo (concorrente) vs nello spazio (distribuito)
- [[global-state-consistency]] — le dipendenze causali non vanno perse nello stato globale
- [[checkpointing-e-logging]] — recuperare senza perdere le dipendenze (causal logging)
- [[causal-consistency]] — modello di consistenza (M3) che ordina le write proprio secondo la relazione causa/effetto
- [[happened-before]] · [[logical-clock]] · [[lamport-scalar-clock]] · [[vector-clock]] — **(C5)** la formalizzazione e gli algoritmi (gancio risolto)

## Sorgenti

- `raw-sources/C2-Logging-Checkpointing.pdf` (slides 4–12)
- `raw-sources/C5-Logical-Clocks.pdf` (Actions, Events, Causes; internal vs external causality, slide 4–6)
- [Pearl, 2009] *Causality: Models, Reasoning, and Inference*.
