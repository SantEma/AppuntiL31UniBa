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
Un provider è un insieme di commutatori di pacchetto e di collegamenti. Gli ISP forniscono ai sistemi periferici svariati tipi di accesso alla rete e rendono disponibile l’accesso a Internet ai fornitori di contenuti, connettendone i server a Internet. Poiché Internet non è altro che connettere tra loro gli end systems, anche gli ISP che forniscono accesso ai sistemi periferici devono essere interconnessi, tali provider di livello gerarchico più basso sono interconnessi a quelli nazionali e internazionali di livello più alto, che sono connessi tra di loro direttamente. Un ISP di livello superiore è costituito da router ad alta velocità interconnessi tramite fibra ottica. Ciascuna rete di un ISP, sia di alto sia di basso livello, è gestita in modo indipendente, fa uso del protocollo IP e si conforma a determinate convenzioni riguardo a nomi e indirizzi.

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
Parliamo brevemente  delle **reti di accesso** (access network. LA rete di accesso è una rete che connette fisicamente un sistema al suo **edge router** (router di bordo), che è il primo router sul percorso dal sistema d’origine a un qualsiasi altro sistema di destinazione collocato al di fuori della stessa rete di accesso.

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
### Commutazioni a pacchetto
Come detto precedentemente, le applicazioni distribuite scambiano **messaggi** che possono contenere qualsiasi cosa il progettista del protocollo desideri.
La sorgente suddivide i messaggi lunghi in parti più piccole note come **pacchetti**. Tra la sorgente e la destinazione, questi pacchetti viaggiano attraverso collegamenti e **commutatori di pacchetto** (di cui, ricordiamo, esistono due tipi principali: i **router** e i **commutatori a livello di collegamento**). I pacchetti vengono trasmessi su ciascun collegamento a una velocità pari alla velocità totale di trasmissione del collegamento stesso. Quindi, se un sistema periferico o un commutatore invia un pacchetto di $L$ bit su un canale con velocità di $R$ bps, il tempo di trasmissione risulta pari a $L/R$ secondi.
#### Trasmissione store-and-forward
La maggior parte dei commutatori di pacchetto utilizza la **trasmissione store-and-forward**. Ciò significa che il commutatore deve ricevere l’intero pacchetto prima di poter cominciare a trasmettere sul collegamento in uscita il primo bit.
> [!example] Esempio di trasmissione store-and-foward
> ![[Pasted image 20261016161316.png]]
> Prendiamo questo scenario, dove un host sorgente invia dati a un host di destinazione attraversando un unico router intermedio, ovvero un percorso di $N=2$ collegamenti. Ogni pacchetto ha una lunghezza di $L$ bit e i collegamenti trasmettono a una velocità di $R$ bit al secondo.
> Se prendiamo lo scenario di invio di un singolo pacchetto vedremo che:
> 1. All'istante $t = 0$, la sorgente comincia a trasmettere il pacchetto.
> 2. All'istante $t = L/R$, l'intero pacchetto è stato trasmesso dalla sorgente e immagazzinato nel buffer del router. Soltanto a questo punto il router può iniziare a inoltrarlo sul secondo collegamento.
> 3. All'istante $t = 2L/R$, il pacchetto è stato completamente trasmesso dal router e ricevuto dal destinatario (trascurando i ritardi di propagazione). Il ritardo totale end-to-end per un pacchetto risulta quindi pari a $2 \cdot \frac{L}{R}$.
> 
> Nel caso di una sequenza di pacchetti (supponiamo che questi siano 3) troveremo che:
> - Il $1^\circ$ pacchetto viene completato a destinazione all'istante $2L/R$.
> - Il $2^\circ$ pacchetto arriva a destinazione all'istante $3L/R$.
> - Il $3^\circ$ pacchetto arriva a destinazione all'istante $4L/R$.
> 
> Più in generale, per inviare un singolo pacchetto su un percorso di $N$ collegamenti identici a velocità $R$, il ritardo complessivo da un capo all'altro è dato dalla formula $$N \cdot \frac{L}{R}$$

Ogni commutatore di pacchetto connette più collegamenti e per ciascuno di questi il commutatore mantiene un **buffer di output** per conservare i pacchetti che sta per inviare su quel collegamento. I buffer di output rivestono un ruolo chiave nella commutazione di pacchett, infatti un pacchetto in arrivo che debba essere inviato attraverso un collegamento occupato dalla trasmissione di un altro, deve attendere nella coda di output. Di conseguenza, oltre ai ritardi store-and-forward, i pacchetti subiscono anche **ritardi di accodamento** nei buffer di output. Tali ritardi sono variabili e dipendono dal livello di traffico nella rete. Dato che la dimensione del buffer è finita, un pacchetto in arrivo può trovare il buffer completamente riempito da altri pacchetti che attendono la trasmissione, creando il fenomeno della **perdita di pacchetto (packet loss)**, ossia verrà eliminato o il pacchetto in arrivo o uno di quelli che si trova già in coda.

> [!example] Esempio di rete a pacchetto completa
> ![[Pasted image 20261016162651.png]]
### Commutazione a circuito
Nelle reti a commutazione di circuito le risorse richieste lungo un percorso (buffer e velocità di trasmissione sui collegamenti) per consentire la comunicazione tra sistemi periferici sono riservate per l’intera durata della sessione di comunicazione. Questa cosa, come visto precedentemente, non accade nelle reti a commutazione di pacchetto


> [!example] Esempio di rete a circuito
> ![[Pasted image 20261016163148.png]]
> La figura mostra una rete a commutazione di circuito in cui i quattro commutatori sono interconnessi tramite quattro collegamenti. Ciascuno di questi ultimi dispone di quattro circuiti, in modo che ogni collegamento possa supportare quattro connessioni simultanee. Gli host (per esempio PC e workstation) sono tutti direttamente connessi a uno dei commutatori. 
> Quando due host desiderano comunicare la rete stabilisce una **connessione end-to-end** (o connessione punto a punto) dedicata a loro. Affinché A invii messaggi a B, la rete deve prima riservare un circuito su ciascuno dei due collegamenti. Nell’esempio, la connessione punto a punto usa il secondo circuito del primo collegamento e il quarto circuito del secondo. Poiché ogni collegamento ospita quattro circuiti, per ogni collegamento utilizzato dalla connessione punto a punto la connessione ottiene un quarto della capacità trasmissiva totale del collegamento per la durata della connessione stessa














> [!example] FDM vs TDM
> ![[Pasted image 20261006192732.png]]

> [!example] Esempio di rete a commutatore di pacchetto
> ![[Pasted image 20261006193343.png]]


> [!example] Esempio di trasmissione store and forward
> ![[Pasted image 20261006193844.png]]

> [!example] Esempio di rete a commutatore di pacchetto
> ![[Pasted image 20261006194149.png]]


