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
Un **proxy server** è un’entità di rete che soddisfa richieste HTTP al posto del web server effettivo. Il proxy ha una propria memoria su disco (una cache) in cui conserva copie di oggetti recentemente richiesti.