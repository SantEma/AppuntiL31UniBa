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
### UDP
UDP fa praticamente il minimo che un protocollo di trasporto debba fare. A parte la funzione di multiplexing/demultiplexing e una forma di controllo degli errori molto semplice, non aggiunge nulla a IP.

Come abbiamo visto prima, UDP prende i messaggi dal processo applicativo, aggiunge il numero di porta di origine e di destinazione per il multiplexing/demultiplexing, aggiunge altri due piccoli campi e passa il segmento risultante al livello di rete. Questi incapsula il segmento in un datagramma IP e quindi effettua un tentativo di consegnarlo all’host di destinazione in modalità best-effort. Se il segmento arriva a destinazione, UDP utilizza il numero di porta di destinazione per consegnare i dati del segmento al processo applicativo corretto. 

In UDP non esiste uno scambio di informazioni preliminare (vedremo che si chiamerà handshake) tra le entità di invio e di ricezione a livello di trasporto. Per questo motivo, si dice che UDP è **non orientato alla connessione** (connectionless).

UDP ha diversi vantaggi rispetto a TCP:
- **Controllo più preciso a livello di applicazione su quali dati sono inviati e quando**: non appena un processo applicativo passa dei dati a UDP, quest’ultimo li impacchetta in un segmento che trasferisce immediatamente al livello di rete. TCP, come vedremo, dispone di un meccanismo di controllo della congestione che ritarda l’invio a livello di trasporto quando uno o più collegamenti tra l’origine e la destinazione diventano eccessivamente congestionati. TCP continua, inoltre, a inviare il segmento fino a quando viene notificata la sua ricezione da parte della destinazione, incurante del tempo richiesto per un trasporto affidabile. Dato che le applicazioni in tempo reale spesso richiedono una velocità minima di invio e non sopportano ritardi eccessivi nella trasmissione dei pacchetti mentre tollerano una certa perdita di dati, il modello di servizio TCP non si adatta particolarmente bene a queste esigenze.
- **Nessuna connessione stabilita**: come vedremo più avanti, TCP utilizza un handshake a tre vie prima di iniziare il trasferimento dei dati. UDP invece “spara” dati a raffica senza alcun preliminare formale. Pertanto, UDP non introduce alcun ritardo nello stabilire una connessione.
- **Nessuno stato di connessione**: TCP mantiene lo stato della connessione nei sistemi periferici, incluso buffer di ricezione e di invio, parametri per il controllo di congestione e parametri sul numero di sequenza e di acknowledgment (notifica, riscontro).  UDP, invece, non conserva lo stato della connessione e non tiene traccia di questi parametri. Per questo motivo, un server dedicato a una particolare applicazione può generalmente supportare molti più client attivi quando l’applicazione utilizza UDP anziché TCP.
- **Minor spazio usato per l’intestazione del pacchetto**: l'intestazione dei pacchetti TCP aggiunge 20 byte, mentre UDP solo 8.
#### Struttura dei segmenti UDP

> [!info] Componenti di un segmento UDP
> ![[Pasted image 20261104171258.png]]
> In ordine, descriviamo cosa fa ogni campo:
> - **Numero di porta di origine e destinazione**: sono dedicati all'indirizzamento dei processi mediante l'indicazione dei numeri di porta. Il campo del numero di porta di origine e il campo del numero di porta di destinazione occupano ciascuno 16 bit, permettendo di spaziare all'interno dell'intervallo di valori compreso tra 0 e 65535. La porta di destinazione è indispensabile all'host ricevente per effettuare il demultiplexing del segmento, consentendo di instradare il carico utile verso la corretta socket e il relativo processo applicativo. Il numero di porta di origine viene invece utilizzato dall'host mittente come parte dell'indirizzo di ritorno, fornendo al destinatario l'informazione necessaria per instradare le eventuali risposte applicative nella direzione opposta.
> - **Lunghezza**: Rappresenta la lunghezza del segmento, anch'esso codificato su 16 bit. Questo valore specifica la dimensione complessiva del segmento UDP espressa in byte, includendo sia la somma degli 8 byte di intestazione sia la lunghezza del campo dati. Poiché l'intestazione fissa occupa 8 byte, il valore minimo teorico assumibile da questo campo è pari a 8, corrispondente a un segmento privo di carico utile applicativo.
> - **Checksum**: L'ultimo campo dell'intestazione, della dimensione di 16 bit, è destinato al checksum di Internet per la rilevazione degli errori sui bit. Tale valore consente all'host destinatario di verificare se i bit che compongono il segmento abbiano subito alterazioni o corruzioni durante il trasferimento lungo il percorso di rete o durante la permanenza nei buffer dei router intermedi.
#### Checksum UDP
Il checksum di UDP rappresenta un meccanismo fondamentale per il rilevamento degli errori sui bit, progettato per verificare se i dati contenuti all'interno del segmento abbiano subito alterazioni durante il trasferimento dalla sorgente alla destinazione (poichè non esiste garanzia che tra tutti i collegamenti, da origine a destinazione, controllino gli errori).

Lato mittente, il calcolo del checksum si basa sull'aritmetica del complemento a 1 applicata a sequenze di dati trattate come parole a 16 bit nel segmento.

> [!example] Esempio di checksum UDP
> 
Come esempio supponiamo di avere le seguenti tre parole di 16 bit: $$00110011001100000 \\ 0101010101010101 \\ 1000111100001100$$ 
La somma delle prime due è: $$0110011001100000 \\ \underline{0101010101010101} \\ 1011101110110101$$ 
Sommando la terza parola al risultato precedente otteniamo: $$1011101110110101 \\ \underline{1000111100001100} \\ 0100101011000010$$
Notiamo che il riporto di quest’ultima somma è stato sommato al primo bit. Il complemento a 1 si ottiene convertendo i bit 0 in 1 e viceversa. Di conseguenza, il checksum sarà $1011010100111101$. In ricezione, si sommano le tre parole iniziali e il checksum. Se non ci sono errori nel pacchetto, l’addizione darà $1111111111111111$
### Principi del trasferimento dati affidabile

> [!important] Guardare libro
> Tutto questo sottocapitolo è utile ai fini di comprensione, ma non importante ai fini dell'esame vero e proprio. Lo trovate nel libro Reti di calcolatori e internet: Un approccio top-down. nei capitolo 3.4.1, 3.4.2, 3.4.3 e 3.4.4.

Per comprendere a fondo comunque il TCP, è necessario chiarire brevemente in che modo un protocollo di trasporto riesca a recuperare i pacchetti che vanno persi lungo la rete. Poiché il livello di rete sottostante (IP) non garantisce che i pacchetti arrivino a destinazione, il mittente ha bisogno di una conferma esplicita, detta **acknowledgment** (ACK), che il destinatario invia indietro per notificare la corretta ricezione dei dati. Tuttavia, se un segmento dati o il relativo ACK vengono smarriti, il mittente rischierebbe di attendere all'infinito.

Per risolvere questo problema, i protocolli di trasferimento affidabile (come lo stesso TCP) impiegano un meccanismo basato su un contatore a scadenza (countdown timer). Quando il mittente trasmette un segmento, avvia un timer impostato su un determinato intervallo di tempo, chiamato **timeout**. Se il riscontro ACK arriva prima dello scadere del tempo, la trasmissione è considerata riuscita; in caso contrario, allo scadere del timer si verifica un evento di **timeout** e il mittente ritrasmette il segmento. In linea teorica, per essere certi al cento per cento che un pacchetto sia andato perso, bisognerebbe attendere il ritardo massimo possibile nel caso peggiore, ma nelle reti reali questo valore è quasi impossibile da stimare e comporterebbe attese troppo lunghe prima di rimediare a una perdita. Nella pratica, quindi, si sceglie un intervallo di tempo ragionevole trascorso il quale la perdita è giudicata probabile, anche se non sicura. Ciò comporta che un segmento semplicemente molto ritardato possa far scattare un **timeout prematuro**, inducendo il mittente a spedire una seconda copia del segmento; i numeri di sequenza permettono comunque al destinatario di riconoscere e scartare eventuali pacchetti duplicati.

### TCP (trasporto orientato alla connessione)
TCP viene detto **orientato alla connessione** (connection-oriented) in quanto, prima di effettuare lo scambio dei dati, i processi devono effettuare l’handshake, ossia devono inviarsi reciprocamente alcuni segmenti preliminari per stabilire i parametri del successivo trasferimento dati.

La “connessione” TCP non è un circuito end-to-end TDM o FDM, come in una rete a commutazione di circuito, in quanto lo stato della connessione risiede completamente nei due sistemi periferici. Dato che il protocollo TCP va in esecuzione solo sui sistemi periferici e non negli elementi di rete intermedi (router e switch a livello di collegamento), questi ultimi non salvano lo stato della connessione TCP, avendo accesso unicamente a datagrammi.

Una connessione TCP offre un servizio **full-duplex**: su una connessione TCP tra il processo A su un host e il processo B su un altro host, i dati a livello di applicazione possono fluire dal processo A al processo B nello stesso momento in cui fluiscono in direzione opposta. Una connessione TCP è anche **punto a punto** (point-to-point), ossia ha luogo tra un singolo mittente e un singolo destinatario. Il cosiddetto “multicast”, ossia il trasferimento di dati da un mittente a molti destinatari in un’unica operazione, con TCP non è possibile.

Come vedremo più avanti, un client e un server che comunicano via TCP instaurano la loro connessione tramite un **handshake a tre vie**, finita la quale i due applicativi permettono lo scambio di dati reciproco.

> [!example] Sguardo veloce all'handshake a 3 vie
> ![[Pasted image 20261124143402.png]]
> TCP dirige i dati al **buffer di invio** della connessione, uno dei buffer riservato durante l’handshake a tre vie, da cui, di tanto in tanto, preleverà blocchi di dati e li passerà al livello di rete.
> 

La massima quantità di dati prelevabili e posizionabili in un segmento viene limitata dalla **dimensione massima di segmento** (MSS, Maximum Segment Size). Questo valore viene generalmente impostato determinando prima la lunghezza del frame più grande che può essere inviato a livello di collegamento dall'host mittente locale, la cosiddetta **unità trasmissiva massima** (MTU, Maximum Transmission Unit) e poi scegliendo un MSS tale che il segmento TCP (una volta incapsulato in un datagramma IP) stia all’interno di un singolo frame a livello di collegamento, considerando anche la lunghezza dell’intestazione TCP/IP normalmente pari a 40 byte. Si noti che l’MSS rappresenta la massima quantità di dati a livello di applicazione nel segmento e non la massima dimensione del segmento TCP con intestazioni incluse. 

TCP accoppia ogni blocco di dati del client a una intestazione TCP, andando pertanto a formare **segmenti TCP**. Questi vengono passati al sottostante livello di rete, dove sono incapsulati separatamente nei datagrammi IP a livello di rete che vengono poi immessi nella rete. Quando all’altro capo TCP riceve un segmento, i dati del segmento vengono memorizzati nel buffer di ricezione della connessione TCP L’applicazione legge il flusso di dati da questo buffer. Ogni lato della connessione presenta un proprio buffer di invio e di ricezione.

#### Struttura dei segmenti TCP
> [!info] Schema di un segmento TCP 
> ![[Pasted image 20261124144756.png]]
> Come UDP, troviamo come intestazione il **numero di porta di origine e destinazione**, utilizzati per il multiplexing/demultiplexing dei dati da e verso le applicazioni a livello superiore, oltre al campo **Checksum**.
> In più troviamo:
> - **Numero di sequenza e acknowledgment**: entrambi da 32 bit, vengono utilizzati dal mittente e dal destinatario TCP per implementare il trasferimento affidabile;
> - **Finestra di ricezione**: da 16 bit viene utilizzato per il controllo di flusso. Vedremo il suo uso per indicare il numero di byte che il destinatario è disposto ad accettare; 
> - **Lunghezza dell’intestazione**: di 4 bit, specifica la lunghezza dell’intestazione TCP in multipli di 32 bit. L’intestazione TCP ha lunghezza variabile a causa del campo delle opzioni TCP. Generalmente, il campo delle opzioni è vuoto e, pertanto, la lunghezza consueta è di 20 byte;
> - **Opzioni**: facoltativo e di lunghezza variabile, viene utilizzato quando mittente e destinatario negoziano la dimensione massima del segmento (MSS) o come fattore di scala per la finestra nelle reti ad alta velocità. Viene, inoltre, definita l’opzione di time-stamping;
> **Flag**: Grande 6 bit. Il bit **ACK** viene usato per indicare che il valore trasportato nel campo di acknowledgment è valido; ossia, il segmento contiene un acknowledgment per un segmento che è stato ricevuto con successo. I bit **RST**, **SYN** e **FIN** vengono utilizzati per impostare e chiudere la connessione. I bit **CWR** ed **ECE** sono usati nel controllo di congestione esplicito. Se il bit PSH ha valore 1 il destinatario dovrebbe inviare immediatamente i dati al livello superiore. Infine, si utilizza il bit URG per indicare nel segmento la presenza di dati che l’entità mittente a livello superiore ha marcato come “urgenti”. La posizione dell’ultimo byte di dati urgenti viene denotata dal campo **Puntatore ai dati urgenti** (urgent data pointer field), di 16 bit. Quando ci sono dati urgenti, TCP deve informare l’entità destinataria al livello superiore e passarle un puntatore alla fine dei dati urgenti.
##### Numeri di sequenza e numeri di acknowledgment
TCP concepisce i dati trasferiti come un flusso di byte non strutturati ma ordinati. Tale impostazione si riflette direttamente sul meccanismo di numerazione: in TCP i numeri di sequenza non identificano i singoli segmenti trasmessi, bensì i singoli byte appartenenti al flusso di dati. Di conseguenza, il **numero di sequenza di un segmento**
corrisponde al numero, all'interno del flusso complessivo, del primo byte contenuto nel campo dati di quel segmento.

> [!example] Esempio di numeri di sequenza e acknowledgment
> Per chiarire questo concetto, si consideri un processo sull'Host A che deve inviare un file di $500.000$ byte a un processo sull'Host B attraverso una connessione TCP. Il livello di trasporto sull'Host A suddivide il flusso in segmenti di lunghezza accettabile per i livelli sottostanti (tali per cui la somma tra la dimensione massima del segmento MSS e le intestazioni TCP e IP corrisponda alla MTU) e numera implicitamente ogni singolo byte del flusso. Assumendo che il parametro MSS sia pari a $1000$ byte e che il primo byte del file sia numerato con $0$, TCP costruisce esattamente $500$ segmenti. Al primo segmento, che trasporta i byte da $0$ a $999$, viene assegnato il numero di sequenza $0$; al secondo segmento, che trasporta i byte da $1000$ a $1999$, viene assegnato il numero di sequenza $1000$; al terzo segmento viene assegnato il numero di sequenza $2000$, e così via fino all'ultimo byte del file, numerato $499.999$. Ciascuno di questi valori viene poi inserito nel campo Numero di sequenza dell'intestazione del relativo segmento TCP, consentendo al TCP del destinatario di riassemblare i segmenti nell'ordine corretto.
> ![[Pasted image 20261124160137.png]]

La comprensione dei numeri di acknowledgment è leggermente più complessa, poiché è strettamente legata alla natura **full-duplex** di TCP. In una connessione TCP, infatti, l'Host A può inviare e ricevere dati dall'Host B simultaneamente all'interno della medesima connessione. I segmenti che giungono dall'Host B recano un numero di sequenza relativo al flusso di dati diretto da B verso A. Il **numero di acknowledgment** che l'Host A inserisce nei segmenti inviati a B rappresenta il numero di sequenza del byte successivo che l'Host A si aspetta di ricevere dall'Host B, esprimibile formalmente come $\text{ACK} = \text{LastByteAcked} + 1$.

A titolo esemplificativo, se l'Host A ha ricevuto correttamente dall'Host B tutti i byte numerati da $0$ a $535$ e si accinge a inviare un segmento verso B, esso è in attesa del byte $536$ e di quelli successivi nel flusso proveniente da B; pertanto, A inserisce il valore $536$ nel campo Numero di acknowledgment del segmento spedito a B.

Consideriamo ora uno scenario ulteriore in cui l'Host A abbia ricevuto dall'Host B un primo segmento contenente i byte da $0$ a $535$ e un altro segmento contenente i byte da $900$ a $1000$, mentre, per qualche ragione legata alla rete, i byte intermedi compresi tra $536$ e $899$ non siano ancora giunti a destinazione. Anche in questa circostanza, per poter ricostruire fedelmente e senza interruzioni il flusso di dati di B, l'Host A sta ancora attendendo il byte $536$ e i successivi. Per tale ragione, il segmento successivo inviato da A verso B continuerà a riportare il valore $536$ nel campo Numero di acknowledgment. Poiché TCP conferma esclusivamente i byte ricevuti fino al primo byte mancante nel flusso, si afferma che il protocollo implementa **acknowledgment cumulativi** (cumulative acknowledgment).

Il secondo esempio appena analizzato solleva una questione delicata: l'Host A ha ricevuto il terzo segmento (relativo ai byte da $900$ a $1000$) prima del secondo (relativo ai byte da $536$ a $899$), trovandosi dunque a gestire un segmento arrivato fuori sequenza. Sorprendentemente, le specifiche RFC di TCP non prescrivono un comportamento obbligatorio per questo caso, lasciando tale scelta progettuale a chi implementa il protocollo, lasciando nella pratica due approcci fondamentali:
1. Il destinatario può scartare immediatamente i segmenti non ordinati, semplificando così la propria architettura interna; 
2. Il destinatario può conservare in un buffer i byte giunti fuori ordine e attendere l'arrivo dei byte mancanti per colmare le lacune del flusso. La seconda soluzione risulta evidentemente più efficiente in termini di utilizzo della banda di rete e costituisce la scelta adottata nella quasi totalità delle implementazioni reali;

Abbiamo supposto per tutti gli esempi che il numero di sequenza iniziale fosse zero, quando nella realtà entrambi gli host che partecipano a una connessione TCP selezionano in modo casuale il proprio numero di sequenza iniziale. Questo accorgimento riduce al minimo il rischio che un segmento rimasto ancora in circolazione nella rete, appartenente a una connessione precedente ormai terminata tra la stessa coppia di host (e che utilizzava i medesimi numeri di porta), possa essere erroneamente accettato come segmento valido all'interno della nuova connessione.
#### Timeout e stima del tempo di andata e ritorno (RTT)
TCP utilizza un meccanismo di **timeout** (tempo scaduto) e ritrasmissione per recuperare i segmenti persi.
Nonostante l'idea di base sia concettualmente semplice, la sua traduzione pratica in un protocollo reale e complesso come TCP solleva diverse problematiche progettuali. La questione più immediata riguarda la durata dell'intervallo di timeout, che deve risultare necessariamente superiore al **tempo di andata e ritorno** sulla connessione, ovvero il **Round-Trip Time** (RTT). L'RTT rappresenta il tempo impiegato da un segmento per viaggiare dal mittente al destinatario e per il corrispondente riscontro ACK di tornare al mittente, includendo i ritardi di elaborazione, di accodamento nei router intermedi, di trasmissione e di propagazione. Se l'intervallo di timeout fosse inferiore all'RTT, il timer scadrebbe sistematicamente prima del normale arrivo degli ACK, generando continue ritrasmissioni inutili.
##### Stima del tempo di andata e ritorno (RTT)
In primis, andiamo a comprendere come il protocollo stimi il tempo di andata e ritorno tra mittente e destinatario. L'RTT misurato sperimentalmente per un singolo segmento viene indicato con il nome di $SampleRTT$ e corrisponde all'intervallo di tempo che intercorre tra l'istante in cui il segmento viene inviato (cioè consegnato al livello IP) e l'istante in cui il mittente riceve il relativo acknowledgment. Anziché calcolare un $SampleRTT$ per ogni singolo segmento trasmesso, la maggior parte delle implementazioni TCP effettua una sola misurazione alla volta. Ciò significa che, in ogni istante, il tempo viene cronometrato esclusivamente per uno dei segmenti attualmente in transito e ancora privi di riscontro, portando ad acquisire approssimativamente un nuovo campione $SampleRTT$ ogni RTT.

Una regola cruciale stabilisce che TCP non calcola mai il valore di $SampleRTT$ per i segmenti che sono stati ritrasmessi, limitandosi a effettuare la misurazione soltanto sui segmenti inviati una sola volta. Questa precauzione nasce dal fatto che, quando un segmento viene ritrasmesso e il mittente riceve un ACK, quest'ultimo potrebbe riferirsi indistintamente alla prima trasmissione (giunta in ritardo) oppure alla ritrasmissione, rendendo impossibile stabilire con certezza il reale tempo di percorrenza.

Poiché il traffico di rete varia continuamente a causa della congestione nei router intermedi e del diverso carico di elaborazione sugli host periferici, i valori di $SampleRTT$ tendono a fluttuare sensibilmente da un segmento all'altro, rendendo ogni singolo campione potenzialmente atipico. Per ottenere una valutazione affidabile dell'RTT reale, TCP mantiene quindi una media dei campioni raccolti, chiamata $EstimatedRTT$. Ogni volta che viene misurato un nuovo valore di $SampleRTT$, il protocollo aggiorna la stima mediante la seguente espressione di assegnamento: $$\text{EstimatedRTT} = (1 - \alpha) \cdot \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT}$$.

Secondo RFC 6298, il valore raccomandato per il parametro $\alpha$ è $0,125$ (ovvero $1/8$), per cui la formula operativa diviene: $$\text{EstimatedRTT} = 0,875 \cdot \text{EstimatedRTT} + 0,125 \cdot \text{SampleRTT}$$. Il nuovo valore di $EstimatedRTT$ risulta quindi una combinazione ponderata della stima precedente e dell'ultimo campione rilevato. Questa formulazione assegna un peso maggiore ai campioni più recenti rispetto a quelli più datati, una scelta del tutto naturale poiché le misure recenti riflettono in modo più fedele le condizioni correnti di congestione della rete. In statistica, una media costruita con questa struttura ricorsiva prende il nome di **media mobile esponenziale ponderata** (EWMA, Exponential Weighted Moving Average), dove il termine "esponenziale" deriva dal fatto che il peso attribuito a un determinato campione decresce esponenzialmente man mano che sopravvengono nuovi aggiornamenti.$$\text{EstimatedRTT} = (1 - \alpha) \cdot \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT}$$

> [!example] Campioni e stima RTT
> ![[Pasted image 20261125154026.png]]
> Nella Come illustrato, vediamo una connessione TCP reale tra `gaia.cs.umass.edu` (nel Massachusetts) e `fantasia.eurecom.fr` (nel sud della Francia) con $\alpha = 1/8$ — l'andamento di $EstimatedRTT$ ha l'effetto di "smussare" i picchi e le brusche oscillazioni dei singoli campioni $SampleRTT$.

Oltre a stimare il valore medio dell'RTT, è indispensabile misurarne anche la variabilità nel tempo. A tale scopo, l'RFC 6298 definisce la variazione dell'RTT, denotata con $DevRTT$, come una stima di quanto i valori di $SampleRTT$ tendano generalmente a discostarsi da $EstimatedRTT$. La sua formula di aggiornamento è: $\text{DevRTT} = (1 - \beta) \cdot \text{DevRTT} + \beta \cdot |\text{SampleRTT} - \text{EstimatedRTT}|$. Anche $DevRTT$ costituisce una media mobile esponenziale ponderata (EWMA), calcolata in questo caso sul valore assoluto della differenza tra l'ultimo campione e la stima corrente. Il valore suggerito dallo standard per il parametro $\beta$ è $0,25$. Quando i campioni $SampleRTT$ presentano fluttuazioni contenute e la rete è stabile, $DevRTT$ assume un valore piccolo; al contrario, in presenza di forti oscillazioni dei ritardi, $DevRTT$ assume un valore elevato.
##### Impostazione e gestione del timeout di ritrasmissione
Una volta calcolati dinamicamente i valori di $EstimatedRTT$ e $DevRTT$, occorre determinare come impostare l'intervallo di timeout di ritrasmissione di TCP, indicato come $TimeoutInterval$. La scelta deve bilanciare due esigenze contrapposte, seguendo una strategia di ritrasmissione adattativa che consiste nell'aspettare quanto basta per poter considerare perso un segmento, ma non più del necessario. Da un lato, $TimeoutInterval$ non può essere inferiore a $EstimatedRTT$, poiché in tal caso si verificherebbero numerose ritrasmissioni premature e non necessarie. Dall'altro lato, non dovrebbe neppure essere eccessivamente più grande di $EstimatedRTT$, altrimenti in caso di perdita effettiva di un segmento TCP attenderebbe troppo a lungo prima di ritrasmetterlo, degradando pesantemente le prestazioni e introducendo gravi ritardi nel trasferimento dei dati.

La soluzione adottata da TCP consiste nell'impostare il timeout pari a $EstimatedRTT$ aumentato di un opportuno margine di sicurezza, il quale deve essere ampio quando i valori di $SampleRTT$ fluttuano notevolmente e ridotto quando la rete è stabile. È esattamente questo il ruolo svolto da $DevRTT$, che entra nella formula standard per il calcolo dell'intervallo di timeout: $\text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}$.

Per completare la gestione dinamica dei timer, l'RFC 6298 prescrive infine alcune regole operative specifiche: 
- All'inizio di una connessione, prima che sia stato possibile raccogliere misure di RTT, viene raccomandato di impostare un valore iniziale di $TimeoutInterval$ pari a $1$ secondo. 
- Ogni volta che si verifica un evento di timeout, il valore di $TimeoutInterval$ viene raddoppiato ($\text{TimeoutInterval} = \text{TimeoutInterval} \cdot 2$) anziché essere lasciato invariato; tale accorgimento serve a evitare che si inneschi un timeout prematuro anche per un segmento successivo il cui acknowledgment potrebbe essere sul punto di arrivare (oltre a fornire una prima forma di risposta alla congestione della rete). Tuttavia, non appena giunge regolarmente un riscontro che consente di misurare un nuovo campione e di aggiornare $EstimatedRTT$ (e $DevRTT$), il valore di $TimeoutInterval$ torna a essere ricalcolato secondo la formula standard $\text{EstimatedRTT} + 4 \cdot \text{DevRTT}$.
#### Trasferimento dati affidabile
Il servizio offerto dal livello di rete di Internet (ossia il protocollo IP) è inaffidabile e di tipo **best-effort**, ossia non garantisce né la consegna dei datagrammi, né il loro arrivo nell'ordine originale, né tantomeno l'integrità dei bit trasportati. Attraversando la rete, i datagrammi IP possono saturare i buffer dei router intermedi ed essere scartati, giungere a destinazione in ordine casuale oppure subire alterazioni dei bit; poiché i segmenti del livello di trasporto sono incapsulati all'interno dei datagrammi IP, essi risultano direttamente esposti a tutte queste problematiche. Su queste basi inaffidabili, TCP costruisce un **servizio di trasferimento dati affidabile** (reliable data transfer service), garantendo che il flusso di byte letto dal processo applicativo dal proprio buffer di ricezione TCP sia privo di errori, privo di lacune, privo di duplicazioni e perfettamente ordinato: in altre parole, il flusso ricevuto coincide esattamente con il flusso originariamente spedito.

Per realizzare questo servizio, TCP impiega molti dei principi teorici del trasferimento dati affidabile, tra cui il rilevamento degli errori, i numeri di sequenza, gli acknowledgment cumulativi, le ritrasmissioni e i timer. Dal punto di vista concettuale sarebbe più immediato associare un timer individuale a ciascun segmento trasmesso e non ancora riscontrato; tuttavia, nelle implementazioni reali la gestione simultanea di molti timer comporterebbe un notevole sovraccarico computazionale e di risorse. Per tale motivo, le procedure standard raccomandate dall'IETF nell'RFC 6298 prevedono l'utilizzo di un **unico timer di ritrasmissione** per connessione, anche in presenza di più segmenti in transito ancora privi di conferma.

Per analizzare con chiarezza il meccanismo di trasferimento affidabile di TCP, esamineremo in due fasi: inizialmente si esamina un modello semplificato del mittente TCP che sfrutta esclusivamente i timeout per recuperare i segmenti perduti, per poi passare a una descrizione più completa che integra il recupero anticipato basato sugli acknowledgment duplicati. In tutta l'analisi si ipotizza un trasferimento unidirezionale di un file di grandi dimensioni dall'Host A all'Host B, trascurando temporaneamente i vincoli imposti dal controllo di flusso e dal controllo di congestione e assumendo che ciascun blocco di dati proveniente dall'applicazione abbia dimensione inferiore a MSS.

> [!example] Il mittente TCP semplificato
> Nel modello semplificato del mittente TCP, il funzionamento ruota attorno a due variabili di stato inizializzate al valore `InitialSeqNumber`: la variabile `NextSeqNum`, che indica il numero di sequenza del prossimo byte da trasmettere, e la variabile `SendBase`, che rappresenta il numero di sequenza del byte più vecchio già inviato ma non ancora riscontrato. Di conseguenza, $\text{SendBase} - 1$ corrisponde al numero di sequenza dell'ultimo byte che il mittente sa per certo essere stato ricevuto correttamente e in ordine dal destinatario. 
> Il comportamento del mittente è governato da un ciclo infinito che reagisce a tre eventi principali legati alla trasmissione e alla ritrasmissione dei dati:
> 
> 1. Il primo evento è la **ricezione di dati provenienti dal processo applicativo** sovrastante. Al verificarsi di questo evento, TCP incapsula i dati in un nuovo segmento impostando come numero di sequenza il valore corrente di `NextSeqNum`. Successivamente, se il timer di ritrasmissione non è già attivo per un segmento precedente, TCP provvede ad avviarlo nel momento in cui il segmento viene passato al livello IP; è utile concettualizzare quest'unico timer come sempre associato al segmento più vecchio ancora in attesa di riscontro, con una durata di scadenza pari a `TimeoutInterval`, calcolato dinamicamente a partire da `EstimatedRTT` e `DevRTT`. Infine, il mittente aggiorna il contatore dei byte da inviare ponendo $\text{NextSeqNum} = \text{NextSeqNum} + \text{lunghezza(dati)}$.
> 
> 2. Il secondo evento è il **timeout del timer di ritrasmissione**. Quando il timer scade senza che sia giunto il riscontro atteso, TCP reagisce ritrasmettendo il segmento non ancora confermato avente il numero di sequenza più basso — ossia il segmento più vecchio privo di acknowledgment, responsabile della scadenza del timer — e riavviando immediatamente il timer stesso.
> 
> 3. Il terzo evento è la **ricezione di un segmento di acknowledgment** recante un valore valido $y$ nel campo ACK. Poiché TCP adotta acknowledgment cumulativi, ricevere il valore $y$ certifica che il destinatario ha ricevuto correttamente e in sequenza tutti i byte che precedono il byte $y$. Il mittente confronta dunque $y$ con la propria variabile `SendBase`: se vale la condizione $y > \text{SendBase}$, l'acknowledgment conferma uno o più segmenti precedentemente in sospeso. In tal caso, il mittente aggiorna la propria base ponendo $\text{SendBase} = y$ e verifica se esistano ancora segmenti trasmessi privi di riscontro: qualora vi siano ancora segmenti non confermati in transito, il timer viene riavviato per monitorare il nuovo segmento più vecchio in sospeso. La logica operativa di questo mittente semplificato può essere sintetizzata nel seguente pseudocodice:
> ```
>     NextSeqNum = InitialSeqNumber
>     SendBase = InitialSeqNumber
> 
>     loop (per sempre) {
>         switch (evento)
> 
>         evento: dati ricevuti dall'applicazione a livello superiore
>             crea il segmento TCP con numero di sequenza NextSeqNum
>             if (il timer attualmente non è in funzione)
>                 avvia il timer
>             passa il segmento a IP
>             NextSeqNum = NextSeqNum + lunghezza(dati)
>             break;
> 
>         evento: timeout del timer
>             ritrasmetti il segmento che non ha ricevuto ACK con il
>                 più piccolo numero di sequenza
>             avvia il timer
>             break;
> 
>         evento: ACK ricevuto, con valore del campo ACK pari a y
>             if (y > SendBase) {
>                 SendBase = y
>                 if (esistono attualmente segmenti senza ACK)
>                     avvia il timer
>             }
>             break;
>     }
> ```
> 

Nonostante la forte semplificazione del modello appena descritto, esso racchiude già meccanismi molto efficaci che emergono chiaramente dall'analisi di tre scenari tipici di scambio tra l'Host A e l'Host B.

Nel **primo scenario**, relativo alla perdita di un acknowledgment, l'Host A trasmette all'Host B un segmento con numero di sequenza $92$ contenente $8$ byte di dati (ovvero i byte da $92$ a $99$) e resta in attesa di un segmento di risposta con numero di acknowledgment pari a $100$. Il segmento giunge regolarmente all'Host B, il quale spedisce $\text{ACK} = 100$, ma tale riscontro viene smarrito lungo il percorso inverso. Allo scadere del timer di timeout, l'Host A ritrasmette lo stesso segmento ($\text{Seq} = 92$, $8$ byte di dati). Quando l'Host B riceve la ritrasmissione, verifica il numero di sequenza e constata immediatamente che si tratta di dati già ricevuti e memorizzati in precedenza; pertanto scarta i byte duplicati del segmento ritrasmesso e rinvia $\text{ACK} = 100$.

Nel **secondo scenario**, l'Host A invia consecutivamente due segmenti: il primo ha numero di sequenza $92$ e trasporta $8$ byte di dati, mentre il secondo ha numero di sequenza $100$ e trasporta $20$ byte di dati (i byte da $100$ a $119$). Entrambi i segmenti arrivano integri all'Host B, il quale genera due riscontri separati: $\text{ACK} = 100$ per il primo segmento e $\text{ACK} = 120$ per il secondo. Si ipotizzi però che, a causa dei ritardi di rete, nessuno dei due acknowledgment raggiunga l'Host A prima della scadenza del timer associato al primo segmento. Al verificarsi del timeout, l'Host A ritrasmette esclusivamente il segmento più vecchio non riscontrato, ovvero quello con numero di sequenza $92$, e riavvia il timer. Subito dopo, giungono finalmente ad A i riscontri $\text{ACK} = 100$ e $\text{ACK} = 120$: purché l'acknowledgment del secondo segmento ($\text{ACK} = 120$) arrivi prima della scadenza del nuovo intervallo di timeout, il secondo segmento ($\text{Seq} = 100$) non verrà mai ritrasmesso.

Nel **terzo scenario**, emerge il vantaggio operativo degli acknowledgment cumulativi. L'Host A trasmette nuovamente gli stessi due segmenti del caso precedente ($\text{Seq} = 92$ con $8$ byte e $\text{Seq} = 100$ con $20$ byte) ed entrambi raggiungono l'Host B. In questa circostanza, l'acknowledgment relativo al primo segmento ($\text{ACK} = 100$) va perduto nella rete, ma prima che scada il timer di timeout l'Host A riceve il secondo riscontro, recante $\text{ACK} = 120$. Poiché gli acknowledgment di TCP sono cumulativi, l'Host A deduce con certezza che l'Host B ha ricevuto correttamente tutti i byte fino al numero $119$ compreso; di conseguenza, la perdita di $\text{ACK} = 100$ viene compensata automaticamente e l'Host A non ritrasmette nessuno dei due segmenti.

#### Controllo di flusso
#### Gestione della connessione TCP
