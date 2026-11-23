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
Il problema del trasferimento dati affidabile viene affrontato in un contesto generale poiché non riguarda esclusivamente il livello di trasporto, ma si presenta anche al livello di collegamento e al livello di applicazione.

Dal punto di vista del modello di servizio, l’astrazione offerta alle entità dei livelli superiori consiste in un canale affidabile attraverso il quale è possibile trasferire dati.
> [!example] Esempio di astrazione di un canale affidabile
> ![[Pasted image 20261123151823.png]]
> Disporre di un canale affidabile significa garantire che nessun bit dei dati trasmessi venga corrotto (ovvero alterato da $0$ a $1$ o viceversa), che nessun bit vada perduto e che tutti i bit siano consegnati esattamente nell'ordine di invio; questo coincide precisamente con il modello di servizio fornito da TCP alle applicazioni Internet (che vedremo più avanti).
> **Il compito di un protocollo di trasferimento dati affidabile (reliable data transfer protocol)** consiste proprio nell'implementare tale astrazione di servizio. La complessità di questo compito deriva dal fatto che il livello sottostante al protocollo può essere inaffidabile: ad esempio, TCP opera al di sopra del livello di rete IP, il quale offre un servizio best-effort non affidabile end-to-end. 

> [!example] Contesto trasferimento dati affidabile
> ![[Pasted image 20261123151707.png]]
> Per studiare il problema in modo sistematico, si sviluppano in parallelo il lato mittente e il lato ricevente di un protocollo di trasferimento dati affidabile, introducendo modelli del canale sottostante progressivamente più complessi e realistici, in grado di corrompere i bit o di perdere interi pacchetti. Nell'esempio assumeremo che il canale sottostante non riordini i pacchetti, facendoli giungere a destinazione nell’ordine in cui sono stati inviati, pur potendo subire alterazioni o andare smarriti.
> 
> Definiamo anche interfacce software per il funzionamento di questi principi. Sul lato mittente, il livello superiore invoca il protocollo attraverso la chiamata `rdt_send()`, passando i dati da consegnare al livello superiore del destinatario (dove `rdt` sta per reliable data transfer). Per trasmettere effettivamente un pacchetto sul canale inaffidabile sottostante, il protocollo richiama la funzione `udt_send()`, dove `udt` indica unreliable data transfer. Sul lato ricevente, l’arrivo di un pacchetto dal canale inaffidabile provoca la chiamata a `rdt_rcv()` e, quando il protocollo è pronto a passare i dati estratti al livello superiore, invoca la funzione `deliver_data()`.

#### Costruzione di un protocollo di trasferimento dati affidabile
Per giungere alla progettazione di un protocollo di trasferimento dati affidabile completo e pienamente funzionante, si procede in modo incrementale analizzando una sequenza di protocolli via via più articolati, ciascuno progettato per compensare un nuovo tipo di difetto del canale sottostante.

Prima di analizzare i vari funzionamenti, fissiamo velocemente le convenzioni di come andremo a descrivere tutti i protocolli. 

> [!important] Convenzione per descrizione dei protocolli 
> Ogni protocollo verrà descritto mediante delle **macchine a stati finiti** (FSM, Finite-State Machine).
> Gli stati sono rappresentati da nodi e le transizioni da frecce orientate; lo stato iniziale della macchina viene indicato da una freccia tratteggiata. Ogni transizione è etichettata con una linea orizzontale: al di sopra della linea è riportato l’evento che innesca la transizione, mentre al di sotto sono elencate le azioni eseguite in risposta a tale evento. Qualora un evento non determini alcuna azione, oppure un’azione venga intrapresa senza il verificarsi di un evento specifico, si utilizza la lettera greca maiuscola $\Lambda$ rispettivamente sotto o sopra la linea orizzontale per denotare in modo esplicito l’assenza di azione o di evento.
##### Trasferimento dati affidabile su un canale perfettamente affidabile (rdt 1.0)
Il punto di partenza è il caso ideale e più semplice, in cui il canale sottostante è completamente affidabile, privo cioè sia di errori sui bit sia di perdite di pacchetti. Il protocollo risultante, denominato `rdt1.0`, è banale e viene descritto mediante due FSM separate, una per il mittente e una per il destinatario.

> [!example] Rappresentazione del protocollo rdt 1.0
> ![[Pasted image 20261123152840.png]]
> Nel protocollo `rdt1.0`, sia la FSM del mittente sia quella del destinatario possiedono un unico stato, per cui tutte le transizioni riportano necessariamente dallo stato a sé stesso. Il mittente si trova costantemente nello stato di attesa di una chiamata dall'alto: quando il livello superiore invoca `rdt_send(data)`, il mittente incapsula i dati creando un pacchetto attraverso `packet = make_pkt(data)` e lo immette nel canale chiamando `udt_send(packet)`. In modo speculare, il destinatario rimane nello stato di attesa di una chiamata dal basso: alla ricezione di un pacchetto tramite l’evento `rdt_rcv(packet)`, estrae il contenuto informativo mediante `extract(packet, data)` e lo consegna al livello superiore chiamando `deliver_data(data)`.

In questo scenario ideale non esiste una reale distinzione funzionale tra un’unità di dati e un pacchetto, e tutti i pacchetti viaggiano esclusivamente dal mittente verso il destinatario. Poiché nulla può andare storto, il lato ricevente non ha alcuna necessità di inviare messaggi di riscontro al mittente. Inoltre, si assume implicitamente che il destinatario sia sempre in grado di ricevere ed elaborare i dati alla stessa velocità di invio del mittente, motivo per cui non ha mai bisogno di richiedere al mittente di rallentare la trasmissione.
##### Trasferimento dati affidabile su un canale con errori su bit (rdt 2.0)
Un modello decisamente più realistico prevede che il canale sottostante possa corrompere i bit all’interno di un pacchetto, un fenomeno che accade concretamente nei componenti fisici di rete durante la trasmissione, la propagazione o la memorizzazione nei buffer. In questa fase si continua comunque a ipotizzare che nessun pacchetto vada perduto e che tutti i pacchetti trasmessi giungano a destinazione nello stesso ordine in cui sono stati inviati, pur potendo contenere bit alterati.

Questo meccanismo fa uso di **notifiche positive** (positive acknowledgments) e di **notifiche negative** (negative acknowledgments), messaggi di controllo che permettono al ricevente di informare il mittente su ciò che è stato ricevuto correttamente e su ciò che deve essere ripetuto. Nelle reti di calcolatori, i protocolli di trasferimento dati affidabile basati su tali ritrasmissioni sono definiti protocolli **ARQ (Automatic Repeat reQuest)**.

Per gestire la presenza di errori sui bit, un protocollo ARQ richiede fondamentalmente tre capacità operative:
1. Il rilevamento degli errori (error detection): consente al destinatario di accorgersi se i bit di un pacchetto sono stati alterati durante il transito, mediante l’inserimento di bit di controllo ridondanti nel pacchetto, come il campo checksum;
2. Il riscontro del destinatario (receiver feedback): poiché mittente e destinatario risiedono su sistemi periferici distinti, l’unico modo che il mittente ha per conoscere l’esito della ricezione è ricevere esplicitamente un feedback, ovvero un pacchetto di notifica positiva (`ACK`) o negativa (`NAK`); 
3. La ritrasmissione (retransmission): un pacchetto ricevuto con errori viene inviato nuovamente dal mittente;

> [!example] Rappresentazione del protocollo rdt 2.0
> ![[Pasted image 20261123163559.png]]
> Nel protocollo `rdt2.0`, la FSM del mittente si articola in due stati distinti. Nello stato iniziale di attesa di chiamata dall’alto, alla chiamata `rdt_send(data)`, il mittente costruisce il pacchetto includendo i dati e il relativo checksum (`sndpkt = make_pkt(data, checksum)`), lo trasmette con `udt_send(sndpkt)` e passa allo stato di attesa di `ACK` o `NAK`. In questo secondo stato, il mittente non può accettare nuovi dati dal livello superiore: prima di poter trasmettere un nuovo pacchetto deve avere la certezza che il destinatario abbia ricevuto correttamente quello corrente. Per questa caratteristica, protocolli come `rdt2.0` vengono classificati come protocolli stop-and-wait. Se nello stato di attesa il mittente riceve un pacchetto `ACK`, sa che la trasmissione è andata a buon fine e torna nello stato di attesa di chiamata dall’alto (senza ulteriori azioni, $\Lambda$); se invece riceve un `NAK`, ritrasmette l’ultimo pacchetto inviato (`udt_send(sndpkt)`) e rimane in attesa di riscontro. Sul lato ricevente, la FSM di `rdt2.0` mantiene un unico stato: alla ricezione di un pacchetto, se esso risulta integro, il destinatario estrae e consegna i dati al livello superiore e risponde con un `ACK`; se invece si verifica la condizione `rdt_rcv(rcvpkt) && corrupt(rcvpkt)`, ovvero il pacchetto presenta errori sui bit, il destinatario risponde inviando un `NAK`.

Nonostante la sua apparente semplicità ed efficacia, il protocollo `rdt2.0` presenta un grave difetto progettuale, ossia non tiene conto della possibilità che anche i pacchetti di controllo `ACK` e `NAK` possano essere corrotti durante il transito sul canale inaffidabile. Se il mittente riceve un `ACK` o un `NAK` alterato, non è in grado di stabilire se il destinatario abbia ricevuto correttamente o meno l’ultimo pacchetto dati. Per affrontare questa ambiguità si potrebbero ipotizzare tre strade. La prima richiederebbe di inviare una richiesta di ripetizione del riscontro, ma finirebbe in una regressione infinita qualora anche tali richieste venissero corrotte. La seconda consisterebbe nell'aggiungere abbastanza bit di controllo da consentire non solo il rilevamento ma anche la correzione autonoma degli errori, una strategia però insufficiente quando il canale può anche perdere pacchetti. La terza soluzione, più pratica, consiste nel far sì che il mittente ritrasmetta semplicemente il pacchetto dati corrente ogni volta che riceve un `ACK` o un `NAK` corrotto. Tuttavia, questo approccio introduce un nuovo problema, ossia la comparsa di pacchetti duplicati sul canale dal mittente al destinatario: poiché il destinatario non può sapere se l’ultimo `ACK` o `NAK` da lui inviato sia giunto integro al mittente, non è in grado di determinare a priori se il pacchetto in arrivo contenga dati nuovi oppure rappresenti la ritrasmissione del pacchetto precedente.

###### Gestione di ACK/NAK corrotti e numeri di sequenza: (rdt2.1)
La soluzione semplice ed efficace al problema dei pacchetti duplicati, adottata in quasi tutti i protocolli reali di trasferimento dati compreso TCP, consiste nell’aggiungere un nuovo campo all’intestazione del pacchetto dati affinché il mittente vi inserisca un numero di sequenza (sequence number). In questo modo, al destinatario è sufficiente esaminare il numero di sequenza per stabilire con certezza se il pacchetto appena ricevuto costituisca una ritrasmissione oppure un nuovo pacchetto. Poiché `rdt2.1` è un semplice protocollo stop-and-wait, un numero di sequenza lungo appena $1$ bit (che assume alternativamente i valori $0$ e $1$) è del tutto sufficiente: se il numero di sequenza del pacchetto in arrivo coincide con quello dell’ultimo pacchetto ricevuto, si tratta di una ritrasmissione; se invece è diverso, si tratta di un pacchetto nuovo. Dal momento che si continua ad assumere che il canale non smarrisca pacchetti, i pacchetti `ACK` e `NAK` in `rdt2.1` non hanno ancora bisogno di trasportare il numero di sequenza del pacchetto cui si riferiscono, in quanto il mittente ha la certezza che qualsiasi `ACK` o `NAK` ricevuto (alterato o meno) sia stato generato in risposta al pacchetto dati trasmesso più di recente.

La versione corretta del protocollo, chiamata `rdt2.1`, comporta il raddoppio del numero di stati sia nella FSM del mittente sia in quella del destinatario, poiché lo stato deve ora tenere traccia del fatto che il pacchetto attualmente trasmesso o atteso abbia numero di sequenza $0$ oppure $1$.

La FSM del mittente di `rdt2.1` comprende quindi quattro stati: attesa di chiamata $0$ dall’alto, attesa di `ACK` o `NAK` $0$, attesa di chiamata $1$ dall’alto e attesa di `ACK` o `NAK` $1$. Quando si trova nello stato di attesa di chiamata $0$ dall’alto e riceve i dati tramite `rdt_send(data)`, il mittente genera il pacchetto con `sndpkt = make_pkt(0, data, checksum)`, lo trasmette mediante `udt_send(sndpkt)` e transita nello stato di attesa di `ACK` o `NAK` $0$. In tale stato, se arriva un pacchetto corrotto oppure un `NAK` — condizione espressa da `rdt_rcv(rcvpkt) && (corrupt(rcvpkt) || isNAK(rcvpkt))` — il mittente ritrasmette il pacchetto corrente eseguendo `udt_send(sndpkt)` e rimane nel medesimo stato. Se invece arriva un pacchetto integro di tipo `ACK` — condizione `rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) && isACK(rcvpkt)` — il mittente non intraprende alcuna azione ($\Lambda$) e passa allo stato di attesa di chiamata $1$ dall’alto. Da qui il comportamento è del tutto simmetrico: i nuovi dati vengono incapsulati con numero di sequenza $1$ (`sndpkt = make_pkt(1, data, checksum)`), trasmessi e confermati prima di tornare allo stato iniziale per il pacchetto con numero di sequenza $0$.

La FSM del destinatario di `rdt2.1` presenta due stati: attesa del pacchetto $0$ dal basso e attesa del pacchetto $1$ dal basso. Quando il destinatario riceve un pacchetto integro avente il numero di sequenza atteso, estrae e consegna i dati al livello superiore, costruisce e invia un `ACK` protetto da checksum e si sposta nello stato successivo per attendere l’altro numero di sequenza. Quando invece riceve un pacchetto corrotto, invia un `NAK` (anch’esso dotato di checksum) e rimane nello stato corrente. Infine, se riceve un pacchetto integro ma fuori sequenza — ossia un duplicato del pacchetto precedente, la cui presenza indica che l’`ACK` precedente è andato corrotto — il destinatario non consegna nuovamente i dati al livello superiore, ma risponde comunque con un `ACK` positivo per sbloccare il mittente, rimanendo in attesa del numero di sequenza corrente.

###### Protocollo privo di NAK (rdt2.2)
Il protocollo `rdt2.1` impiega sia riscontri positivi sia riscontri negativi: il destinatario invia un `ACK` quando riceve un pacchetto corretto o fuori sequenza e un `NAK` quando riceve un pacchetto alterato. Tuttavia, è possibile ottenere esattamente lo stesso risultato funzionale di un `NAK` eliminando del tutto i riscontri negativi e facendo sì che il destinatario spedisca, al posto del `NAK`, un `ACK` relativo al pacchetto più recente ricevuto correttamente. In questo modo, quando il mittente riceve due `ACK` riferiti al medesimo pacchetto — detti `ACK` duplicati — deduce inequivocabilmente che il destinatario non ha ricevuto in modo corretto il pacchetto successivo a quello confermato due volte.

Questa evoluzione porta al protocollo `rdt2.2`, un protocollo di trasferimento dati affidabile privo di `NAK` per un canale soggetto a errori sui bit. La differenza sottile ma fondamentale tra `rdt2.1` e `rdt2.2` risiede nella gestione esplicita del numero di sequenza all’interno dei messaggi di riscontro: il destinatario deve ora includere nel pacchetto `ACK` il numero di sequenza del pacchetto confermato (passando esplicitamente l’argomento `ACK, 0` oppure `ACK, 1` alla funzione `make_pkt()`), mentre il mittente deve verificare il numero di sequenza riportato nell’`ACK` ricevuto (passando il valore $0$ oppure $1$ come argomento nella funzione `isACK()`). Pertanto, se il mittente in attesa di `ACK 0` riceve un pacchetto corrotto oppure un `ACK 1` (che funge da `ACK` duplicato per il pacchetto precedente), ritrasmette il pacchetto $0$; solo la ricezione di un `ACK 0` non corrotto gli permette di avanzare verso la trasmissione del pacchetto $1$. In modo corrispondente, il destinatario in attesa del pacchetto $0$, qualora riceva un pacchetto corrotto oppure un duplicato con numero di sequenza $1$, risponde sempre rinviando `ACK 1`.