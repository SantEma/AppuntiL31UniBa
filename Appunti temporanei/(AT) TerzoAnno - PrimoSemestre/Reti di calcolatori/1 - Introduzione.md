## Internet come infrastruttura fisica
Internet è una rete di calcolatori che interconnette miliardi di dispositivi di calcolo in tutto il mondo.
In gergo, questi dispositivi sono detti **host** (ospite) o **sistemi periferici** (end system).

I vari sistemi periferici sono connessi tra loro tramite una **rete di collegamenti** (communication link) e **commutatori di pacchetti** (packet switch)

Collegamenti diversi possono trasmettere dati a velocità differenti, e **tale velocità (o tasso) di trasmissione (transmission rate) viene misurata in bit/secondo (bps).** Quando un sistema periferico vuole inviare dati a un altro sistema periferico, suddivide i dati in sottoparti e aggiunge un’intestazione a ciascuna di esse: l'insieme delle informazioni risultanti viene chiamato **pacchetto** (packet).
I pacchetti sono inviati attraverso la rete alla destinazione, dove vengono riassemblati per ottenere i dati originari. 

Un commutatore di pacchetto prende un pacchetto che arriva da uno dei collegamenti in ingresso e lo ritrasmette su uno di quelli in uscita. Esistono commutatori di pacchetto di varia forma e natura, ma i due principali nell’odierna Internet sono i **router** e i **commutatori a livello di collegamento** (link layer switch). I commutatori a livello di collegamento sono solitamente usati nelle reti di accesso, mentre i router nel nucleo della rete. Dal sistema di invio a quello di ricezione, la sequenza di collegamenti e commutatori di pacchetto attraversata da ogni singolo pacchetto è nota come **percorso** (route o path) attraverso la rete.

I sistemi periferici accedono a internet tramite i vari **Internet Service Provider (ISP)**, che comprandono vari generi (residenziali reti universitarie etc.).
Un provider è un insieme di commutatori di pacchetto e di collegamenti. Gli ISP forniscono ai sistemi periferici svariati tipi di accesso alla rete e rendono disponibile l’accesso a Internet ai fornitori di contenuti, connettendone i server a Internet. Poiché Internet non è altro che connettere tra loro gli end systems, anche gli ISP che forniscono accesso ai sistemi periferici devono essere interconnessi, tali provider di livello gerarchico più basso sono interconnessi a quelli nazionali e internazionali di livello più alto, che sono connessi tra di loro direttamente. Un ISP di livello superiore è costituito da router ad alta velocità interconnessi tramite fibra ottica. Ciascuna rete di un ISP, sia di alto sia di basso livello, è gestita in modo indipendente, fa uso del protocollo IP e si conforma a determinate convenzioni riguardo a nomi e indirizzi. 








> [!example] FDM vs TDM
> ![[Pasted image 20261006192732.png]]

> [!example] Esempio di rete a commutatore di pacchetto
> ![[Pasted image 20261006193343.png]]


> [!example] Esempio di trasmissione store and forward
> ![[Pasted image 20261006193844.png]]

> [!example] Esempio di rete a commutatore di pacchetto
> ![[Pasted image 20261006194149.png]]


