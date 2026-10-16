## Internet come infrastruttura fisica
Internet è una rete di calcolatori che interconnette miliardi di dispositivi di calcolo in tutto il mondo.
In gergo, questi dispositivi sono detti **host** (ospite) o **sistemi periferici** (end system).

I vari sistemi periferici sono connessi tra loro tramite una **rete di collegamenti** (communication link) e **commutatori di pacchetti** (packet switch)

Collegamenti diversi possono trasmettere dati a velocità differenti, e **tale velocità (o tasso) di trasmissione (transmission rate) viene misurata in bit/secondo (bps).** Quando un sistema periferico vuole inviare dati a un altro sistema periferico, suddivide i dati in sottoparti e aggiunge un’intestazione a ciascuna di esse: l'insieme delle informazioni risultanti viene chiamato **pacchetto** (packet).
I pacchetti sono inviati attraverso la rete alla destinazione, dove vengono riassemblati per ottenere i dati originari. 

Un commutatore di pacchetto prende un pacchetto che arriva da uno dei collegamenti in ingresso e lo ritrasmette su uno di quelli in uscita. Esistono commutatori di pacchetto di varia forma e natura, ma i due principali nell'odierna Internet sono i **router** e i **commutatori a livello di collegamento** (link layer switch). I commutatori a livello di collegamento sono solitamente usati nelle reti di accesso, mentre i router nel nucleo della rete. Dal sistema di invio a quello di ricezione, la sequenza di collegamenti e commutatori di pacchetto attraversata da ogni singolo pacchetto è nota come **percorso** (route o path) attraverso la rete.

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
## Specifica dell'infrastruttura fisica di internet
[da finire]





> [!example] FDM vs TDM
> ![[Pasted image 20261006192732.png]]

> [!example] Esempio di rete a commutatore di pacchetto
> ![[Pasted image 20261006193343.png]]


> [!example] Esempio di trasmissione store and forward
> ![[Pasted image 20261006193844.png]]

> [!example] Esempio di rete a commutatore di pacchetto
> ![[Pasted image 20261006194149.png]]


