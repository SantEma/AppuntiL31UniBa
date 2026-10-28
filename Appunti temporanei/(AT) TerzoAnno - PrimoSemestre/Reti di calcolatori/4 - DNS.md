Gli host all’interno di Internet possono essere identificati in vari modi. 
Gli **hostname**, quali `www.facebook.com`, `www.google.com` e `gaia.cs.umass.edu`, risultano intuitivi per le persone, ma forniscono ben poca informazione sulla loro collocazione all’interno di Internet. Per tali motivi, gli host vengono identificati anche dai cosiddetti **indirizzi IP**.

**Un indirizzo IP**(v4) consiste di quattro byte e presenta una rigida struttura gerarchica. Ha una forma del tipo `121.7.106.83`, in cui ogni punto separa uno dei byte espressi con un numero decimale compreso tra 0 e 255. Un indirizzo IP è gerarchico perché, leggendolo da sinistra a destra, otteniamo informazioni sempre più specifiche sulla collocazione dell’host in Internet (ossia sulla rete di cui fa parte all’interno della rete di reti).

### Servizio fornito dal DNS
Al fine di conciliare hostname e indirizzi IP è necessario un servizio in grado di tradurre i primi nei secondi.
Si tratta del **DNS (Domain Name System)**, che è composto da:
1. Un database distribuito implementato in una gerarchia di **DNS server**;
2. Un protocollo a livello di applicazione che consente agli host di interrogare il database.

Il DNS è specificato negli RFC 1034 e 1035 e viene comunemente utilizzato da altri protocolli a livello di applicazione, incluso HTTP, per tradurre i nomi di host forniti dall’utente in indirizzi IP.
Opera sulla porta 53 del protocollo UDP, ma in alcuni casi può utilizzare TCP, ad esempio:
- **Risposte troppo grandi**: su UDP il messaggio DNS è limitato a 512 byte (se la risposta non ci sta, il server la tronca e imposta il flag TC (truncated), con conseguente ripetizione da parte del client su TCP).
- **Zone transfer**: il server autoritativo secondario (es. `dns2.networkutopia.com`) copia tutti i record della zona dal primario (`dns1`).

L’RFC 6891, chiamato **Extension Mechanisms for DNS** (EDNS), permette risposte UDP più grandi.

> [!example] Esempio di risoluzione DNS in un browser
> Consideriamo che cosa succede quando un browser (ossia un client HTTP) in esecuzione sull’host di un utente richiede l’URL `www.someschool.edu/index.html`. L’host dell’utente, per essere in grado di inviare un messaggio di richiesta HTTP al web server `www.someschool.edu`, deve come prima cosa ottenere il suo indirizzo IP. Ciò avviene come segue:
> 1. La stessa macchina utente esegue il lato client dell’applicazione DNS.
> 2. Il browser estrae il nome dell’host, `www.someschool.edu`, dall’URL e lo passa al lato client dell’applicazione DNS.
> 3. Il client DNS invia un’interrogazione (query) contenente l’hostname a un DNS server.
> 4. Il client DNS prima o poi riceve una risposta, che include l’indirizzo IP corrispondente all’hostname.
> 5. Una volta ricevuto l’indirizzo IP dal DNS, il browser può dare inizio a una connessione TCP verso il processo server HTTP collegato alla porta 80 di quell’indirizzo IP.

Oltre alla traduzione degli hostname in indirizzi IP, il DNS mette a disposizione altri importanti servizi:
- **Host aliasing**: un host dal nome complicato può avere uno o più sinonimi (alias). Per esempio, `relay1.west-coast.enterprise.com` potrebbe avere, diciamo, due sinonimi quali `enterprise.com` e `www.enterprise.com`. In questo caso, si dice che il nome `relay1.west-coast.enterprise.com` è un **hostname canonico**. I sinonimi, se presenti, sono generalmente più facili da ricordare rispetto ai nomi canonici. Il DNS può essere invocato da un’applicazione per ottenere l’hostname canonico di un sinonimo, così come l’indirizzo IP dell’host.
- **Mail server aliasing**: per ovvi motivi è fortemente auspicabile che gli indirizzi di posta elettronica siano facili da ricordare. Per esempio, se Bob ha un account Yahoo, il suo indirizzo di posta elettronica potrebbe essere semplicemente `bob@yahoo.com`. Tuttavia, l’hostname del server di posta Yahoo è molto più complicato e assai meno facile da ricordare rispetto a `yahoo.com`. Per esempio, il nome canonico potrebbe assomigliare a `relay1.west-coast.yahoo.com`. Un’applicazione di posta può invocare il DNS per ottenere il nome canonico di un sinonimo fornito, così come l’indirizzo IP dell’host. Infatti, il record MX (che vedremo più avanti) permette al server di posta di una società e al web server di avere hostname (alias) identici. Per esempio, il web server di un’azienda e il suo mail server possono essere entrambi chiamati `enterprise.com`.
- **Distribuzione del carico di rete (load distribution)**: il DNS viene anche utilizzato per distribuire il carico tra server replicati, per esempio dei web server. I siti con molto traffico vengono replicati su più server, ognuno eseguito su un host diverso con un indirizzo IP differente. Nel caso di web server replicati, va dunque associato a ogni hostname canonico un insieme di indirizzi IP. Il database DNS contiene questo insieme di indirizzi. Quando i client effettuano una query DNS per un nome associato a un insieme di indirizzi, il server risponde con l’intero insieme di indirizzi, ma ne varia l’ordinamento a ogni risposta. Dato che generalmente un client invia il suo messaggio di richiesta HTTP al primo indirizzo IP elencato nell’insieme, la rotazione DNS distribuisce il traffico sui server replicati. La rotazione viene anche utilizzata affinché più server di posta possano condividere lo stesso nome. Inoltre, le società di distribuzione di contenuti quali Akamai hanno fatto uso del DNS in modi più sofisticati per effettuare la diffusione di contenuti web (si veda la sezione sulle CDN nel file [[3 - Web]]).
### Panoramica di funzionamento del DNS
Il DNS utilizza un grande numero di server, organizzati in maniera gerarchica e distribuiti nel mondo. Nessun DNS server ha le corrispondenze per tutti gli host in Internet, che sono invece distribuite tra tutti i DNS server. In prima approssimazione, esistono tre classi di DNS server: i **root server**, i **top-level domain (TLD) server** e i **server autoritativi**, organizzati in una gerarchia:

> [!info] Schema della gerarchia DNS
> ![[Pasted image 20261027152256.png]]
> Più approfonditamente:
> - **Root server**: in Internet esistono più di 1000 root server, dislocati in tutto il mondo. Tali root server sono copie di 13 differenti root server gestiti da 12 diverse organizzazioni, coordinate attraverso la Internet Assigned Numbers Authority (IANA 2020). I root server forniscono gli indirizzi IP dei TLD server.
> - **Top-level domain (TLD) server**: questi server si occupano dei domini di primo livello quali `com`, `org`, `net`, `edu` e `gov`, e di tutti i domini di primo livello relativi ai vari paesi, come `it`, `uk`, `fr`, `ca` e `jp`. I TLD server forniscono gli indirizzi IP dei server autoritativi.
> - **DNS server autoritativi**: ogni organizzazione dotata di host pubblicamente accessibili tramite Internet (quali web server e mail server) deve fornire record DNS pubblicamente accessibili che associno i nomi di tali host a indirizzi IP. Il DNS server autoritativo dell’organizzazione ospita questi record. Un’organizzazione può scegliere di implementare il proprio server autoritativo o di pagare un fornitore di servizi per ospitare questi record su un suo server. La maggior parte delle università e delle grandi società implementa e gestisce dei propri server autoritativi primario e secondario (di backup).
>   
> Esiste un altro importante tipo di DNS server, detto **DNS server locale**, che non appartiene strettamente alla gerarchia di server, ma che è comunque centrale nell’architettura DNS. Ciascun ISP, come un’università, un dipartimento universitario, un’azienda o un ISP residenziale, ne ha uno nella sua rete . Quando un host si connette a un ISP, quest’ultimo gli fornisce un indirizzo IP tratto da uno o più dei suoi DNS server locali e, quando l’host effettua una richiesta DNS, questa viene inoltrata esattamente al server locale, che opera da proxy e inoltra la query alla gerarchia dei server DNS.

> [!example] Esempio di gerarchia di DNS
> Consideriamo un semplice esempio. Supponiamo che l’host `cse.nyu.edu` voglia l’indirizzo IP di `gaia.cs.umass.edu`. Supponiamo, inoltre, che il DNS server locale per `cse.nyu.edu` sia `dns.nyu.edu`, mentre un server autoritativo per `gaia.cs.umass.edu` sia `dns.umass.edu`. Come mostrato nell’immagine, l’host `cse.nyu.edu` dapprima invia un messaggio di richiesta DNS al proprio server locale `dns.nyu.edu`. Il messaggio contiene il nome da tradurre, ossia `gaia.cs.umass.edu`. Il server locale inoltra il messaggio di richiesta a un root server. Quest’ultimo prende nota del suffisso `edu` e restituisce al server locale un elenco di indirizzi IP per i TLD server responsabili di `edu`. Il server locale rinvia quindi il messaggio di richiesta a uno di questi ultimi. Il TLD server prende nota del suffisso `umass.edu` e risponde con l’indirizzo IP del server autoritativo per l’Università del Massachusetts, ossia `dns.umass.edu`. Infine, il DNS server locale rimanda il messaggio di richiesta direttamente a `dns.umass.edu`, che risponde con l’indirizzo IP di `gaia.cs.umass.edu`. Si noti che in questo esempio, per ottenere la mappatura di un hostname, sono stati inviati ben **otto messaggi DNS**: quattro messaggi di richiesta e quattro messaggi di risposta (se invece l’università avesse un server DNS intermedio che delega il sottodominio a un server autoritativo dipartimentale `dns.cs.umass.edu`, verrebbero inviati **dieci messaggi DNS**).
> ![[Pasted image 20261027161634.png]]
> L’esempio mostrato nell’immagine fa uso sia di **query ricorsive** sia di **query iterative**. La richiesta inviata da `cse.nyu.edu` a `dns.nyu.edu` è ricorsiva, in quanto richiede a `dns.nyu.edu` di ottenere l’associazione per conto del richiedente. Le successive tre richieste sono invece iterative, dato che tutte le risposte sono restituite direttamente a `dns.nyu.edu`.
>
> ![[Pasted image 20261027163405.png]]
> In questo secondo schema vediamo invece una catena di query DNS interamente ricorsive.
> Nella pratica, la risoluzione DNS avviene di norma in modalità ricorsiva dall’host richiedente al server DNS locale, mentre le restanti richieste (dal server locale verso i server della gerarchia) sono iterative.

### DNS caching
Il DNS sfrutta in modo estensivo il **caching** per migliorare le prestazioni di ritardo e per ridurre il numero di messaggi DNS che “rimbalzano” su Internet. L’idea alla base del DNS caching è molto semplice: in una concatenazione di richieste, il DNS server che riceve una risposta DNS (contenente, per esempio, la traduzione da hostname a indirizzo IP) può mettere in cache le informazioni contenute.

> [!example] Esempio di DNS caching
> ![[Pasted image 20261027161634.png]]
> Nell’immagine, ogni volta che il server locale `dns.nyu.edu` riceve una risposta da qualche DNS server, può conservare in cache le informazioni contenute nella risposta. Se una coppia hostname/indirizzo IP è nella cache di un DNS server e giunge al server un’altra richiesta con lo stesso hostname, il DNS server può fornire l’indirizzo IP desiderato, anche se non è autoritativo per tale indirizzo. Dato che gli host e le associazioni tra nome e indirizzo IP non sono in alcun modo permanenti, i DNS server invalidano le informazioni in cache dopo un periodo di tempo fissato (in genere di due giorni). Supponiamo, per esempio, che un host `apricot.nyu.edu` richieda a `dns.nyu.edu` l’indirizzo IP dell’hostname `cnn.com`. Supponiamo, inoltre, che poche ore dopo un altro host della stessa organizzazione, per esempio `kiwi.nyu.edu`, faccia richiesta a `dns.nyu.edu` con lo stesso hostname. Grazie al caching, il DNS server locale sarà in grado di restituire immediatamente l’indirizzo IP di `cnn.com` a questo secondo host richiedente, senza dover interrogare alcun altro DNS server. Un DNS server locale può, inoltre, memorizzare in cache gli indirizzi IP dei TLD server, consentendogli di aggirare i root server nella catena di richieste (e ciò avviene di frequente).

### Record e messaggi DNS
I server che implementano il database distribuito di DNS memorizzano i cosiddetti **record di risorsa (RR, resource record)**, tra cui quelli che forniscono le corrispondenze tra nomi e indirizzi. Ogni messaggio di risposta DNS trasporta uno o più record di risorsa.

Un record di risorsa contiene i seguenti campi:
- **Name**: il nome di dominio o host a cui fa riferimento il record (es. `example.com` o `www.example.com`). Funziona come chiave di ricerca durante una risoluzione DNS.
- **Type**: specifica la natura della risorsa e determina come deve essere interpretato il valore. Esempi comuni includono **A** (indirizzo IPv4), **AAAA** (indirizzo IPv6), **CNAME** (alias di un altro nome), **MX** (server di posta) o **NS** (server autoritativo del dominio).
- **Value**: il dato concreto associato al nome, il cui formato dipende dal Type specificato (es. l’indirizzo IP `93.184.216.34` per un tipo A, oppure un nome canonico per un CNAME).
- **TTL (Time To Live)**: il tempo massimo (espresso in secondi) per cui il record può rimanere memorizzato nella cache di un resolver o server DNS locale prima di dover effettuare una nuova interrogazione al server autoritativo.

> [!info] Tabella dei tipi di record
> ![[Pasted image 20261027174412.png]]

#### Messaggio DNS
> [!info] Formato dei messaggi DNS
> ![[Pasted image 20261027172728.png]]
> La semantica dei campi dei messaggi DNS è la seguente:
> - I primi 12 byte rappresentano la **sezione di intestazione** (header section), che a sua volta contiene un certo numero di campi. Il primo è un numero di 16 bit che identifica la richiesta. Tale identificatore viene copiato nei messaggi di risposta a una richiesta, consentendo al client di far corrispondere le risposte ricevute con le query inviate. Troviamo poi il campo **flag**, suddiviso in diversi bit. Il primo di questi, il bit di richiesta/risposta (query/reply), indica se il messaggio è una richiesta (0) o una risposta (1). Un bit (authoritative) viene impostato nei messaggi di risposta quando il DNS server è autoritativo per il nome richiesto. Un ulteriore bit, chiamato **richiesta di ricorsione** (recursion-desired flag), viene impostato quando un client (sia esso un host o un DNS server) desidera che il DNS server effettui ricorsione quando non dispone del record. Si imposta un campo da 1 bit di **ricorsione disponibile** (recursion-available) all’interno di una risposta se il DNS server supporta la ricorsione. Nell’intestazione troviamo anche quattro campi il cui nome comincia con “numero di”, che indicano il numero di occorrenze delle quattro sezioni di tipo dati che seguono l’intestazione.
> - La **sezione delle domande** contiene informazioni sulle richieste che stanno per essere effettuate. Inoltre, include:
> 	- un campo **nome** con il nome che sta per essere richiesto;
> 	- un campo **tipo** che indica il tipo della domanda sul nome: per esempio, un indirizzo di host associato a un nome (tipo A) o il server di posta per un nome (tipo MX).
> - In una risposta proveniente da un DNS server, la **sezione delle risposte** contiene i record di risorsa relativi al nome originariamente richiesto. Ricordiamo che in ogni record di risorsa troviamo Name, Type (A, NS, CNAME o MX), Value e TTL. Una risposta può restituire più RR, dato che un hostname può avere più indirizzi IP.
> - La **sezione autoritativa** contiene i record di altri server autoritativi.
> - La **sezione aggiuntiva** racchiude altri record utili: per esempio, se il campo di risposta relativo a una richiesta MX contiene un record di risorsa che fornisce l’hostname canonico del server di posta, la sezione aggiuntiva contiene un record di tipo A che fornisce l’indirizzo IP relativo all’hostname canonico del server di posta.

> [!info] Inserimento di record nel database DNS
> ![[Pasted image 20261027181000.png]]

### Esempi di DNS

> [!example] Trace di una query DNS con `dig`
> Tramite il comando `dig +trace +nodnssec www.facebook.com` è possibile osservare l’intera catena di risoluzione iterativa a partire dalla radice della gerarchia DNS (l’opzione `+nodnssec` omette i record crittografici DNSSEC per rendere l’output più compatto):
> ```bash
> $ dig +trace +nodnssec [www.facebook.com](https://www.facebook.com)
> 
> ; <<>> DiG 9.10.6 <<>> +trace +nodnssec [www.facebook.com](https://www.facebook.com)
> ;; global options: +cmd
> .                       55548   IN  NS    m.root-servers.net.
> .                       55548   IN  NS    b.root-servers.net.
> .                       55548   IN  NS    c.root-servers.net.
> .                       55548   IN  NS    d.root-servers.net.
> ...
> .                       55548   IN  NS    l.root-servers.net.
> ;; Received 239 bytes from 8.8.8.8#53(8.8.8.8) in 24 ms
> 
> com.                    172800  IN  NS    m.gtld-servers.net.
> com.                    172800  IN  NS    f.gtld-servers.net.
> com.                    172800  IN  NS    e.gtld-servers.net.
> com.                    172800  IN  NS    j.gtld-servers.net.
> com.                    172800  IN  NS    b.gtld-servers.net.
> com.                    172800  IN  NS    g.gtld-servers.net.
> ...
> ;; Received 844 bytes from 199.9.14.201#53(b.root-servers.net) in 185 ms
> 
> facebook.com.           172800  IN  NS    a.ns.facebook.com.
> facebook.com.           172800  IN  NS    b.ns.facebook.com.
> facebook.com.           172800  IN  NS    e.ns.facebook.com.
> facebook.com.           172800  IN  NS    c.ns.facebook.com.
> ;; Received 288 bytes from 192.52.178.30#53(k.gtld-servers.net) in 38 ms
> 
> [www.facebook.com](https://www.facebook.com).       3600    IN  CNAME star-mini.c10r.facebook.com.
> ;; Received 74 bytes from 129.134.30.12#53(a.ns.facebook.com) in 20 ms
> ```
> Analizziamo l’esecuzione passo passo:
> 1. **Ottenimento dei root server dal resolver di default**: come primo passo, il resolver locale configurato sulla macchina (`8.8.8.8`, ossia Google Public DNS, interrogato sulla porta `53`) restituisce in 24 ms la lista dei **13 root server** (da `a.root-servers.net.` a `m.root-servers.net.`).
> 2. **La radice della gerarchia (`.`)**: nella prima colonna dei record restituiti, il singolo punto `.` rappresenta la **radice** (root) della gerarchia dei nomi DNS, a cui sono associati i record di tipo `NS` dei root server.
> 3. **Selezione del root server**: tra i 13 root server disponibili, `dig` seleziona e contatta il root server **b** (`b.root-servers.net`, indirizzo IP `199.9.14.201`) poiché è quello con **RTT minore**.
> 4. **Delega al dominio `.com` e TTL**: il root server `b.root-servers.net` esamina il suffisso `.com` e risponde (844 byte in 185 ms) indicando i record `NS` dei server autoritativi per il dominio di primo livello `com.`, ciascuno associato a un **TTL di 172800 secondi** (pari esattamente a **due giorni**, ossia 48 ore).
> 5. **Server gTLD**: i server restituiti (`a.gtld-servers.net.` … `m.gtld-servers.net.`) sono i **gTLD** (generic Top-Level Domain) server che gestiscono il dominio generico `.com`.
> 6. **Delega ai server autoritativi di Facebook e load balancing**: `dig` interroga uno dei server gTLD (`k.gtld-servers.net`, indirizzo IP `192.52.178.30`), che risponde in 38 ms rimandando ai **quattro name server di Facebook** (`a.ns.facebook.com.`, `b.ns.facebook.com.`, `e.ns.facebook.com.`, `c.ns.facebook.com.`). Questi sono i **server autoritativi** per il dominio `facebook.com.` e vengono restituiti ordinati in modo da garantire il **load balancing** (distribuzione del carico).
> 7. **Risposta del server autoritativo (record CNAME)**: infine viene interrogato il primo server autoritativo della lista (`a.ns.facebook.com`, indirizzo IP `129.134.30.12`), che risponde in 20 ms con un record di tipo **CNAME** (con TTL di 3600 secondi, ossia un’ora): `www.facebook.com.` è infatti un **alias** che punta al nome canonico `star-mini.c10r.facebook.com.`.

> [!example] Zone transfer con `dig`
> Tramite il comando `dig axfr zonetransfer.me @nsztm1.digi.ninja` è possibile richiedere un trasferimento di zona completo (zone transfer) per il dominio (d'esempio) `zonetransfer.me`:
> ```bash
> $ dig axfr zonetransfer.me @nsztm1.digi.ninja
> 
> zonetransfer.me.          7200    IN  SOA   nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
> zonetransfer.me.          7200    IN  NS    nsztm1.digi.ninja.
> zonetransfer.me.          7200    IN  NS    nsztm2.digi.ninja.
> zonetransfer.me.          7200    IN  MX    0 ASPMX.L.GOOGLE.COM.
> zonetransfer.me.          7200    IN  MX    10 ALT1.ASPMX.L.GOOGLE.COM.
> ; (altri 5 record MX)
> zonetransfer.me.          7200    IN  A     5.196.105.14
> www.zonetransfer.me.      7200    IN  A     5.196.105.14
> testing.zonetransfer.me.  301     IN  CNAME www.zonetransfer.me.
> vpn.zonetransfer.me.      4000    IN  A     174.36.59.154
> owa.zonetransfer.me.      7200    IN  A     207.46.197.32
> internal.zonetransfer.me. 300     IN  NS    intns1.zonetransfer.me.
> contact.zonetransfer.me.  2592000 IN  TXT   "Remember to call or email Pippa on +44 123 4567890..."
> ; (altri record: TXT, AAAA, SRV, LOC, DNSKEY, ...)
> zonetransfer.me.          7200    IN  SOA   nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
> ;; XFR size: 52 records (messages 1, bytes 3311)
> ```
> Analizziamo l’esecuzione passo passo:
> 8. **Query AXFR su TCP**: la query di tipo **AXFR** (Authoritative Zone Transfer) richiede la copia dell’intera zona `zonetransfer.me` direttamente al suo server autoritativo `@nsztm1.digi.ninja`. Poiché il trasferimento di zona comporta l’invio di una grande quantità di dati in modo affidabile, `dig` invia la richiesta su **TCP** anziché su UDP.
> 9. **Formato dei Resource Record e record SOA di apertura**: ogni riga dell’output è un **record di risorsa** composto da cinque campi: nome, TTL (in secondi), classe (`IN`, ossia Internet), tipo e valore. Il primo record è il **SOA** (Start of Authority), che specifica il server primario della zona (`nsztm1.digi.ninja.`), la casella email del responsabile (`robin.digi.ninja.`, che corrisponde a `robin@digi.ninja`), il numero di serie (`2019100801`, usato dai server secondari per verificare eventuali aggiornamenti) e i quattro parametri temporali espressi in secondi (*refresh* `172800`, *retry* `900`, *expire* `1209600` e *TTL minimo* `3600`).
> 10. **Record NS, MX, A e CNAME**: seguono i record operativi del dominio:
> 	- **NS**: elenca i server autoritativi della zona (`nsztm1.digi.ninja.` e `nsztm2.digi.ninja.`).
> 	- **MX**: indica i server di posta del dominio; il valore numerico che precede l’hostname rappresenta la **priorità** (più basso è il numero, più alta è la preferenza: `0 ASPMX.L.GOOGLE.COM.` è il mail server principale, mentre `10 ALT1.ASPMX.L.GOOGLE.COM.` è il secondario).
> 	- **A** e **CNAME**: forniscono gli indirizzi IPv4 degli host (`5.196.105.14`) e gli alias (come `testing.zonetransfer.me.`, alias CNAME di `www.zonetransfer.me.`).
> 11. **Esposizione dei nomi interni e sicurezza dell’AXFR**: il trasferimento di zona rivela **tutti gli host e i sottodomini della zona, inclusi quelli non pubblicizzati** (`vpn.zonetransfer.me.`, `owa.zonetransfer.me.`, la delega interna `internal.zonetransfer.me.`) e perfino informazioni di contatto contenute nei record **TXT** (`"Remember to call or email Pippa..."`). Per evitare questa fuga di informazioni sulla struttura della rete interna, sui server DNS reali il trasferimento AXFR deve essere **concesso esclusivamente ai server autoritativi secondari**.
> 12. **Record SOA di chiusura**: l’elenco della zona si chiude ripetendo lo **stesso record SOA** iniziale, che segnala formalmente al client la fine del trasferimento.
> 13. **Dimensione complessiva e limite di UDP**: la riga conclusiva (`XFR size: 52 records (messages 1, bytes 3311)`) mostra che sono stati trasferiti **52 record in un unico messaggio DNS da 3311 byte**. Su UDP standard, dove i messaggi DNS sono limitati a **512 byte**, tale trasferimento non sarebbe stato possibile.

