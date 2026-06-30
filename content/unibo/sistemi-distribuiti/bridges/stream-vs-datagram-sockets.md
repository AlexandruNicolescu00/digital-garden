---
title: "Stream Socket (TCP) vs Datagram Socket (UDP)"
course: Sistemi Distribuiti
category: bridge
topics: [socket, tcp, udp, stream, datagram, connection-oriented, connectionless]
difficulty: intermedio
sources: [communication_slides.pdf]
created: 2026-06-27
updated: 2026-06-27
---

# Stream Socket (TCP) vs Datagram Socket (UDP)

Le due famiglie di [[socket]] incarnano i due grandi paradigmi della comunicazione di rete: **connection-oriented + affidabile** (stream/TCP) vs **connectionless + best-effort** (datagram/UDP). La scelta tra le due è una decisione di **Implementation** ([[se-workflow-distribuito|workflow SE]]).

## Tabella di confronto

| Caratteristica | **Stream socket** (TCP) | **Datagram socket** (UDP) |
|----------------|-------------------------|---------------------------|
| Unità di comunicazione | **stream** di byte, lunghezza illimitata | **datagram**: pacchetto autocontenuto di dimensione finita (max 64 KiB in UDP) |
| Connessione | **connection-oriented**: va stabilita tra **2 (e solo 2)** endpoint | **connectionless**: nessuna connessione; ogni send/recv è **indipendente** |
| Affidabilità | **affidabile e ordinato** (garantito da TCP) | **inaffidabile**: messaggi persi, ritardati, fuori ordine, duplicati |
| Direzionalità | **full-duplex**, ogni connessione = 2 stream diretti | per-datagram; il destinatario è specificato a ogni invio |
| Client vs Server | **distinzione netta**: API e funzionalità diverse | **nessuna distinzione**: ogni socket fa da client o server in qualsiasi momento |
| Cardinalità | **one-to-one** (più peer → più connessioni) | one-to-one, ma supporta **broadcast e multicast** |
| API Python (tipo) | `SOCK_STREAM` | `SOCK_DGRAM` |
| Primitive chiave | `connect` / `listen`+`accept`, `sendall` / `recv`, `shutdown` | `bind`, `sendto` / `recvfrom` |
| Confini di messaggio | **assenti** → serve framing (es. length-prefix) | **intrinseci**: 1 datagram = 1 messaggio |

## Vantaggi / svantaggi

### Stream (TCP)
- **Pro**: affidabilità e ordine *gratis*; connessione persistente → i peer reagiscono alla chiusura (graceful termination naturale, cfr. [[udp-group-chat|chat]]).
- **Contro**: setup della connessione; **flow control** può causare **deadlock** se il design invia tutto prima di ricevere ([[tcp-echo-deadlock]]); one-to-one → N connessioni per N peer; serve **framing** dei messaggi sullo stream.

### Datagram (UDP)
- **Pro**: leggero, niente connessione, **broadcast/multicast** nativi → ideale per scenari **peer-to-peer/group** (ogni datagram porta il proprio destinatario); confini di messaggio impliciti.
- **Contro**: **inaffidabile** → l'applicazione deve gestire perdite/duplicati (retry o passare a un protocollo affidabile); nessuna nozione di sessione → terminazione e identità vanno gestite a mano.

## Quando scegliere cosa

- **Datagram/UDP** quando: comunicazione **uno-a-molti** (broadcast/multicast), messaggi piccoli e indipendenti, tolleranza alle perdite (streaming real-time, telemetria, discovery, *group chat*).
- **Stream/TCP** quando: serve **affidabilità e ordine**, sessioni persistenti, trasferimenti lunghi, interazione **richiesta-risposta** strutturata su cui costruire protocolli applicativi.

> 🔑 La differenza riemerge a livello superiore: il **publish-subscribe broadcast/multicast** ([[protocolli-interazione-comuni]]) è naturale su UDP; il **request-response/RPC** affidabile vive tipicamente su TCP.

## Connessioni

- [[socket]] — il concetto comune di cui questi sono i due tipi.
- [[udp-group-chat]] — esempio datagram (P2P, le 5 issue).
- [[tcp-echo-deadlock]] — esempio stream (flow control → deadlock).
- [[protocolli-interazione-comuni]] · [[interaction-patterns]] — **(M2)** i pattern di alto livello costruiti su questi socket.
- [[features-design-distribuito]] — **(M2)** retry/timeout per rendere affidabile UDP.
- [[pitfalls-sistemi-distribuiti]] — **(M4)** "the network is reliable" è falso: UDP lo mostra senza filtri.
- [[distributed-pong]] — **(M2)** UDP scelto per il real-time; inaffidabilità simulata via `UDP_DROP_RATE`.

## Sorgenti

- `raw-sources/communication_slides.pdf` (slide 6, 9, 28 — *Two types of sockets / Datagram / Stream sockets*), G. Ciatto, Distributed Systems — Module 2, A.Y. 2025/2026.
