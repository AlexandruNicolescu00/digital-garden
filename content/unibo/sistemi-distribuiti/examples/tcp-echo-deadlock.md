---
title: "Esempio: TCP Echo e il Deadlock da Flow Control"
course: Sistemi Distribuiti
category: example
topics: [socket, tcp, stream, flow-control, deadlock, buffer, echo]
difficulty: intermedio
sources: [communication_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Esempio: TCP Echo e il Deadlock da Flow Control

## Problema

Implementare una semplice applicazione **echo** con [[stream-vs-datagram-sockets|stream socket (TCP)]]: il client inoltra il proprio *standard input* al server, che lo **rispedisce indietro** (echo), e il client lo stampa — come `cat` in Unix, ma con il server come intermediario. Esempio didattico per capire gli stream socket e **cosa succede quando lo stream è molto lungo**.

*(Codice: `unibo-fc-isi-ds/lab-snippets`, `snippets/lab3/`.)*

## Tecnica applicata

[[socket|Stream socket]] (`SOCK_STREAM`, TCP). **Server**: `bind` → `listen(1)` → `accept()` → loop `recv(BUFFER_SIZE)` / `sendall(buffer)` finché `recv` ritorna `b''`. **Client**: `connect` → invia lo stdin a blocchi → riceve l'echo → stampa.

```python
# Server (cuore del loop)
sock, addr = server.accept()
while True:
    buffer = sock.recv(BUFFER_SIZE)
    if not buffer: break          # connessione finita
    sock.sendall(buffer)          # echo
```

## Soluzione

### Attempt 1 (errato) — invia-tutto-poi-ricevi-tutto
Il client esegue **due fasi sequenziali**: prima invia *l'intero* stdin (`while: sendall`), poi `shutdown(SHUT_WR)`, poi riceve *tutto* l'echo (`while: recv`).

```python
while True:                       # FASE 1: invia tutto
    buffer = sys.stdin.buffer.read(BUFFER_SIZE)
    if not buffer: break
    sock.sendall(buffer)
sock.shutdown(socket.SHUT_WR)     # niente più da inviare
while True:                       # FASE 2: solo ora ricevo
    buffer = sock.recv(BUFFER_SIZE)
    ...
```

Per messaggi **corti** funziona. Per uno **stream lungo** (es. `python rand.py | ... client`, una sequenza infinita di numeri) → **deadlock**.

### Attempt 2 (corretto) — interleaving send/receive
La soluzione è **alternare** invio e ricezione: dopo ogni blocco inviato, il client riceve subito l'eco corrispondente.

```python
while True:
    buffer_local = sys.stdin.buffer.read(BUFFER_SIZE)
    if buffer_local:
        sock.sendall(buffer_local)
        buffer_remote = sock.recv(BUFFER_SIZE)     # ricevo subito l'eco
        if buffer_local != buffer_remote:          # verifica integrità dell'eco
            print("Wrong echoed data", file=sys.stderr); break
        sys.stdout.buffer.write(buffer_remote); sys.stdout.buffer.flush()
    else:
        break
sock.close()
```

## Errori comuni

### Il deadlock da TCP flow control (il cuore dell'esempio)
La catena causale dell'Attempt 1 con stream lungo:
1. l'input del client è **troppo lungo**;
2. il client inizia a ricevere l'echo **solo dopo** aver inviato *tutto*;
3. anche se il server fa l'echo un blocco alla volta…
4. … il **buffer in ingresso del client si satura** (nessuno lo svuota: il client è ancora in fase di invio) e, per il **flow control di TCP**, il server **rallenta e infine si blocca** sull'invio; a catena il client si blocca sull'invio → nessuno avanza più.
5. **Stream molto lungo + questa implementazione = deadlock.**

> ⚠️ **Non è un bug del codice "riga per riga"**: è *esattamente come funziona TCP* (back-pressure del flow control). L'errore è **architetturale/di protocollo applicativo**: separare nettamente "invia tutto" e "ricevi tutto" su un canale full-duplex con buffer finiti.

### Altri punti
- **Read/write illimitate** — violano la *golden rule* dei [[socket]] (sapere sempre quanti byte si scambiano): saturano rete/memoria.
- **Confini di messaggio** — lo stream non ha confini di messaggio; per messaggi distinti serve il **length-prefixing** (cfr. [[socket]]), non rilevante per l'echo ma essenziale nella TCP chat.

## Generalizzazione

È un caso di **mismatch tra modello di interazione e semantica del canale**: un canale [[stream-vs-datagram-sockets|stream full-duplex]] con buffer **finiti** e **back-pressure** richiede che produzione e consumo procedano **insieme**. La lezione si generalizza a qualunque pipeline con buffer limitati (produttore/consumatore): se il consumatore non drena mentre il produttore riempie, il sistema va in **deadlock per saturazione**. È la fallacia "[[pitfalls-sistemi-distribuiti|bandwidth is infinite]]" / "latency is zero" resa concreta: i buffer e la banda **non** sono infiniti.

## Connessioni

- [[socket]] · [[stream-vs-datagram-sockets]] — stream socket e flow control TCP.
- [[udp-group-chat]] — la controparte UDP (inaffidabile, ma senza flow control / deadlock).
- [[pitfalls-sistemi-distribuiti]] — "bandwidth is infinite" / "latency is zero" smentite dal flow control.
- [[features-design-distribuito]] — heartbeat/timeout per rilevare blocchi; back-pressure come tema di design.

## Sorgenti

- `raw-sources/communication_slides.pdf` (slide 37–44 — *Example: TCP Echo*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
- Codice: https://github.com/unibo-fc-isi-ds/lab-snippets (`snippets/lab3/`).
