---
title: "Esempio: UDP Group Chat (datagram socket)"
course: Sistemi Distribuiti
category: example
topics: [socket, udp, datagram, peer-to-peer, thread, callback, graceful-termination]
difficulty: intermedio
sources: [communication_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Esempio: UDP Group Chat (datagram socket)

## Problema

Implementare una semplice **chat di gruppo peer-to-peer** con [[stream-vs-datagram-sockets|datagram socket (UDP)]]: ogni partecipante invia i messaggi a **tutti** gli altri. Ogni partecipante è identificato da un nickname e/o da un endpoint; ogni messaggio contiene **nickname del mittente, testo, timestamp**. UI a riga di comando.

*(Codice di riferimento: `unibo-fc-isi-ds/lab-snippets`, `snippets/lab2/`.)*

## Tecnica applicata

[[socket|Datagram socket]] in Python (`SOCK_DGRAM`): `bind` su una porta locale, `sendto` verso ogni peer, `recvfrom` per ricevere. Una classe `Peer` **incapsula** il socket e l'insieme dei peer, esponendo `send_all(message)` (cicla `sendto` su tutti i peer) e `receive()`.

```python
class Peer:
    def __init__(self, port, peers=None):
        self.peers = {address(*p) for p in (peers or set())}
        self.__socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.__socket.bind(address(port=port))
    def send_all(self, message):
        if not isinstance(message, bytes): message = message.encode()
        for peer in self.peers:
            self.__socket.sendto(message, peer)
```

## Soluzione (raffinamento progressivo)

### Attempt 1 — versione "sbagliata"
Loop sincrono: `input()` → `send_all` → `print(receive())`. **Funziona a malapena** ed espone **5 issue** (vedi sotto).

### Attempt 2 — thread + callback (`AsyncPeer`)
Si risolve il **blocking** dedicando un **thread separato (daemon)** alla ricezione, che invoca una **callback** a ogni messaggio:

```python
class AsyncPeer(Peer):
    def __init__(self, port, peers=None, callback=None):
        super().__init__(port, peers)
        self.__callback = callback or (lambda *_: None)
        threading.Thread(target=self.__handle_incoming_messages, daemon=True).start()
    def __handle_incoming_messages(self):
        while True:
            message, address = self.receive()
            self.__callback(message, address)
```

Ora invio e ricezione non si bloccano a vicenda; la chat minimale diventa un `while True: peer.send_all(message(input(), username))` con `callback=lambda msg, _: print(msg)`.

### Attempt 3 — terminazione graceful
Si introduce un **messaggio speciale** `EXIT_MESSAGE`; alla ricezione, il peer **rimuove il mittente** dalla lista. All'uscita locale (chiusura terminale) si cattura l'eccezione e si **notificano** gli altri:

```python
EXIT_MESSAGE = "<LEAVES THE CHAT>"
# nel receiver:  if message.endswith(EXIT_MESSAGE): self.peers.remove(address)
# nel main loop:
try:
    peer.send_all(message(input(), username))
except (EOFError, KeyboardInterrupt):     # Ctrl+D (EOF) / Ctrl+C
    peer.send_all(message(EXIT_MESSAGE, username)); peer.close(); exit(0)
```

**Rationale**: non serve terminare l'app quando un peer esce — la chat continua e il peer uscente viene "dimenticato"; altri possono unirsi in qualsiasi momento.

## Errori comuni (le 5 issue dell'Attempt 1)

1. **Blocking I/O su un solo thread** — sia `receive()` sia `input()` sono **bloccanti**: il peer resta bloccato in attesa di un messaggio *o* dell'input utente, non può fare entrambi. → **fix**: thread multipli (1 per sorgente di input + 1) → *Attempt 2*.
2. **Bootstrap client/server** — a runtime sono peer, ma **inizialmente uno fa da client e uno da server**: il client deve conoscere l'indirizzo del server, che non è noto a priori. → **fix**: un **server centrale che fa da broker** per i partecipanti.
3. **Mancanza di terminazione graceful** — l'unico modo di uscire è terminare a forza (Ctrl+C); il peer remoto **non viene notificato**. → **fix**: messaggio di terminazione → *Attempt 3*.
4. **Mancanza di autenticazione** — i peer dichiarano la propria identità *onestamente* nel payload, ma **non è verificata** (nessun controllo di unicità del nickname o corrispondenza indirizzo↔nickname): un peer **malevolo può impersonare** altri. → **fix**: crittografia a chiave pubblica (fuori scope) o server centrale con protocollo di autenticazione.
5. **UDP è inaffidabile** — messaggi persi, ritardati, fuori ordine o duplicati; il codice **non li gestisce affatto**. → **fix**: meccanismi di **retry** o protocollo affidabile (TCP). È la fallacia "[[pitfalls-sistemi-distribuiti|the network is reliable]]" resa concreta.

> Solo le issue 1 e 3 vengono risolte nel corso dell'esempio; 2, 4, 5 restano aperte e motivano il passaggio a TCP ([[tcp-echo-deadlock|TCP echo]] / TCP chat).

## Generalizzazione

È un caso istanza di **comunicazione P2P best-effort one-to-many** su [[stream-vs-datagram-sockets|datagram/UDP]]: il modello dove non c'è distinzione client/server e ogni datagram porta il proprio destinatario. Le 5 issue sono il microcosmo dei grandi temi del corso: concorrenza (thread), [[infrastruttura-e-componenti|ruoli/broker]], [[features-design-distribuito|graceful shutdown / autenticazione / affidabilità]], [[modelli-di-fallimento|modelli di guasto]]. Il pattern **callback + thread di ricezione** è l'ingresso allo stile [[architectural-styles|event-based]].

## Connessioni

- [[socket]] · [[stream-vs-datagram-sockets]] — il concetto e il confronto.
- [[tcp-echo-deadlock]] — la controparte TCP (affidabile ma con flow control).
- [[pitfalls-sistemi-distribuiti]] — l'issue 5 è la fallacia n.1.
- [[features-design-distribuito]] — autenticazione, retry, terminazione: feature di design qui assenti.
- [[protocolli-interazione-comuni]] — broadcast/multicast; il broker come fix dell'issue 2.

## Sorgenti

- `raw-sources/communication_slides.pdf` (slide 13–27 — *Example: UDP Group Chat*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
- Codice: https://github.com/unibo-fc-isi-ds/lab-snippets (`snippets/lab2/`).
