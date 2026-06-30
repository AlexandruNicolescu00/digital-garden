---
title: "Vector Clock"
course: Sistemi Distribuiti
category: theorem
topics: [vector clock, strong consistency, causalità, tempo logico, matrix clock]
difficulty: avanzato
sources: [C5-Logical-Clocks.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Vector Clock

## 1. Il problema che risolve

Gli [[lamport-scalar-clock|scalar clock]] garantiscono `a → b ⟹ C(a) < C(b)`, **ma non il viceversa**: `C(a) < C(b)` **non** implica `a → b`. Quindi:
- i time value possono essere **totalmente ordinati anche quando gli eventi non lo sono**;
- quando gli eventi sono **non correlati**, confrontarne i tempi è **privo di significato**;
- gli scalar clock **non sanno dire** se `a` e `b` sono concorrenti.

> Serve qualcosa in più — la codifica delle [[happened-before|causal histories]] — per stabilire se `a` e `b` sono **(non) correlati / concorrenti**.

## 2. Definizione

> Un **vector clock** di un sistema di `n` processi è un **array/vettore di `n` logical clock**, uno per processo.
> - ⚠️ **Ipotesi:** il numero `n` di processi è **noto** (si possono usare vettori più piccoli, ma le proprietà sono dimostrate con `n`).

Ogni processo `pᵢ` mantiene un vettore `vcᵢ` tale che:
- `vcᵢ[i]` = numero di eventi occorsi finora in `pᵢ` (**il proprio scalar clock locale**); ogni nuovo evento in `pᵢ` incrementa `vcᵢ[i]`;
- `vcᵢ[j] = k` significa che `pᵢ` **sa che** (almeno) `k` eventi sono occorsi in `pⱼ` (lo scalar clock di `pⱼ` secondo la **migliore conoscenza** di `pᵢ`).

Ogni messaggio di `pᵢ` è **piggybacked** col vettore corrente `vcᵢ` come timestamp.

## 3. Regole

1. **Prima di ogni azione** in `pᵢ`:
   ```
   vcᵢ[i] ← vcᵢ[i] + 1
   ```
2. **All'invio** di `m` da `pᵢ` a `pⱼ`: il messaggio è timestampato col vettore
   ```
   ts(m) ← vcᵢ
   ```
3. **Alla ricezione** di `m` (da `pᵢ`, timestamp `vcᵢ`), `pⱼ` aggiusta componente per componente:
   ```
   ∀k :  vcⱼ[k] ← max(vcⱼ[k], ts(m)[k])
   ```

## 4. Relazione d'ordine e strong consistency

Etichettando ogni evento `m` con `vc(m)`, si definisce:

> **`vc(m₁) < vc(m₂)  ⟺  ( ∀i : vc(m₁)ᵢ ≤ vc(m₂)ᵢ )  ∧  ( ∃i : vc(m₁)ᵢ < vc(m₂)ᵢ )`**

Questa relazione è **antisimmetrica** e **transitiva**, cattura i semplici logical clock ma soprattutto **cattura la relazione causale**:

> **`vc(m₁) < vc(m₂)  ⟺  m₁ → m₂`**   → i vector clock forniscono **strong consistency**.

⚠️ La relazione d'ordine su `vc` **non è di per sé totale**: due eventi **concorrenti** danno vettori **non confrontabili** (né `<` né `>`) → è proprio così che si **rileva la concorrenza** `m₁ ∥ m₂`.

## 5. Risultato

- ogni processo sa **quanti eventi hanno preceduto** l'invio del messaggio ricevuto presso il mittente → la "**chain of events**" è preservata e condivisa tra i processi;
- ogni `ts(m)[i]` si riferisce agli eventi che precedono causalmente `m` **dentro** `pᵢ`;
- `ts(m)` tiene traccia di **tutti** gli eventi che possono aver causalmente preceduto l'invio di `m`, da cui `m` può **causalmente dipendere**.

→ è la base per implementare la [[causal-consistency]] (M3) e il **causal logging/delivery**.

## 6. Oltre: matrix clock

Anche nei vector clock avviene ancora un po' di *"squashing"*: i **matrix clock** estendono ulteriormente il tempo logico (ogni processo tiene una matrice `n × n`: cosa `pᵢ` sa di ciò che `pⱼ` sa di `pₖ`) — ma il modulo si ferma qui.

## 7. Connessioni

- [[lamport-scalar-clock]] — ciò che i vector clock estendono (da clock a strong consistency).
- [[logical-clock]] — il framework (qui la condizione `⟺`).
- [[happened-before]] — la causalità che il vettore ricostruisce esattamente.
- [[causal-consistency]] (M3) — i vector clock sono il meccanismo che la realizza.
- [[scalar-vs-vector-clock]] — confronto diretto.
- [[causalita]] · [[checkpointing-e-logging]] (C2) — causal logging / dipendenze causali.

## 8. Sorgenti

- `raw-sources/C5-Logical-Clocks.pdf` (Vector Clocks), slide 28–35.
- Riferimenti: Lamport 1978; Kshemkalyani & Singhal 2011; Baquero & Preguiça 2016.
