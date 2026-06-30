---
title: "Computing Needs Time (CPS & la Natura del Tempo)"
course: Sistemi Distribuiti
category: concept
topics: [tempo, cyber-physical systems, CPS, Lee, relatività, semantica]
difficulty: intermedio
sources: [M6-Computing-with-Time.pdf]
created: 2026-06-26
updated: 2026-06-26
---

# Computing Needs Time (CPS & la Natura del Tempo)

## 1. Il problema: la computazione ha bisogno del tempo

La tesi di fondo [Lee, 2009] è che il **tempo** è trattato male dalle astrazioni computazionali. Due spinte convergenti:
- i dispositivi computazionali sono sempre più **special-purpose** immersi nel mondo (auto, dispositivi medici, robot industriali, IoT [Atzori et al., 2010; Gubbi et al., 2013]) — sempre più potenti, ma con dependability potenzialmente a rischio;
- i computer **general-purpose** devono sempre più **interagire con processi fisici** (media, sensing, controllo di dispositivi, sistemi pervasivi).

### Cyber-Physical Systems (CPS)
> I **Cyber-Physical Systems** emergono dall'integrazione di sistemi/processi **fisici** con il **computing in rete**: incorporano profondamente computazione e comunicazione che interagiscono con processi fisici per dare nuove capacità ai sistemi fisici. *Fondere computing + networking + sistemi fisici per creare nuova scienza, tecniche e prodotti.*

Esempi: dispositivi medici ad alta affidabilità, controllo del traffico, automotive avanzato, process control, avionica, controllo di infrastrutture critiche (energia, acqua, comunicazioni), robotica distribuita (telepresenza, telemedicina), smart structures.

> ⚠️ Le **fondamenta del computing** (Turing [1937], Church, von Neumann [Burks et al., 1982]) riguardano la **trasformazione di dati**, **non** la dinamica dei processi fisici. Per questo *"il passaggio del tempo è essenzialmente assente nel computing"* — un problema proprio per i CPS. → critica diretta alle [[computer-science-foundations|fondamenta della CS]] e alla nozione di [[computazione]].

## 2. Le quattro misconcezioni sul tempo [Lee, 2009]

| Misconcezione | Perché è fuorviante |
|---------------|---------------------|
| *"computing takes time"* | non è solo che l'efficienza ha limiti: il tempo è **sempre astratto via** dal computing, che quindi lo **omette** |
| *"time is a resource"* | sì, ma di tipo diverso: praticamente **illimitato** e comunque speso; dovrebbe essere una **proprietà semantica**, non una risorsa generica |
| *"time is a nonfunctional property"* | con la Turing Machine la funzione si calcola astraendo dal tempo; ma in un CPS la **funzione** è definita tramite **azioni** che occorrono nel **tempo fisico**, con una **durata** |
| *"real time è un problema di QoS"* | nei CPS il tempo non è (solo) efficienza: è **predicibilità e ripetibilità**; precisione/variabilità sono QoS, ma il tempo **stesso** dovrebbe far parte della **semantica** dei programmi |

> 🎯 **Conclusione di Lee:** le astrazioni computazionali di base andrebbero **ripensate per includere il tempo**.

## 3. Quale nozione di tempo? Il tempo dopo la relatività

Cos'è il tempo? (Britannica: *un periodo misurato o misurabile, un continuum privo di dimensioni spaziali*). Ma dopo la relatività [Einstein, 1920; Rovelli, 2017]:

> - **non** esiste più un tempo **vero**, né un tempo **unico**;
> - la fisica non descrive come le cose evolvono *nel* tempo, ma **come evolvono nei propri tempi** e **come i tempi evolvono l'uno rispetto all'altro**;
> - non c'è più una **direzione**, né un **presente** ("now"); il tempo **non è indipendente**;
> - il tempo è solo **relazione tra cambiamenti**: gli eventi accadono, e il tempo serve a **relazionarne le dinamiche**.

> 🔗 Questa è la giustificazione filosofica del **tempo logico**: se non esiste un tempo unico/assoluto neppure in fisica, a maggior ragione in un sistema distribuito conviene una nozione di tempo **relazionale** ancorata agli eventi e alla [[causalita|causalità]] (→ [[logical-clock]]). È anche l'eco dell'unità spazio-temporale persa in [[distribuzione-spaziale-temporale]].

## 4. Connessioni

- [[tempo-nei-sistemi-distribuiti]] — il problema del tempo specificamente nei SD (fisico vs logico).
- [[computer-science-foundations]] · [[computazione]] (M2) — le fondamenta che, secondo Lee, omettono il tempo.
- [[distributed-pervasive-systems]] (M5) · [[situatedness]] (M4) — i CPS come sistemi situati che sentono/controllano il fisico.
- [[logical-clock]] (C5) — il tempo relazionale come risposta all'assenza di tempo assoluto.
- [[dependability]] (M1) — la complessità crescente dei CPS a scapito dell'affidabilità.
- [[computing-with-space]] (M7) — il modulo gemello sullo **spazio**; confronto in [[tempo-vs-spazio-nel-computing]].

## 5. Sorgenti

- `raw-sources/M6-Computing-with-Time.pdf` (Prologue; Time & Computation), slide 4–12.
- Riferimenti: Lee 2009 (*Computing needs time*); Einstein 1920; Rovelli 2017 (*L'ordine del tempo*); Turing 1937; Burks et al. 1982; Atzori et al. 2010; Gubbi et al. 2013.
