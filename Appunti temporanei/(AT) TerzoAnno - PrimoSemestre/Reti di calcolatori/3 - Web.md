Il Web è stata la prima applicazione Internet che ha catturato l’attenzione del pubblico, cambiando profondamente il modo di interagire all'interno e all'esterno degli ambienti di lavoro. Il Web è:
- un sistema **on-demand**, ossia i suoi contenuti sono fruibili a richiesta dell'utente;
- un sistema **distribuito**, ossia i documenti sono memorizzati su molteplici server remoti;
- un sistema **ipermediale**, ossia un sistema che permette l’accesso interattivo a collezioni di documenti di diverso tipo (testo, figure, immagini, animazioni, audio, video) collegati tra loro.
## HTTP
**HTTP (HyperText Transfer Protocol)**, protocollo a livello di applicazione del Web (definito nelle RFC 1945, RFC 9112 e RFC 9113, corrispondenti rispettivamente a HTTP/1.0, HTTP/1.1 e HTTP/2), costituisce il cuore del Web. Questo protocollo è implementato in due programmi, **client e server**, in esecuzione su sistemi periferici diversi che comunicano tra loro scambiandosi messaggi HTTP. Il protocollo definisce sia la struttura dei messaggi sia la modalità con cui client e server si scambiano i messaggi. 

> [!example] Comportamento richiesta-risposta HTTP
> ![[Pasted image 20261019174548.png]]

Una **pagina web (web page)**, detta anche documento, è costituita da oggetti. Un **oggetto (object)** è semplicemente un file (quale un file HTML, un’immagine JPEG, un file JavaScript, un foglio di stile CSS, una clip video e così via) indirizzabile tramite un URL. La maggior parte delle pagine web è composta da un file **HTML principale** (base HTML file) e da diversi oggetti referenziati da esso.

Il file HTML referenzia gli altri oggetti nella pagina tramite il loro URL. Ogni URL ha due componenti: il nome dell’host del server che ospita l’oggetto e il percorso dell’oggetto.

> [!example] Esempio di URL
> Per esempio, l’URL `http://www.someSchool.edu/someDepartment/picture.gif` ha `www.someSchool.edu` come nome dell’host e `/someDepartment/picture.gif` come percorso.

Un **browser web** implementa il lato client di HTTP. Un **web server**, che implementa il lato server di HTTP, ospita oggetti web, indirizzabili tramite URL. 

HTTP definisce in che modo i client web richiedono le pagine ai web server e come questi ultimi le trasferiscono ai client. Quando l’utente richiede una pagina web (per esempio, cliccando su un collegamento ipertestuale), il browser invia al server messaggi di richiesta HTTP per gli oggetti nella pagina. Il server riceve le richieste e risponde con messaggi di risposta HTTP contenenti gli oggetti.

HTTP utilizza TCP come protocollo di trasporto. Il client HTTP per prima cosa inizia una connessione TCP con il server e, una volta stabilita, i processi client e server accedono a TCP attraverso le proprie socket. Il client invia richieste e riceve risposte HTTP tramite la propria interfaccia socket; analogamente il server riceve richieste e invia messaggi di risposta attraverso la propria interfaccia socket. Quando il client ha mandato un messaggio alla sua interfaccia socket, questo non è più in suo possesso, ma si trova “nelle mani” di TCP, che mette a disposizione un servizio di trasferimento dati affidabile; ciò implica che ogni messaggio di richiesta HTTP emesso da un processo client arriverà intatto al server e viceversa. Questo è uno dei grandi vantaggi di un’architettura organizzata a livelli; infatti HTTP non si deve preoccupare dei dati smarriti o di come TCP recuperi le perdite o riordini i dati all'interno della rete, ma delega tutto ai livelli inferiori.

È importante notare che il server invia i file richiesti ai client senza memorizzare alcuna informazione di stato a proposito del client, per cui, in caso di ulteriore richiesta dello stesso oggetto da parte dello stesso client, il server procederà nuovamente all’invio, non avendo mantenuto alcuna traccia dell'invio precedentemente effettuato. Dato che i server HTTP non mantengono informazioni sui client, HTTP è classificato come **protocollo senza memoria di stato (stateless protocol)**.
### Connessioni persistenti e non
In molte applicazioni per Internet, client e server comunicano per un lungo periodo di tempo, con il client che inoltra una serie di richieste e il server che risponde a ciascuna di esse. 
A seconda dell’applicazione e del suo impiego la serie di richieste potrebbe essere effettuata in sequenza, periodicamente a intervalli regolari o in maniera intermittente.
Essendo questa comunicazione in TCP, chi sviluppa le app ha due possibilità di implementazione su ciascuna coppia di richiesta-risposta:
- Trasmetterle su una connessione TCP separata (**connessione non persistente**)
- Inviarle tutte sulla stessa connessione TCP (**connessione persistente**)

> [!example] Esempio di connessione non persistente
> Supponiamo esista una pagina web, che consista di un file HTML principale e di 10 immagini JPEG, con tutti gli undici oggetti presenti sullo stesso server. Ipotizziamo che l’URL del file HTML principale sia `http://www.someSchool.edu/someDepartment/home.index`. 
> Seguendo il trasferimento della pagina web dal server al client:
> 1. Il processo client HTTP inizializza una connessione TCP con il server `www.someSchool.edu` sulla porta 80, che è la porta di default per HTTP. Associate alla connessione TCP ci saranno una socket per il client e una per il server. 
> 2. Il client HTTP, tramite la propria socket, invia al server un messaggio di richiesta HTTP che include il percorso `/someDepartment/home.index.` 
> 3. Il processo server HTTP riceve il messaggio di richiesta attraverso la propria socket associata alla connessione, recupera l’oggetto `/someDepartment /home.index` dalla memoria (centrale o di massa), lo incapsula in un messaggio di risposta HTTP che viene inviato al client attraverso la socket. 
> 4. Il processo server HTTP comunica a TCP di chiudere la connessione. Questo, però, non termina la connessione finché non sia certo che il client abbia ricevuto integro il messaggio di risposta. 
> 5. Il client HTTP riceve il messaggio di risposta. La connessione TCP termina. Il messaggio indica che l’oggetto incapsulato è un file HTML. Il client estrae il file dal messaggio di risposta, esamina il file HTML e trova i riferimenti ai 10 oggetti JPEG. 
> 6. Vengono quindi ripetuti i primi quattro passi per ciascuno degli oggetti JPEG referenziati.

Questo è il comportamento della versione HTTP/1.0.
Alcuni browser possono essere configurati dagli utenti per controllare il grado di parallelismo. In modalità di default, la maggior parte dei browser apre da 5 a 10 connessioni TCP parallele, ognuna delle quali gestisce una transazione di richiesta-risposta. Se lo desidera, l’utente può impostare il numero di connessioni parallele a 1, caso in cui le 10 connessioni vengono stabilite in serie.

> [!info] RTT (Rount Trip Time)
> ![[Pasted image 20261022150629.png]]
> L'RTT (Round Trip Time) rappresenta il tempo impiegato da un piccolo pacchetto per viaggiare dal client al server, per poi tornare al client.
> Poiché la richiesta e la risposta viaggiano sulla stessa connessione, il tempo totale per ricevere un oggetto è pari a **2 RTT più il tempo di trasmissione dell'oggetto da parte del server**: un RTT è necessario per stabilire la connessione TCP (handshake) e un RTT per l'invio della richiesta e la ricezione della risposta. Se la pagina è composta da più oggetti, viene aperta una nuova connessione per ciascuno di essi: ogni oggetto scaricato impone quindi un sovraccarico al server (che deve gestire ogni volta una nuova connessione TCP) e subisce un tempo di setup aggiuntivo di 2 RTT.

> [!info] Connessioni persistenti
> Con HTTP 1.1, nelle connessioni persistenti il server **mantiene aperta la connessione TCP** dopo aver inviato una risposta, così che le successive richieste e risposte tra lo stesso client e lo stesso server possano transitare sulla medesima connessione.
> Queste richieste di oggetti possono essere effettuate una di seguito all’altra senza aspettare le risposte delle richieste pendenti (**pipelining**, comportamento di default). In generale, il server HTTP chiude la connessione quando essa rimane inattiva per un dato lasso di tempo (un intervallo configurabile). Quando il server riceve richieste in sequenza, invia gli oggetti con la stessa modalità. .
### Formato dei messaggi HTTP
Le specifiche HTTP includono la definizione dei due formati di messaggi, richiesta e risposta
#### URI e URL
Prima di continuare con i vari formati dei messaggi, definiamo cosa sono URI e URL

> [!info] Definizione di URI
> Lo **Uniform Resource Identifier** è una sequenza di caratteri che identifica universalmente ed univocamente una risorsa
> La RFC3986 definisce con precisione le tre parti dell'acronimo, e la loro finalità:
> - **Uniform**: consiste nella identità del contenuto al variare delle forme in cui è presentato. Ad esempio, un libro cartaceo o l'ebook corrispondente. Dà parecchi vantaggi: tipi diversi di identificatori di risorse possono essere usati nello stesso contesto, anche se i meccanismi usati per accedere a tali risorse possono essere diversi; è possibile usare il medesimo meccanismo di interpretazione semantica su tipi diversi di identificatori di risorse; è possibile aggiungere nuovi tipi di risorse senza interferire con il modo in cui gli identificatori già esistenti sono usati; gli identificatori possono essere riutilizzati in diversi contesti, pertanto nuove applicazioni o protocolli possono usare gli identificatori già esistenti.
> - **Resource**: Una risorsa è tutto ciò che ha una identità. Non tutte le risorse tuttavia sono reperibili in rete: ad esempio un essere umano, un libro o un'azienda. Più precisamente, una risorsa è la mappatura concettuale di un'entità (o di un insieme di entità), e non è necessariamente legata all'entità in un determinato istante: infatti la risorsa può restare costante anche se le entità che la compongono cambiano nel corso del tempo, a patto che resti costante il legame concettuale (si pensi alla risorsa "Wikipedia": nonostante l'entità cambi nel tempo, poiché il contenuto del sito cambia, la risorsa che concettualmente identifica "Wikipedia" rimane la stessa).
> - **Identifier**: Un identificatore è un oggetto che viene impiegato come riferimento a qualcosa che ha un'identità. Nel caso della URI, tale oggetto è una sequenza di caratteri con una sintassi ben definita.

> [!info] Definizione di URL
> Un URL è un URI che identifica una risorsa tramite la sua "collocazione" ("location") in un grafo. Di fatto, non identifica la risorsa per nome, ma con il modo con cui la si può reperire. Per esempio, l'URL `http://www.example.com/)` è un URI che identifica una risorsa (l'home page di un sito web) e lascia intendere che una rappresentazione di tale risorsa (il codice HTML della versione corrente di tale home page) è ottenibile via HTTP da un host di rete chiamato `www.example.com/` .
> 
> Si forma con la seguente stringa:
> ```http
 >protocollo://[username[:password]@]host[:porta][</percorso>][?querystring][#fragment]
>```
>- **protocollo**: Identifica il protocollo, tipicamente di livello applicativo, da utilizzare per l'accesso al server. I protocolli più comuni sono HTTP, HTTPS, FTP, MMS ecc. Se il protocollo non viene specificato, generalmente il browser utilizza "http" come predefinito. In alcuni casi l'utente può vedere la pagina caricata con protocollo https, ma solo perché il web server è già stato contattato con protocollo http e ha risposto al browser con un codice di stato di reindirizzamento (3xx, di solito 301 o 302).
>- **://**: Separatore tra il protocollo e il resto dell'URL: di solito l'host, o opzionalmente lo username.
>- **username:password@** (opzionale): Subito dopo il protocollo, è possibile specificare le credenziali di autenticazione (username e password) per l'accesso alla risorsa. L'autenticazione in URL è però estremamente rischiosa, in quanto le credenziali vengono inviate al server in chiaro, ovvero non cifrate.
>- **host** — Identifica il server su cui risiede la risorsa. Può essere rappresentato direttamente da un indirizzo IP o (più comunemente) da un nome di dominio che il software converte in indirizzo IP avvalendosi del servizio DNS (che vedremo più avanti).
>- **porta** (opzionale): Identifica la porta del servizio di rete alla quale inoltrare la richiesta. Il numero di porta può essere omesso quando corrisponde alla porta standard associata al protocollo indicato dall'URL (ad esempio 80 per HTTP o 443 per HTTPS).
>- **percorso** (opzionale) — Percorso (pathname) nel file system del server che identifica la risorsa (generalmente una pagina web, una immagine o un file multimediale). Se il nome del file non viene specificato, il server può essere configurato per restituire un file predefinito.
>- **querystring** (opzionale) — Se richiesto, al termine dell'URL è possibile aggiungere una query string separandola con il simbolo "?". La query string è una stringa di caratteri che consente di passare al server uno o più parametri. Di norma, la query string ha questo formato: `?parametro1=valore&parametro2=valore2`.
>- **fragment** (opzionale) — Se presente, indica una parte o una posizione all'interno della risorsa. Come per la query string è possibile utilizzare più parametri, con la differenza che, essendo utilizzati dal client per sapere come muoversi all'interno di una risorsa, non verranno inviati al server. Di norma, il fragment ha questo formato: `#parametro1=valore&parametro2=valore2`.
#### Messaggio di richiesta HTTP
> [!info] Contenuto generale del messaggio di richiesta HTTP
> Ogni pacchetto di richiesta HTTP (in questo caso, versione 1.1) è formato in questa maniera:
> ![[Pasted image 20261022151312.png]]
> - **Riga di richiesta**, che presenta 3 campi al suo interno:
> 	- **Metodo**: **azione** che il client vuole eseguire sulla risorsa identificata dall'URL di destinazione. Può assumere valori tra `GET` (generalmente più utilizzato), `POST`, `HEAD`, `PUT` e `DELETE`.
> 	- **URL**: indica l'oggetto della richiesta.
> 	- **Versione**: versione HTTP che si intende utilizzare.
> - **Righe di intestazione**, le più comuni sono:
> 	- `Host`: nome del server a cui si riferisce l'URL. È obbligatorio nelle richieste conformi HTTP/1.1 perché permette l'uso dei virtual host basati sui nomi
> 	- `User-Agent`: identificazione del tipo di client, tipo browser, produttore, versione etc.
> 	- `Cookie`: utilizzati dalle applicazioni web per archiviare e recuperare informazioni a lungo termine sul lato client. Spesso usati per memorizzare un token di autenticazione o per tracciare le attività dell'utente.
> - **Riga vuota**: composta dai caratteri di ritorno a capo (**carriage return**) e di nuova linea (**line feed**).
> - **Corpo dell'entità**

> [!info] Metodi HTTP e campi di intestazione possibili
> ![[Pasted image 20261025164604.png]]
> ![[Pasted image 20261025164628.png]]

> [!example] Esempio di richiesta HTTP
> ```http
> GET /wiki/Pagina_principale HTTP/1.1
> Host: it.wikipedia.org
> User-Agent: Mozilla/5.0 (compatible; Konqueror/3.2; Linux) (KHTML, like Gecko)
> Accept: text/html, image/jpeg, image/png, text/*, image/*, */*
> Accept-Charset: iso-8859-1, utf-8;q=0.5, *;q=0.5 
> Accept-Language: it
> Connection: Keep-Alive
> ```

> [!example] Esempio di richiesta HTTP POST 
> ```http
> POST /wiki/Speciale:Preferenze HTTP/1.1
> Host: it.wikipedia.org
> User-Agent: Mozilla/5.0 (compatible; Konqueror/3.2; Linux) (KHTML, like Gecko)
> Accept: text/html, image/jpeg, image/png, text/*, image/*, */*
> Accept-Charset: iso-8859-1, utf-8;q=0.5, *;q=0.5 
> Accept-Language: it
> Connection: Keep-Alive
> Cache-control:no-cache
> Content-length:1291
> Content-type:application/x-www-form-urlencoded
> ```
#### Messaggio di risposta HTTP
> [!info] Contenuto generale del messaggio di risposta HTTP
> Ogni pacchetto di risposta HTTP (in questo caso, versione 1.1) è formato in questa maniera:
> ![[Pasted image 20261022152724.png]]
> - **Riga di stato**, che presenta 3 campi al suo interno:
> 	- **Versione**: versione HTTP utilizzata dal server per generare la risposta
> 	- **Codice di stato**: numero a 3 cifre che indica l'esito della richiesta. Possono essere di diverse classi come `1xx` informativo, `2xx` successo (`200 OK`), `3xx` redirezione (`301 Moved Permanently`), `4xx` errore del client (`404 Not Found`), `5xx` errore del server (`500 Internal Server Error`).
> 	- **Frase**: breve descrizione testuale del codice, destinata alla lettura umana
> - **Righe di intestazione**, le più comuni sono:
> 	- `Server`: indica il tipo e la versione del server. Può essere visto come l'equivalente dell'header di richiesta User-Agent.
> 	- `Content-Type`: indica il tipo di contenuto restituito. La codifica di tali tipi (detti Media type) è registrata presso lo IANA (Internet Assigned Number Authority); essi sono detti tipi MIME (Multipurpose Internet Mail Extensions), la cui codifica è descritta nel documento RFC 1521. Alcuni usuali tipi MIME incontrati in una risposta HTTP sono:
> 		- text/html: Documento HTML
> 		- text/plain: Documento di testo non formattato
> 		- text/xml: Documento XML
> 		- image/jpeg: Immagine di formato JPEG
> 	- `Content-Length`: dimensione del corpo
> 	- `Date`: data/ora della risposta
> 	- `Last-Modified`: ultima modifica della risorsa, usato per la cache
> - **Riga vuota**: composta dai caratteri di ritorno a capo (**carriage return**) e di nuova linea (**line feed**)
> - **Corpo dell'entità**

> [!example] Esempio di risposta HTTP
> ```http
> HTTP/1.1 200 OK
> Date: Fri, 22 Feb 2019 10:50:37 GMT
> Content-Type: text/html; charset=UTF-8
> Content-Length: 22208
> Connection: keep-alive
> Server: mw1215.eqiad.wmnet
> Content-language: it
> Content-Encoding: gzip
> Last-Modified: Fri, 22 Feb 2019 08:46:20 GMT
> Age: 20548
> Cache-Control: private, s-maxage=0, max-age=0, must-revalidate
> Vary: Accept-Encoding,Cookie,Authorization
> [...]
>  
> <!DOCTYPE html>
> <html class="client-nojs" lang="it" dir="ltr">
> <head>
> <meta charset="UTF-8"/>
> <title>Wikipedia, l'enciclopedia libera</title>
> [...]
> </body>
> </html>
> ```
### Cookies
Come detto precedentemente, i server HTTP sono privi di stato. Generalmente è auspicabile che i web server possano autenticare gli utenti sia per limitare l'accesso da parte di questi ultimi, sia per fornire contenuti in funzione della loro identità.

Grazie ai **cookies**, questo diventa possibile. La tecnologia dei cookies presenta quattro componenti principali:
1. Riga di intestazione nel messaggio di risposta HTTP; 
2. Riga di intestazione nel messaggio di richiesta HTTP;
3. File mantenuto sul sistema dell'utente e gestito dal browser
4. Database sul sito

> [!example] Esempio di utilizzo di cookie
> ![[Pasted image 20261023142333.png]]
> Susan, dal suo PC con Internet Explorer, visita `Amazon.com` per la prima volta (aveva già visitato eBay). Quando la sua richiesta HTTP arriva al server Amazon:
> - il sito genera un **identificatore univoco** (`1678`)
> - crea una **voce corrispondente nel proprio database** backend
> - risponde con un messaggio HTTP contenente l'intestazione `Set-cookie: 1678`
> 
> Il browser di Susan riceve la risposta, vede l'intestazione `Set-cookie:` e aggiunge una riga al suo **file cookie**, che ora contiene:
> ```
> ebay:   8734
> amazon: 1678
> ```
> In ogni nuova richiesta HTTP verso Amazon, il browser include automaticamente l'intestazione `Cookie: 1678`. Il server legge questo valore, lo cerca nel database e può così riconoscere Susan e proporre contenuti/azioni personalizzati per il suo account

Da questo esempio, possiamo evincere che i cookies possano essere utilizzati per l'identificazione dei singoli utenti, creando quindi un livello di sessione utente al di sopra di HTTP.
### Proxy server
Un **proxy server** (conosciuta anche come **web cache**) è un’entità di rete che soddisfa richieste HTTP al posto del web server effettivo. Il proxy ha una propria memoria su disco (una cache) in cui conserva copie di oggetti recentemente richiesti.

> [!example] Esempio di proxy server
> ![[Pasted image 20261023153342.png]]
> Supponiamo che un client, con un browser configurato, faccia una richiesta a `http://www.someschool.edu/campus.gif`. In sequenza succede che:
> 1. Il browser stabilisce una connessione TCP con il proxy server e invia una richiesta HTTP per l’oggetto specificato;
> 2. Il proxy controlla la presenza di una copia dell’oggetto memorizzata localmente. Se l’oggetto viene rilevato, il proxy lo inoltra all’interno di un messaggio di risposta HTTP al browser;
> 3. Se, invece, la cache non dispone dell’oggetto, il proxy apre una connessione TCP verso il server di origine; ossia, nel nostro esempio, `www.someschool.edu`. Quindi, il proxy invia al server una richiesta HTTP per l’oggetto. Una volta ricevuta tale richiesta, il server di origine invia al proxy l’oggetto all’interno di una risposta HTTP;
> 4. Quando il proxy riceve l’oggetto, ne salva una copia nella propria memoria locale e ne inoltra un’altra copia, all’interno di un messaggio di risposta HTTP, al browser (sulla connessione TCP esistente tra il browser e il proxy).

Notiamo che il proxy è contemporaneamente server e client: quando riceve richieste da un browser e gli invia risposte agisce da server, quando invia richieste e riceve risposte da un server di origine funziona da client.

I proxy si sono sviluppati in internet per due ragioni:
1. Può ridurre sostanzialmente i tempi di risposta alle richieste dei client, in particolare se l'ampiezza di banda che costituisce il collo di bottiglia tra il client e il server è molto inferiore rispetto all'ampiezza di banda minima tra client e proxy;
2. Può ridurre in modo sostanziale il traffico sul collegamento di accesso ad Internet, con il vantaggio di non dover aumentare l'ampiezza di banda e ottenere quindi una riduzione dei costi

> [!example] Esempio di efficenza del proxy
> Immaginiamo una rete aziendale costituita da una rete locale veloce, una LAN da 100 Mbps, a cui sono collegati una manciata di computer. Questa rete ha però un unico punto di contatto con il mondo esterno: un **link di accesso da 15 Mbps** che, attraverso due router, la collega a Internet pubblico, dove si trovano i cinque server di origine da cui gli utenti scaricano le pagine web.
> ![[Pasted image 20261025161633.png]]
> Il traffico che attraversa questa rete è semplice da quantificare. Gli oggetti richiesti (immagini, pagine, file) hanno in media una dimensione di 1 Mbit, e i browser degli utenti generano in media 15 richieste HTTP al secondo. Quando un utente fa una richiesta, il tempo che passa prima che veda il risultato sullo schermo è dato dalla somma di tre componenti: il tempo di attraversamento della LAN, il tempo di attraversamento del link di accesso e il ritardo di Internet.
> Stimiamo, tramite una formula sul ritardo di accodamento trattato nel primo capitolo, il ritardo nella rete locale (ricordando che, più il valore del ritardo si avvicina ad 1, peggio è):
> $$\text{LAN}: (15 \text{req/s} × 1 \text{Mbit}) / 100 \text{Mbps} = 0,15$$
> Mentre, nella rete di accesso:
> $$\text{Link di accesso}: (15 \text{req/s} × 1 \text{Mbit}) / 15 \text{Mbps} = 1$$
> Ovviamente, per non creare incovenzienze a chi usa la rete, è necessario risolvere questo ritardo.
> La prima strada è la più intuitiva, ossia potenziare il link di accesso, portandolo da 15 a 100 Mbps. Il calcolo è immediato l'intensità di traffico scenderebbe a circa 0,015 e il problema sparirebbe. C'è però un ostacolo concreto: questo intervento richiede la **sostituzione fisica della linea di accesso**, un'operazione costosa che spesso deve essere pagata al provider, magari con una tariffa proporzionata alla banda installata. Tecnicamente funziona, economicamente potrebbe essere pesante come costo.
> Nel secondo caso, si installa un proxy server all'interno della rete
> ![[Pasted image 20261025163206.png]]
> Quando un browser chiede un oggetto, la richiesta passa prima per la cache, che se lo possiede lo restituisce immediatamente (circa 10 millisecondi) senza doverlo andare a recuperare su Internet; se invece non lo ha, inoltra la richiesta al server di origine come farebbe normalmente.
> La frazione di richieste soddisfatte direttamente dalla cache si chiama **hit rate**, e nella pratica si colloca tipicamente tra 0,2 e 0,7.
> Nell'esempio assumiamo una hit rate del 40%: il 40% delle richieste viene servito localmente in pochi millisecondi, mentre solo il restante 60% attraversa il link di accesso. Il traffico sul collo di bottiglia scende così da 15 a 9 richieste al secondo, l'intensità di traffico passa da 1 a 0,6, e il ritardo torna su valori accettabili.
#### Reverse Proxy e CDN
Un reverse proxy è un server che si trova di fronte ai server Web e inoltra le richieste del client a tali server Web.
È il contrario simmetrico del proxy server visto precedentemente.

> [!example] Esempio di reverse proxy
> ![[Pasted image 20261025164019.png]]

Un reverse proxy è utile per diverse motivazioni:
- **Bilanciamento del carico (Load balancing)**: un sito Web popolare visitato ogni giorno da milioni di utenti potrebbe non essere in grado di gestire tutto il traffico del sito in entrata con un unico server di origine. Tuttavia, il sito può essere distribuito tra un pool di server diversi, in cui ogni server gestisce le richieste per lo stesso sito. In questo caso, un reverse proxy può fornire una soluzione di bilanciamento del carico che distribuirà il traffico in entrata in modo uniforme tra i diversi server per evitare il sovraccarico di un singolo server. Nel caso in cui un server si guasti completamente, altri server possono intervenire per gestire il traffico.
- **Protezione dagli attacchi**: in presenza di un reverse proxy, un sito Web o un servizio non ha mai bisogno di rivelare l'indirizzo IP del proprio server di origine. Di conseguenza, è molto più difficile per gli autori di attacchi sfruttare un attacco mirato contro di loro, ad esempio un attacco DDoS. Gli autori di attacchi saranno invece in grado di prendere di mira solo il reverse proxy.
- **Caching**: un reverse proxy può anche memorizzare nella cache i contenuti, con conseguente miglioramento delle prestazioni. Ad esempio, se un utente a Parigi visita un sito Web con reverse proxy con server Web a Los Angeles, l'utente potrebbe effettivamente connettersi a un server del reverse proxy locale a Parigi, che dovrà quindi comunicare con un server di origine a Los Angeles. Il server proxy può quindi memorizzare nella cache (o salvare temporaneamente) i dati di risposta. Gli utenti parigini successivi che navigheranno nel sito riceveranno quindi la versione memorizzata nella cache locale dal server del reverse proxy parigino, con prestazioni molto più veloci.
### HTTP/2
HTTP/2, standardizzato nel 2015, è stata la prima nuova versione di HTTP da HTTP/1.1.

Gli obiettivi principali di HTTP/2 sono quello di ridurre la latenza percepita attivando il multiplexing di richiesta e risposta su una singola connessione TCP, di fornire supporto per la priorità delle richieste e il server push, e di fornire una compressione efficiente dei campi di intestazione HTTP. 
HTTP/2 non modifica metodi, codici di stato, URL e campi di intestazione di HTTP, ma cambia il modo in cui i dati vengono formattati e trasportati tra il client e il server.

Per capire la necessità di introdurre HTTP/2 si ricordi che HTTP/1.1 utilizza connessioni TCP persistenti, consentendo l’invio di una pagina web dal serveal client su un'unica connessione TCP. Avendo solo una connessione TCP per pagina web, il numero di socket sul server è limitato e ogni pagina web trasportata ottiene una equa condivisione della larghezza di banda di rete. Tuttavia gli sviluppatori di browser web hanno rapidamente scoperto che l’invio di tutti gli oggetti in una pagina web su una singola connessione TCP ha il problema dell’**Head of Line (HOL) blocking** (“blocco in testa alla coda”). 

> [!example] Esempio di HOL
> Si consideri una pagina web che contiene una pagina di base HTML, un video clip di grandi dimensioni collocato nella parte superiore della pagina e 8 piccoli oggetti disposti sotto il video. Si supponga inoltre che, lungo il percorso tra il server e il client, ci sia un collegamento a collo di bottiglia con velocità medio-bassa, come può essere per esempio un collegamento wireless lento.
> 
> Se tutti gli oggetti della pagina dovessero viaggiare su un'unica connessione TCP, sarebbero costretti a passare in fila attraverso quel collo di bottiglia. Il video clip, per le sue dimensioni, impiegherebbe molto tempo ad attraversarlo, e nel frattempo i piccoli oggetti, che si trovano dietro di lui nella coda, dovrebbero semplicemente aspettare il suo completamento prima di poter arrivare a destinazione. È proprio questo l'essenza del blocco HOL: l'oggetto in testa alla coda (il grande video) trattiene tutti quelli che gli stanno dietro, anche se questi sono minimi e potrebbero essere scaricati in pochi istanti. Il risultato, dal punto di vista dell'utente, è una pagina che si popola a fatica, con il video ancora lontano dal concludersi e gli elementi minori che non compaiono affatto.
> 
> I browser HTTP/1.1 aggirano in genere questo ostacolo con una mossa semplice ed efficace: aprono più connessioni TCP parallele verso il server. In questo modo gli oggetti della stessa pagina vengono inviati simultaneamente su canali diversi, così che i piccoli oggetti non debbano più attendere la fine del video: possono arrivare ed essere riprodotti nel browser molto più rapidamente, riducendo in modo sensibile il ritardo percepito dall'utente.
> 

#### Framing HTTP/2
La soluzione HTTP/2 del blocco HOL consiste nel suddividere ogni messaggin piccoli frame e alternare i messaggi di richiesta e risposta sulla stessa conessione TCP. 

> [!example] Riprendendo l'esempio precedente...
>  Si consideri nuovamente l’esempio di una pagina web costituita da un unico grande video clip e 8 oggetti più piccoli. Pertanto il server riceverà 9 richieste simultanee da qualsiasi browser che voglia vedere questa pagina web. Per ciascuna di queste richieste, il server deve inviare al browser 9 messaggi di risposta HTTP concorrenti.
> 
> Supponiamo che tutti i fotogrammi abbiano una lunghezza fissata, il video clip sia composto da 1000 frame e ognuno degli oggetti più piccoli sia costituito da due frame. Se si intervallano i frame, il primo frame di tutti gli oggetti più piccoli viene inoltrato dopo aver inviato un frame del video clip. Quindi, dopo aver inviato il secondo frame del video clip, viene inoltrato l’ultimo frame di ciascuno degli oggetti piccoli. Pertanto, tutti gli oggetti più piccoli vengono inviati dopo aver inoltrato un totale di 18 frame. Se i frame non venissero intervallati, gli oggetti più piccoli verrebbero inviati solo dopo aver inoltrato 1016 frame. Quindi, il meccanismo di framing HTTP/2 può ridurre significativamente il ritardo percepito dall’utente.

La capacità di suddividere un messaggio HTTP in frame indipendenti, intervallarli e poi ricostruirli dall’altra parte è il principale miglioramento apportato da HTTP/2. Quando un server vuole inviare una risposta HTTP, la risposta viene elaborata dal sottolivello di framing, dove viene scomposta in frame. Il campo di intestazione della risposta diventa un frame e il corpo del messaggio viene suddiviso in più frame aggiuntivi. I frame della risposta sono quindi intervallati dal sottolivello di framing nel server con i frame di altre risposte e inviati tramite singola connessione TCP persistente. I frame, man mano che arrivano al client, vengono prima ricostruiti nei messaggi di risposta originali nel sottolivello di framing e quindi elaborati dal browser come al solito. Allo stesso modo, le richieste HTTP del client vengono suddivise in frame e intervallate.
### Streaming HTTP e DASH
Nello streaming HTTP il video viene semplicemente memorizzato in un server HTTP come un file ordinario con un URL specifico. Quando un utente vuole vedere un video, il client stabilisce una connessione TCP con il server e invia una richiesta GET HTTP per il suo URL. Il server invia il file video, all’interno di un messaggio di risposta HTTP, più velocemente possibile dati il traffico e i protocolli di rete. Sul lato client i byte vengono memorizzati in un buffer dell’applicazione client. Quando il numero di byte nel buffer supera una soglia fissata, l’applicazione client inizia la riproduzione: periodicamente prende i frame video dal buffer, li decomprime e li visualizza all’utente. Quindi l’applicazione di video streaming consiste nel visualizzare i frame mentre li riceve, memorizzando nel buffer gli ultimi.

Lo streaming HTTP, sebbene molto usato dalle aziende del settore, quali YouTube, presenta un grande svantaggio: i client ricevono tutti la stessa versione codificata del video, nonostante abbiano a disposizione una larghezza di banda che può variare sensibilmente da un caso all’altro e nel tempo. Per superare il problema è stato sviluppato un nuovo tipo di streaming basato su HTTP, chiamato **streaming dinamico adattativo su HTTP** (Dynamic Adaptive Streaming Over HTTP, DASH). In DASH, i video vengono codificati in diverse versioni, ognuna avente un bit rate differente e quindi un differente livello di qualità. Il client richiede pezzi di segmenti video lunghi alcuni secondi da versioni differenti in modo dinamico. Quando la banda disponibile è elevata, il client seleziona automaticamente i blocchi da una versione con alto bit rate, mentre quando la banda disponibile è poca, seleziona una versione a basso bit rate. Il client seleziona un blocco alla volta con un messaggio di richiesta GET HTTP.

Da una parte, DASH permette a client con accessi a Internet diversi di fare streaming dei video con codifiche diverse: client con connessioni lente possono ricevere versioni a basso bit rate e quindi a bassa qualità, mentre client con connessioni in fibra possono ricevere una versione ad alta qualità. D’altro canto, DASH permette anche a un client di adattare la scelta della versione alla banda disponibile se varia durante la sessione.
 
Con DASH, i video nelle varie versioni sono memorizzati nel server HTTP, ognuno a un URL diverso. Il server HTTP ha anche un file di descrizione (detto anche **manifest file, o file manifesto**) che per ogni versione fornisce il rispettivo URL insieme al bit rate. Il client richiede innanzitutto il file di descrizione per venire a conoscenza delle varie versioni disponibili. Quindi seleziona a ogni istante un blocco, specificando nel messaggio di richiesta GET di HTTP un URL e un intervallo di byte. Mentre scarica i blocchi, il client misura la banda di ricezione ed esegue un algoritmo per selezionare il blocco successivo. Naturalmente il client, se ha molti dati del video memorizzati nel buffer e la banda di ricezione misurata è larga, sceglie una versione ad alto bit rate per il blocco successivo; al contrario, se ha pochi dati nel buffer e banda bassa, sceglie una versione a basso rate. Pertanto DASH permette al client di cambiare liberamente il livello di qualità.

