Un protocollo a livello di trasporto mette a disposizione una **comunicazione logica (logical communication)** tra processi applicativi di host differenti. Per comunicazione logica si intende, dal punto di vista dell’applicazione, che tutto proceda come se gli host che eseguono i processi fossero direttamente connessi; in realtà, gli host si possono trovare agli antipodi del pianeta, connessi da numerosi router e da svariati tipi di collegamenti. I processi applicativi usano la comunicazione logica fornita dal livello di trasporto per scambiare messaggi, senza preoccuparsi dei dettagli dell’infrastruttura fisica utilizzata per trasportarli.
## Panoramica del livello di trasporto di Internet
Internet, e più in generale una rete TCP/IP, mette a disposizione del livello di applicazione due diversi protocolli di trasporto:
- **UDP** (User Datagram Protocol), che fornisce alle applicazioni un servizio non affidabile e non orientato alla connessione
- **TCP** (Transmission Control Protocol), che offre un servizio affidabile e orientato alla connessione.

Ogni pacchetto mandato dal livello di trasporto lo chiameremo **segmento**.

Prima di procedere nella nostra introduzione su UDP e TCP risulta utile soffermarci brevemente sul livello di rete di Internet  Il protocollo a livello di rete di Internet ha un nome: **IP (Internet Protocol)**. Tale protocollo fornisce comunicazione logica tra host. Il suo modello di servizio prende il nome di **best-effort delivery service** o, più comunemente, **best-effort** (“massimo sforzo”): questo significa che IP fa “del suo meglio” per consegnare i segmenti tra host comunicanti, ma non offre garanzie. In particolare, non assicura né la consegna dei segmenti né il rispetto dell’ordine originario e non garantisce neppure l’integrità dei dati all'interno dei segmenti. Per queste ragioni si dice che IP offra un **servizio non affidabile** (unreliable service). Ricordiamo anche che ciascun host presenta quantomeno **un indirizzo a livello di rete: il suo indirizzo IP**. 

Il principale compito di TCP e UDP è l'estensione del servizio di consegna IP tra sistemi periferici a quello di consegna tra processi di esecuzione sui sistemi periferici.
Questo passaggio viene chiamato **multiplexing** e **demultiplexing a livello di trasporto** (transport layer multiplexing/demultiplexing).
### Multiplexing e Demultiplexing
Per introdurre il multiplexing e il demultiplexing, pensiamo a questo esempio:

> [!example] Esempio di comunicazione tra due computer
> Supponiamo che vi troviate di fronte ad un  computer su cui scarichiamo pagine web mentre sono in esecuzione una sessione FTP e due sessioni Telnet. Si avranno pertanto quattro processi applicativi in esecuzione: due Telnet, uno FTP e uno HTTP. 

#### Multiplexing e demultiplexing non orientati alla connessione (UDP)

#### Multiplexing e demultiplexing orientati alla connessione(TCP)