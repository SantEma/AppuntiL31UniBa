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

> [!example] Esempio di comunicazione tra due processi su due host (parte 1)
> Si considerino due host: **Host A**, il computer locale presso il quale si trova l'utente, e **Host B**, il computer remoto sul quale si ipotizza, per semplicità e senza perdita di generalità, che risiedano un server Telnet, un server FTP e un server web. I quattro processi suddetti assumono pertanto il ruolo di processi client, ciascuno associato ad almeno una socket.
> 
> Ogni processo applicativo gestisce una o più **socket**, interfacce attraverso le quali i dati fluiscono dal processo verso la rete e viceversa. Il livello di trasporto dell'host in ricezione non consegna pertanto i dati direttamente al processo, bensì ad una socket intermedia; poiché in un medesimo istante possono coesistere più socket in uno stesso host, ciascuna deve disporre di un identificatore univoco, il cui formato dipende dal fatto che si tratti di socket UDP o TCP.
> 
> Prima di continuare, guardiamo cosa sono i numeri di porta

Ogni host, per indirizzare al processo giusto i segmenti di rete a livello di trasporto, richiede che:
1. Le socket abbiano identificatori unici; 
2. Ciascun segmento presenti campi che indichino la socket cui va consegnato il segmento, ossia:
	- **Campo del numero di porta di origine**
	- **Campo del numero di porta di intestazione**

I numeri di porta sono grandi 16 bit e vanno da 0 a 65535, ma esistono dei **numeri di porta noti**, riservati a dei protocolli applicativi ben noti 
In generale, un segmento a livello di trasporto si presenta in questa maniera:
![[Pasted image 20261104145113.png]]

> [!example] Esempio di comunicazione tra due processi su due host (parte 2)
> Riprendendo l'esempio:
> Il livello di trasporto di Host A raccoglie i frammenti di dati in uscita dalle socket dei quattro processi, incapsula ciascuno di essi in un segmento dotato di apposita intestazione (appena visto) e consegna i segmenti così ottenuti al livello di rete sottostante. Poiché i quattro flussi attraversano il medesimo canale da host a host (un medesimo indirizzo IP), il livello di trasporto effettua il **multiplexing** dei dati provenienti da socket distinte su un unico canale di comunicazione verso Host B.
> 
> Il livello di trasporto di Host B riceve dal livello di rete i segmenti in arrivo, ne esamina i campi di intestazione deputati allo scopo, identifica la socket destinataria e vi inoltra il carico utile, che giunge così al processo applicativo competente. Tale operazione è detta **demultiplexing**. Si osservi che le due funzioni vengono esercitate da entrambi gli host in entrambe le direzioni: anche Host A, in ricezione, deve smistare tra i propri quattro processi client i segmenti provenienti da Host B.
> 
> Grazie al concorso di porte, socket e intestazioni di trasporto, il servizio di trasporto da host a host offerto dal livello di rete viene elevato a servizio **da processo a processo**: ciascun segmento, tra i flussi che intercorrono tra i due computer, viene consegnato esattamente al processo applicativo competente.
> ![[Pasted image 20261104150551.png]]
> 
#### Multiplexing e demultiplexing non orientati alla connessione (UDP)

#### Multiplexing e demultiplexing orientati alla connessione(TCP)