Il Web è stata la prima applicazione Internet che ha catturato l’attenzione del pubblico, cambiando profondamente il modo di interagire all'interno e all'esterno degli ambienti di lavoro. Il Web è:
- un sistema **on-demand**, ossia tutte le sue richieste sono accessibili quando si vuole 
- un sistema **distribuito**, diversi documenti sono distrubuiti su diversi server remoti
- un sistema **ipermediale**, ossia permette l’accesso interattivo a collezioni di documenti di diverso tipo (testo, figure, immagini, animazioni, audio, video) collegati tra loro
## HTTP
**HTTP (HyperText Transfer Protocol)**, protocollo a livello di applicazione del Web (definito nelle RFC 1945, RFC 7230 e RFC 7540), costituisce il cuore del Web. Questo protocollo è implementato in due programmi, **client e server**, in esecuzione su sistemi periferici diversi che comunicano tra loro scambiandosi messaggi HTTP. Il protocollo definisce sia la struttura dei messaggi sia la modalità con cui client e server si scambiano i messaggi. 

> [!example] Comportamento richiesta-risposta HTTP
> ![[Pasted image 20261019174548.png]]

Una **pagina web (web page)**, detta anche documento, è costituita da oggetti. Un **oggetto (object)** è semplicemente un file (quale un file HTML, un’immagine JPEG, un file Javascript, un foglio di stile CSS, una clip video e così via) indirizzabile tramite un URL. La maggioranza delle pagine web consiste di un file **HTML principale** (base HTML file) e diversi oggetti referenziati da esso.

Il file HTML referenzia gli altri oggetti nella pagina tramite il loro URL. Ogni URL ha due componenti: il nome dell’host del server che ospita l’oggetto e il percorso dell’oggetto.
> [!example] Esempio di URL
> Per esempio, l’URL `http://www.someSchool.edu/someDepartment/picture.gif` ha `www.someSchool.edu` come nome dell’host e `/someDepartment/picture.gif` come percorso.

Un **browser web** (come Chrome) implementa il lato client di HTTP. Un **web server**, che implementa il lato server di HTTP, ospita oggetti web, indirizzabili tramite URL. 

HTTP definisce in che modo i client web richiedono le pagine ai web server e come questi ultimi le trasferiscono ai client. Quando l’utente richiede una pagina web (per esempio, cliccando su un collegamento ipertestuale), il browser invia al server messaggi di richiesta HTTP per gli oggetti nella pagina. Il server riceve le richieste e risponde con messaggi di risposta HTTP contenenti gli oggetti.