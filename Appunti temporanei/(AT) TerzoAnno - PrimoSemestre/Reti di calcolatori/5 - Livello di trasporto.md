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
Una socket UDP è identificata esclusivamente da una **coppia (2-tupla)** composta da:

1. **Indirizzo IP di destinazione**
2. **Numero di porta di destinazione**

Quando un segmento UDP arriva a un host, il livello di trasporto esamina unicamente il numero di porta di destinazione e lo indirizza alla socket corrispondente, indipendentemente dall'indirizzo IP o dalla porta di origine. Di conseguenza, due segmenti UDP provenienti da host differenti o da porte di origine diverse, ma diretti allo stesso IP e alla stessa porta di destinazione, verranno incanalati nella **medesima socket** e consegnati allo stesso processo applicativo


> [!example] Esempio di multiplexing e demultiplexing non orientati alla connessione 
> ![[Pasted image 20261104155207.png]]
> Si consideri uno scenario in cui un processo residente sull'Host A, associato alla porta UDP 19157 intenda trasmettere un blocco di dati applicativi a un processo in esecuzione sull'Host B, attestato sulla porta UDP 46428. Il livello di trasporto dell'Host A assembla il segmento inserendo i dati dell'applicazione, il numero di porta di origine 19157 e il numero di porta di destinazione 46428. Successivamente, il segmento viene passato al livello di rete, che lo incapsula in un datagramma IP e lo trasmette attraverso la rete verso l'Host B.
> 
> All'arrivo del datagramma presso l'Host B, il livello di trasporto locale analizza il campo della porta di destinazione del segmento, rilevando il valore 46428, e dirotta il segmento verso la socket identificata da tale porta. Nel caso in cui un ulteriore host, ad esempio un ipotetico Host C, inviasse contemporaneamente un segmento UDP indirizzato alla medesima porta 46428 dell'Host B, il livello di trasporto dell'Host B indirizzerebbe anche quest'ultimo pacchetto alla medesima socket, senza che vi sia alcuna distinzione o creazione di un canale separato.
> 
> Infine, il numero di porta di origine 19157 incluso nel segmento iniziale funge da indirizzo di ritorno. Quando l'Host B risponde all'Host A, il relativo livello di trasporto effettua l'inversione delle porte: imposta la porta di destinazione del messaggio di risposta pari a 19157 e la porta di origine pari a 46428.


#### Multiplexing e demultiplexing orientati alla connessione(TCP)
Una socket TCP è identificata univocamente da una **quaterna (4-tupla)** composta da:
1. **Indirizzo IP di origine**
2. **Numero di porta di origine**
3. **Indirizzo IP di destinazione**
4. **Numero di porta di destinazione**

Quando un segmento TCP giunge a destinazione, l'host esamina **tutti e quattro i valori** per effettuare il demultiplexing verso la socket corretta. A differenza di UDP, due segmenti TCP in arrivo che presentano differenti IP di origine o differenti porte di origine vengono diretti a **due socket distinte**, anche se condividono lo stesso IP e la stessa porta di destinazione (con l'unica eccezione dei segmenti iniziali di richiesta di connessione SYN)

> [!example] Esempio di multiplexing e demultiplexing orientati alla connessione 
> ![[Pasted image 20261104155514.png]]
> Diversamente da quanto accade con UDP, due segmenti TCP in arrivo aventi indirizzi IP di origine distinti o numeri di porta di origine differenti vengono indirizzati a due socket separate, anche qualora condividano lo stesso indirizzo IP e lo stesso numero di porta di destinazione. L'unica eccezione a questa regola è rappresentata dai segmenti di richiesta di instaurazione della connessione aventi il bit SYN impostato a 1.
> 
> Nel ciclo di vita di un'applicazione server TCP, la gestione delle connessioni richiede l'impiego di due tipologie distinte di socket. Il server mantiene inizialmente una socket di benvenuto (welcoming socket), legata a un determinato numero di porta (ad esempio la porta 80 per i server Web o la porta 12000), rimanendo in ascolto delle richieste di connessione in ingresso da parte dei client. Quando un client intende avviare una sessione, crea una propria socket e trasmette un segmento TCP contenente il bit SYN impostato a 1, specificando la porta di destinazione del server e una porta di origine dinamica generata dal proprio sistema operativo.
> 
> Al ricevimento del segmento SYN, il sistema operativo del server individua il processo in ascolto sulla porta di destinazione e invoca il metodo per accettare la connessione del programma, il quale instanzia una nuova socket di connessione (connection socket) interamente dedicata a quel determinato client. Il livello di trasporto del server registra la quaterna identificativa formata dall'indirizzo IP sorgente, dalla porta sorgente, dal proprio indirizzo IP e dalla porta di destinazione. Tutti i successivi segmenti che recheranno nell'intestazione dati corrispondenti a questa specifica quaterna verranno instradati verso la nuova socket di connessione dedicata.
> 
> Prendiamo in considerazione l'Host B con un server HTTP (in ascolto sulla porta 80) contattato contemporaneamente da due client distinti, denominati Host A e Host C. L'Host C avvia due sessioni HTTP distinte verso l'Host B, assegnando a esse due differenti numeri di porta di origine, ad esempio la porta 26145 e la porta 7532. Nel medesimo istante, l'Host A avvia anch'esso una sessione HTTP verso l'Host B e il suo sistema operativo assegna in modo indipendente la porta sorgente 26145, che risulta numericamente identica a una delle porte usate dall'Host C.
> 
> Nonostante i segmenti inviati dall'Host A e uno dei flussi dell'Host C condividano sia la porta d'origine (26145) sia la porta di destinazione (80), l'Host B è in grado di effettuare il demultiplexing in modo del tutto corretto e privo di ambiguità. Il livello di trasporto dell'Host B distingue infatti le tre sessioni tramite le rispettive quaterne:
>  1. La prima connessione dell'Host C è legata alla quaterna (IP_C, 26145, IP_B, 80);
>  2. La seconda connessione dell'Host C corrisponde alla quaterna (IP_C, 7532, IP_B, 80) 
>  3.La connessione dell'Host A è associata alla quaterna (IP_A, 26145, IP_B, 80). 
>  
>  Ciascuna di queste tre quaterne indirizza i dati a una socket di connessione separata e riservata.

