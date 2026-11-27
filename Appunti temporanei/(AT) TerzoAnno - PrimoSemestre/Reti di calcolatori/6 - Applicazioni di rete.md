# Introduzione alle applicazioni di rete

Questa lezione introduce i concetti fondamentali relativi alle applicazioni di rete, analizzando le definizioni di base, i modelli di interazione client-server e peer-to-peer, il funzionamento della Socket API e la scelta del tipo di servizio di trasporto[cite: 1]. Vengono inoltre esaminati i meccanismi di indirizzamento dei processi e di identificazione dei servizi, la gestione della concorrenza nei server, i flussi di interazione orientati alla connessione e senza connessione e, infine, le principali modalità di collaudo delle applicazioni di rete[cite: 1].

## Definizioni base

Un'applicazione di rete, definita anche applicazione distribuita, ha lo scopo di fornire o realizzare un servizio destinato a un utente finale ed è costituita da più programmi in esecuzione, ovvero processi, che operano su host diversi[cite: 1]. All'interno della comunicazione si distinguono due ruoli fondamentali: il **client**, ossia il processo che dà inizio alla comunicazione, e il **server**, ossia il processo che rimane in attesa di essere contattato[cite: 1].

Nel contesto specifico di un'applicazione Internet, i processi comunicano sfruttando i servizi di trasporto messi a disposizione da Internet, vale a dire TCP oppure UDP, ai quali accedono attraverso la Socket API[cite: 1]. Lo scambio dei messaggi tra i processi segue le regole di un protocollo di livello applicazione, il quale può essere pubblico, se definito da uno standard condiviso, oppure privato, quando viene progettato direttamente dagli sviluppatori dell'applicazione, come accade nelle esercitazioni di laboratorio[cite: 1].

Quando un'applicazione è conforme agli standard di Internet, essa si basa su protocolli pubblici descritti formalmente nei documenti chiamati Request for Comments (RFC) e gestiti dall'Internet Engineering Task Force (IETF)[cite: 1]. Esempi rappresentativi di applicazioni e protocolli conformi a tali standard sono FTP per il trasferimento di file, SMTP per la posta elettronica, HTTP per il Web e IRC per la chat[cite: 1].

## Modelli client-server e peer-to-peer

### Modello client-server

Una larga parte delle applicazioni di rete adotta come paradigma comunicativo il modello client-server[cite: 1]. In questa architettura il server offre un servizio, tipicamente consistente nell'accesso a risorse condivise, rispondendo su richiesta dopo essere rimasto in uno stato di attesa passiva, mentre il client richiede il servizio avviando attivamente la comunicazione[cite: 1]. L'interazione tra le due parti è quindi di natura asimmetrica e segue lo schema richiesta-risposta: nella prima fase il client trasmette una richiesta al server, mentre nella seconda fase il server restituisce la propria risposta al client[cite: 1].

### Caratteristiche di un client e di un server

Le due entità presentano proprietà operative ben distinte[cite: 1]. Il **client** viene eseguito sul computer locale ed è invocato direttamente dall'utente; esso prende l'iniziativa di stabilire il contatto con il server, invia una richiesta che può eventualmente includere dei dati e possiede la capacità di accedere a più servizi[cite: 1].

Il **server**, al contrario, viene eseguito su un computer remoto e si avvia automaticamente all'inizializzazione del sistema[cite: 1]. Una volta attivo, rimane in attesa delle richieste di servizio provenienti dai client attraverso un ciclo continuo (loop) che persiste finché non giunge una richiesta[cite: 1]. Quando riceve una richiesta, il server elabora e spedisce una risposta, anch'essa opzionalmente corredata di dati, ed è progettato per accettare richieste da client arbitrari gestendo una specifica istanza di servizio per ciascun client[cite: 1].

### Modello peer-to-peer

Nel modello peer-to-peer (P2P) i componenti dell'applicazione operano in modo paritetico[cite: 1]. L'interazione risulta simmetrica, dal momento che ogni nodo può comportarsi sia come client sia come server[cite: 1]. A differenza del modello centralizzato, le informazioni vengono suddivise tra i vari nodi della rete anziché essere concentrate in un unico punto, dando origine a un'architettura decentralizzata in cui ogni peer è in grado di instaurare connessioni dirette con altri peer attivi nella rete[cite: 1].

### Applicazioni P2P

Sebbene spesso associato a tecnologie moderne, il modello P2P è impiegato da molto tempo nei livelli di rete e di collegamento dati, trovando applicazione in ambiti come l'Internet routing e la posta elettronica, quest'ultima basata su un approccio ibrido[cite: 1]. L'adozione del paradigma P2P nella progettazione di applicazioni di rete vere e proprie è invece più recente e si concentra soprattutto in settori quali il file sharing, l'elaborazione distribuita e la comunicazione[cite: 1].

## Socket API

La Socket API, dove API sta per Application Programming Interface, rappresenta l'interfaccia attraverso la quale i processi accedono alla rete[cite: 1]. In particolare, una socket è un dispositivo di interfaccia collocato tra un processo, che ricade sotto il controllo dello sviluppatore dell'applicazione, e la rete, ovvero il sottostante livello di trasporto come TCP completo dei suoi buffer e delle sue variabili, il quale è gestito dal sistema operativo[cite: 1].

Grazie alla propria socket, un processo in esecuzione su un host o su un server può sia spedire messaggi diretti a un altro processo sia ricevere messaggi in arrivo attraverso Internet[cite: 1]. Dal punto di vista operativo, la comunicazione tramite socket si fonda sul modello di input/output tipico di Unix, scandito dalla sequenza di operazioni `open-read-write-close`, in cui il canale viene gestito tramite un descrittore di file rappresentato dalla coppia `file descriptor = (pathname, flag)`[cite: 1].

## Scelta del tipo di servizio

Quando si sviluppa un'applicazione di rete è necessario selezionare il servizio di trasporto più adeguato tra quelli offerti da Internet, scegliendo fra TCP e UDP[cite: 1].

Il protocollo **TCP** fornisce un servizio orientato alla connessione: in questo caso il client stabilisce preliminarmente la connessione verso il server, dopodiché client e server possono scambiarsi messaggi multipli di dimensione arbitraria e, al termine dello scambio, il client procede a chiudere la connessione[cite: 1]. La caratteristica distintiva di TCP è che garantisce l'affidabilità del servizio di comunicazione[cite: 1].

Il protocollo **UDP** fornisce invece un servizio privo di connessione: il client prepara il messaggio e lo spedisce direttamente al server, rispettando il vincolo che l'intero messaggio deve essere contenuto all'interno di un singolo datagramma UDP, e il server provvede poi a rispondere[cite: 1]. A differenza di TCP, il servizio offerto da UDP non è affidabile[cite: 1].

## Indirizzamento dei processi e identificazione di un servizio

### Servizi multipli offerti da un host

Un sistema operativo multi-tasking consente di ospitare contemporaneamente più di un server sullo stesso calcolatore[cite: 1]. Ciascun server mette a disposizione un servizio specifico per i client che lo richiedono; trattandosi di processi indipendenti, i diversi server possono gestire in modo simultaneo le richieste che provengono dai client[cite: 1].

### Indirizzamento dei processi

Per consentire la comunicazione tra processi distribuiti, ogni processo viene identificato univocamente in rete tramite la coppia `(indirizzo IP, numero di porta)`, dove l'indirizzo IP individua l'host destinatario e il numero di porta, codificato su $16$ bit, individua il singolo processo attivo su quell'host[cite: 1]. Ogni messaggio trasmesso include sia l'indirizzo e la porta del mittente sia quelli del destinatario, permettendo così al sistema operativo ricevente di recapitare il messaggio al processo corretto sulla base della porta di destinazione[cite: 1].

Dal lato server, il processo si lega esplicitamente mediante la chiamata `bind()` a una porta nota a priori ai client[cite: 1]. I servizi standard utilizzano porte con numero inferiore a $1024$ (come la porta 80 per il protocollo HTTP), il cui utilizzo su sistemi Linux richiede privilegi di amministratore[cite: 1]. Un server sviluppato autonomamente, come quelli realizzati nelle esercitazioni di laboratorio, adotta invece una porta libera superiore a $1023$, per esempio la porta 57015[cite: 1]. Poiché una porta può essere associata a una sola socket per volta, qualora risulti già occupata la chiamata `bind()` fallisce[cite: 1].

Dal lato client, il processo raggiunge il server specificandone l'indirizzo IP e la porta[cite: 1]. Il client non sceglie manualmente il proprio numero di porta: esso gli viene assegnato temporaneamente dal sistema operativo, e il server lo apprende direttamente al momento della ricezione della richiesta[cite: 1].

### Socket e numeri di porta

Ogni applicazione comunica con l'esterno attraverso una o più socket, che il codice manipola mediante un descrittore in modo analogo a un file[cite: 1]. Ciascuna socket è collegata a una porta del protocollo di trasporto impiegato, con numerazione compresa nell'intervallo da $1$ a $65535$ sia per TCP sia per UDP, entrambi operanti al di sopra del protocollo IP[cite: 1]. Gli spazi di numerazione delle porte di TCP e UDP sono del tutto separati: di conseguenza, la porta 80 TCP e la porta 80 UDP costituiscono due porte distinte[cite: 1]. Inoltre, una stessa applicazione ha la facoltà di utilizzare più socket contemporaneamente, combinando all'occorrenza sia socket TCP sia socket UDP[cite: 1].

## Concorrenza nei server

### Server iterativi e server concorrenti

Un server realizzato in modo iterativo elabora le richieste di un solo client alla volta; ciò comporta un problema evidente qualora una singola richiesta necessiti di molto tempo per essere evasa, poiché tutti gli altri client rimangono bloccati in attesa del suo completamento[cite: 1].

La soluzione consiste nell'impiego di un **server concorrente**, capace di servire più client nello stesso momento[cite: 1]. In un server concorrente basato su TCP, il processo resta in ascolto di nuove richieste di connessione su una apposita **socket di benvenuto**[cite: 1]. Ogni volta che una connessione viene accettata, il server riceve una nuova **socket di connessione** e la assegna a un nuovo **thread**, definito come un flusso di esecuzione indipendente all'interno del medesimo processo padre, con il quale condivide lo spazio di memoria[cite: 1]. Mentre il thread appena creato gestisce l'intera interazione con quel client utilizzando la socket di connessione, il server principale torna immediatamente in attesa di ulteriori connessioni sulla socket di benvenuto[cite: 1].

### Connessioni e server concorrenti

L'impiego della concorrenza introduce una questione di indirizzamento interno: poiché un server concorrente genera un thread per ogni connessione accettata e le richieste simultanee vengono gestite da thread concorrenti, sapendo che ogni client è identificato dalla coppia `(indirizzo IP, numero di porta)`, che ogni server è identificato dalla coppia `(indirizzo IP, numero di porta)` e che i due scambiano molteplici messaggi, occorre stabilire come il software del livello di trasporto del server riesca a recapitare ciascun messaggio in arrivo al thread appropriato[cite: 1].

Il meccanismo risolutivo prevede che ogni connessione TCP sia identificata in modo univoco da una quadrupla composta da `(indirizzo IP server, numero di porta server, indirizzo IP client, numero di porta client)`[cite: 1]. In virtù di tale identificazione, ogni messaggio ricevuto viene instradato dal livello di trasporto esattamente verso la socket di connessione caratterizzata dalla medesima quadrupla, consegnandolo così al thread incaricato di servire quello specifico client[cite: 1].

## Interazione client/server orientata alla connessione e senza connessione

### Interazione client/server orientata alla connessione

Nell'interazione orientata alla connessione, tipica di TCP, il server predispone inizialmente il canale creando la socket con `socket()`, associandola all'indirizzo e alla porta tramite `bind()` e ponendola in stato di ascolto con `listen()`; subito dopo invoca `accept()` per attendere l'arrivo di un client[cite: 1]. Parallelamente, il client crea la propria socket con `socket()` e avvia la fase di instaurazione della connessione chiamando `connect()`, che si completa in corrispondenza della `accept()` sul server[cite: 1].

Una volta stabilita la connessione, il client invia i dati della richiesta utilizzando `send()`, mentre il server li riceve tramite `recv()`[cite: 1]. Il server procede quindi all'elaborazione della richiesta e trasmette i dati della risposta con `send()`, che vengono letti dal client mediante `recv()`[cite: 1]. Conclusa la comunicazione, sia il server sia il client rilasciano le risorse chiudendo le rispettive socket attraverso la chiamata `close()`[cite: 1].

### Interazione client/server senza connessione

Nell'interazione senza connessione, caratteristica di UDP, non ha luogo alcuna fase preventiva di apertura della connessione[cite: 1]. Il server crea la socket con `socket()`, la associa alla propria porta tramite `bind()` e si pone direttamente in attesa dei messaggi invocando `recvfrom()`[cite: 1]. Il client crea la propria socket con `socket()` e spedisce immediatamente i dati della richiesta al server utilizzando `sendto()`[cite: 1].

Il server acquisisce la richiesta tramite `recvfrom()`, la elabora e invia i dati della risposta al client chiamando `sendto()`, che il client raccoglie con `recvfrom()`[cite: 1]. Al termine dello scambio, tanto il server quanto il client chiudono la propria socket mediante `close()`[cite: 1].

## Collaudo di applicazioni di rete

Per collaudare un'applicazione di rete durante lo sviluppo non è necessario disporre di due macchine distinte: client e server possono infatti essere eseguiti sullo stesso host sfruttando l'indirizzo di loopback `127.0.0.1`, noto anche come `localhost`[cite: 1].

Per effettuare il test del server è possibile sostituire il client con il programma `netcat` (invocato con il comando `nc`), che consente di digitare manualmente le richieste da tastiera e visualizzare le risposte restituite dal server[cite: 1]. Nel caso di un server TCP in ascolto sulla porta 57015 si utilizza il comando `nc 127.0.0.1 57015`, mentre nel caso di un server UDP si aggiunge il flag `-u`, eseguendo `nc -u 127.0.0.1 57015`[cite: 1].

Per effettuare il test del client si può invece farlo dialogare con un server già funzionante, oppure si può avviare `netcat` in modalità di ascolto (tramite l'opzione `-l`) sulla porta attesa dal server, così da leggere le richieste spedite dal client e inviare manualmente le risposte da tastiera[cite: 1]. Per simulare un server TCP sulla porta 57015 si esegue `nc -l 57015`, mentre per un server UDP si esegue `nc -u -l 57015`[cite: 1].