# 🏥 Case Study: X-rays Computed Tomography (CT)
**Fonte:** intro_tomography_ci2026.pdf — Computational Imaging 2024-25  
**Argomenti coperti:** Storia dei raggi X e della CT, Modello matematico (Lambert-Beer, Sinogramma), Safer Tomography (ALARA), Ricostruzione come problema inverso lineare, Digital Breast Tomosynthesis (DBT).

---

## 📑 Indice
- [[#📜 Storia dell'Imaging a Raggi X e della CT]]
- [[#📐 Il Modello Matematico della CT]]
- [[#🛡️ Safer Tomographic Protocols]]
- [[#🔄 La Ricostruzione CT come Problema Inverso Lineare]]
- [[#🎀 Case Study: Digital Breast Tomosynthesis (DBT)]]
- [[#📊 Riepilogo Formule Chiave]]

---

## 📜 Storia dell'Imaging a Raggi X e della CT

### Le Origini
*   **Raggi X:** Scoperti nel 1895 dallo scienziato tedesco Wilhelm Conrad Röntgen.
*   **Radiografia:** Tecnica di imaging che utilizza i raggi X (o radiazioni ionizzanti simili) per visualizzare la forma interna di un oggetto. La prima immagine "medica" fu la mano della moglie di Röntgen (22 dicembre 1895).
*   **Tomografia:** Imaging per sezioni. La parola deriva dal greco: *tomos* (sezione) e *grapho* (scrivere).
*   **TAC (Tomografia Assiale Computerizzata):** La metodologia circolare alla base è stata proposta dall'ingegnere Godfrey Hounsfield e dal fisico Allan Cormack, vincitori del Premio Nobel per la medicina nel 1979.

### Evoluzione delle Generazioni di CT
| Generazione | Caratteristiche Principali | Tempi di Scansione |
| :--- | :--- | :--- |
| **1ª (1971/1973)** | *Parallel beam*, *pencil beam* (un raggio alla volta). Un solo pixel nel detector. Traslazione sorgente, poi rotazione sorgente-detector (180°). | 4-5 min per scan; 20 min per ricostruzione. |
| **2ª** | Piccolo range di raggi (3-20°). Detector con 3-30 pixel. Traslazione e poi rotazione a step. | 15-30 sec per scan. |
| **3ª (1990s)** | Ampio range angolare (35-50°). Detector con 300-800 pixel. Solo rotazione sorgente-detector. | ~1 sec per scan. |
| **3ª Volumi (Multi-slice)** | Acquisizione su traiettoria a spirale/circonferenza. Rotazione in ~1 sec, corpo intero in ~40 sec. In commercio dal 1998. | ~0.5 sec per scan. |
| **4ª** | Detector fisso (600-1200 pixel) che racchiude il paziente a 360°. Solo la sorgente ruota. | ~1 sec per scan e ricostruzione. |

### Cone Beam CT (CBCT)
La **Cone Beam Computed Tomography** (o C-arm CT, flat panel CT) è una tecnica in cui i raggi X sono divergenti e formano un cono. È diventata fondamentale nella pianificazione dei trattamenti in odontoiatria implantare, otorinolaringoiatria (ENT), ortopedia e radiologia interventistica.

---

## 📐 Il Modello Matematico della CT

### La Legge di Lambert-Beer
Per un singolo materiale, la sorgente emette un raggio a singola energia con intensità $I_0$ che attraversa un volume di larghezza $ds$. Il detector conta $I$ fotoni, dove $dI = I_0 - I$ è il numero di fotoni assorbiti.
$$ I = I_0 e^{-\mu ds} $$
dove $\mu = \mu_\lambda$ è il **coefficiente di attenuazione** del materiale, relativo alla lunghezza d'onda $\lambda$ del raggio X.

### Il Caso Reale
Sia $\mu(x, y)$ il coefficiente di attenuazione dei raggi X nel punto $(x, y)$ dell'oggetto. Si misura l'intensità $I$ del raggio X, emesso con intensità $I_0$, che ha attraversato l'oggetto lungo il segmento $L$ (formante un angolo $\Phi$ rispetto agli assi cartesiani).
$$ -\log\left(\frac{I}{I_0}\right) = \int_L \mu(x, y) d\ell \ge 0 $$

> ✅ **Sistema di Riferimento**
> Per un *parallel beam* e inizialmente $\theta = 0$, si utilizza il sistema di coordinate $\theta = (\cos\Phi, \sin\Phi)$ e $\theta^\perp = (-\sin\Phi, \cos\Phi)$, ponendo $\mu(x, y) = \mu(t, s)$.

### Il Sinogramma
La CT esegue una scansione circolare registrando molti profili, uno da ogni angolo. L'insieme di tutti i profili acquisiti rappresentati nel piano $(t, \theta)$ è chiamato **sinogramma**.
*   **Setting discreto:**
    *   Le proiezioni sono calcolate da un numero finito di angoli $\theta_k \in \{\theta_1, \dots, \theta_{n_{angoli}}\}$ (es. 300-1000 angoli in 180 gradi).
    *   I coefficienti di attenuazione sono misurati solo in un numero finito di punti $t_i$ (risoluzione del detector, es. 600 pixel per un oggetto di $512 \times 512$ voxel).
    *   L'insieme $m(t_i, \theta_k) = P_{\theta_k}(t_i)$ costituisce il sinogramma.

> ⚠️ **Attenzione: Il Rumore nel Sinogramma**
> Il sinogramma acquisito non corrisponde esattamente al modello matematico a causa di effetti fisici (rumore):
> 1.  **Shot noise (Poisson noise):** Fluttuazione casuale del conteggio dei fotoni X dalla sorgente.
> 2.  **White noise (Gaussiano a media zero):** Introdotto dal detector (CCD, sCMOS, FPD) durante la trasformazione da dati analogici a digitali.

---

## 🛡️ Safer Tomographic Protocols

Nella progettazione delle procedure CT, la priorità è ridurre i rischi dovuti alle radiazioni ionizzanti secondo il **principio ALARA** (*As Low As Reasonably Achievable*). Questo principio impone di evitare l'esposizione a radiazioni che non hanno un beneficio diretto, anche se la dose è piccola.

### Applicazioni della Safer CT
*   Pediatric CT
*   Mobile stroke unit
*   Dental CT
*   Breast Tomosynthesis

### Strategie di Riduzione della Dose
Il principio ALARA si realizza in due modi principali:
1.  **Low-dose CT (Full-dose CT):** Si riduce la dose per ogni scansione preservando la geometria "completa" (oltre 1000 proiezioni su traiettoria circolare).
2.  **Riduzione del numero di scansioni:**
    *   **Sparse-view CT (o few-view CT):** Grande passo angolare tra proiezioni consecutive.
    *   **Limited-angle CT (Tomosynthesis):** Range angolare limitato per le proiezioni.

---

## 🔄 La Ricostruzione CT come Problema Inverso Lineare

### Back Projection e Central Slice Theorem
L'obiettivo della CT è calcolare la funzione $\mu(x, y)$ dalle proiezioni $m(t, \theta)$. La ricostruzione intuitiva si basa sulla **Back Projection**: si "ri-proiettano" sui voxel dell'oggetto i valori misurati per ogni pixel del detector ad ogni angolo di scansione.

> ⚠️ **Il Problema della Sfocatura**
> Le immagini ottenute con la semplice *Back Projection* appaiono molto sfocate (*blurred*).
> **Soluzione:** Lavorare nel dominio di Fourier sulle componenti ad alta frequenza del sinogramma e poi trasformare nel dominio dell'immagine (Filtered Back Projection). In questo modo non è necessario discretizzare l'integrale di proiezione.

Il fondamento matematico è il **Central Slice Theorem** (o *Fourier Slice Theorem*):
*   FT 1D della proiezione Radon $P_{\theta=0}(t)$: $\hat{P}_{\theta=0}(u) = \int_{-\infty}^{+\infty} P_{\theta=0}(t) e^{-2\pi i ut} dt$
*   FT 2D di $\mu(t, s)$: $\hat{\mu}(u, v) = \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} \mu(t, s) e^{-2\pi i (ut+vs)} ds dt$
*   **Relazione:** $\hat{\mu}(u, 0) = \hat{P}_{\theta=0}(u)$

### Il Problema Inverso
Il problema inverso della ricostruzione di immagini è **ill-posed**:
*   Nel caso di *low-dose CT*, il rumore viene amplificato nella soluzione.
*   Nel caso di *sparse-view CT*, esistono infinite soluzioni possibili.
Le strategie basate su FBP non sono buoni risolutori in questi casi. Si utilizzano quindi formulazioni *model-based* che gestiscono le informazioni *prior* sull'oggetto e forzano l'esistenza di una soluzione unica e accurata tramite **regularization**.

### Formulazione come Sistema Lineare (Caso 2D)
Consideriamo una sezione di larghezza $\delta$ mm partizionata in una griglia di $N_x \times N_y$ voxel. Sia $N = N_x \times N_y$ e $x_j$ un singolo voxel (il cui valore corrisponde al coefficiente di attenuazione $\mu$).

L'integrale di proiezione Radon per un angolo fisso $\theta$ è:
$$ \log\left(\frac{I_i}{I_0}\right) = -\sum_{j=1}^N a_{i,j} x_j $$
Ponendo $m_i = -\log(I_i / I_0)$, si ottiene:
$$ \sum_{j=1}^N a_{i,j} x_j = m_i $$

Il sistema globale per tutti gli angoli è $Ax = b$, dove:
*   $x \in \mathbb{R}^{N_x N_y}$
*   $b = [m_{\theta_1}; \dots; m_{\theta_{n_{angoli}}}] \in \mathbb{R}^{n_{pixel} n_{angoli}}$
*   $A = [A_{\theta_1}; \dots; A_{\theta_{n_{angoli}}}] \in \mathbb{R}^{n_{pixel} n_{angoli} \times N_x N_y}$

Il vettore delle misurazioni contiene il rumore: $b = b_{exact} + \eta$, dove $\eta$ è un mix di rumore Poissoniano e Gaussiano.

### Calcolo degli Elementi della Matrice $A$
La matrice $A$ è **molto sparsa**. I suoi elementi $a_{i,j}$ (che rappresentano il contributo dell'$j$-esimo voxel all'$i$-esimo pixel del detector) possono essere calcolati in due modi:

| Metodo | Descrizione |
| :--- | :--- |
| **Ray driven** | Calcola il rapporto tra la lunghezza dell'intersezione del raggio con il voxel e la lunghezza dell'intersezione del raggio con l'oggetto. |
| **Pixel/Voxel driven** | Calcola il rapporto tra l'area dell'ombra del voxel sul pixel e l'area del pixel. |

---

## 🎀 Case Study: Digital Breast Tomosynthesis (DBT)

La **Digital Breast Tomosynthesis (DBT)** è una tecnica di *3D limited angle Tomography* che rappresenta un'alternativa recente alla mammografia per gli screening, permettendo ricostruzioni 3D della mammella.

### Setting Geometrico e Vantaggi
*   **Oggetto:** Discretizzato in $N_x \times N_y \times N_z$ voxel (con $N_z \ll N_x$).
*   **Detector:** Discretizzato in $n_x \times n_y$ pixel.
*   **Sorgente:** Si muove lungo un piccolo arco circolare (15-30 gradi).
*   **DBT Setting:** 11/13 proiezioni, grande passo angolare (3 gradi), raggi X "soft".
*   **Vantaggi clinici:** Rispetto alla mammografia 2D tradizionale, la DBT riduce l'impatto dei tessuti sovrapposti sul tumore, rendendo più facile il rilevamento da parte del radiologo.

### Requisiti Software e Dati Reali
*   **Requisiti:** Accuratezza *in-plane* di 90/100 $\mu m$; ricostruzioni in 45/60 secondi.
*   **Dati Reali (es. Phanton BR3D):**
    *   Volume oggetto: $1168 \times 3328 \times 55$ voxel.
    *   Dimensione voxel: $0.090 \times 0.90 \times 1$ mm.
    *   Sinogramma: $1290 \times 3190 \times 13$ pixel.
    *   Dimensione pixel: $0.085 \times 0.85$ mm.

> ⚠️ **La Sfida Computazionale**
> Con queste dimensioni, la matrice $A$ avrebbe dimensioni $\approx 2.13 \cdot 10^8 \times 4.5 \cdot 10^7$.
> Poiché $A$ ha $1.9 \cdot 10^{10}$ elementi non nulli, richiederebbe circa **320 TB** di memoria per essere memorizzata (cosa infattibile!).
> Di conseguenza, $A$ deve essere **ri-computata ad ogni step** che la coinvolge. Sono necessarie schede **GPU** per eseguire computazioni parallele.

---

## 📊 Riepilogo Formule Chiave

| Concetto | Formula / Notazione Matematica | Descrizione |
| :--- | :--- | :--- |
| **Legge di Lambert-Beer** | $I = I_0 e^{-\mu ds}$ | Intensità trasmessa attraverso un singolo materiale di spessore $ds$. |
| **Proiezione (Caso Reale)** | $-\log\left(\frac{I}{I_0}\right) = \int_L \mu(x, y) d\ell$ | Integrale di linea del coefficiente di attenuazione lungo il raggio $L$. |
| **Central Slice Theorem** | $\hat{\mu}(u, 0) = \hat{P}_{\theta=0}(u)$ | La FT 1D di una proiezione è uguale a una fetta centrale della FT 2D dell'immagine. |
| **Sistema Lineare CT** | $Ax = b$ | Formulazione discreta del problema di ricostruzione. |
| **Proiezione Radon Discreta** | $\sum_{j=1}^N a_{i,j} x_j = m_i$ | Contributo dei voxel $x_j$ alla misurazione $m_i$ del detector. |
| **Rumore nelle misurazioni** | $b = b_{exact} + \eta$ | Il dato misurato $b$ include il rumore $\eta$ (Poisson + Gauss). |