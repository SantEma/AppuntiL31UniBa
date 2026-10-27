Gli host all'interno di Internet possono essere identificati in vari modi. 
Gli **hostname** quali `www.facebook.com`, `www.google.com` e `gaia.cs.umass.edu`, risultano intuitivi per le persone, ma forniscono ben poca informazione sulla loro collocazione all'interno di Internet. Per tali motivi, gli host vengono identificati anche dai cosiddetti **indirizzi IP**.

**Un indirizzo IP**(v4) consiste di quattro byte e presenta una rigida struttura gerarchica. Ha una forma del tipo `121.7.106.83`, in cui ogni punto separa uno dei byte espressi con un numero decimale compreso tra 0 e 255. Un indirizzo IP è gerarchico perché, leggendolo da sinistra a destra, otteniamo informazioni sempre più specifiche sulla collocazione dell’host in Internet (ossia sulla sua appartenenza a quale rete all'interno della rete di reti).
### Servizio fornito dal DNS
Al fine di conciliare hostname e indirizzi IP è necessario un servizio in grado di tradurre il primo al secondo.
Si tratta del **DNS (Domain Name System)**, che è composto da:
1. Un database distribuito implementato in una gerarchia di **DNS server**;
2. Un protocollo a livello di applicazione che consente agli host di interrogare il database

