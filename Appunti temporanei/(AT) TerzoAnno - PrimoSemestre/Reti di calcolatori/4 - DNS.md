Gli host all'interno di Internet possono essere identificati in vari modi. 
Gli **hostname** quali `www.facebook.com`, `www.google.com` e `gaia.cs.umass.edu`, risultano intuitivi per le persone, ma forniscono ben poca informazione sulla loro collocazione all'interno di Internet. Per tali motivi, gli host vengono identificati anche dai cosiddetti **indirizzi IP**.

**Un indirizzo IP**(v4) consiste di quattro byte e presenta una rigida struttura gerarchica. Ha una forma del tipo `121.7.106.83`, in cui ogni punto separa uno dei byte espressi con un numero decimale compreso tra 0 e 255. Un indirizzo IP è gerarchico perché, leggendolo da sinistra a destra, otteniamo informazioni sempre più specifiche sulla collocazione dell’host in Internet (ossia sulla sua appartenenza a quale rete all'interno della rete di reti).
### Servizio fornito dal DNS
Al fine di conciliare hostname e indirizzi IP è necessario un servizio in grado di tradurre il primo al secondo.
Si tratta del **DNS (Domain Name System)**, che è composto da:
1. Un database distribuito implementato in una gerarchia di **DNS server**;
2. Un protocollo a livello di applicazione che consente agli host di interrogare il database

DNS viene comunemente utilizzato da altri protocolli a livello di applicazione, incluso HTTP, per tradurre i nomi di host forniti dall'utente in indirizzi IP. 

> [!example] Esempio di risoluzione DNS in un browser
> Consideriamo che cosa succede quando un browser (ossia un client HTTP) in esecuzione sull’host di un utente richiede l’URL `www.someschool.edu/index.html`. L’host dell’utente, per essere in grado di inviare un messaggio di richiesta HTTP al web server `www.someschool.edu`, deve come prima cosa ottenere il suo indirizzo IP. Ciò avviene come segue. 
> 1. La stessa macchina utente esegue il lato client dell’applicazione DNS. 
> 2. Il browser estrae il nome dell’host, `www.someschool.edu`, dall’URL e lo passa al lato client dell’applicazione DNS. 
> 3. Il client DNS invia una interrogazione (query) contenente l’hostname a un DNS server. 
> 4. Il client DNS prima o poi riceve una risposta, che include l’indirizzo IP corrispondente all’hostname. 
> 5. Una volta ricevuto l’indirizzo IP dal DNS, il browser può dare inizio a una connessione TCP verso il processo server HTTP collegato alla porta 80 di quell’indirizzo IP.


