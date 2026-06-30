---
title: "Socket (Meccanismi di Comunicazione di Basso Livello)"
course: Sistemi Distribuiti
category: concept
topics: [socket, comunicazione, rete, tcp, udp, endpoint, porte, berkeley-sockets]
difficulty: intermedio
sources: [communication_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Socket (Meccanismi di Comunicazione di Basso Livello)

## Contesto

Finora la comunicazione tra nodi è stata descritta come "**inviare messaggi**" — ma *come* esattamente si inviano e ricevono i messaggi? I **protocolli di rete** (TCP, UDP) sono la risposta a livello di rete; i **socket** sono il modo in cui questi protocolli si **usano nei programmi**. I socket sono quindi il **substrato concreto** su cui poggiano tutte le astrazioni di comunicazione di più alto livello ([[protocolli-interazione-comuni|RPC, HTTP, RMI]], qualunque protocollo applicativo).

> La quasi totalità dei linguaggi fornisce un wrapper sulla **Berkeley sockets API** (in Python: il modulo `socket`): API *antica e stabile*, *didattica* (espone meccanismi ricorrenti: connection-less/oriented, message/stream-based) ed *elementare* (si costruiscono sopra astrazioni più alte).

## Definizione

> Un **socket** è una **rappresentazione astratta dell'endpoint locale** di un percorso di comunicazione di rete.

**Interpretazione:** è il **gateway di un processo verso la rete**, che fornisce un mezzo di comunicazione **full-duplex, multiplexabile, point-to-point o point-to-multipoint** verso altri processi distribuiti sulla rete. Un socket è identificato dalla coppia **`IP address : port`** (l'endpoint).

## Le proprietà del socket

1. **Distributed processes** — i socket fanno comunicare processi: anche processi sulla **stessa macchina** possono comunicare via socket; lo **stesso socket** può essere condiviso tra thread.
2. **Communication (esplicita)** — lo scambio di informazione è **esplicito**: i dati si inviano/ricevono via metodi *ad-hoc* del socket.
3. **Point-to-point** — ogni socket media l'interazione tra **due** processi (in opposizione al point-to-multipoint, dove un socket comunica con più processi).
4. **Multiplexable** — si possono creare **più socket indipendenti** su porte diverse.
5. **Full-duplex** — i dati possono fluire in **entrambi i versi simultaneamente** (chi riceve può inviare e viceversa).

### Porte
Le **porte** sono interi positivi a 2 byte nel range **0–65535**:
- range **0–1023**: riservato a protocolli *well-known*;
- range **1024–65535**: per uso *custom*.

Una macchina può avere **più indirizzi IP** (uno per interfaccia di rete); `127.0.0.1` (*localhost*) è l'indirizzo di **loopback**.

### Byte-orientation
Sia i pacchetti che gli stream sono mezzi **byte-oriented**: l'**unità di comunicazione è il byte**. **Il socket non si cura del contenuto** dei dati scambiati → è l'**applicazione** che interpreta i byte (serializzazione/encoding, default **UTF-8**; encoding e decoding devono essere **consistenti** tra mittente e destinatario).

## I due tipi di socket

| | **Stream socket** | **Datagram socket** |
|--|------------------|---------------------|
| Unità | stream di byte di **lunghezza illimitata** | **pacchetti** (datagram) di dimensione finita |
| Protocollo tipico | **TCP** | **UDP** (max datagram 64 KiB) |
| Connessione | **connection-oriented** (affidabile, ordinato) | **connectionless** (ogni invio è indipendente) |
| Client/Server | **distinzione netta** (API diverse) | **nessuna distinzione** (ogni socket può fare da client o server) |
| Cardinalità | **one-to-one** (servono più connessioni per più peer) | supporta **broadcast e multicast** |

→ confronto approfondito in [[stream-vs-datagram-sockets]].

### Gergo
- **client socket** / **server socket**: chi **inizia** vs chi **accetta** la comunicazione.
- **local** / **remote**: dal punto di vista del client socket, il server socket è *remoto* e viceversa.

## L'API in pratica (Python `socket`)

```python
import socket
# datagram (UDP):  socket.SOCK_DGRAM   |   stream (TCP): socket.SOCK_STREAM
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)   # AF_INET = IPv4
sock.bind(('0.0.0.0', 12345))   # bind: associa il socket a indirizzo:porta locali
```

- **`bind`** — associa il socket a `(address, port)` locali così che l'OS sappia dove consegnare i dati in arrivo. `'0.0.0.0'` = tutte le interfacce; porta `0` = l'OS sceglie una porta libera (tipico dei client).
- **Datagram**: `sendto(payload, (ip, port))` e `recvfrom(bufsize)` → restituisce `(data, sender)`; ogni datagram porta con sé l'endpoint destinatario.
- **Stream — client**: `connect((ip, port))` (bloccante, può dare `ConnectionRefusedError`/`TimeoutError`) → poi `sendall(data)` / `recv(BUFFER_SIZE)`; `shutdown(SHUT_WR/SHUT_RD)` segnala "niente più dati"; `close()`.
- **Stream — server**: `listen(backlog)` (non bloccante; `backlog` = code di connessioni pendenti) → loop `client_sock, addr = accept()` (bloccante finché non arriva una connessione); il socket *listener* serve **solo ad accettare**, il nuovo `client_sock` per scambiare dati.
- **Golden rule**: essere **sempre consapevoli di quanti byte** si inviano/ricevono per volta (evitare read/write illimitate che saturano rete o memoria); `BUFFER_SIZE` tipicamente una piccola potenza di 2 (es. 4096).

### Framing dei messaggi su stream (length-prefixing)
Uno stream è un flusso continuo senza confini di messaggio. Per scambiare **messaggi distinti** su una connessione persistente, il pattern comune è **prefissare ogni messaggio con la sua lunghezza** (es. un intero a 32 bit big-endian): il ricevente legge prima i 4 byte di lunghezza, poi *esattamente* quel numero di byte.

```python
payload = length.to_bytes(4, 'big') + message.encode()   # mittente
length  = int.from_bytes(sock.recv(4), 'big'); msg = sock.recv(length)  # ricevente
```

### Comunicazione asincrona e callback
La ricezione è **bloccante**: per non bloccare l'unico thread si usa un **thread separato** per la ricezione, che invoca una **callback**. Una *callback* è una funzione passata come dato (un riferimento), progettata per essere chiamata da un'altra funzione — spesso "indietro" verso il layer applicativo originario. → è l'ossatura di un design **event-based** ([[architectural-styles|event-based]]).

## Intuizione

Il socket è il punto in cui le **fallacie dei sistemi distribuiti** diventano codice concreto: UDP è *inaffidabile* (messaggi persi, ritardati, fuori ordine, duplicati → la fallacia "[[pitfalls-sistemi-distribuiti|the network is reliable]]"), TCP introduce *flow control* e quindi possibili **deadlock** se si invia tutto prima di ricevere ([[tcp-echo-deadlock]]). Scegliere stream vs datagram è una decisione del passo di **Implementation** del [[se-workflow-distribuito|workflow]] (*quali network protocol usare?*).

## Esempi

- [[udp-group-chat]] — chat P2P con datagram socket; raffinamento progressivo (blocking → thread/callback → terminazione graceful); le 5 issue classiche.
- [[tcp-echo-deadlock]] — il deadlock da TCP flow control e la soluzione (interleaving send/receive).
- [[distributed-pong]] — caso studio: **UDP** scelto per un videogioco real-time (bassa latenza > affidabilità).

## Connessioni

- [[stream-vs-datagram-sockets]] — il confronto TCP/stream vs UDP/datagram.
- [[protocolli-interazione-comuni]] — **(M2)** request-response, pub-sub, RPC/RMI: astrazioni costruite *sopra* i socket.
- [[interaction-patterns]] — **(M2)** i socket sono il *come* fisico dei messaggi tra partecipanti.
- [[infrastruttura-e-componenti]] — **(M2)** client/server come *ruoli*; qui client/server *socket* come endpoint.
- [[middleware]] — il middleware astrae i socket offrendo modelli di comunicazione di più alto livello.
- [[pitfalls-sistemi-distribuiti]] — **(M4)** "the network is reliable" / "latency is zero" / "bandwidth is infinite": rese concrete da UDP e dal flow control TCP.
- [[se-workflow-distribuito]] — **(M2)** la scelta del protocollo di rete è un concern di Implementation.
- [[features-design-distribuito]] — **(M2)** retry/timeout sopra UDP; heartbeat; autenticazione (assente nei socket "nudi").

## Sorgenti

- `raw-sources/communication_slides.pdf` (slide 3–12, 28–36 — *Sockets, Datagram & Stream sockets*), G. Ciatto, *Communication Mechanisms for Distributed Systems*, Distributed Systems — Module 2, A.Y. 2025/2026.
- Riferimenti: Berkeley sockets API; Python `socket` module; *Socket Programming in Python (Real Python Guide)*.
