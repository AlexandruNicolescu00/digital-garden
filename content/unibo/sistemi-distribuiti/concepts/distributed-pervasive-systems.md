---
title: "Distributed Pervasive Systems"
course: Sistemi Distribuiti
category: concept
topics: [pervasive, mobile, instabilità, sensor network, BAN, situatedness]
difficulty: intermedio
sources: [M5-Sorts-Distributed-Systems.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Distributed Pervasive Systems

## 1. Definizione

La terza delle [[sorts-sistemi-distribuiti|tre classi]], in cui **l'instabilità è la condizione di default**: dispositivi mobili con batterie e connessione di rete **sporadica**.

> **Caratteristiche principali:**
> - un sistema pervasivo distribuito è **parte di ciò che ci circonda** (immerso nell'ambiente);
> - generalmente **manca di controllo amministrativo umano**.

→ è la realizzazione "di sistema" della **[[situatedness]]** (M4) e della **[[pervasivita-computazione-interazione|pervasività]]** (M0).

## 2. I tre requisiti [Grimm et al., 2004]

| Requisito | Significato |
|-----------|-------------|
| **Embrace contextual changes** | un dispositivo deve essere **continuamente consapevole** che il suo ambiente può cambiare in qualsiasi momento |
| **Encourage ad hoc composition** | molti dispositivi saranno usati in **modi diversi da utenti diversi** → composizione spontanea |
| **Recognise sharing as the default** | i dispositivi entrano nel sistema per **accedere/fornire informazione** → l'informazione dev'essere facile da leggere, memorizzare, gestire e condividere |

> 🔗 "Embrace contextual changes" è precisamente la **context-awareness** della [[situatedness]]; "sharing as default" eco della *resource availability* ([[obiettivi-sistemi-distribuiti]]).

## 3. Tre esempi

### Home systems
Costruiti attorno a reti domestiche: **non** si può chiedere alle persone di fare da amministratori di rete competenti → devono essere **self-configuring e self-maintaining**. Gestiscono enormi quantità di **informazione personale eterogenea**, da fonti dentro e fuori casa.

### Health care systems
Sistemi personali costruiti attorno a una **Body Area Network (BAN)**, minimizzando l'**impatto sulla persona** (es. non impedire il libero movimento). Domande aperte: dove/come memorizzare i dati monitorati? come prevenire la perdita di dati cruciali? quale infrastruttura per generare/propagare **alert**? come dare feedback online dai medici? come ottenere **robustezza estrema**? quali **security policy**?

### Sensor networks
Tecnologia **abilitante** dei sistemi pervasivi: nuvole di sensori **spazialmente distribuiti** (da decine a migliaia di nodi) che **acquisiscono, elaborano e trasmettono** informazione ambientale.

> **Vista come database distribuiti:** sorgenti distribuite di informazione interrogabili nel tempo. **Due estremi** (entrambi cattivi):
> 1. i sensori **inviano** solo i dati senza cooperare → troppo **consumo di rete**;
> 2. i sensori fanno **tutta** la computazione e restituiscono i risultati → troppo **consumo di energia** dei nodi.

> **Soluzione — in-network data-processing:** costruire un **albero** tra i sensori, far passare le query attraverso l'albero, **aggregare** i risultati ai vari livelli. Domande: come costruire (dinamicamente) un albero efficiente? come controllare l'aggregazione? cosa succede quando i link falliscono?

## 4. Connessioni

- [[sorts-sistemi-distribuiti]] — la classe di cui questa è la terza.
- [[situatedness]] (M4) — embrace contextual changes = context-awareness; immersione nell'ambiente.
- [[pervasivita-computazione-interazione]] (M0) — la pervasività come contesto motivante.
- [[obiettivi-sistemi-distribuiti]] (M4) — sharing as default ↔ resource availability; mancanza di admin ↔ self-configuration.
- [[scalabilita]] (M4) — il trade-off rete vs energia nei sensori; in-network processing come distribution.
- [[modelli-di-fallimento]] · [[dependability]] (M1) — robustezza, link failure, perdita dati.
- [[smart-contract]] (C4) — combinazione SC+IoT per il tracciamento (caso d'uso pervasivo).
- [[computing-needs-time]] (M6) — i pervasive systems come **Cyber-Physical Systems**: sensing/controllo del fisico, il tempo come proprietà semantica.
- [[physical-clock-synchronization]] (M6) — RBS per la sincronizzazione nelle reti di sensori wireless.
- [[physical-space-computational-systems]] · [[spatial-computing]] (M7) — i pervasive systems come *situated systems* (embedded in space); l'IoT rende la computazione situata ineludibile.

## 5. Sorgenti

- `raw-sources/M5-Sorts-Distributed-Systems.pdf` (Distributed Pervasive Systems), slide 30–38.
- Riferimenti: Grimm et al. 2004; Tanenbaum & van Steen 2017.
