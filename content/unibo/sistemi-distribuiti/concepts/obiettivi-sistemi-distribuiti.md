---
title: "Obiettivi (Goals) dei Sistemi Distribuiti"
course: Sistemi Distribuiti
category: concept
topics: [goals, resource availability, transparency, openness, scalability, situatedness]
difficulty: base
sources: [M4-Definitions-Goals-for-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Obiettivi (Goals) dei Sistemi Distribuiti

## 1. Definizione

Un sistema distribuito *è facile da costruire* (HW, SW e rete si trovano e si assemblano facilmente) — ma a prima vista la distribuzione **introduce problemi anziché risolverli** (vedi [[pitfalls-sistemi-distribuiti|le 8 fallacie]]). Allora **perché** costruirne uno? Perché può raggiungere **obiettivi** che ne valgono lo sforzo. M4 [Tanenbaum & van Steen, 2017] elenca **quattro goal classici + uno** aggiunto da Omicini:

1. **Resource availability** — rendere disponibili risorse (remote) distribuite;
2. **[[trasparenza|Transparency]]** — nascondere la distribuzione quando non è rilevante;
3. **[[openness-sistemi-distribuiti|Openness]]** — promuovere l'apertura;
4. **[[scalabilita|Scalability]]** — promuovere la scalabilità;
5. **[[situatedness|Situatedness]]** — promuovere l'immersione nell'ambiente *(non un goal classico)*.

> ⚠️ Nota dalla conclusione del modulo: oggigiorno gli obiettivi riguardano sempre più anche **robustezza, reliability, fault tolerance** ([[dependability]]) — non solo i quattro classici.

## 2. Resource availability (1° goal)

Le risorse sono **fisicamente distribuite**: un buon motivo per costruire un SD è renderle disponibili *come se appartenessero a un unico sistema*.

> **Cos'è una risorsa?** Qualunque cosa che (i) possa in qualche modo essere connessa a un sistema computazionale, e (ii) chiunque possa legittimamente usare. Es. stampanti, scanner, storage, sensori distribuiti.

Rendendo possibile l'**interazione utenti↔risorse**, i SD sono **abilitatori** di sharing, scambio di informazione, collaborazione (es. **grid systems** [Foster et al., 2001]). → Si lega alle [[motivazioni-sistemi-distribuiti|motivazioni]] di M0 (resource sharing).

## 3. Gli altri quattro goal (in breve)

| Goal | Idea | Pagina |
|------|------|--------|
| **Transparency** | nascondere proprietà non rilevanti → maggiore astrazione per l'utente | [[trasparenza]] |
| **Openness** | lavorare con componenti non fissati a design time; standard, IDL | [[openness-sistemi-distribuiti]] |
| **Scalability** | reggere la crescita in dimensione, geografia, domini amministrativi | [[scalabilita]] |
| **Situatedness** | percepire/agire sull'ambiente; context-awareness (spazio/tempo) | [[situatedness]] |

## 4. Le due sfide trasversali

Per realizzare questi goal, molte entità **autonome ed eterogenee** devono:
- **collaboration** — *collaborare* per ottenere **coerenza** (lavorare come un unico sistema coerente);
- **amalgamation** — *amalgamarsi* per ottenere **uniformità** (apparire come un unico sistema uniforme).

> *Come si ottiene in generale? Esiste una soluzione generale?* → La risposta principe è il **[[middleware]]**: per *separazione*, abilita l'interazione significativa e nasconde le differenze, offrendo un'interfaccia comune.

## 5. Connessioni

- [[sistema-distribuito]] — le definizioni di cui questi sono gli obiettivi di progetto.
- [[trasparenza]], [[openness-sistemi-distribuiti]], [[scalabilita]], [[situatedness]] — i singoli goal.
- [[middleware]] — il mezzo per collaboration & amalgamation.
- [[pitfalls-sistemi-distribuiti]] — perché i goal non sono banali da raggiungere.
- [[motivazioni-sistemi-distribuiti]] (M0) — le motivazioni "a monte" (perché servono i SD).
- [[dependability]] (M1) — robustezza/reliability/fault tolerance come goal ormai centrali.

## 6. Sorgenti

- `raw-sources/M4-Definitions-Goals-for-Distributed-Systems.pdf` (Goals — overview & Resource Availability), slide 19–21, 65.
- Riferimenti: Tanenbaum & van Steen 2017; Foster et al. 2001 (grid).
