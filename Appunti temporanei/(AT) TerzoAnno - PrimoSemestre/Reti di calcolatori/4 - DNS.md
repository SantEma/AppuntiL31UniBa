Gli host all'interno di Internet possono essere identificati in vari modi. 
Gli **hostname** quali `www.facebook.com`, `www.google.com` e `gaia.cs.umass.edu`, risultano intuitivi per le persone, ma forniscono ben poca informazione sulla loro collocazione all'interno di Internet. Per tali motivi, gli host vengono identificati anche dai cosiddetti **indirizzi IP**.

**Un indirizzo IP**(v4) consiste di quattro byte e presenta una rigida struttura gerarchica. Ha una forma del tipo `121.7.106.83`, in cui ogni punto separa uno dei byte espressi con un numero decimale compreso tra 0 e 255. Un indirizzo IP è gerarchico perché, leggendolo da sinistra a destra, otteniamo informazioni sempre più specifiche sulla collocazione dell’host in Internet (ossia sulla sua appartenenza a quale rete all'interno della rete di reti).
### Servizio fornito dal DNS
Al fine di conciliare hostname e indirizzi IP è necessario un servizio in grado di tradurre il primo al secondo.
Si tratta del **DNS (Domain Name System)**, che è composto da:
1. Un database distribuito implementato in una gerarchia di **DNS server**;
2. Un protocollo a livello di applicazione che consente agli host di interrogare il database

DNS è specificato nelle RFC 1034, 1035 e viene comunemente utilizzato da altri protocolli a livello di applicazione, incluso HTTP, per tradurre i nomi di host forniti dall'utente in indirizzi IP.
Opera sulla porta 53 del protocollo UDP, ma in alcuni casi può utilizzare TCP, ad esempio:
- **Risposte troppo grandi**: su UDP il messaggio DNS è limitato a 512 byte (se la risposta non ci sta, il server la tronca e imposta il flag TC (truncated), con conseguente ripetizione dal parte del client su TCP)
- **Zone transfer**: server autoritativo secondario (es. `dns2.networkutopia.com`) copia tutti i record della zona dal primario (dns1).
L'RFC 6891, chiamata **Extension Mechanism for DNS** permette risposte UDP più grandi.

> [!example] Esempio di risoluzione DNS in un browser
> Consideriamo che cosa succede quando un browser (ossia un client HTTP) in esecuzione sull’host di un utente richiede l’URL `www.someschool.edu/index.html`. L’host dell’utente, per essere in grado di inviare un messaggio di richiesta HTTP al web server `www.someschool.edu`, deve come prima cosa ottenere il suo indirizzo IP. Ciò avviene come segue. 
> 1. La stessa macchina utente esegue il lato client dell’applicazione DNS. 
> 2. Il browser estrae il nome dell’host, `www.someschool.edu`, dall’URL e lo passa al lato client dell’applicazione DNS. 
> 3. Il client DNS invia una interrogazione (query) contenente l’hostname a un DNS server. 
> 4. Il client DNS prima o poi riceve una risposta, che include l’indirizzo IP corrispondente all’hostname. 
> 5. Una volta ricevuto l’indirizzo IP dal DNS, il browser può dare inizio a una connessione TCP verso il processo server HTTP collegato alla porta 80 di quell’indirizzo IP.


Oltre alla traduzione degli hostname in indirizzi IP, DNS mette a disposizione altri importanti servizi.:
- **Host aliasing**: un host dal nome complicato può avere uno o più sinonimi (alias). Per esempio, `relay1.west-coast.enterprise.com` potrebbe avere, diciamo, due sinonimi quali `enterprise.com` e `www.enterprise.com`. In questo caso, si dice che il nome `relay1.west-coast.enterprise.com` è un **hostname canonico**. I sinonimi, se presenti, sono generalmente più facili da ricordare rispetto ai nomi canonici. Il DNS può essere invocato da un’applicazione per ottenere l’hostname canonico di un sinonimo, così come l’indirizzo IP dell’host. 
- **Mail server aliasing**: per ovvi motivi è fortemente auspicabile che gli indirizzi di posta elettronica siano facili da ricordare. Per esempio, se Bob ha un account Hotmail, il suo indirizzo di posta elettronica potrebbe essere semplicemente `bob@yahoo.com`. Tuttavia, l’hostname del server di posta Hotmail è molto più complicato e assai meno facile da ricordare rispetto a `yahoo.com`. Per esempio, il nome canonico potrebbe assomigliare a `relay1.west-coast.yahoo.com`. Un’applicazione di posta può invocare il DNS per ottenere il nome canonico di un sinonimo fornito, così come l’indirizzo IP dell’host. Infatti, il record MX (che vedremo più avanti) permette al server di posta di una società e al web server di avere hostname (alias) identici. Per esempio, il web server di un’azienda e il suo mail server possono essere entrambi chiamati enterprise.com.
- **Distribuzione del carico di rete (load distribution)**: il DNS viene anche utilizzato per distribuire il carico tra server replicati, per esempio dei web server. I siti con molto traffico vengono replicati su più server, ognuno eseguito su un host diverso con un indirizzo IP differente. Nel caso di web server replicati, va dunque associato a ogni hostname canonico un insieme di indirizzi IP. Il database DNS contiene questo insieme di indirizzi. Quando i client effettuano una query DNS per un nome associato a un insieme di indirizzi, il server risponde con l’intero insieme di indirizzi, ma ne varia l’ordinamento a ogni risposta. Dato che generalmente un client invia il suo messaggio di richiesta HTTP al primo indirizzo IP elencato nell’insieme, la rotazione DNS distribuisce il traffico sui server replicati. La rotazione viene anche utilizzata affinché più server di posta possano condividere lo stesso nome. Inoltre, le società di distribuzione di contenuti quali Akamai hanno fatto uso del DNS in modi più sofisticati per effettuare la diffusione di contenuti web (si veda la sezione sulle CDN nel file [[3 - Web]]).

### Panoramica di funzionamento del DNS
 Il DNS utilizza un grande numero di server, organizzati in maniera gerarchica e distribuiti nel mondo. Nessun DNS server ha le corrispondenze per tutti gli host in Internet, che sono invece distribuite tra tutti i DNS server. In prima approssimazione, esistono tre classi di DNS server: i **root server, i top-level domain (TLD) server** e i **server autoritativi**, organizzati in una gerarchia:
 
> [!info] Schema della gerarchia DNS
> ![[Pasted image 20261027152256.png]]



