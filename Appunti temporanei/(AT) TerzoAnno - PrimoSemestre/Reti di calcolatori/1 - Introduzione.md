## Internet come infrastruttura fisica
Internet è una rete di calcolatori che interconnette miliardi di dispositivi di calcolo in tutto il mondo.
In gergo, questi dispositivi sono detti **host** (ospite) o **sistemi periferici** (end system).

I vari sistemi periferici sono connessi tra loro tramite una **rete di collegamenti** (communication link) e **commutatori di pacchetti** (packet switch)

Collegamenti diversi possono trasmettere dati a velocità differenti, e **tale velocità (o tasso) di trasmissione (transmission rate) viene misurata in bit/secondo (bps).**
Quando un sistema periferico vuole inviare dati a un altro sistema periferico, suddivide i dati in sottoparti e aggiunge un’intestazione a ciascuna di esse: l'insieme delle informazioni risultanti viene chiamato **pacchetto** (packet).
I pacchetti sono inviati attraverso la rete alla destinazione, dove vengono riassemblati per ottenere i dati originari. 

Un commutatore di pacchetto prende un pacchetto che arriva da uno dei collegamenti in ingresso e lo ritrasmette su uno di quelli in uscita. Esistono commutatori di pacchetto di varia forma e natura, ma i due principali nell'odierna Internet sono i **router** e i **commutatori a livello di collegamento** (link layer switch). I commutatori a livello di collegamento sono solitamente usati nelle reti di accesso, mentre i router nel nucleo della rete. 
Dal sistema di invio a quello di ricezione, la sequenza di collegamenti e commutatori di pacchetto attraversata da ogni singolo pacchetto è nota come **percorso** (route o path) attraverso la rete.

I sistemi periferici accedono a internet tramite i vari **Internet Service Provider (ISP)**, che comprendono vari generi (residenziali reti universitarie etc.).
Un provider è un insieme di commutatori di pacchetto e di collegamenti. Gli ISP forniscono ai sistemi periferici svariati tipi di accesso alla rete e rendono disponibile l’accesso a Internet ai fornitori di contenuti, connettendone i server a Internet. Poiché Internet non è altro che connettere tra loro gli end systems, anche gli ISP che forniscono accesso ai sistemi periferici devono essere interconnessi, tali provider di livello gerarchico più basso sono interconnessi a quelli nazionali e internazionali di livello più alto, che sono connessi tra di loro direttamente. Un ISP di livello superiore è costituito da router ad alta velocità interconnessi tramite fibra ottica.

Sistemi periferici, commutatori di pacchetto e altre parti di Internet fanno uso di protocolli che controllano l’invio e la ricezione di informazioni all’interno della rete. Due dei principali protocolli Internet sono il **Transmission Control Protocol (TCP), e l’Internet Protocol (IP)**.
Generalmente questi standard sono definiti tramite una taskforce precisa di ingegneri (IETF) e dei documenti chiamati RFC (Request for Comment)
## Internet come descrizione di servizi
Date quindi tutte le definizioni precedenti, abbiamo descritto internet nella sua forma "fisica", ma possiamo descriverlo anche come un ampia infrastruttura che fornisce servizi delle applicazioni (posta elettronica, navigazione Web).

Queste applicazioni sono dette **applicazioni distribuite** (distributed applications), in quanto coinvolgono più sistemi periferici che si scambiano reciprocamente dati. L’aspetto più rilevante è che le applicazioni Internet vengono eseguite sui sistemi periferici e non sui commutatori di pacchetto del nucleo della rete. Sebbene i commutatori di pacchetto consentano lo scambio di dati tra sistemi periferici, non hanno a che fare con le applicazioni che sono sorgenti e destinazioni dei dati.

I sistemi periferici collegati a Internet forniscono una **interfaccia socket** (socket interface), che specifica come un programma eseguito su un sistema periferico possa chiedere a Internet di recapitare dati a un programma eseguito su un altro sistema periferico. L’interfaccia socket è un insieme di regole che il programma mittente deve seguire in modo che i dati siano recapitati al programma di destinazione.

> [!info] Infrastruttura base di internet 
> ![[Pasted image 20261016154516.png]]
> I sistemi periferici vengono anche detti **host** in quanto ospitano (ed eseguono) programmi applicativi quali browser, web server o software di lettura e gestione della posta elettronica. Talvolta gli host vengono ulteriormente suddivisi in due categorie: **client e server.** In modo informale:
> - I **client** sono host che richiedono dei servizi e tendono a essere PC, laptop, smartphone e via dicendo; 
> - I **server** sono host che si occupano di erogare dei servizi e sono sostanzialmente macchine più potenti che memorizzano e distribuiscono pagine web o flussi video, ritrasmettono la posta elettronica e così via.
### Protocolli
> [!info] Definizione formale di protocolli
> Un **protocollo** definisce il formato e l’ordine dei messaggi scambiati tra due o più entità in comunicazione, così come le azioni intraprese in fase di trasmissione e/o di ricezione di un messaggio o di un altro evento.

Internet, insieme alla reti di calcolatori in generale, fanno uso estensivo di protocolli, il quale ognuno di essi è diverso per il compito che deve svolgere. Qualsiasi attività su internet che coinvolge due o più entità remote in comunicazione viene governata da un protocollo (ad esempio quelle delle schede di rete tra due calcolatori fisicamente connessi, di congestione).
> [!example] Esempio del protocollo HTTP per la richiesta di una pagina Web
> ![[Pasted image 20261012144741.png]]
## Reti di accesso
Parliamo brevemente delle **reti di accesso** (access network.) La rete di accesso è una rete che connette fisicamente un sistema al suo **edge router** (router di bordo), che è il primo router sul percorso dal sistema d’origine a un qualsiasi altro sistema di destinazione collocato al di fuori della stessa rete di accesso.

Le principali tipologie di reti di accesso si suddividono in tre categorie principali:
1. **Accesso residenziale a larga banda**:
	 - **DSL (Digital Subscriber Line)**: connette il modem dell'abitazione alla centrale locale della compagnia telefonica tramite un apparato chiamato DSLAM.
	 - **FTTH (Fiber to the Home)**: fornisce una connessione diretta dalla centrale locale fin dentro l'abitazione dell'utente tramite architetture ottiche (come le PON).
	 - **5G Fixed Wireless Access (FWA)**: eroga connettività residenziale ad alta velocità inviando il segnale in modalità wireless dalla stazione base dell'operatore direttamente al modem di casa.
2. **Accesso aziendale e domestico (LAN)**:
	- **Ethernet e Wi-Fi**: tecnologie per reti locali (LAN) ampiamente impiegate in aziende, università e abitazioni per interconnettere i dispositivi a uno switch o a un *access point* wireless collegato al router di bordo.
3. **Accesso wireless su scala geografica**:
	- **Reti cellulari (3G, 4G LTE e 5G)**: collegano i dispositivi mobili ad alta velocità su ampie distanze tramite le stazioni base dei gestori di telefonia.
## Nucleo della rete
### Commutazione a pacchetto
Come detto precedentemente, le applicazioni distribuite scambiano **messaggi** che possono contenere qualsiasi cosa il progettista del protocollo desideri.
La sorgente suddivide i messaggi lunghi in parti più piccole note come **pacchetti**. Tra la sorgente e la destinazione, questi pacchetti viaggiano attraverso collegamenti e **commutatori di pacchetto** (di cui, ricordiamo, esistono due tipi principali: i **router** e i **commutatori a livello di collegamento**). 
I pacchetti vengono trasmessi su ciascun collegamento a una velocità pari alla velocità totale di trasmissione del collegamento stesso. Quindi, se un sistema periferico o un commutatore invia un pacchetto di $L$ bit su un canale con velocità di $R$ bps, il tempo di trasmissione risulta pari a $L/R$ secondi.
#### Trasmissione store-and-forward
La maggior parte dei commutatori di pacchetto utilizza la **trasmissione store-and-forward**. Ciò significa che il commutatore deve ricevere l’intero pacchetto prima di poter cominciare a trasmettere sul collegamento in uscita il primo bit.
> [!example] Esempio di trasmissione store-and-foward
> ![[Pasted image 20261016161316.png]]
> Prendiamo questo scenario, dove un host sorgente invia dati a un host di destinazione attraversando un unico router intermedio, ovvero un percorso di $N=2$ collegamenti. Ogni pacchetto ha una lunghezza di $L$ bit e i collegamenti trasmettono a una velocità di $R$ bit al secondo.
> Se prendiamo lo scenario di invio di un singolo pacchetto vedremo che:
> 1. All'istante $t = 0$, la sorgente comincia a trasmettere il pacchetto.
> 2. All'istante $t = \frac{L}{R}$, l'intero pacchetto è stato trasmesso dalla sorgente e immagazzinato nel buffer del router. Soltanto a questo punto il router può iniziare a inoltrarlo sul secondo collegamento.
> 3. All'istante $t = \frac{2L}{R}$, il pacchetto è stato completamente trasmesso dal router e ricevuto dal destinatario (trascurando i ritardi di propagazione). Il ritardo totale end-to-end per un pacchetto risulta quindi pari a $2 \cdot \frac{L}{R}$.
> 
> Nel caso di una sequenza di pacchetti (supponiamo che questi siano 3) troveremo che:
> - Il $1^\circ$ pacchetto viene completato a destinazione all'istante $\frac{2L}{R}$.
> - Il $2^\circ$ pacchetto arriva a destinazione all'istante $\frac{3L}{R}$.
> - Il $3^\circ$ pacchetto arriva a destinazione all'istante $\frac{4L}{R}$.
> 
> Più in generale, per inviare un singolo pacchetto su un percorso di $N$ collegamenti identici a velocità $R$, il ritardo complessivo da un capo all'altro è dato dalla formula $$N \cdot \frac{L}{R}$$

Ogni commutatore di pacchetto connette più collegamenti e per ciascuno di questi il commutatore mantiene un **buffer di output** per conservare i pacchetti che sta per inviare su quel collegamento. I buffer di output rivestono un ruolo chiave nella commutazione di pacchetto, infatti un pacchetto in arrivo che debba essere inviato attraverso un collegamento occupato dalla trasmissione di un altro, deve attendere nella coda di output.

> [!example] Esempio di rete a pacchetto completa
> ![[Pasted image 20261016162651.png]]
#### Ritardi di elaborazione nelle reti a commutazione a pacchetto
Un pacchetto, a ogni "tappa" che attraversa, subisce un **ritardo**.
Internet funziona esattamente in questa maniera, non riservando nessuna risorsa al pacchetto, che viene semplicemente immesso nella rete, offrendo una connessione **best effort** (senza garanzia)
> [!example] Esempio di ritardo in una rete (con più router)
> ![[Pasted image 20261016172505.png]]

Come vediamo sopra, i pacchetti subiscono nelle reti a commutazione di pacchetto dei vari ritardi, che possono essere categorizzati in diversi maniere:
- **Ritardo di elaborazione (processing delay)**: Tempo richiesto per esaminare l’intestazione del pacchetto e per determinare dove dirigerlo. Questo può anche includere altri fattori, tra i quali il tempo richiesto per controllare errori a livello di bit eventualmente occorsi nel pacchetto durante la trasmissione dal nodo a monte al router $A$. Nei router ad alta velocità questi ritardi sono solitamente dell’ordine dei microsecondi o inferiori. Dopo l’elaborazione, il router dirige il pacchetto verso la coda che precede il collegamento al router B.
- **Ritardo di accodamento**: una volta in coda, il pacchetto subisce un **ritardo di accodamento (queuing delay)** mentre attende la trasmissione sul collegamento. La lunghezza di tale ritardo per uno specifico pacchetto dipenderà dal numero di pacchetti precedentemente arrivati, accodati e in attesa di trasmissione sullo stesso collegamento. Se la coda è vuota e non è in corso la trasmissione di altri pacchetti, il ritardo di accodamento per il nostro pacchetto è nullo, in caso contrario potrebbe essere elevato. Generalmente si è nell'ordine dei microsecondi o millisecondi
- **Ritardo di trasmissione**: Assumendo che i pacchetti siano trasmessi secondo la politica first-come-first-served (il primo che arriva è il primo a essere servito), come avviene comunemente nelle reti a commutazione di pacchetto, il nostro pacchetto può essere trasmesso solo dopo la trasmissione di tutti quelli che lo hanno preceduto nell'arrivo. Considerando quindi una lunghezza $L$ di un pacchetto (in bit) e la velocità di trasmissione $R$ tra un router e l'altro (espressa in bit per secondo), il ritardo viene misurato in $\frac{L}{R}$, ossia il tempo richiesto per trasmettere tutti i bit del pacchetto. Generalmente si è nell'ordine dei microsecondi o millisecondi
- **Ritardo di propagazione**: Una volta immesso sul collegamento, un bit deve propagarsi fino al router $B$. Il bit viaggia alla velocità di propagazione del collegamento, che dipende dal mezzo fisico (fibra ottica, doppino in rame e così via) ed è compresa nell'intervallo che va dai $2 \cdot 10^{8} m/s$ ai $3 \cdot 10^{8} m/s$, corrispondente quest’ultimo alla velocità della luce. Il ritardo di propagazione è dato da $\frac{d}{v}$, dove $d$ è la distanza tra i due router, mentre $v$ è la velocità di propagazione nel collegamento. Nelle reti molto estese i ritardi di propagazione sono dell’ordine dei millisecondi.
##### Ritardo end-to end e traceroute
Per studiare il concetto di ritardo end to end (quindi tra due host), supponiamo l’esistenza di $N–1$ router tra l’host sorgente e quello di destinazione. Ipotizziamo, inoltre, per il momento, che la rete non sia congestionata (e che quindi i ritardi di accodamento siano trascurabili), il ritardo di elaborazione a ciascun router e presso il mittente sia $d_{elab}$, la velocità di trasmissione in uscita a ogni router e all'host sorgente sia di $R$ bps e la propagazione su ciascun collegamento sia $d_{prop}$ . 
I ritardi totali di nodo si accumulano e danno un ritardo complessivo end-to-end pari a: $$d_{end-to-end}=N(d_{elab}+d_{trasm}+d_{prop})$$
Dove $d_{trasm}=\frac{L}{R}$ e $L$ è la dimensione del pacchetto. 

Per ottenere una misura efficiente dei ritardi in una rete di calcolatori possiamo fare uso del programma diagnostico **Traceroute**. Si tratta di un semplice programma eseguibile su qualsiasi host di Internet. Quando l’utente specifica il nome di un host di destinazione, il modulo dell’utente invia un certo numero di pacchetti speciali verso tale destinazione. Durante il loro percorso verso la destinazione, questi pacchetti passano attraverso una serie di router. Quando un router riceve uno di questi pacchetti speciali, invia un breve messaggio che torna all'origine. Il messaggio contiene il nome e l’indirizzo del router. 

Più nello specifico, si supponga l’esistenza di $N – 1$ router tra l’origine e la destinazione. L’origine invia N pacchetti speciali nella rete, ciascuno dei quali ha come indirizzo la destinazione ultima. Tali N pacchetti speciali sono etichettati da $1$ a $N$, in sequenza. Quando l’$n$-esimo router riceve il pacchetto marcato con $n$, invece di instradarlo verso la sua destinazione, invia un messaggio che torna verso l’origine. Quando l’host di destinazione riceve il pacchetto speciale $N$-esimo, anch'esso restituisce un messaggio all'origine. Esso registra il tempo intercorso tra l’invio di un pacchetto e la ricezione del corrispondente messaggio di ritorno e memorizza anche il nome e l’indirizzo del router (o del destinatario) che restituisce il messaggio. In questo modo, l’origine può ricostruire il percorso intrapreso dai pacchetti ed è inoltre in grado di determinare i ritardi di andata e ritorno per tutte le tratte.

> [!example] Esempio di traceroute
> ![[Pasted image 20261016175359.png]]
#### Perdita di pacchetti
Come sappiamo, le code di un router hanno capacità finita (variabile in dimensione dal costo e architettura.Poiché la capacità delle code è finita, un pacchetto può trovare la coda piena. Non essendo possibile memorizzare tale pacchetto, il router lo eliminerà e il pacchetto andrà perduto. 

Dal punto di vista dei sistemi periferici, la perdita sembrerà come se il pacchetto fosse stato inviato in rete, ma non fosse più riemerso alla destinazione. I La frazione di pacchetti perduti aumenta in proporzione all’intensità di traffico. Quindi, le prestazioni di un nodo sono spesso misurate non solo in termini di ritardo, ma anche della probabilità di perdita di pacchetti.

Come vedremo successivamente, un pacchetto generalmente porta con se un valore molto importante chiamato **TTL (Time To Live)**. Questo valore (fisso per alcuni pacchetti) viene diminuito di 1 ogni volta che il pacchetto attraversa un nodo della rete, se questo arriva a 0 viene direttamente scartato. Il TTL previene il fenomeno dei pacchetti fantasma all'interno della rete, ossia di pacchetti che, per un motivo o per l'altro, non riescono a raggiungere la loro destinazione, girando ininterrottamente per Internet.
#### Throughput
Oltre al ritardo e alla perdita di pacchetti, un’altra misura critica delle prestazioni in una rete di calcolatori è il **throughput end-to-end**. Per darne una definizione, considerate il trasferimento di un file voluminoso da $A$ a $B$, attraverso la rete che potrebbe essere, per esempio, un grosso videoclip da trasmettere da un computer a un altro. **Il throughput (capacità totale)** istantaneo in ogni istante di tempo è la velocità (in bps) alla quale $B$ sta ricevendo il file.
Se il file consiste di $F$ bit e il trasferimento richiede $T$ secondi affinché $B$ riceva tutti gli $F$ bit, allora il throughput medio del trasferimento del file è di $\frac{F}{T}$ bps.

> [!example] Vari esempi di throughput
> ![[Pasted image 20261016180457.png]]
> Nel primo scenario, viene considerato un modello semplificato a due soli collegamenti, in cui un server trasferisce un file a un client attraverso un router intermedio. Se indichiamo con $R_s$ la velocità del collegamento tra il server e il router e con $R_c$ la velocità del collegamento tra il router e il client, è possibile visualizzare la trasmissione assimilando i bit a un fluido che scorre all'interno di condotte. Qualora la velocità del primo tratto sia inferiore a quella del secondo ($R_s < R_c$), il flusso dei dati procederà alla velocità consentita dal server, determinando un throughput pari a $R_s$. Al contrario, se il collegamento in uscita dal router è più lento di quello in ingresso ($R_c < R_s$), il router non sarà in grado di inoltrare i bit al ritmo con cui li riceve, causando un accumulo di dati nel proprio buffer e limitando la velocità finale di ricezione a $R_c$. Da questo comportamento discende il principio per cui il throughput end-to-end è vincolato dalla velocità del collegamento più lento lungo il percorso, noto come collegamento **collo di bottiglia (bottleneck link)**, risultando pari a $\min(R_s, R_c)$. Di conseguenza, il tempo richiesto per trasferire un file di dimensione $F$ bit può essere stimato tramite la formula:
> $$ \frac{F}{\min(R_s, R_c)} $$
> 
> Generalizzando questo modello a un percorso composto da $N$ collegamenti in serie con velocità $R_1, R_2, \dots, R_N$, la velocità complessiva di trasferimento rimane determinata dal collegamento più lento dell'intera catena, ossia da $\min(R_1, R_2, \dots, R_N)$. Se si applica questa trattazione alla realtà dell'Internet odierna, occorre osservare che il nucleo della rete (network core) è fortemente sovradimensionato con collegamenti ad altissima velocità e scarsamente congestionati. In assenza di altro traffico rilevante, le prestazioni end-to-end sono quindi dominate dalle reti di accesso del server e del client, riducendo nuovamente la formula del throughput a $\min(R_s, R_c)$.
> 
> ![[Pasted image 20261016180652.png]]
> Esaminiamo una situazione più complessa e aderente alla realtà in cui la rete è condivisa tra più flussi contemporanei.
> Ipotizzando che dieci coppie di server e client effettuino trasferimenti simultanei attraverso un collegamento centrale del core di capacità $R$, il throughput di ciascun flusso dipende dal livello di congestione di tale tratta. Se la capacità $R$ è molto elevata rispetto a $R_s$ e $R_c$, ciascun download otterrà comunque la velocità massima consentita dalle proprie reti d'accesso, pari a $\min(R_s, R_c)$. Tuttavia, se il collegamento condiviso è congestionato (per esempio con $R_s = 2\text{ Mbps}$, $R_c = 1\text{ Mbps}$ e $R = 5\text{ Mbps}$ ripartiti equamente tra i dieci flussi) a ogni download spetterà una quota di banda di soli $500\text{ kbps}$. In questa circostanza, il collo di bottiglia non risiede più nelle reti d'accesso ma nella banda allocata sul collegamento centrale, riducendo il throughput end-to-end di ciascuna sessione a $500\text{ kbps}$.
### Commutazione a circuito
Nelle reti a commutazione di circuito le risorse richieste lungo un percorso (buffer e velocità di trasmissione sui collegamenti) per consentire la comunicazione tra sistemi periferici sono riservate per l’intera durata della sessione di comunicazione. Questa cosa, come visto precedentemente, non accade nelle reti a commutazione di pacchetto.

> [!example] Esempio di rete a circuito
> ![[Pasted image 20261016163148.png]]
> Consideriamo una rete a commutazione di circuito in cui i quattro commutatori sono interconnessi tramite quattro collegamenti. Ciascuno di questi ultimi dispone di quattro circuiti, in modo che ogni collegamento possa supportare quattro connessioni simultanee. Gli host sono tutti direttamente connessi a uno dei commutatori. 
> Quando due host desiderano comunicare la rete stabilisce una **connessione end-to-end** (o connessione punto a punto) dedicata a loro. Affinché $A$ invii messaggi a $B$, la rete deve prima riservare un circuito su ciascuno dei due collegamenti. Nell'esempio, la connessione punto a punto usa il secondo circuito del primo collegamento e il quarto circuito del secondo. Poiché ogni collegamento ospita quattro circuiti, per ogni collegamento utilizzato dalla connessione punto a punto la connessione ottiene un quarto della capacità trasmissiva totale del collegamento per la durata della connessione stessa. Se la velocità di trasmissione tra switch adiacenti fosse pari a 1Mbps, ogni connessione avrebbe come misura di capacità trasmissiva dedicata 250kbps
#### Multiplexing nelle reti a commutazione di circuito
Un circuito all'interno di un collegamento è implementato tramite 2 modalità
- **Multiplexing a divisione di frequenza (FDM, frequency-division multiplexing)**: lo spettro di frequenza di un collegamento viene suddiviso tra le connessioni stabilite tramite il collegamento, dedicando una banda di frequenza a ciascuna connessione per la durata della connessione stessa. La larghezza di banda viene chiamata **ampiezza di banda (bandwidth)**
- **Multiplexing a divisione di tempo (TDM, time-division multiplexing)**: il tempo viene suddiviso in frame (intervalli) di durata fissa, a loro volta ripartiti in un numero fisso di slot (porzioni) temporali. Quando la rete stabilisce una connessione attraverso un collegamento, le dedica uno slot di tempo in ogni frame, e tali slot sono dedicati unicamente a quella connessione, con uno slot temporale disponibile in ciascun frame alla trasmissione dei dati di connessione.

> [!example] FDM vs TDM
> ![[Pasted image 20261006192732.png]]

Sebbene la commutazione di pacchetto e quella di circuito siano entrambe presenti negli odierni sistemi di telecomunicazioni, la tendenza è certamente in direzione della commutazione di pacchetto.
Una rete che ancora oggi fa uso della commutazione a circuito è quella del telefono.
### Una rete di reti
Abbiamo precedentemente visto che i sistemi periferici si collegano a Internet tramite un ISP di accesso che può fornire connettività attraverso una rete cablata o senza fili con svariate tecnologie.
Tuttavia, la connessione degli utenti finali e dei fornitori di contenuti alla rete di un ISP è solo una piccola parte per connettere i miliardi di utenti che costituiscono Internet. Per completarla bisogna interconnettere gli stessi ISP, e ciò avviene creando una **rete di reti (network of networks)**

> [!example] Varie interconnessioni tra ISP
> ![[Pasted image 20261016164818.png]]
> 

Gli ISP di accesso si connettono a diversi livelli in una rete odierna (dal basso verso l'alto):
- **ISP regionali o nazionali**: reti di ampia estensione che coprono intere regioni o nazioni (come TIM, Fastweb o la rete GARR per le università italiane). Acquistano la connettività da provider più grandi e la forniscono agli ISP di accesso.
- **Punti di interconnessione locale(IXP), PoP e Multi-Homing**: Per evitare che ogni singolo pacchetto debba risalire l'intera gerarchia mondiale, a questo livello si inseriscono strutture intermedie e accordi tra reti vicine, che si suddividono in:
	 - **PoP (Point of Presence)**: gruppi di router tramite i quali un ISP cliente si collega al proprio fornitore affittando una linea ad alta velocità
	 - **Multi-homing**: la possibilità per un ISP di collegarsi a due o più fornitori superiori per garantire continuità di servizio in caso di guasto su una linea
	 - **Peering e IXP (** **Internet Exchange Point** **)**: punti fisici d'incontro (IXP) gestiti da terzi in cui più ISP si collegano per scambiarsi traffico direttamente e a costo zero (peering), senza dover pagare un provider di transito
- **ISP di primo livello** (detti globali, o Tier-1): rappresentano la dorsale (backbone) mondiale di Internet. Sono una decina di grandi multinazionali delle telecomunicazioni (come Cogent o Sparkle) con reti in fibra ottica ad altissima velocità che coprono continenti e oceani. Non pagano nessuno per il transito e fanno peering gratuito tra loro per garantire la raggiungibilità di qualsiasi punto del pianeta
- **Reti private dei grandi fornitori di contenuti** (come Google, Meta, Microsoft, Amazon): queste aziende gestiscono proprie reti private mondiali ad altissime prestazioni che collegano i loro data center distribuiti nel mondo. Per ridurre i costi di transito e garantire tempi di risposta minimi, esse scavalcano i provider di livello 1 facendo peering diretto con gli ISP di accesso o collegandosi direttamente agli IXP. Si servono dei Tier-1 unicamente come ultima risorsa per le destinazioni non raggiungibili direttamente, rendendo così la topologia di Internet complessivamente più "piatta"