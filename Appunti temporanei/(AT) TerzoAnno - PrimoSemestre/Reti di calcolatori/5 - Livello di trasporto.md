Un protocollo a livello di trasporto fornisce una **comunicazione logica** (logical communication) tra processi applicativi in esecuzione su host differenti. Dal punto di vista dell'applicazione, tutto avviene come se gli host comunicanti fossero direttamente connessi, anche quando si trovano agli antipodi del pianeta e sono separati da numerosi router e collegamenti eterogenei. I processi applicativi sfruttano tale astrazione per scambiare messaggi senza preoccuparsi dei dettagli dell'infrastruttura fisica sottostante.
## Panoramica del livello di trasporto di Internet
Internet, e più in generale ogni rete TCP/IP, mette a disposizione del livello di applicazione due protocolli di trasporto:
- **UDP** (User Datagram Protocol), che offre un servizio non affidabile e non orientato alla connessione;
- **TCP** (Transmission Control Protocol), che offre un servizio affidabile e orientato alla connessione.

Per uniformità, chiameremo **segmento** ogni pacchetto generato dal livello di trasporto (anche se per UDP si utilizza talvolta il termine datagramma).

Il livello di rete sottostante di Internet è governato dal protocollo **IP** (Internet Protocol), il quale fornisce una comunicazione logica tra **host** (ciascuno dotato di almeno un indirizzo IP). Il modello di servizio di IP è definito best-effort ("massimo sforzo"): IP tenta di consegnare i pacchetti tra gli host comunicanti, ma non garantisce né l'effettiva consegna, né il rispetto dell'ordine originario, né l'integrità dei dati. Per questo motivo si dice che IP offre un servizio non affidabile (unreliable service).

Il compito fondamentale di TCP e UDP consiste nell'estendere il servizio di consegna IP tra sistemi periferici a un servizio di consegna tra **processi in esecuzione** su tali sistemi. Questa operazione prende il nome di **multiplexing e demultiplexing a livello di trasporto** (transport-layer multiplexing/demultiplexing).
## Multiplexing e demultiplexing
Per comprendere il funzionamento del multiplexing e del demultiplexing, analizziamo un esempio pratico.
> [!example] Esempio di comunicazione tra processi su due host (parte 1)
> Si considerino due host: l'Host A (il computer locale dell'utente) e l'Host B (un host remoto su cui girano un server Telnet, un server FTP e un server web). Sull'Host A sono attivi quattro processi client che comunicano con tali server: due sessioni Telnet, una sessione FTP e una sessione HTTP.
>
> Ogni processo applicativo dispone di una o più **socket**, interfacce attraverso le quali i dati transitano tra il processo e la rete. Il livello di trasporto dell'host ricevente non consegna i dati direttamente al processo, bensì a una socket intermedia. Poiché su un medesimo host possono coesistere più socket contemporaneamente, ciascuna deve possedere un identificatore univoco, il cui formato varia a seconda che si tratti di una socket UDP o TCP.

Per indirizzare ogni segmento alla socket corretta, il livello di trasporto richiede due elementi:
- identificatori univoci per le socket;
- due campi appositi nell'intestazione di ciascun segmento: il **numero di porta di origine** e il **numero di porta di destinazione**.

I numeri di porta sono codificati su 16 bit e assumono valori compresi tra 0 e 65535. La IANA suddivide questo spazio in tre fasce:

- **0 – 1023 (porte di sistema o well-known ports):** riservate da IANA ai servizi standard IETF (es. 21 per FTP, 23 per Telnet, 25 per SMTP, 80 per HTTP);
- **1024 – 49151 (porte registrate o registered ports):** assegnate da IANA su richiesta di terze parti e utilizzabili da applicazioni utente non di sistema;
- **49152 – 65535 (porte private o dinamiche):** assegnate temporaneamente e automaticamente dal sistema operativo quando un'applicazione client avvia una comunicazione.

In generale, l'intestazione di un segmento di trasporto include i seguenti campi di indirizzamento:
![[Pasted image 20261104145113.png]]

> [!example] Esempio di comunicazione tra processi su due host (parte 2)
> Riprendendo lo scenario precedente, il livello di trasporto dell'Host A raccoglie i blocchi di dati provenienti dalle socket dei quattro processi, li incapsula in segmenti dotati delle rispettive intestazioni e li passa al livello di rete. Poiché i quattro flussi condividono lo stesso canale IP tra Host A e Host B (la medesima coppia di indirizzi IP sorgente e destinazione), il livello di trasporto esegue il **multiplexing** dei dati su un unico canale verso Host B.
>
> Sul lato ricevente, il livello di trasporto dell'Host B esamina i campi di intestazione dei segmenti in arrivo dal livello di rete, individua la socket destinataria e vi consegna il carico utile affinché giunga al processo applicativo corretto. Tale operazione è detta **demultiplexing**. Entrambe le funzioni operano in ambedue le direzioni: anche l'Host A, in ricezione, esegue il demultiplexing per smistare tra i propri quattro client le risposte provenienti dall'Host B.
>
> Grazie alla combinazione di porte, socket e intestazioni di trasporto, il servizio host-to-host offerto dal livello di rete viene così esteso a un servizio process-to-process.

![[Pasted image 20261104150551.png]]
### Multiplexing e demultiplexing non orientati alla connessione (UDP)
Una socket UDP è identificata esclusivamente da una coppia (2-tupla) composta da:
- indirizzo IP di destinazione;
- numero di porta di destinazione.

Quando un segmento UDP giunge a un host, il livello di trasporto esamina unicamente il numero di porta di destinazione per consegnarlo alla socket corrispondente, ignorando indirizzo IP e porta di origine. Di conseguenza, segmenti UDP provenienti da host o porte di origine diversi, ma diretti allo stesso IP e alla stessa porta di destinazione, confluiscono nella medesima socket e vengono consegnati allo stesso processo applicativo.
> [!example] Esempio di multiplexing e demultiplexing non orientati alla connessione
> ![[Pasted image 20261104155207.png]]
>
> Un processo sull'Host A (porta UDP 19157) trasmette dati a un processo sull'Host B (porta UDP 46428). Il livello di trasporto dell'Host A costruisce il segmento con porta di origine 19157 e porta di destinazione 46428, passandolo al livello di rete che lo incapsula in un datagramma IP verso l'Host B.
>
> All'arrivo, il livello di trasporto dell'Host B legge la porta di destinazione 46428 e consegna il segmento alla socket corrispondente. Se contemporaneamente un Host C inviasse un segmento UDP alla stessa porta 46428 dell'Host B, il pacchetto verrebbe indirizzato alla medesima socket senza creare alcun canale separato.
>
> Il numero di porta di origine (19157) funge invece da indirizzo di ritorno: quando l'Host B risponde all'Host A, inverte le porte, impostando 46428 come porta di origine e 19157 come porta di destinazione.
### Multiplexing e demultiplexing orientati alla connessione (TCP)
Una socket TCP è identificata univocamente da una quaterna (4-tupla) composta da:
- indirizzo IP di origine;
- numero di porta di origine;
- indirizzo IP di destinazione;
- numero di porta di destinazione.

All'arrivo di un segmento TCP, l'host esamina tutti e quattro i campi per individuare la socket destinataria. A differenza di UDP, due segmenti TCP con diverso IP di origine o diversa porta di origine vengono indirizzati a due socket distinte anche se condividono lo stesso IP e la stessa porta di destinazione (fatta eccezione per i segmenti iniziali di richiesta di connessione con flag `SYN = 1`).
> [!example] Esempio di multiplexing e demultiplexing orientati alla connessione
> ![[Pasted image 20261104155514.png]]
>
> Un server TCP gestisce due tipi di socket: una **socket di benvenuto** (welcoming socket), in ascolto su una porta nota (es. porta 80 per HTTP o porta 12000) per ricevere richieste di connessione, e una **socket di connessione** (connection socket) dedicata a ciascun client. Quando un client avvia una sessione, invia un segmento con `SYN = 1`, specificando la porta di destinazione del server e una porta di origine dinamica. Alla ricezione, il server accetta la connessione e istanzia una nuova socket di connessione associata alla specifica quaterna del client; tutti i segmenti successivi con quella quaterna verranno instradati verso tale socket dedicata.
>
> Si consideri un server HTTP sull'Host B (porta 80) contattato simultaneamente dagli Host A e C. L'Host C apre due sessioni HTTP usando le porte sorgente 26145 e 7532; contemporaneamente, l'Host A apre una sessione HTTP cui il proprio sistema operativo assegna casualmente la stessa porta sorgente 26145.
>
> Nonostante la coincidenza delle porte di origine (26145) e di destinazione (80) tra Host A e una delle sessioni di Host C, l'Host B esegue il demultiplexing senza ambiguità distinguendo le tre quaterne:
>
> 1. prima connessione dell'Host C: `(IP_C, 26145, IP_B, 80)`;
> 2. seconda connessione dell'Host C: `(IP_C, 7532, IP_B, 80)`;
> 3. connessione dell'Host A: `(IP_A, 26145, IP_B, 80)`.
>
> Ciascuna quaterna corrisponde quindi a una socket di connessione separata.
## UDP
UDP svolge il minimo indispensabile per un protocollo di trasporto: oltre al multiplexing/demultiplexing e a un semplice controllo degli errori, non aggiunge altro ai servizi di IP. Ricevuto un messaggio dall'applicazione, UDP aggiunge le porte di origine e destinazione e altri due brevi campi, quindi consegna il segmento al livello di rete. Questo incapsula il segmento in un datagramma IP e tenta di recapitarlo all'host destinatario in modalità best-effort; se il pacchetto arriva a destinazione, UDP utilizza la porta di destinazione per consegnare i dati al processo corretto.

Poiché non prevede alcuno scambio preliminare (handshake) tra mittente e destinatario prima di trasmettere dati, UDP è un protocollo **non orientato alla connessione** (connectionless).

Le principali caratteristiche di UDP (RFC 768, 1980) sono:
- **Destinatari:** supporta comunicazioni unicast, multicast e broadcast, mentre TCP è esclusivamente punto a punto;
- **Dimensione:** la lunghezza massima del segmento UDP è limitata da quella del datagramma IP che lo incapsula ($\le 64\text{ KB}$, ovvero al massimo 65535 byte per il datagramma IPv4 completo);
- **Controllo degli errori:** affidato a un checksum opzionale in IPv4 (se omesso, il campo è posto a `0`) e obbligatorio in IPv6; consente solo il rilevamento degli errori, non il loro recupero;
- **Assenza di garanzie:** non prevede conferme di ricezione, ritrasmissioni, riordinamento dei dati, controllo di flusso o controllo di congestione;
- **Impieghi tipici:** DNS, SNMP (Simple Network Management Protocol), semplici interazioni richiesta-risposta (come le chiamate a procedura remota, RPC) e applicazioni multimediali in tempo reale.

Rispetto a TCP, UDP offre quattro vantaggi principali:
- **Controllo più preciso a livello applicativo sui dati inviati e sui tempi di invio:** UDP impacchetta e inoltra immediatamente i dati al livello di rete senza introdurre i ritardi dovuti al controllo di congestione o alle ritrasmissioni obbligatorie di TCP; ciò lo rende più adatto alle applicazioni real-time, che richiedono una velocità minima costante e tollerano piccole perdite di dati ma non ritardi elevati;
- **Nessun ritardo di instaurazione della connessione:** non richiede alcun handshake preliminare e può trasmettere immediatamente;
- **Nessuno stato di connessione:** non mantiene buffer dedicati, parametri di controllo della congestione né numeri di sequenza e riscontro, consentendo a un server di gestire molti più client contemporanei;
- **Intestazione ridotta:** l'intestazione UDP occupa soltanto 8 byte, contro i 20 byte minimi di TCP.
### Struttura dei segmenti UDP
> [!info] Componenti di un segmento UDP
> ![[Pasted image 20261104171258.png]]

L'intestazione UDP è composta da quattro campi da 16 bit ciascuno (8 byte complessivi):

- **Porta di origine e porta di destinazione (16 bit ciascuno):** identificano i processi comunicanti (valori da 0 a 65535). La porta di destinazione consente al ricevente il demultiplexing verso la socket corretta; la porta di origine fornisce l'indirizzo di ritorno per eventuali risposte.
- **Lunghezza (16 bit):** specifica la dimensione complessiva del segmento UDP in byte, comprensiva degli 8 byte di intestazione e del carico utile applicativo. Il valore minimo è 8 byte (segmento senza dati); il massimo teorico codificabile è 65535 byte, ma nella pratica il limite effettivo è imposto dalla dimensione massima del datagramma IP che lo incapsula ($\le 64\text{ KB}$).
- **Checksum (16 bit):** rileva eventuali alterazioni dei bit verificatesi durante il transito lungo i collegamenti o durante la permanenza nei buffer dei router intermedi. È opzionale in IPv4 (valore `0` se disabilitato) e obbligatorio in IPv6.
### Checksum UDP
Il checksum UDP permette di verificare se i bit del segmento abbiano subito corruzioni tra sorgente e destinazione, poiché non vi è garanzia che tutti i collegamenti intermedi effettuino il controllo degli errori e inoltre i bit possono alterarsi anche nella memoria dei router.

Sul lato mittente, il calcolo applica la somma in aritmetica del complemento a 1 a tutte le parole da 16 bit del segmento e ne esegue il complemento a 1 finale (inversione dei bit).

> [!example] Esempio di calcolo del checksum UDP
> Si considerino tre parole da 16 bit: `0110011001100000`, `0101010101010101` e `1000111100001100`.
>
> ```text
> Somma delle prime due parole:
>     0110011001100000
>   + 0101010101010101
>   ------------------
>     1011101110110101
>
> Somma della terza parola al risultato precedente:
>     1011101110110101
>   + 1000111100001100
>   ------------------
>   1 0100101011000001
>
> Riporto circolare (wrap-around) del bit di overflow al bit meno significativo:
>     0100101011000001
>   + 0000000000000001
>   ------------------
>     0100101011000010
> ```
>
> Invertendo tutti i bit (complemento a 1) si ottiene il checksum: `1011010100111101`.
>
> In ricezione si sommano le tre parole ricevute e il checksum (sempre con wrap-around del riporto): in assenza di errori il risultato è composto da tutti `1`, ossia `1111111111111111`.

Il checksum fornisce solo rilevamento e non recupero degli errori: se il destinatario rileva un'alterazione, il segmento viene scartato oppure, in alcune implementazioni, consegnato all'applicazione con una segnalazione di errore.
## Principi del trasferimento dati affidabile
> [!important] Riferimento al libro
> Questo argomento è utile ai fini della comprensione teorica, ma non costituisce oggetto diretto d'esame. Per l'approfondimento completo si vedano i paragrafi 3.4.1–3.4.4 del Capitolo 3 di "Reti di calcolatori e Internet. Un approccio top-down".

Per comprendere a fondo TCP è comunque utile richiamare come un protocollo di trasporto recuperi i pacchetti smarriti su un livello di rete non affidabile come IP. Poiché IP non garantisce la consegna, il mittente necessita di una conferma esplicita, detta **acknowledgment (`ACK`)**, inviata dal destinatario per notificare la corretta ricezione dei dati. Se però il segmento dati o il relativo `ACK` vanno perduti, il mittente rischierebbe un'attesa indefinita.

Per evitare tale blocco si utilizza un **timer a scadenza** (countdown timer): alla trasmissione di un segmento il mittente avvia un timer impostato su un intervallo di **timeout**. Se l'`ACK` giunge prima della scadenza, l'invio ha avuto successo; altrimenti, allo scadere del timer il mittente ritrasmette il segmento. Poiché nelle reti reali il ritardo massimo nel caso peggiore non è noto e sarebbe comunque troppo elevato per garantire buone prestazioni, si sceglie un timeout ragionevole trascorso il quale la perdita è ritenuta probabile. Un segmento semplicemente molto ritardato può quindi causare un timeout prematuro e una ritrasmissione superflua, ma i **numeri di sequenza** permettono al destinatario di riconoscere e scartare i duplicati.
## TCP (trasporto orientato alla connessione)
TCP è definito **orientato alla connessione** (connection-oriented) perché, prima di scambiarsi dati applicativi, i due processi devono eseguire un handshake preliminare per negoziare i parametri della comunicazione. La "connessione" TCP non è un circuito fisico TDM o FDM come nelle reti a commutazione di circuito: si tratta di una connessione logica il cui stato risiede esclusivamente nei due sistemi periferici. I router e gli switch intermedi non conservano alcuno stato TCP e gestiscono unicamente i datagrammi IP.

Una connessione TCP è inoltre:
- **full-duplex:** i dati applicativi possono fluire simultaneamente dal processo A al processo B e viceversa;
- **punto a punto (point-to-point):** collega sempre un singolo mittente e un singolo destinatario (il multicast non è supportato).

Client e server instaurano la connessione tramite un **handshake a tre vie** (three-way handshake), al termine del quale i due processi applicativi possono scambiarsi dati reciprocamente.
> [!example] Sguardo rapido all'handshake a tre vie
> ![[Pasted image 20261124143402.png]]
> I dati prodotti dal processo mittente vengono collocati nel **buffer di invio** della connessione, riservato durante l'handshake, da cui TCP preleva periodicamente blocchi da incapsulare in segmenti.
> 
> La quantità massima di dati applicativi inseribili in un singolo segmento è limitata dalla **dimensione massima di segmento** (**MSS**, Maximum Segment Size). L'MSS viene determinato a partire dall'**unità trasmissiva massima** (**MTU**, Maximum Transmission Unit), ossia il frame più grande trasmissibile dal livello di collegamento dell'host mittente, sottraendo la lunghezza delle intestazioni TCP e IP (tipicamente 40 byte complessivi). Su Ethernet, ad esempio, con $\text{MTU} = 1500\text{ byte}$ si ha tipicamente $\text{MSS} = 1500 - 20 - 20 = 1460\text{ byte}$. L'MSS rappresenta quindi il solo carico utile applicativo, escluse le intestazioni.
> 
> Ogni blocco di dati viene dotato di un'intestazione TCP formando un segmento, che il livello di rete incapsula separatamente in un datagramma IP. All'estremo ricevente, i dati dei segmenti ricevuti vengono depositati nel **buffer di ricezione**, da cui l'applicazione destinataria legge il flusso di byte. Ciascun lato della connessione mantiene un proprio buffer di invio e un proprio buffer di ricezione.
### Struttura dei segmenti TCP
> [!info] Schema di un segmento TCP
> ![[Pasted image 20261124144756.png]]

Come in UDP, l'intestazione TCP contiene i numeri di **porta di origine** e **porta di destinazione** (16 bit ciascuno) per il multiplexing/demultiplexing e il campo **Checksum** (16 bit). A questi si aggiungono:

- **Numero di sequenza e numero di acknowledgment (32 bit ciascuno):** utilizzati per implementare il trasferimento dati affidabile;
- **Finestra di ricezione (16 bit):** impiegata per il controllo di flusso, indica il numero di byte che il destinatario è disposto ad accettare;
- **Lunghezza dell'intestazione (4 bit):** esprime la dimensione dell'intestazione TCP in parole da 32 bit (4 byte). Poiché il valore massimo codificabile su 4 bit è 15, l'intestazione varia tra 20 byte (parte fissa, senza opzioni) e $15 \times 4 = 60\text{ byte}$ (con fino a 40 byte di opzioni);
- **Opzioni (facoltativo, fino a 40 byte):** consente di negoziare parametri come l'MSS, il fattore di scala della finestra nelle reti ad alta velocità e il timestamping;
- **Flag (8 bit):** controllano lo stato e la gestione della connessione:
	- `ACK`: indica che il valore nel campo numero di acknowledgment è valido;
	- `RST`, `SYN`, `FIN`: gestiscono l'apertura e la chiusura (o il reset) della connessione;
	- `CWR`, `ECE`: utilizzati nella notifica esplicita di congestione (ECN);
	- `PSH`: se posto a `1`, richiede al destinatario di consegnare immediatamente i dati all'applicazione;
	- `URG`: se posto a `1`, segnala la presenza di dati urgenti, la cui fine è indicata dal campo **Puntatore ai dati urgenti** (16 bit).

> [!important] Quanti sono i bit di flag?
> Nell'RFC 793 originale i flag erano 6 (`URG`, `ACK`, `PSH`, `RST`, `SYN`, `FIN`) affiancati da 6 bit riservati. Con l'introduzione dell'ECN (RFC 3168) sono stati aggiunti `CWR` ed `ECE` sottraendo 2 bit ai riservati: oggi i flag sono quindi **8** e i bit riservati sono **4**, come confermato dall'RFC 9293 (2022).
### Numeri di sequenza e numeri di acknowledgment
TCP tratta i dati come un **flusso di byte** non strutturato ma ordinato. Di conseguenza, i numeri di sequenza non numerano i segmenti, bensì i singoli byte: il **numero di sequenza** di un segmento è l'indice del primo byte del suo carico utile all'interno del flusso complessivo.
> [!example] Esempio di numeri di sequenza
> Un processo sull'Host A deve inviare all'Host B un file di 500.000 byte. Assumendo $\text{MSS} = 1000\text{ byte}$ (tale che la somma tra MSS e intestazioni TCP/IP non superi la MTU) e numerando il primo byte con 0, TCP suddivide il flusso in 500 segmenti: il primo trasporta i byte 0–999 con numero di sequenza 0, il secondo i byte 1000–1999 con numero di sequenza 1000, il terzo ha numero di sequenza 2000 e così via fino all'ultimo byte (499.999). Tali valori permettono al destinatario di riassemblare correttamente il flusso.
>
> ![[Pasted image 20261124160137.png]]

Il **numero di acknowledgment** riflette la natura full-duplex di TCP: il valore che l'Host A inserisce nel campo `ACK` dei segmenti diretti all'Host B indica il numero di sequenza del **prossimo byte atteso in ordine** da B (`NextByteExpected`).

Per esempio, se l'Host A ha ricevuto correttamente da B i byte da 0 a 535, inserirà il valore 536 nel campo `ACK` del segmento successivo inviato a B. Se invece l'Host A riceve i byte da 0 a 535 e poi i byte da 900 a 1000, mentre i byte intermedi da 536 a 899 sono ancora mancanti, A continuerà a inviare `ACK = 536`, poiché il byte 536 è il primo byte mancante per ricostruire il flusso senza lacune. Poiché TCP riscontra esclusivamente i byte ricevuti in modo contiguo fino al primo buco, implementa **acknowledgment cumulativi** (cumulative acknowledgments).

Le RFC di TCP non impongono un comportamento unico per la gestione dei segmenti arrivati fuori ordine, lasciando due opzioni progettuali:
1. scartare immediatamente i segmenti fuori sequenza (implementazione più semplice);
2. conservarli nel buffer di ricezione in attesa dei byte mancanti per colmare le lacune (soluzione più efficiente per la banda e adottata nella quasi totalità delle implementazioni reali).

Infine, nella realtà i due host non partono da 0 ma scelgono casualmente il proprio numero di sequenza iniziale (Initial Sequence Number, ISN). Ciò evita che vecchi segmenti rimasti in rete da una connessione precedente tra gli stessi host e le stesse porte vengano accettati erroneamente nella nuova sessione.
### Timeout e stima del tempo di andata e ritorno (RTT)
Per recuperare i segmenti smarriti, TCP combina timeout e ritrasmissioni. La durata dell'intervallo di timeout deve superare il tempo di andata e ritorno (**RTT**, Round-Trip Time)  comprensivo dei ritardi di elaborazione, accodamento, trasmissione e propagazione per evitare che il timer scada prima del normale arrivo degli `ACK`, generando ritrasmissioni inutili.
#### Stima del tempo di andata e ritorno (RTT)
Il tempo misurato sperimentalmente tra l'invio di un segmento (consegna al livello IP) e la ricezione del corrispondente `ACK` è chiamato $\text{SampleRTT}$. La maggior parte delle implementazioni cronometra un solo segmento in transito alla volta, raccogliendo circa un campione $\text{SampleRTT}$ per ogni RTT.

L'algoritmo di Karn stabilisce che **non si misura mai $\text{SampleRTT}$ sui segmenti ritrasmessi**: alla ricezione dell'`ACK`, infatti, il mittente non potrebbe distinguere se il riscontro si riferisca alla trasmissione originaria o alla ritrasmissione.

Poiché il traffico di rete fluttua continuamente, TCP mantiene una stima media dei campioni, detta $\text{EstimatedRTT}$, aggiornata a ogni nuova misura tramite una **media mobile esponenziale ponderata** (**EWMA**, Exponential Weighted Moving Average):

$\text{EstimatedRTT} = (1 - \alpha) \cdot \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT}$

L'RFC 6298 raccomanda $\alpha = 0{,}125$ ($1/8$), ottenendo:

$\text{EstimatedRTT} = 0{,}875 \cdot \text{EstimatedRTT} + 0{,}125 \cdot \text{SampleRTT}$

In questo modo i campioni più recenti ricevono un peso maggiore rispetto a quelli datati, il cui contributo decresce esponenzialmente nel tempo.
> [!example] Campioni e stima RTT
> ![[Pasted image 20261125154026.png]]
>
> In una connessione reale tra `gaia.cs.umass.edu` (Massachusetts) e `fantasia.eurecom.fr` (Francia) con $\alpha = 1/8$, la curva $\text{EstimatedRTT}$ smussa i picchi e le brusche oscillazioni dei singoli campioni $\text{SampleRTT}$.

Oltre al valore medio, l'RFC 6298 stima la variabilità dell'RTT tramite la variabile $\text{DevRTT}$, anch'essa una EWMA calcolata sullo scarto assoluto tra l'ultimo campione e la stima corrente (con parametro raccomandato $\beta = 0{,}25$):

$\text{DevRTT} = (1 - \beta) \cdot \text{DevRTT} + \beta \cdot \vert{}\text{SampleRTT} - \text{EstimatedRTT}\vert{}$

Quando la rete è stabile, $\text{DevRTT}$ rimane piccolo; in presenza di forti oscillazioni dei ritardi, $\text{DevRTT}$ cresce.
#### Impostazione e gestione del timeout di ritrasmissione
L'intervallo di timeout ($\text{TimeoutInterval}$) deve bilanciare due esigenze opposte: non essere inferiore a $\text{EstimatedRTT}$ (evitando ritrasmissioni premature) e non risultare eccessivamente lungo (evitando ritardi nel recupero delle perdite). TCP aggiunge dunque a $\text{EstimatedRTT}$ un margine di sicurezza proporzionale a $\text{DevRTT}$:

$\text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}$

L'RFC 6298 prescrive inoltre di inizializzare $\text{TimeoutInterval} = 1\text{ s}$ all'apertura della connessione (prima di disporre di misure di RTT) e di raddoppiarlo a ogni evento di timeout consecutivo, come illustrato più avanti.
### Trasferimento dati affidabile
Sul servizio best-effort e inaffidabile di IP, TCP costruisce un trasferimento dati affidabile che consegna al processo applicativo un flusso di byte privo di errori, lacune e duplicazioni, nell'ordine esatto di invio. Per contenere il sovraccarico computazionale, l'RFC 6298 prevede un **unico timer di ritrasmissione** per connessione, anche in presenza di più segmenti in transito ancora privi di `ACK`.

Per illustrare il meccanismo procederemo in due fasi: analizzeremo prima un modello semplificato basato sui soli timeout e poi il meccanismo completo con la ritrasmissione rapida. Si assume un trasferimento unidirezionale dall'Host A all'Host B di blocchi inferiori a MSS, trascurando temporaneamente controllo di flusso e controllo di congestione.
> [!example] Il mittente TCP semplificato
> Il mittente mantiene due variabili inizializzate a `InitialSeqNumber`: `NextSeqNum` (numero di sequenza del prossimo byte da trasmettere) e `SendBase` (numero di sequenza del byte più vecchio inviato ma non ancora confermato; $\text{SendBase} - 1$ è quindi l'ultimo byte certamente ricevuto in ordine dal destinatario).
>
> Un ciclo infinito gestisce tre eventi principali:
>
> 1. **Dati ricevuti dall'applicazione:** TCP crea un segmento con numero di sequenza `NextSeqNum`, avvia il timer (se non è già attivo, associandolo idealmente al segmento più vecchio senza `ACK` con scadenza pari a $\text{TimeoutInterval}$), passa il segmento a IP e aggiorna `NextSeqNum = NextSeqNum + lunghezza(dati)`.
> 2. **Timeout del timer:** TCP ritrasmette il segmento non ancora confermato con il numero di sequenza più basso e riavvia il timer.
> 3. **Ricezione di un `ACK` con valore $y$:** grazie agli `ACK` cumulativi, se $y > \text{SendBase}$, il mittente aggiorna `SendBase = y` e, se restano altri segmenti in transito privi di riscontro, riavvia il timer per il nuovo segmento più vecchio in sospeso.

La logica del mittente semplificato è riassunta dal seguente pseudocodice:
```text
NextSeqNum = InitialSeqNumber
SendBase = InitialSeqNumber

loop (per sempre) {
    switch (evento)
    evento: dati ricevuti dall'applicazione a livello superiore
        crea il segmento TCP con numero di sequenza NextSeqNum
        if (il timer attualmente non è in funzione)
            avvia il timer
        passa il segmento a IP
        NextSeqNum = NextSeqNum + lunghezza(dati)
        break;
    evento: timeout del timer
        ritrasmetti il segmento che non ha ricevuto ACK con il
            più piccolo numero di sequenza
        avvia il timer
        break;
    evento: ACK ricevuto, con valore del campo ACK pari a y
        if (y > SendBase) {
            SendBase = y
            if (esistono attualmente segmenti senza ACK)
                avvia il timer
        }
        break;
}
```

L'efficacia di questo meccanismo emerge chiaramente in tre scenari tipici tra Host A e Host B:
> [!example] Scenari di timeout e ACK cumulativi
> ![[Pasted image 20261125171656.png]]
>
> **Scenario 1 (perdita di un `ACK`):** l'Host A invia un segmento con `Seq = 92` e 8 byte di dati (byte 92–99). L'Host B lo riceve correttamente e risponde con `ACK = 100`, che però viene smarrito. Allo scadere del timer, A ritrasmette `Seq = 92`; B riconosce i byte come duplicati, li scarta e rinvia `ACK = 100`.
>
> ![[Pasted image 20261125171707.png]]
>
> **Scenario 2 (timeout prematuro):** l'Host A invia due segmenti consecutivi: `Seq = 92` (8 byte, 92–99) e `Seq = 100` (20 byte, 100–119). Entrambi arrivano a B, che spedisce `ACK = 100` e `ACK = 120`, ma nessuno dei due arriva ad A prima dello scadere del timer. A ritrasmette solo il segmento più vecchio (`Seq = 92`) e riavvia il timer; se `ACK = 120` arriva prima della nuova scadenza, il secondo segmento non viene ritrasmesso.
>
> ![[Pasted image 20261125171723.png]]
>
> **Scenario 3 (vantaggio degli `ACK` cumulativi):** l'Host A invia gli stessi due segmenti del caso precedente. `ACK = 100` va perduto, ma prima del timeout giunge `ACK = 120`. Poiché l'`ACK` è cumulativo, A deduce che B ha ricevuto tutti i byte fino al 119 e non ritrasmette alcun segmento.
#### Raddoppio dell'intervallo di timeout
Nelle implementazioni reali, a ogni evento di timeout TCP non ricalcola $\text{TimeoutInterval}$ dalle stime correnti di $\text{EstimatedRTT}$ e $\text{DevRTT}$, ma **raddoppia** il valore precedente ($\text{TimeoutInterval} = 2 \cdot \text{TimeoutInterval}$). Per esempio, se il timer scade a 0,75 s, viene reimpostato a 1,5 s e, in caso di ulteriore scadenza, a 3 s. Alla ricezione di nuovi dati dall'applicazione o di un `ACK` valido che aggiorni le stime dell'RTT, $\text{TimeoutInterval}$ torna a essere calcolato con la formula standard $\text{EstimatedRTT} + 4 \cdot \text{DevRTT}$.

Questa crescita esponenziale funge da prima forma di controllo della congestione: poiché un timeout segnala spesso l'intasamento dei router, diradare progressivamente le ritrasmissioni evita di aggravare la congestione (secondo un principio analogo al backoff esponenziale di Ethernet CSMA/CD).
#### Ritrasmissione rapida
Attendere la scadenza del timer per recuperare un segmento perduto può introdurre ritardi elevati. Il mittente può però accorgersi prima della perdita grazie agli **ACK duplicati**, cioè riscontri ripetuti relativi a un segmento già confermato.

Le regole di generazione degli `ACK` lato ricevente sono formalizzate nell'RFC 5681:
> [!example] Regole di generazione degli ACK lato ricevente (RFC 5681)
> ![[Pasted image 20261125171950.png]]

Assumendo che il destinatario conservi nel buffer i segmenti fuori ordine, quando arriva un segmento con numero di sequenza maggiore di quello atteso viene rilevata una lacuna nel flusso. Non disponendo di messaggi di riscontro negativo (`NAK`), il ricevente invia immediatamente un `ACK` duplicato per l'ultimo byte ricevuto in ordine.

Poiché il mittente trasmette spesso raffiche di segmenti consecutivi, la perdita di un segmento genera una sequenza di `ACK` duplicati. Quando il mittente riceve **tre `ACK` duplicati** per lo stesso valore (quattro `ACK` identici in totale, contando l'originale), interpreta l'evento come forte indicazione della perdita del segmento successivo ed esegue subito una **ritrasmissione rapida** (fast retransmit, RFC 5681) senza attendere il timeout.
> [!example] Ritrasmissione rapida dopo 3 ACK duplicati
> ![[Pasted image 20261125173104.png]]
>
> Come illustrato in figura, l'Host A trasmette in sequenza cinque segmenti: `Seq = 92` (8 byte), `Seq = 100` (20 byte), `Seq = 120` (15 byte), `Seq = 135` (6 byte) e `Seq = 141` (16 byte). Il primo arriva regolarmente e B risponde con l'`ACK` originale `ACK = 100`, mentre il secondo (`Seq = 100`) viene smarrito. All'arrivo dei tre segmenti successivi (`Seq = 120`, `135` e `141`), B rileva la lacuna dal byte 100 e invia immediatamente tre `ACK` duplicati con valore `ACK = 100`. Alla ricezione del terzo duplicato, A ritrasmette subito `Seq = 100` prima della scadenza del timer.

Integrando la ritrasmissione rapida nello pseudocodice del mittente, il ramo di ricezione dell'`ACK` diventa:
```text
evento: ACK ricevuto, con valore del campo ACK pari a y
    if (y > SendBase) {
        SendBase = y
        if (esistono attualmente segmenti che non hanno ricevuto un ACK)
            avvia il timer
    }
    else { /* ACK duplicato per un segmento già confermato */
        incrementa il numero di ACK duplicati ricevuti per y
        if (numero di ACK duplicati ricevuti per y == 3) {
            /* ritrasmissione rapida */
            rispedisci il segmento con numero di sequenza y
        }
    }
    break;
```
### Controllo di flusso
I byte ricevuti correttamente e in ordine da TCP vengono depositati nel buffer di ricezione, da cui il processo applicativo li legge con i propri tempi. Se l'applicazione legge lentamente mentre il mittente invia troppo velocemente, il buffer di ricezione può andare in overflow.

TCP previene questo problema offrendo un servizio di **controllo di flusso** (flow control), che adegua la velocità di invio del mittente alla velocità di lettura dell'applicazione destinataria. Questo meccanismo va distinto dal **controllo di congestione** (congestion control): entrambi rallentano il mittente, ma il primo protegge il buffer del singolo ricevente, mentre il secondo tutela la rete IP nel suo complesso.
> [!example] Esempio di controllo di flusso TCP
> ![[Pasted image 20261126123855.png]]
>
> L'Host A trasferisce un file di grandi dimensioni all'Host B, il quale alloca un buffer di ricezione di dimensione $\text{RcvBuffer}$ e mantiene due variabili: $\text{LastByteRead}$ (ultimo byte letto dall'applicazione) e $\text{LastByteRcvd}$ (ultimo byte ricevuto dalla rete e memorizzato nel buffer). Per evitare l'overflow deve sempre valere $\text{LastByteRcvd} - \text{LastByteRead} \le \text{RcvBuffer}$.
>
> Lo spazio libero residuo nel buffer costituisce la **finestra di ricezione** $\text{rwnd}$ (receive window):
>
> $\text{rwnd} = \text{RcvBuffer} - (\text{LastByteRcvd} - \text{LastByteRead})$
>
> Poiché arrivi dalla rete e letture dell'applicazione cambiano continuamente, $\text{rwnd}$ è dinamica. L'Host B inizializza $\text{rwnd} = \text{RcvBuffer}$ e comunica periodicamente il valore aggiornato all'Host A nell'intestazione dei segmenti di ritorno. Sul lato mittente, l'Host A tiene traccia di $\text{LastByteSent}$ (ultimo byte inviato) e $\text{LastByteAcked}$ (ultimo byte riscontrato) e vincola i dati in transito senza `ACK` alla condizione:
>
> $\text{LastByteSent} - \text{LastByteAcked} \le \text{rwnd}$
>
> **Caso limite ($\text{rwnd} = 0$):** se il buffer di B si riempie del tutto ($\text{rwnd} = 0$) e B non ha dati applicativi da inviare ad A, quando il processo su B riprende a leggere liberando spazio il livello TCP di B non spedirebbe spontaneamente alcun segmento di aggiornamento (TCP trasmette solo se ha dati o un `ACK` da inviare), lasciando A bloccato. Per evitare questo stallo, le specifiche TCP impongono che, quando $\text{rwnd} = 0$, il mittente A continui a inviare periodicamente segmenti sonda contenenti 1 solo byte di dati: l'Host B risponde con un `ACK` che, non appena il buffer si svuota, notificherà un valore di $\text{rwnd} > 0$, consentendo la ripresa del trasferimento.
### Gestione della connessione TCP
Le procedure di apertura e chiusura di una connessione TCP sono rilevanti per due motivi:
- l'apertura della connessione introduce ritardi tangibili per l'utente (ad esempio nella navigazione web);
- la gestione della connessione è bersaglio di diffusi attacchi informatici, come il **SYN flooding**, un attacco Denial of Service (DoS) in cui l'aggressore invia moltissimi segmenti `SYN` senza completare l'handshake, esaurendo le risorse del server destinate alle connessioni semi-aperte.
#### Instaurazione di una connessione TCP
> [!example] Esempio di instaurazione di una connessione TCP (handshake a tre vie)
> ![[Pasted image 20261126144531.png]]
>
> Quando un processo client intende connettersi a un processo server, TCP esegue tre passaggi sequenziali:
>
> 1. **Segmento `SYN`:** il client invia un segmento privo di dati applicativi con flag `SYN = 1` e un numero di sequenza iniziale scelto casualmente (`client_isn`), misura essenziale anche per prevenire determinati attacchi (CERT 2001-09, RFC 4987).
> 2. **Segmento `SYNACK`:** ricevuto il `SYN`, il server alloca i buffer e le variabili della connessione e risponde con un segmento privo di dati applicativi avente flag `SYN = 1` e `ACK = 1`, numero di acknowledgment `ack = client_isn + 1` e un proprio numero di sequenza iniziale casuale (`server_isn`).
> 3. **Segmento `ACK` finale:** alla ricezione del `SYNACK`, il client alloca i propri buffer e le variabili di stato, quindi invia un segmento con `SYN = 0`, `ACK = 1`, `seq = client_isn + 1` e `ack = server_isn + 1`. Questo terzo segmento può già trasportare dati applicativi nel carico utile.

Completati i tre passaggi, definiti **handshake a tre vie** (three-way handshake), i due host possono scambiarsi segmenti dati con `SYN = 0`.
#### Chiusura di una connessione TCP
> [!example] Esempio di chiusura di una connessione TCP
> ![[Pasted image 20261126150034.png]]
>
> Ciascuno dei due processi può avviare la terminazione della connessione per rilasciare buffer e variabili di stato. Supponendo che l'iniziativa parta dal client, quest'ultimo invia un segmento con flag `FIN = 1`. Il server risponde con un segmento `ACK` e, quando è pronto a chiudere il proprio lato, invia a sua volta un segmento con `FIN = 1`. Il client replica con l'`ACK` finale: il server rilascia subito le proprie risorse alla ricezione di tale `ACK`, mentre il client dealloca le proprie al termine di una breve attesa temporizzata (`TIME_WAIT`).

#### Ciclo di vita e stati TCP
Nel corso della connessione, client e server attraversano una sequenza definita di **stati TCP** (TCP states) in risposta ai comandi applicativi e ai segmenti scambiati.
> [!info] Tipica sequenza di stati visitati da un client TCP
> ![[Pasted image 20261126150440.png]]
>
> Partendo da `CLOSED`, quando l'applicazione avvia la connessione (es. creando un oggetto `Socket`), il client invia `SYN` ed entra in `SYN_SENT`. Alla ricezione del `SYNACK` risponde con `ACK` e passa in `ESTABLISHED`, stato in cui avviene lo scambio dati.
>
> Quando il client decide di chiudere la connessione, invia `FIN = 1` ed entra in `FIN_WAIT_1`; ricevuto l'`ACK` del server transita in `FIN_WAIT_2` in attesa del `FIN` del server. Alla ricezione di quest'ultimo invia l'`ACK` finale ed entra in `TIME_WAIT`.
>
> Lo stato `TIME_WAIT` (di durata tipica pari a 30 secondi, 1 minuto o 2 minuti a seconda dell'implementazione) consente al client di ritrasmettere l'ultimo `ACK` qualora vada smarrito in rete. Scaduto tale intervallo, il client torna in `CLOSED` e rilascia tutte le risorse, inclusa la porta occupata.

> [!info] Tipica sequenza di stati visitati da un server TCP
> ![[Pasted image 20261126150742.png]]
>
> Da `CLOSED`, il server apre una socket in ascolto entrando in `LISTEN`. Alla ricezione di un `SYN`, risponde con `SYNACK` e passa in `SYN_RCVD`; ricevuto l'`ACK` del client entra in `ESTABLISHED`.
>
> Quando il client avvia la chiusura, il server riceve `FIN`, risponde con `ACK` e transita in `CLOSE_WAIT`. Non appena anche il lato server è pronto a terminare, invia il proprio `FIN` spostandosi in `LAST_ACK`; alla ricezione dell'ultimo `ACK` dal client torna direttamente in `CLOSED`.
### Principi di congestione di rete
La **congestione di rete** si verifica quando troppe sorgenti immettono traffico a una velocità superiore alla capacità di smaltimento dei router lungo il percorso. I pacchetti accumulano lunghi ritardi di accodamento finché i buffer finiti dei router vanno in overflow e scartano i pacchetti in eccesso. Le ritrasmissioni eseguite dai mittenti per recuperare le perdite (aggravate da eventuali timeout prematuri dovuti agli alti ritardi) aumentano ulteriormente il carico, esponendo la rete al **collasso da congestione**. A ciò si aggiunge uno spreco sistematico: ogni pacchetto scartato da un router a valle vanifica tutta la capacità trasmissiva già spesa dai router a monte per inoltrarlo fin lì.

Poiché la semplice ritrasmissione è solo un rimedio sintomatico, è necessario un controllo della congestione che regoli la velocità dei mittenti. In base al ruolo del livello di rete si distinguono due approcci:
- **Controllo di congestione end-to-end:** adottato dal TCP classico, in cui IP non fornisce segnalazioni esplicite e gli host deducono la congestione osservando perdite di pacchetti o aumenti dell'RTT;
- **Controllo di congestione assistito dalla rete:** i router segnalano esplicitamente la congestione inviando un avviso diretto al mittente (choke packet) oppure marcando un campo nei pacchetti diretti al destinatario, affinché questo avvisi il mittente (con un ritardo di almeno un RTT), principio alla base della notifica esplicita ECN.
### Controllo di congestione TCP
Oltre al trasferimento affidabile e al controllo di flusso, TCP integra il controllo della congestione per impedire che le connessioni saturino la rete. Il **TCP classico** (RFC 2581 e RFC 5681) adotta un approccio end-to-end.
#### Controllo di congestione TCP classico
Il mittente TCP regola dinamicamente il proprio tasso di invio in base alla congestione percepita, rispondendo a tre esigenze: come limitare la velocità, come rilevare la congestione e con quale algoritmo variare il ritmo di invio.

Per limitare i dati immessi in rete, il mittente affianca alla finestra di ricezione $\text{rwnd}$ una seconda variabile di stato, la **finestra di congestione** $\text{cwnd}$ (congestion window). La quantità di byte inviati e non ancora confermati deve rispettare il vincolo $\text{LastByteSent} - \text{LastByteAcked} \le \min\{\text{cwnd}, \text{rwnd}\}$, determinando la finestra effettiva di invio:

$\text{EffectiveSendWindow} = \min\{\text{cwnd}, \text{rwnd}\} - (\text{LastByteSent} - \text{LastByteAcked})$

Trascurando il vincolo di $\text{rwnd}$ e i tempi di trasmissione rispetto all'RTT, il mittente invia circa $\text{cwnd}$ byte per ogni RTT, realizzando una velocità trasmissiva media approssimativamente pari a $\text{cwnd} / \text{RTT}\text{ byte/s}$.

In assenza di segnali diretti dai router, TCP interpreta un **evento di perdita** (loss event), cioè un **timeout** oppure **tre `ACK` duplicati**, come sintomo di congestione. Viceversa, la ricezione di nuovi `ACK` non duplicati conferma la buona consegna dei segmenti e autorizza ad ampliare $\text{cwnd}$. Poiché la velocità di crescita di $\text{cwnd}$ è scandita dal ritmo di arrivo degli `ACK`, TCP è definito **auto-temporizzato** (self-clocking).

Ogni mittente opera in modo distribuito e asincrono seguendo tre principi:

1. un segmento perduto indica congestione e impone di ridurre $\text{cwnd}$;
2. un `ACK` non duplicato indica assenza di congestione e consente di aumentare $\text{cwnd}$;
3. il protocollo effettua un continuo **sondaggio della banda** (bandwidth probing), incrementando la velocità finché non si verifica una perdita, rallentando e ricominciando poi a crescere.

L'algoritmo definito in RFC 5681 si articola in tre fasi: **slow start** e **congestion avoidance** (obbligatorie) e **fast recovery** (raccomandata, formalmente opzionale).
##### Slow start
All'apertura della connessione TCP imposta $\text{cwnd} = 1\text{ MSS}$. Con $\text{MSS} = 500\text{ byte}$ (4000 bit) e $\text{RTT} = 200\text{ ms}$ ($0{,}2\text{ s}$), la velocità iniziale è di appena $4000\text{ bit} / 0{,}2\text{ s} = 20\text{ kbit/s}$. Per raggiungere rapidamente la banda disponibile, durante lo slow start il mittente incrementa $\text{cwnd}$ di $1\text{ MSS}$ per ogni nuovo `ACK` ricevuto: si passa così da $1\text{ MSS}$ (20 kbit/s) a $2\text{ MSS}$ (40 kbit/s), poi $4\text{ MSS}$ (80 kbit/s) e così via. Nonostante il nome, la crescita è **esponenziale** ($\text{cwnd}$ raddoppia a ogni RTT).

> [!info] Slow start di TCP
> ![[Pasted image 20261126170553.png]]

La fase di slow start termina al verificarsi di una di tre condizioni:
1. **Timeout:** TCP imposta la soglia di slow start alla metà della finestra corrente ($\text{ssthresh} = \text{cwnd} / 2$, inizialmente posta a un valore elevato), reimposta $\text{cwnd} = 1\text{ MSS}$ e riparte dallo slow start;
2. **Raggiungimento della soglia ($\text{cwnd} \ge \text{ssthresh}$):** poiché raddoppiare ancora la finestra in prossimità del livello che ha provocato l'ultima congestione sarebbe rischioso, TCP entra in congestion avoidance;
3. **Tre `ACK` duplicati:** TCP esegue il fast retransmit del segmento mancante e passa in fast recovery.
##### Congestion avoidance
All'ingresso in **congestion avoidance**, $\text{cwnd}$ si trova circa a metà del valore che aveva generato l'ultima congestione. TCP passa quindi a una **crescita lineare** prudente, aumentando $\text{cwnd}$ di $1\text{ MSS}$ per ogni RTT: alla ricezione di ciascun nuovo `ACK`, la finestra viene incrementata di $\text{MSS} \cdot (\text{MSS} / \text{cwnd})$ byte. Per esempio, con $\text{MSS} = 1460\text{ byte}$ e $\text{cwnd} = 14600\text{ byte}$ (10 segmenti per RTT), ogni `ACK` aggiunge $1/10\text{ MSS}$, producendo esattamente $+1\text{ MSS}$ al termine dell'RTT.

> [!info] Automa a stati finiti del controllo di congestione TCP
> ![[Pasted image 20261126171135.png]]

La reazione a una perdita in congestion avoidance dipende dal tipo di segnalazione:
- **Timeout (congestione grave):** TCP imposta $\text{ssthresh} = \text{cwnd} / 2$, riporta $\text{cwnd} = 1\text{ MSS}$ e rientra in slow start;
- **Tre `ACK` duplicati (perdita isolata, poiché altri segmenti continuano ad arrivare):** TCP reagisce in modo più moderato impostando $\text{ssthresh} = \text{cwnd} / 2$ e $\text{cwnd} = \text{ssthresh} + 3\text{ MSS}$ (conteggiando i tre segmenti già consegnati), esegue il fast retransmit ed entra in fast recovery.
##### Fast recovery e varianti di TCP: Tahoe, Reno e NewReno
In **fast recovery**, per ogni ulteriore `ACK` duplicato relativo al segmento smarrito TCP incrementa $\text{cwnd}$ di $1\text{ MSS}$, poiché un altro segmento ha raggiunto il ricevente. All'arrivo di un nuovo `ACK` che riscontra il segmento ritrasmesso, TCP imposta $\text{cwnd} = \text{ssthresh}$, azzera il contatore dei duplicati e riprende la crescita lineare in congestion avoidance. Se invece scade un timeout durante il fast recovery, imposta $\text{ssthresh} = \text{cwnd} / 2$, riporta $\text{cwnd} = 1\text{ MSS}$ e torna in slow start.

La presenza o meno del fast recovery distingue due versioni storiche di TCP:
- **TCP Tahoe:** privo di fast recovery, reagisce sia ai timeout sia a tre `ACK` duplicati riportando sempre $\text{cwnd} = 1\text{ MSS}$ e ripartendo in slow start;
- **TCP Reno:** implementa il fast recovery.
> [!info] Evoluzione della finestra di congestione TCP (Tahoe vs Reno)
> ![[Pasted image 20261126171928.png]]
>
> Con soglia iniziale $\text{ssthresh} = 8\text{ MSS}$, nei primi otto turni Tahoe e Reno evolvono allo stesso modo: crescono esponenzialmente in slow start fino al turno 4 ($1, 2, 4, 8\text{ MSS}$) e poi linearmente in congestion avoidance fino al turno 8 ($\text{cwnd} = 12\text{ MSS}$). Qui una perdita rilevata tramite tre `ACK` duplicati porta entrambi a fissare $\text{ssthresh} = 12 / 2 = 6\text{ MSS}$. Al turno 9, però, TCP Tahoe crolla a $\text{cwnd} = 1\text{ MSS}$ e risale in slow start fino al turno 12; TCP Reno entra invece in fast recovery con $\text{cwnd} = \text{ssthresh} + 3\text{ MSS} = 9\text{ MSS}$ e rientra rapidamente in congestion avoidance senza ripartire da $1\text{ MSS}$.

**TCP NewReno** (RFC 2582, aggiornato in RFC 6582) risolve un limite di TCP Reno in caso di **perdite multiple nella stessa finestra**. Si supponga l'invio dei segmenti da 1 a 10 con perdita simultanea dei segmenti 3 e 6:
- in **TCP Reno**, tre `ACK` duplicati attivano fast retransmit e fast recovery per il segmento 3; quando arriva l'`ACK` che riscontra 3, 4 e 5 e richiede il segmento 6, Reno lo considera un nuovo `ACK` ed esce subito dal fast recovery. Non avendo abbastanza segmenti successivi per generare tre nuovi `ACK` duplicati, recupererà il segmento 6 solo dopo un costoso timeout (crollando a $1\text{ MSS}$);
- in **TCP NewReno**, all'ingresso in fast recovery si memorizza l'ultimo segmento già spedito nella variabile `recover` (nell'esempio, il segmento 10). Il mittente rimane in fast recovery finché non vengono riscontrati tutti i segmenti fino a `recover`: l'`ACK` che richiede il segmento 6 viene riconosciuto come **ACK parziale** (partial ACK), inducendo la ritrasmissione immediata del segmento 6 senza uscire dal fast recovery. Soltanto all'arrivo dell'`ACK` completo che richiede il segmento 11 (confermando la ricezione fino al segmento 10), TCP NewReno esce dal fast recovery, imposta $\text{cwnd} = \text{ssthresh}$ e torna in congestion avoidance, recuperando ogni perdita al costo di un solo RTT.

A regime (con perdite rilevate tramite triplo `ACK` duplicato), TCP classico alterna un incremento additivo lineare di $+1\text{ MSS}$ per RTT a un decremento moltiplicativo che dimezza $\text{cwnd}$. Tale dinamica è detta **AIMD** (Additive-Increase, Multiplicative-Decrease) e produce il caratteristico andamento a dente di sega (sawtooth).
> [!info] Controllo di congestione a incremento additivo e decremento moltiplicativo (AIMD)
> ![[Pasted image 20261126172855.png]]
##### TCP CUBIC
Dimezzare $\text{cwnd}$ a ogni perdita e risalire lentamente di $1\text{ MSS}$ per RTT può risultare troppo conservativo, specialmente su reti ad alta velocità e grande RTT. **TCP CUBIC** (RFC 8312, aggiornato in RFC 9438), oggi predefinito in gran parte dei sistemi operativi moderni, lascia sostanzialmente invariati slow start e fast recovery, ma modifica la congestion avoidance e riduce la finestra in modo meno drastico alla perdita (tipicamente a $0{,}7 \cdot \text{cwnd}$).

Indicando con $W_{\max}$ il valore di $\text{cwnd}$ all'ultima perdita e con $K$ l'istante futuro in cui la finestra di CUBIC tornerebbe a eguagliare $W_{\max}$ in assenza di nuove perdite, $\text{cwnd}$ cresce come funzione **cubica** della distanza temporale tra l'istante corrente $t$ e $K$, generando tre regimi:

1. **Subito dopo la perdita ($t \ll K$):** incrementi rapidi che riavvicinano velocemente il tasso di invio a $W_{\max}$;
2. **In prossimità di $K$ ($t \approx K$):** la curva si appiattisce, sondando con cautela la banda vicino alla precedente soglia di saturazione;
3. **Oltre $K$ ($t > K$):** se non avvengono perdite (la congestione si è allentata), la crescita torna rapidamente cubica per individuare una nuova soglia superiore.

In questo modo TCP CUBIC rimane più a lungo vicino alla massima capacità sostenibile del collegamento, ottenendo un throughput medio superiore rispetto a TCP Reno.
> [!info] Velocità di invio in congestion avoidance: TCP CUBIC vs TCP Reno
> ![[Pasted image 20261126174749.png]]
### Notifica esplicita di congestione (ECN) e controllo basato sul ritardo
La **notifica esplicita di congestione** (**ECN**, Explicit Congestion Notification, RFC 3168) implementa il controllo di congestione assistito dalla rete tramite la cooperazione tra IP e TCP, consentendo di rilevare la congestione prima che i buffer vadano in overflow.

Il meccanismo sfrutta due bit nel campo Type of Service (ToS) dell'intestazione IP e due flag nell'intestazione TCP, articolandosi in quattro passi:
1. l'Host A dichiara il supporto a ECN impostando i due bit ECN dell'intestazione IP a `10` o `01`;
2. un router intermedio che rileva congestione incipiente, anziché scartare il pacchetto, marca i due bit IP al valore `11` (Congestion Experienced, CE) e lo inoltra;
3. l'Host B, ricevuto un datagramma con `ECN = 11`, avvisa il mittente impostando il flag TCP `ECE = 1` (ECN-Echo) nei segmenti `ACK` di ritorno;
4. l'Host A, ricevuto `ECE = 1`, dimezza preventivamente $\text{cwnd}$ (come per tre `ACK` duplicati) e imposta `CWR = 1` (Congestion Window Reduced) nel segmento successivo affinché B disattivi il flag `ECE`.
> [!info] Schema di controllo di congestione assistito dalla rete (ECN)
> ![[Pasted image 20261126181000.png]]

Un approccio alternativo è il **controllo di congestione basato sul ritardo** (delay-based), impiegato in protocolli come **TCP Vegas** e nel più recente **BBR**: il mittente monitora l'andamento dell'RTT e, rilevando un aumento del ritardo causato dal riempimento delle code nei router, riduce anticipatamente la velocità di invio prima che si verifichino perdite di pacchetti.
#### Fairness
La **fairness** (equità) misura la capacità dei protocolli di trasporto di ripartire equamente la banda di un collegamento condiviso: se $K$ connessioni TCP (con uguale RTT) condividono un collo di bottiglia di capacità $R$, l'algoritmo AIMD tende a far convergere ciascuna connessione verso una quota media pari a $R / K$.

In pratica, questa equità è compromessa da due fattori principali:
- **Convivenza tra TCP e UDP:** UDP non implementa alcun controllo di congestione. Le applicazioni multimediali su UDP (come telefonia IP o videoconferenza) mantengono costante il proprio ritmo di invio anche sotto congestione, aggravando l'intasamento e costringendo il traffico TCP concorrente a ridurre drasticamente la propria velocità. Per questo UDP è considerato unfair (sleale) verso TCP;
- **Connessioni TCP parallele:** poiché il controllo di congestione assegna una quota equa a ogni singola connessione e non a ogni applicazione, un programma (ad esempio un browser web) può aprire più connessioni TCP parallele verso la stessa sorgente per appropriarsi di una frazione di banda complessiva molto maggiore rispetto ad applicazioni che usano una sola connessione;
## Evoluzione della funzionalità del livello di trasporto
Quando né il servizio minimale di UDP né i vincoli di TCP soddisfano appieno le esigenze di un'applicazione, gli sviluppatori possono implementare le funzioni di trasporto direttamente al livello di applicazione. È questo il principio alla base di **QUIC** (Quick UDP Internet Connections, RFC 9000).

Architetturalmente, QUIC opera al **livello di applicazione** (nello spazio utente, user space, anziché nel kernel del sistema operativo) appoggiandosi su **UDP**. Al di sopra di UDP, QUIC reimplementa e ottimizza gestione della connessione, controllo di flusso, controllo di congestione e trasferimento affidabile.
> [!info] Stack di protocolli tradizionali (HTTPS su HTTP/2) e HTTP/3 su QUIC
> ![[Pasted image 20261126181426.png]]
>
> Nello stack tradizionale di HTTP sicuro si impilano IP, TCP, TLS e HTTP/2. Nella nuova architettura, invece, al livello di trasporto risiede UDP, mentre al livello di applicazione QUIC si interfaccia con una versione evoluta di HTTP/2, standardizzata dall'IETF nel giugno 2022 come **HTTP/3 (RFC 9114)**. QUIC funge così da sintesi moderna dei principi di affidabilità, gestione della connessione e controllo di congestione studiati per TCP.

Le caratteristiche distintive di QUIC sono:
- **Orientato alla connessione e sicuro (1 RTT):** come TCP, QUIC stabilisce uno stato di connessione tra i due endpoint (identificato da un `connection ID`), ma integra nativamente e obbligatoriamente la cifratura tramite **TLS 1.3**. Mentre lo stack classico richiede più RTT sequenziali per completare prima l'handshake TCP e poi quello crittografico TLS, QUIC fonde i due processi in un unico scambio, instaurando una connessione autenticata e cifrata in un solo RTT.
- **Flussi indipendenti (streams):** QUIC supporta il multiplexing di più flussi bidirezionali, affidabili e ordinati all'interno di una singola connessione (in HTTP/3 ogni oggetto web utilizza un proprio flusso). Ogni pacchetto QUIC trasporta nell'intestazione `connection ID` e `stream ID`, consentendo anche di aggregare dati di più flussi in un singolo pacchetto incapsulato in UDP.
- **Affidabilità per flusso, controllo di congestione e soluzione all'HOL blocking:** una motivazione decisiva nella progettazione di QUIC è l'eliminazione dell'**HOL blocking** (Head-of-Line blocking) a livello di trasporto. Nel modello tradizionale con **HTTP/2** (o HTTP/1.1 in pipelining) su un'unica connessione TCP+TLS, TCP garantisce l'ordinamento sull'intero flusso di byte della connessione: se i byte di un oggetto vengono smarriti, tutti gli altri flussi restano bloccati fino alla ritrasmissione dei byte mancanti (si veda la figura (a)). QUIC risolve il problema gestendo affidabilità (QUIC RDT), controllo di flusso e cifratura **separatamente per ciascun flusso**, pur mantenendo un unico controllo di congestione a livello di connessione (basato su meccanismi di `ACK`/ritrasmissione analoghi a RFC 5681 e su **TCP NewReno**, RFC 6582). In caso di perdita di un pacchetto UDP, viene rallentato solo il flusso interessato, mentre gli altri continuano a consegnare i dati all'applicazione senza blocchi (si veda la figura (b)).

![[Pasted image 20261126182856.png]]