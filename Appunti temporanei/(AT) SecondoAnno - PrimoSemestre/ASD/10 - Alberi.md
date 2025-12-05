# Grafi
Un **grafo** $G$ è definito da una coppia di insiemi $<N,A>$ dove $N=insieme\ dei\ nomi$ e $A=insieme\ degli\ archi$, dove $A$ è definito come sottoinsieme del prodotto cartesiano $N\times N$.

Si dice che se un arco è definito da un nodo $u_{i}\in N$ ad un nodo $u_{j}\in N$, allora la coppia $(u_{i},u_{j})\in A$ ed in questo caso il grafo è **orientato**.
## Grafo orientato
In questo tipo di grafo la direzione del collegamento è fondamentale per essere tale.
![[Pasted image 20251125125535.png]]
Visivamente il collegamento si rappresenta con una freccia che parte dal primo nodo e punta al secondo ed è **mono-direzionale**, ovvero che una freccia che va da $A$ in $B$ non implica che valga l'inverso.
### Cammino
In un grafo ordinato $G$ un cammino è una sequenza ordinata di nodi $u_{0},u_{1},\dots,u_{k}$ tali che $(u_{i},u_{i_+1})\in A$, per $i=0,1,2,\dots,k-1$. 
Quindi il cammino parte da $u_{0}$ e attraversa tutti i nodi, arriva al nodo $u_{k}$ ed ha lunghezza pari a $k$ stesso.

Non tutti i cammini sono uguali, a seconda di come *saltiamo* tra i nodi, possiamo classificare il cammino in  modi differenti:
- **cammino semplice**: viene definito tale quando nel percorso non avvengono **nodi ripetuti**. In sintesi, non si torna mai su un nodo già visitato ma si esplorano solo quelli nuovi e una sola volta e devono essere tutti percossi.
  ![[Pasted image 20251125132425.png]]
- **cammino chiuso**: il cammino viene definito chiuso quando il punto di partenza $u_{0}$ coincide con il nodo iniziale $u_{k}$, riportandoci all'inizio del percorso.
- **ciclo**: il ciclo è **l'unione tra il cammino semplice e il cammino chiuso**. E' chiuso perché torna al punto di partenza ma è anche semplice perché il ciclo arrivando al punto di partenza come punto di arrivo **ripartirà**, gli altri nodi intermedi sono distinti e non verranno mai ripetuti.
  ![[Pasted image 20251126104716.png]]
### Gradi di connessione nei grafi orientati
Quando si analizza un grafo è importante capire non solo le sue frecce ma anche quanto è **lungo**, per questo si studiano tre situazioni importanti in cui si suddivide il suo grado (dalla matematica discreta è la sua lunghezza):
- **Grafo completo**: un grafo si definisce completo se $\forall(u_{i},u_{j})\in N$ esiste sicuramente un arco che va da $u_{i}$ a $u_{j}\ (A=N\times N)$ 
- **Grafo connesso**: un grafo si definisce connesso se dati $u\in N \wedge v\in N$ esiste un cammino da $u$ a $v$ o uno che va da $v$ a $u$
- **Grafo fortemente connesso**: un grafo si definisce fortemente connesso se $\forall(u,v)\exists\ un\ cammino\ da\ u$ a $v$ ed almeno $un\ cammino\ da\ v$ ad $u$. Basta che un nodo non soddisfi questa proprietà ed il grafo non è fortemente connesso, inoltre un grafo **completo è sempre fortemente connesso**.
  ![[Pasted image 20251126105958.png]]
  In pratica, in un grafo fortemente connesso non ci sono strade senza uscita o sensi unici che bloccano il flusso; da qualsiasi punto si sceglie di partire, si può raggiungere qualsiasi altro punto e poi tornare indietro.
## Grafo non orientato
In questa tipologia non esiste il concetto di freccia ma quello di linea, avendo un collegamento **simmetrico** (infatti si dice che l'arco **incide** su entrambi i nodi). Se due nodi $u_{i}$ e $u_{j}$ sono collegati, la relazione vale in entrambi i sensi.
![[Pasted image 20251126110737.png]]
La loro differenza principale rispetto a quelli orientati sta nella definizione dell'**arco**.
Nel **grafo orientato** la coppia $(A,B)\neq(B,A)$, mentre nel **grafo non orientato** le coppie non sono ordinate quindi scrivere $(A,B)\vee(B,A)$ è la stessa cosa con lo stesso arco.

Due nodi che sono collegati direttamente da un arco si dicono **adiacenti**. Se c'è una linea tra il nodo $u$ e il nodo $v$, allora $u$ è adiacente a $v$ e viceversa.

Per muoverci tra i nodi, la logica è la stessa ma con nomi differenti:
quelle che nel grafo orientato prendevano il nome di **cammino** (ovvero la sequenza di nodi da attraversare) qui prende il nome di **catena**, mentre quello che nel grafo orientato era il **ciclo** (il percorso chiuso che ripartiva dall'inizio), qui prende il nome di **circuito**.
## Alberi
Un albero è un particolare tipo di grafo; si definisce sempre con la coppia $T<N,A>$, dove $N$ sono i nodi e $A$ sono gli archi.
Tuttavia, riprendendo i concetti dei linguaggi di programmazione e della matematica discreta, un albero per essere tale deve seguire delle regole ben precise:
- **Connesso**: Presi due nodi qualsiasi, esiste sempre una strada che permette di arrivare dall'uno all'altro.
- **Aciclico**: Non devono esiste percorsi chiusi o *anelli*; non bisogna mai tornare ad un punto di partenza mentre ci si muove tra gli archi.
- **Numero di archi**: Il numero degli archi $A$ deve rispettare un determinato numero preciso dettato dalla formula $A=N-1$.
- **Radice**: La radice definisce la struttura grafica che intraprenderà l'albero. Si sceglie un nodo $r$ che da quel momento sarà fissato in alto e sarà denominato radice, da esso partiranno a **cascata** tutti gli altri nodi tramite i loro archi. Questo ci permette di ordinare i nodi per **livelli** e creare una gerarchia che verrà usata nelle **strutture dati**.
![[Pasted image 20251126112644.png]]
![[Pasted image 20251126112720.png]]

Analizziamo queste strutture e vediamo che i **nodi con lo stesso padre sono fratelli**, come $u_{3},u_{4},u_{1}$. I nodi **terminali senza figli** sono detti **foglie dell'albero**, come $u_{5}$.
### Proprietà per il suo movimento
1. Ogni nodo ha un solo **padre** ad eccezione della radice che è il capostipite.
2. Essendo l'albero connesso ma aciclico, ne consegue che, se si vuole tornare alla radice esiste **solo un unico cammino** e se ci fossero due modi diversi per arrivare allo stesso nodo, ci sarebbe un ciclo e quindi si va ad **infrangere** la regola che definisce un albero.
3. Un albero è composto da tanti alberi più piccoli l'uno nell'altro. Infatti se si prende un nodo $u$, che non sia la radice, e ne vedessimo la struttura sottostante vedremo un **sotto albero**, con *radice* il nodo che abbiamo selezionato.
   Questo fenomeno prende nome di **struttura ricorsiva**.
   ![[Pasted image 20251126113947.png]]
#### Definizioni importanti
- **Albero ricorsivo**: Un albero è un grafo non orientato che o è vuoto oppure esiste un nodo $r$ detto radice, senza predecessori, con $n>=0$ nodi successori $a_{1},a_{2},\dots,a_{n}$. Tutti gli altri nodi sono ripartiti in $n$ sottoalberi mutuamente disgiunti $T_{1},T_{2},\dots,T_{n}$ aventi rispettivamente $a_{1},a_{2},\dots,a{n}$ come loro radice.

L'albero è spesso utilizzato per rappresentare relazioni gerarchiche tra oggetti; la definizione data presuppone che sui figli sia definita una relazione d'ordine, infatti:
![[Pasted image 20251126115006.png]]
- La **profondità di un nodo** è la lunghezza del percorso dalla radice al nodo.
- Il **livello** è l'insieme dei nodi allo stesso livello di profondità $p$.
- **L'altezza dell'albero** è il massimo livello delle sue foglie.
- **Ordine**: definiamo un albero di ordine $k$, un albero in cui ogni nodo ha al massimo $k$ figli.
## Specifica sintattica e semantica
I tipi di dati che useremo per le operazioni sono:
- **albero**: insieme degli alberi ordinati $T=<N,A>$ in cui ad ogni nodo $n$ in $N$ è associato il livello($n$).  
- **boolean**: insieme dei valori di verità.
- **nodo**: insieme qualsiasi che non sia infinito.

Gli operatori che useremo sono:
<table class="table table-bordered">
  <thead>
    <tr>
      <th>Operatore</th>
      <th>Input (Dominio)</th>
      <th>Output (Codominio)</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>CREAALBERO</code></td>
      <td><code>()</code></td>
      <td><code>albero</code></td>
      <td>Crea un nuovo albero vuoto.</td>
    </tr>
    <tr>
      <td><code>INSRADICE</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>albero</code></td>
      <td>Questo comando serve per inserire il primissimo nodo nell'albero.</td>
    </tr>
    
    <tr>
      <td><code>ALBEROVUOTO</code></td>
      <td><code>(albero)</code></td>
      <td><code>boolean</code></td>
      <td>Serve a controllare se c'è qualcosa nell'albero.</td>
    </tr>
    <tr>
      <td><code>RADICE</code></td>
      <td><code>(albero)</code></td>
      <td><code>nodo</code></td>
      <td>Restituisce il nodo radice dell'albero.</td>
    </tr>
    <tr>
      <td><code>FOGLIA</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>boolean</code></td>
      <td>Verifica se il nodo non ha figli.</td>
    </tr>
    <tr>
      <td><code>ULTIMOFRATELLO</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>boolean</code></td>
      <td>Verifica se il nodo è l'ultimo dei suoi fratelli.</td>
    </tr>

    <tr>
      <td><code>PADRE</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>nodo</code></td>
      <td>Restituisce il genitore del nodo specificato.</td>
    </tr>
    <tr>
      <td><code>PRIMOFIGLIO</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>nodo</code></td>
      <td>Restituisce il primo figlio del nodo.</td>
    </tr>
    <tr>
      <td><code>SUCCFRATELLO</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>nodo</code></td>
      <td>Restituisce il fratello successivo (immediatamente a destra).</td>
    </tr>

    <tr>
      <td><code>INSPRIMOSOTTOALBERO</code></td>
      <td><code>(nodo, albero, albero)</code></td>
      <td><code>albero</code></td>
      <td>Inserisce un intero sottoalbero come primo figlio del nodo dato.</td>
    </tr>
    <tr>
      <td><code>INSSOTTOALBERO</code></td>
      <td><code>(nodo, albero, albero)</code></td>
      <td><code>albero</code></td>
      <td>Inserisce un sottoalbero come fratello successivo del nodo dato.</td>
    </tr>
    <tr>
      <td><code>CANCSOTTOALBERO</code></td>
      <td><code>(nodo, albero)</code></td>
      <td><code>albero</code></td>
      <td>Rimuove il sottoalbero che ha radice nel nodo specificato.</td>
    </tr>
  </tbody>
</table>

Semantica delle operazioni:
<table class="table table-bordered">
  <thead>
    <tr>
      <th style="width: 30%;">Operatore</th>
      <th>Specifica Semantica (Pre e Post Condizioni)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Costruttori</strong></td>
    </tr>
    <tr>
      <td><code>CREAALBERO = T'</code></td>
      <td>
        <strong>POST:</strong> T' = (&emptyset;, &emptyset;) = &Lambda; <em>(albero vuoto)</em>
      </td>
    </tr>
    <tr>
      <td><code>INSRADICE(u, T) = T'</code></td>
      <td>
        <strong>PRE:</strong> T = &Lambda;<br>
        <strong>POST:</strong> T' = (N, A), dove N={u}, livello(u)=0, A=&emptyset;<br>
        <em>oppure:</em> T' = ({u}, &emptyset;), livello(u) = 0
      </td>
    </tr>

    <tr>
      <td><strong>Ispezione</strong></td>
    </tr>
    <tr>
      <td><code>ALBEROVUOTO(T) = b</code></td>
      <td>
        <strong>POST:</strong> b = VERO se T = &Lambda;<br>
        b = FALSO, altrimenti
      </td>
    </tr>
    <tr>
      <td><code>RADICE(T) = u</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda; &rArr; &exists; v &isin; N t.c. livello(v) = 0<br>
        <strong>POST:</strong> u = v
      </td>
    </tr>
    <tr>
      <td><code>FOGLIA(u, T) = b</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, u &isin; N<br>
        <strong>POST:</strong> b = VERO se &not;&exists; v &isin; N t.c. &lt;u,v&gt; &isin; A AND livello(v) = livello(u) + 1<br>
        b = FALSO, altrimenti<br>
        <em>(In alternativa: b=VERO se u non è PADRE di nessuno)</em>
      </td>
    </tr>
    <tr>
      <td><code>ULTIMOFRATELLO(u, T) = b</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, u &isin; N<br>
        <strong>POST:</strong> b = VERO se non esistono altri fratelli di u che lo seguono nella relazione d'ordine;<br>
        b = FALSO, altrimenti
      </td>
    </tr>

    <tr>
      <td  ><strong>Navigazione</strong></td>
    </tr>
    <tr>
      <td><code>PADRE(u, T) = v</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, u &isin; N, livello(u) &gt; 0<br>
        <strong>POST:</strong> v = x t.c. &lt;x, u&gt; &isin; A AND livello(x) = livello(u) - 1
      </td>
    </tr>
    <tr>
      <td><code>PRIMOFIGLIO(u, T) = v</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, u &isin; N, FOGLIA(u, T) = FALSO<br>
        <strong>POST:</strong> v = x t.c. &lt;u, x&gt; &isin; A, livello(x) = livello(u)+1<br>
        <em>(x è il primo figlio secondo la relazione d'ordine definita tra i figli di u)</em>
      </td>
    </tr>
    <tr>
      <td><code>SUCCFRATELLO(u, T) = v</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, u &isin; N, ULTIMOFRATELLO(u, T) = FALSO<br>
        <strong>POST:</strong> v è il fratello di u che lo segue nella relazione d'ordine
      </td>
    </tr>

    <tr>
      <td  ><strong>Modifica Strutturale</strong></td>
    </tr>
    <tr>
      <td><code>INSPRIMOSOTTOALBERO(u, T, T') = T''</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, T' &ne; &Lambda;, T=&lt;N,A&gt;, T'=&lt;N',A'&gt;, u &isin; N<br>
        <strong>POST:</strong> T'' è ottenuto da T aggiungendo l'albero T' la cui radice r' è il nuovo primo figlio di u
      </td>
    </tr>
    <tr>
      <td><code>INSSOTTOALBERO(u, T, T') = T''</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, T' &ne; &Lambda;, T=&lt;N,A&gt;, T'=&lt;N',A'&gt;, u &isin; N, RADICE(T) &ne; u<br>
        <strong>POST:</strong> T'' è l'albero ottenuto da T aggiungendo il sottoalbero T' di radice r' in modo che r' risulti il successivo fratello di u
      </td>
    </tr>
    <tr>
      <td><code>CANCSOTTOALBERO(u, T) = T'</code></td>
      <td>
        <strong>PRE:</strong> T &ne; &Lambda;, u &isin; N<br>
        <strong>POST:</strong> T' è ottenuto da T rimuovendo il sottoalbero di radice u (cioè u e tutti i suoi discendenti)
      </td>
    </tr>
  </tbody>
</table>

![[Pasted image 20251126130752.png]]
![[Pasted image 20251126130844.png]]
## Visita di alberi
La **visita di alberi** è un algoritmo che ci permette di pianificare una *rotta* che consente di **esaminare ogni nodo dell'albero** esattamente una **singola volta**.
A seconda delle esigenze possiamo scegliere diverse strategie per tracciare la rotta, in base alla struttura del nostro albero. La differenza tra i metodi di visita, sta proprio nell'ordine in cui decidiamo di seguire la struttura:
- se scendere subito in profondità lungo un ramo
- se vogliamo guardare prima tutti i nodi vicini sullo stesso livello
### DFS (Depth First Search)
Questa strategia di visita scende in **profondità** ed è nota anche come visita a **scandaglio**.
La strategia è di partire da un nodo e percorrerlo fino in fondo, fino a trovare una foglia, e si sceglie di percorrere un determinato ramo e si prosegue con il medesimo prima di tornare indietro e vedere gli altri. I rami vengono visitati **uno dopo l'altro** ed esistono tre **varianti** di questa visita che dipendono dal momento esatto in cui decidiamo di *leggere* il nodo rispetto ai suoi figli.
### BFS (Breadth First Search)
Questa strategia di visita scende in **ampiezza** ed è nota anche come visita a **ventaglio**.
L'approccio utilizzato è di tipo **orizzontale**, non si scorre più fino in fondo ma si esplora l'albero per **livelli**:
1. Si parte dalla **root**
2. Si visualizzano tutti i figli della radice di livello 1
3. Dopo averli visti tutti si scende per effettuare la stessa operazione ai nodi del livello 2
4. andare fino alla fine
### Gli ordini
A seconda della metodologia scelta, in base al caso più adeguato, i nodi vengono processati in **ordine**, ed esistono quattro tipi di ordinamento:
- **Previsita**:
  In questa modalità si parte da $r$ e si visualizzano i sottoalberi $T_{1},T_{2}$ da sinistra a destra.
  Il prefisso **pre** indica proprio che si visiti prima la radice e poi si vede cosa c'è sotto di lei.![[Pasted image 20251126135611.png]]
  Si parte dalla cima $5$, per poi visitare i figli da sinistra a destra, quindi si visualizza $4$ poi il suo sottoalbero, partendo sempre da sinistra a destra si visualizza $8$ e poi $9$, dato che $8$ non ha figli e da $9$ si visitano le foglie.
  Dato che un sottoalbero è terminato, si risale a $4$ e i segue la stessa operazione con $15$, ed essendo che non ha figli si termina così anche un altro sottoalbero.
  Si procede così via via con il figlio $6$ e si visualizzano i suoi figli in ordine $21$ e $20$.
  Il risultato finale è: $$5,4,8,9,12,11,4,15,6,21,20$$
- **Postvisita**:
  In questa modalità si visitano prima i sottoalberi e poi si sale verso la radice.
  Il prefisso **post** indica proprio che la radice venga visitata dopo aver visto cosa sta sotto di lei.
  ![[Pasted image 20251126161421.png]]
  Si parte dalla cima ma la lasciamo in disparte, prima si analizzano i figli, quindi si andrà al $4$, che ha figli e quindi si trascura e andiamo ai figli di esso, quindi $8$ che è una foglia e si scrive e $9$, esso ha tre foglie e si prendono in ordine da sinistra a destra. Dopo aver registrato i figli di $9$ si può registrare 9 stesso e salendo più su anche il $4$. Si procede fino a risalire al radice con questo modus operandi avendo come risultato finale: $$8,12,11,3,9,4,15,21,20,6,5$$
  L'implementazione di previsita e postvisita è il seguente: 
  ```c++
  PREVISITA(var T:albero; U:nodo){
	  nodo C;
	  {esamina nodo U}; //1
	  if(!FOGLIA(U,T)){ //2
		  C=PRIMOFIGLIO(U,T);
		  while(!ULTIMOFRATELLO(C,T)){
			  PREVISITA(T,C);
			  C=SUCCFRATELLO(C,T);
		  }
		  PREVISITA(T,C);
	  }
  }
  ```
  La prima azione che esegue l'algoritmo è visitare subito il nodo su cui arriva l'istruzione, dopo di che controlla se il nodo ha figli, se è foglia, l'algoritmo si ferma, altrimenti entra nel blocco dell'if per gestire i discendenti, ripetendo le operazioni fino all'ultimo fratello presente. All'uscita del ciclo, la funzione chiama se stessa per vedere gli altri fratelli.
  
  **Scambiando l'ordine dell'istruzione 1 con la 2 si otterrà la Postvisita**.
  Come si può notare, strutturalmente l'algoritmo di navigazione è identico, cambia solo il momento in cui si decide di stampare il dato.
  
- **Invisita**:
  Più arzigogolato è la struttura **in**, dove il prefisso stesso definisce l'interno, ovvero la radice viene visualizzata internamente durante la visita ai sottoalberi iniziali e quelli finali.
  Si parte principalmente dal sottoalbero $T_{1}$, successivamente si visualizza $r$ ed in fine, in ordine, i sottoalberi restanti.
  
  Per applicare questa visita si sceglie un **punto di taglio** $i$, solitamente impostato ad uno.
  La struttura diventa la seguente ![[Pasted image 20251126164014.png]]
  Si analizzano totalmente prima gli $i$ sottoalberi, dopo di che si studia la **radice corrente** ed in fine, si riparte con gli altri sottoalberi mancanti.
  Seguendo l'esempio avremo quindi come risultato finale: $$8,4,12,9,11,3,5,15,21,6,20$$
  L'implementazione è simile alla precedente, con l'aggiunta della nuova clausola $i$:
  ```c++
  INVISTA(var T:albero,U:nodo,i:int){
	if(FOGLIA(U,T))
		  {esamina nodo U}
	else{
		nodo C=PRIMOFIGLIO(U,T);
		int k=0;
		while(!ULTIMOFRATELLO(C,T)&&k<i){
			k=k+1;
			INVISITA(T,C,i);
			C=SUCCFRATELLO(C,T);
		}
		if(ULTIMOFRATELLO(C,T)&&k<i) //ultimofratello==true si esce
			INVISITA(T,C,i);
		{esamina nodo U};
		while(!ULTIMOFRATELLO(C,T)){
			INVISITA(T,C,i);
			C=SUCCFRATELLO(C,T);
		}
		if(k==i)
			INVISITA(T,C,i);
	}
  }
   ```
 
- **Ampiezza**:
  Analizzando sempre l'esempio precedente, qui non andremo a studiare i rami ma i nodi saranno visitati per livello, partendo dalla root fino all'ultima foglia a destra.
  
  Il termine **Ampiezza** indica che ci allarghiamo su tutto l'orizzonte del livello corrente prima di scendere più in profondità nel livello successivo. Questa strategia è completamente diversa dalle tre precedenti. Se prima seguivamo i rami e le parentele, qui **ignoriamo** le parentele e guardiamo l'albero come se fosse fatto a strati.
  L'algoritmo non scende $n$ profondità, ma si muove orizzontalmente.
  
  Al livello $0$ c'è la radice, il valore $5$.
  Al livello $1$ sono presenti i figli della radice, quindi $4,15,6$.
  Al livello $2$ sono presenti i nipoti, sarebbero i sottoalberi, quindi qui si registrerà sotto il $4$, l'$8,9$; sotto la root di $9$ si avranno i **pronipoti** e così via, avendo in fine: $$5,4,15,6,8,9,21,20,12,11,3$$
## Realizzazione
### Realizzazione vettori padri
Questo modello di rappresentazione si basa sull'inserire la struttura dell'albero in un vettore sequenziale, sfruttando le relazioni parentali.
La lunghezza del vettore deve essere pari ad $n$, dove $n$ è anche il numero dei nodi dell'albero. La logica di memorizzazione dei dati si basa su due regole:
- **Identificazione del nodo**: Ogni posizione $i$ dell'array corrisponde ad uno specifico nodo
- **Riferimento al padre**: Il valore presente nella posizione $i$ è l'indicativo al padre
![[Pasted image 20251127163511.png]]
Analizzando il disegno e prendendo il nodo $c$, notiamo come sia posizionato nell'indice $4$ e il contenuto della cella $4$ è il valore $2$ che corrisponde alla cella in cui è presente il nodo padre $b$.

A livello di C.C. questa struttura presenta un ottimo **vantaggio**, ovvero la possibilità di poter risalire velocemente ai padri, tuttavia presenta anche un grande **svantaggio**, se bisogna cancellare o inserire un intero sottoalbero, la via più facile sarebbe quella di riprogrammarlo totalmente da zero.
### Realizzazione con liste di figli
Questa implementazione non presenta migliorie ma semplicemente privilegia la memorizzazione esplicita dei figli piuttosto che contare i padri, tramite l'uso delle liste.

Ogni cella del vettore rappresenta uno specifico nodo contenente due informazioni:
- **Informazione del nodo**
- **Puntatore**
Il puntatore presente nel vettore indirizza ad una **lista** che contiene tanti elementi quanti sono i successori del nodo corrente; se l'elemento corrente non ha figli, quindi è una foglia, il valore del puntatore sarà **nullo**.
![[Pasted image 20251127174901.png]]
### Realizzazione con lista primo figlio/ primo fratello
Questa soluzione è una delle migliori e una delle più eleganti per rappresentare alberi con un numero arbitrario di figli, utilizzando una struttura a dimensione fissa per ogni nodo.
Invece di usare una struttura dinamica per i figli, che porta a gran spreco di memoria, ogni nodo mantiene esattamente due **puntatori**:
- **Puntatore al primo figlio**
- **Puntatore al fratello adiacente successivo**
La sua realizzazione si manifesta tramite l'uso di un array di record, dove ogni riga rappresenta un nodo con tre campi fondamentali:
![[Pasted image 20251127175741.png]]
Possiamo comprendere da questo schema che partendo dalla cella $4$ in cui c'è il padre si controlla qual è il puntatore al suo **primo figlio diretto** (colonna a sinistra), in questo caso $7$, per questo nella cella $7$ visualizziamo il nodo di $d$, con le sue caratteristiche. Tornando ad $a$ (nella colonna di destra) si nota il puntatore al **fratello diretto**, che è nullo poiché non esiste, caso contrario in $d$ in cui esso è presente e punta alla cella $3$ in cui è presente $c$, e si procede in questo modo.

I **vantaggi** di questa struttura sono la possibilità di usare la **stessa quantità di memoria** indipendentemente da quanti figli esso possieda e inoltre si possono rappresentare alberi arbitrario tramite la **logica delle liste concatenate**.
### Realizzazione con vettore dei figli
![[Realizzazione_vett_figli.png]]In questa rappresentazione, ogni nodo dell'albero è una **struttura composta a due elementi**, un riferimento al nodo del genitore (zona blu) e un array contenente i puntatori ai nodi dei figli (zona nera).
La caratteristica principale di questa strategia è che la dimensione dell'array per arrivare ai figli dev'essere **uguale al grado massimo $k$**; questo porta però ad un enorme **svantaggio**, esiste un alto rischio di **spreco di memoria** qualora la topologia dell'albero sia altamente disomogenea.
Questo avviene se molti nodi hanno un grado altamente inferiore al grado massimo, la maggior parte delle celle del vettore rimarrà vuota e **inutilizzata**. Questo è un problema particolarmente evidente nelle **foglie**, che hanno grado $0$ ma **occupano comunque lo spazio per un vettore** di dimensione $k$.
### Realizzazione con puntatori padre - primo figlio - fratello
![[padre_primoF_fratello.png]]Questa soluzione risolve il problema dello spreco di memoria. Invece di prevedere un numero variabile di puntatori, ogni nodo della struttura possiede esattamente **tre campi fissi**, indipendentemente dalla sua posizione nell'albero:
- Un puntatore al padre
- Un puntatore al **primo** figlio
- Un puntatore che riferisce al fratello **immediatamente successivo**
Questa implementazione garantisce che ogni nodo occupi una quantità di **memoria costante**, eliminando gli sprechi delle celle vuote tipiche dei vettori a dimensione fissa.
## Alberi binari
Un albero binario è un particolare tipo di albero ordinato che deve rispettare due regole per essere tale:
- Ogni nodo può avere **al massimo due figli**, da qui il nome binario.
- I figli vengono esclusivamente indicati come **figlio sinistro** e **figlio destro**. La posizione dei figli infatti definisce un aspetto cruciale che identifica questa nuova tipologia di albero.

![[Distinzioni_alberi_binari.png]] 
Vedendo questi tre alberi si nota chiaramente che sono strutturalmente simili ma a livello di significato e di informazioni sono totalmente differenti, si vede chiaramente che il nodo $2$ in $T_{2}$ è a **sinistra**, mentre in $T_{3}$ è a **destra**, andando a cambiare **matematicamente** totalmente il significato.

Anche gli alberi binari seguono le caratteristiche della **struttura ricorsiva** tipica degli alberi base per quanto riguarda i sottoalberi.
Nelle implementazioni come `C++ e JAVA` spesso si accede alla foglia finale di un nodo se a sinistra con `a.left()` altrimenti con `a.right()`. Per accedere invece al padre da cui discendono questi due nodi, l'operazione usata è `j.parent()`, con `j` nome del nodo figlio.
### Equivalenza tra alberi $n$-ari e alberi binari
E' sempre possibile trasformare un generico albero $n$-ario $T$ in un albero binario corrispondente, avente stessi nodi e radice.
![[Pasted image 20251129190343.png]]
La regola di conversione si basa sui puntatori:
Il puntatore **sinistro** diviene una discendenza, quindi un **figlio**, il puntatore **destro** diviene un elemento di orizzontalità, quindi un **fratello**.

Analizzando lo schema seguente possiamo capire con questo esempio il corretto funzionamento:
**Albero generico**:![[Pasted image 20251129191701.png]]
Per trasformare questo albero generico in binario dobbiamo ricordarci che, un nodo di un albero binario non può avere più di due figli e che la conversione si basa sulla regola che ciò che sta a sinistra verticalmente diventa figlio sinistro, ciò che sta a destra orizzontalmente diventa figlio destro.

In questo caso partendo dalla radice $A$ il primo figlio che appare a sinistra è $B$, notiamo come $B$ ha un fratello, esso diventerà suo figlio destro, mentre il figlio $E$ diventa figlio sinistro. $E$ a sua volta ha un fratello che diventerà sempre suo figlio destro.
Tornando sopra a $C$ si nota come esso abbia solo un figlio, quindi $G$ rimane al suo posto sinistro e il fratello successivo di $C$, ovvero $D$, diventerà suo figlio destro.
Infine si otterrà questo **albero binario**:
![[Pasted image 20251129192323.png]]
### Specifica sintattica e semantica alberi binari
<table>
  <thead>
    <tr>
      <th>Categoria</th>
      <th>Operatore / Concetto</th>
      <th>Input / Condizioni</th>
      <th>Descrizione e Risultato (Post-condizione)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3"><strong>Definizioni di Base</strong></td>
      <td><strong>ALBEROBIN</strong></td>
      <td>N/A</td>
      <td>Insieme matematico degli alberi binari <em>T = (N, A)</em>, dove a ogni nodo è associato un livello.</td>
    </tr>
    <tr>
      <td><strong>CREABINALBERO</strong></td>
      <td>Nessuno</td>
      <td>Crea l'albero vuoto.<br><strong>Risultato:</strong> L'albero risultante <em>T'</em> è uguale a &Lambda; (insieme vuoto).</td>
    </tr>
    <tr>
      <td><strong>BINALBEROVUOTO</strong></td>
      <td>Albero <em>T</em></td>
      <td>Controlla l'esistenza dell'albero.<br><strong>Risultato:</strong> Restituisce <em>Vero</em> se <em>T = &Lambda;</em>, altrimenti <em>Falso</em>.</td>
    </tr>

    <tr>
      <td rowspan="2"><strong>Gerarchia (Verso l'alto)</strong></td>
      <td><strong>BINRADICE</strong></td>
      <td><em>T &ne; &Lambda;</em> (Albero non vuoto)</td>
      <td>Restituisce il nodo <em>u</em> che ha livello 0 (<em>livello(u) = 0</em>).</td>
    </tr>
    <tr>
      <td><strong>BINPADRE</strong></td>
      <td>Nodo <em>u</em>, Albero <em>T</em><br>(<em>livello(u) > 0</em>)</td>
      <td>Restituisce il nodo <em>v</em> collegato direttamente a <em>u</em> al livello superiore (<em>livello(v) = livello(u) - 1</em>).</td>
    </tr>

    <tr>
      <td rowspan="4"><strong>Navigazione (Verso il basso)</strong></td>
      <td><strong>SINISTROVUOTO</strong></td>
      <td>Nodo <em>u</em>, Albero <em>T</em></td>
      <td><strong>Ispezione:</strong> Restituisce <em>Vero</em> se il nodo <em>u</em> non ha un figlio sinistro, <em>Falso</em> altrimenti.</td>
    </tr>
    <tr>
      <td><strong>DESTROVUOTO</strong></td>
      <td>Nodo <em>u</em>, Albero <em>T</em></td>
      <td><strong>Ispezione:</strong> Restituisce <em>Vero</em> se il nodo <em>u</em> non ha un figlio destro, <em>Falso</em> altrimenti.</td>
    </tr>
    <tr>
      <td><strong>FIGLIOSINISTRO</strong></td>
      <td>Nodo <em>u</em>, Albero <em>T</em><br>(<em>SINISTROVUOTO</em> = Falso)</td>
      <td>Restituisce il nodo <em>v</em> che è specificamente il figlio sinistro di <em>u</em>.</td>
    </tr>
    <tr>
      <td><strong>FIGLIODESTRO</strong></td>
      <td>Nodo <em>u</em>, Albero <em>T</em><br>(<em>DESTROVUOTO</em> = Falso)</td>
      <td>Restituisce il nodo <em>v</em> che è specificamente il figlio destro di <em>u</em>.</td>
    </tr>

    <tr>
      <td rowspan="2"><strong>Modifica Strutturale (Bottom-Up)</strong></td>
      <td><strong>COSTRBINALBERO</strong><br><em>(Costruttore)</em></td>
      <td>Albero <em>T</em> (sinistro), Albero <em>T'</em> (destro)</td>
      <td>Unisce due alberi esistenti sotto una nuova radice.<br><strong>Risultato:</strong> Crea una nuova radice <em>r''</em>. <em>T</em> diventa il sottoalbero sinistro, <em>T'</em> diventa il sottoalbero destro.</td>
    </tr>
    <tr>
      <td><strong>CANCSOTTOBINALBERO</strong><br><em>(Distruttore)</em></td>
      <td>Nodo <em>u</em>, Albero <em>T</em></td>
      <td>Operatore di "potatura".<br><strong>Risultato:</strong> Rimuove il nodo <em>u</em> e, a cascata, elimina l'intero sottoalbero che ha <em>u</em> come radice (tutti i discendenti).</td>
    </tr>
  </tbody>
</table>

Per gestire i dati, dobbiamo aggiornare il nostro vocabolario introducendo un nuovo tipo e due nuove funzioni per quanto riguardando gli alberi $n$-ari:
<table>
  <thead>
    <tr>
      <th>Categoria</th>
      <th>Operatore / Tipo</th>
      <th>Sintassi (Input &rarr; Output)</th>
      <th>Specifica Semantica e Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Nuovi Tipi</strong></td>
      <td><strong>TIPOELEM</strong></td>
      <td><em>N/A (Definizione astratta)</em></td>
      <td>Rappresenta il tipo di informazione salvata nel nodo (l'etichetta).<br>Può essere un <em>int</em>, una <em>String</em> o un oggetto complesso.</td>
    </tr>

    <tr>
      <td rowspan="2"><strong>Manipolazione Dati</strong></td>
      <td><strong>LEGGINODO</strong><br><em>(Lettura)</em></td>
      <td>(NODO, ALBEROBIN) &rarr; TIPOELEM</td>
      <td>Funzione per recuperare il dato contenuto in un nodo.<br><strong>Pre-condizione (PRE):</strong> Il nodo <em>n</em> deve esistere in <em>T</em> (<em>n &isin; N</em>). Non si può leggere un nodo inesistente.<br><strong>Post-condizione (POST):</strong> Restituisce esattamente il valore <em>a</em> (di tipo TIPOELEM) associato al nodo in quel momento.
      </td>
    </tr>
    <tr>
      <td><strong>SCRIVINODO</strong><br><em>(Scrittura)</em></td>
      <td>(TIPOELEM, NODO, ALBEROBIN) &rarr; ALBEROBIN</td>
      <td>Funzione per aggiornare o scrivere il dato in un nodo.<br><strong>Pre-condizione (PRE):</strong> Il nodo <em>n</em> deve esistere in <em>T</em>.<br><strong>Post-condizione (POST):</strong> Produce un nuovo stato dell'albero <em>T'</em>. La struttura (nodi/archi) rimane identica; cambia solo l'associazione del valore al nodo <em>n</em>, che ora contiene il nuovo dato <em>a</em>.
      </td>
    </tr>
  </tbody>
</table>

L'algebra finora studiata non era casuale, ci permetteva di determinare una precisa **scelta progettuale** durante le costruzione degli alberi, ovvero quella di partire dalle foglie per arrivare alla radice collegando tutti i sottoalberi ma non in tutti i casi è la scelta progettuale favorevole. Infatti molto spesso è utile creare gli alberi partendo proprio dalla radice fino alle foglie.
La nuova logica si basa sulla creazione di un **contenitore vuoto** dove verrà posta la radice e poi via via si scenderà verso il basso.

Questo cambio di progettazione non modifica gli operatori esistenti, rimangono validi quelli di **cancellazione, navigazione, creazione, controllo e test**, l'operatore di costruzione `COSTRBINALBERO` sarà l'unico operatore ad essere eliminato e sostituito da **tre nuovi operatori**, progettati per inserire i nodi uno alla volta:
<table>
  <thead>
    <tr>
      <th>Operatore</th>
      <th>Specifica Sintattica<br>(Input &rarr; Output)</th>
      <th>Specifica Semantica e Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>INSBINRADICE</strong></td>
      <td>(NODO, ALBEROBIN) &rarr; ALBEROBIN</td>
      <td><strong>Descrizione:</strong> Inserisce il nodo <em>u</em> come radice.<br><strong>Pre-condizione (PRE):</strong> L'albero <em>T</em> deve essere necessariamente vuoto (<em>T = &Lambda;</em>).<br><br><strong>Post-condizione (POST):</strong> Il nuovo albero <em>T'</em> è composto da un solo nodo (<em>u</em>) che ha livello 0. L'insieme degli archi è ancora vuoto (<em>A = &empty;</em>).
      </td>
    </tr>

    <tr>
      <td><strong>INSFIGLIOSINISTRO</strong></td>
      <td>(NODO, ALBEROBIN) &rarr; ALBEROBIN</td>
      <td><strong>Descrizione:</strong> Aggiunge un nodo come figlio sinistro di <em>u</em>.<br><strong>Pre-condizione (PRE):</strong>L'albero <em>T</em> non è vuoto e il nodo <em>u</em> esiste.<br><strong>Fondamentale:</strong> Il nodo <em>u</em> non deve avere già un figlio sinistro (<em>SINISTROVUOTO(u,T) = TRUE</em>).<br><br><strong>Post-condizione (POST):</strong> L'insieme dei nodi si arricchisce del nuovo nodo (<em>N' = N &cup; {v}</em>), che viene collegato strutturalmente a sinistra di <em>u</em>.
      </td>
    </tr>

    <tr>
      <td><strong>INSFIGLIODESTRO</strong></td>
      <td>(NODO, ALBEROBIN) &rarr; ALBEROBIN</td>
      <td><strong>Descrizione:</strong> Aggiunge un nodo come figlio destro di <em>u</em>.<br><strong>Pre-condizione (PRE):</strong>L'albero <em>T</em> non è vuoto e il nodo <em>u</em> esiste.<br><strong>Fondamentale:</strong> Il nodo <em>u</em> non deve avere già un figlio destro (<em>DESTROVUOTO(u,T) = TRUE</em>).<br><br><strong>Post-condizione (POST):</strong> L'insieme dei nodi si arricchisce del nuovo nodo (<em>N' = N &cup; {v}</em>), che viene collegato strutturalmente a destra di <em>u</em>.
      </td>
    </tr>
  </tbody>
</table>
#### Invisita per alberi binari
Data la struttura sinistra-destra degli alberi binari, la selezione simmetrica risulta più semplice rispetto a quella degli alberi $n$-ari dove bisognava decidere *dove* andare a tagliare.
Anche qui viene posto per convenienza $i=1$ e l'algoritmo alla base esegue rigorosamente in sequenza queste tre operazioni in maniera ricorsiva:
1. Si scende ricorsivamente attuando tutta la parte a **sinistra** del sottoalbero.
2. Si **legge** il nodo corrente.
3. Si scende ricorsivamente completando tutta la parte a **destra** del sottoalbero.
![[Pasted image 20251202113647.png]]
Questa foto spiega il procedimento adottato:
Si parte dalla radice e bisogna completare prima tutti i sottoalberi a sinistra, poi si può leggere la radice ed infine ciò che sta a destra, salendo poi per tutti gli alberi più grandi che li racchiudono.
Infatti partendo da $h$ si va verso sinistra dove c'è $a$ ma da $a$ si può ancora andare a sinistra verso $d$, che essendo foglia verrà letto. Non essendoci altri valori a sinistra di $d$ si può leggere anche $a$ e ora si analizza il sottoalbero alla sua destra. Anche qui $l$ possiede figli a sinistra e sarà letto prima $o$ poi la radice $l$ e infine la sua foglia a destra $q$. Dopo aver completato tutto il grande sottoalbero a sinistra della radice madre si può leggere la radice madre stessa e analizzare la sua parte destra con lo stesso algoritmo ricorsivo eseguito precedentemente, arrivando al valore finale mostrato in figura.

La sua implementazione (**da imparare a memoria**) sul C++ è la seguente:
```c++
visitaSimmetrica(ALBEROBIN T, NODO u){
	if(!SINISTROVUOTO(u,t)){
		visitaSimmetrica(T,FIGLIOSINISTRO(u,T));
	}
	{esamina nodo u}
	if(!DESTROVUOTO(u,T)){
		visitaSimmetrica(T,FIGLIODESTRO(u,T));
	}
}
```
L'algoritmo non procede in modo lineare, ma orchestra il flusso attraverso tre fasi rigorose per ogni nodo visitato. Inizialmente, dà priorità assoluta alla **discesa a sinistra**: se esiste un sottoalbero sinistro, l'algoritmo "*congela*" il nodo corrente e scende ricorsivamente fino a toccare il fondo. Solo quando ritorna da quella profondità si attiva la **fase centrale**, in cui il nodo viene effettivamente esaminato o stampato. Conclusa questa operazione, il flusso si sposta sulla **discesa a destra**: se esiste un sottoalbero destro, l'algoritmo scende nuovamente per esplorarlo, risalendo definitivamente al padre solo dopo aver completato anche questo percorso.
Se il nodo $u$ è una foglia, le due condizioni `if` saranno **false**; l'algoritmo eseguirà solo l'esame del nodo (fase centrale) e terminerà quella specifica istanza ricorsiva, permettendo al processo di risalire al padre.
#### Rappresentazione sequenziale degli alberi binari tramite vettore
Questa tecnica, definita come **implementazione implicita**, non sfrutta più i puntatori tra i nodi ma le **proprietà matematiche** degli array.
L'idea centrale è associare univocamente la posizione di un nodo ad un indice preciso del vettore senza salvare la struttura dell'albero nei dati ma intrinsecamente negli indici.

Per quanto riguarda le **regole di posizionamento** dei dati, la radice sarà sempre posizionata nell'indice $1$, i figli di un determinato nodo in posizione $i$ invece si troveranno obbligatoriamente a:
- **sinistra** quando la loro cella è data da $\text{posizione padre}\times i$
- **destra** quando la loro cella è data da $\text{posizione padre}\times i+1$
![[Pasted image 20251202120206.png]]
Possiamo notare questa regola con l'immagine presente, in cui la radice $8$ è nell'indice del vettore $1$ ed i figli del nodo $3$ sono rispettivamente nella posizione di indice $6$ per il figlio sinistro, che è dato dal livello $2$ (in cui risiede il nodo dal valore $5$) $\times$ $3$ che è l'indice del padre, ed il figlio destro in posizione $7$, dato dal livello $2$ (in cui risiede il nodo dal valore $5$) $\times$ $3$ che è l'indice del padre $+1$, come la formula vuole.

Questa metodologia funziona perfettamente con gli alberi privi di **buchi** ed ha una C.C. pari a $O(1)$, l'accesso è garantito a qualsiasi parente dato un indice.
##### Buchi
 I buchi si presentano quando un albero che non è **completo** utilizza la metodologia precedente per gestire i nodi ed è molto rischioso in alcuni linguaggi gestire delle celle non inizializzate.
 
 Per gestire questo problema, si arricchisce la struttura di ogni elemento del vettore, ogni posizione diventa un record composto dall'**informazione** e un **flag**.
 Il flag **vero** viene settato al momento in cui si riconosce una posizione con un nodo valido dell'albero mentre sarà settato a **falso** se il nodo è mancante e quindi sarà ignorato.
 ![[Pasted image 20251202122507.png]]
 Tuttavia questo tipo di implementazione è raramente usata per gli alberi dinamici a causa di tre gravi inefficienze:
 1. **Spreco di memoria**: le celle marcate a **falso** occupano comunque spazio fisico nella RAM.
 2. **Costo di modifica**: spostare un nodo eseguendo la logica matematica legata all'indice porta al ricalcolo fisico di tutti i suoi discendenti.
 3. **Rigidità**: la dimensione dev'essere definita a priori, essendo la struttura basata su un array, perciò si necessità di ricreare l'albero interamente da zero se si sfora il limite definito inizialmente.
#### Realizzazione con la lista
A differenza della realizzazione con vettori, la realizzazione con la lista si concentra sull'uso totale della memoria, in cui ogni nodo è allocato in una zona qualsiasi della memoria **heap** e i collegamenti sono mantenuti tramite **puntatori**.

In questa implementazione ogni nodo è un record che contiene, dato e puntatore ai parenti. Ogni nodo $u$ è composto da tre campi:
- **KEY/VALUE**: il dato memorizzato
- **LEFT**: un puntatore che contiene l'id di memoria del figlio sinistro
- **RIGHT**: un puntatore che contiene l'id di memoria del figlio destro
In alcune implementazioni è presente il campo **PARENT** che facilità la salita verso l'indietro al padre ,ma non è sempre presente.

La radice è l'unico punto di **accesso** all'albero. Il programma mantiene un puntatore esterno che punta al primo nodo. Se un nodo $u$ ha un figlio sinistro, il campo `u.left` conterrà l'indirizzo di quel figlio. Se non ha un figlio sinistro, il campo `u.left` conterrà un valore speciale NULL. Lo stesso vale per il figlio destro. Le foglie, sono nodi che hanno entrambi i puntatori ( `left` e `right`) impostati a NULL.

Questa implementazione **risolve** tutti i problemi elencati dall'uso del vettore ma presenta dei nuovi problemi, ogni nodo **occupa più spazio** perché oltre al dato contiene gli indirizzi di memoria e non si potrà più **calcolare matematicamente** la posizione di un nodo, bisognerà partire dalla radice e seguire tutta la mappatura sequenziale data dai puntatori.

***Esempio***:
![[Pasted image 20251203140210.png]]
Per comprendere questo esempio ed il risultato finale, bisogna spiegare la mappatura come viene gestita, se sono presenti le **parentesi vuote** $()$ allora il dato è vuoto altrimenti se è presente $(valore\ ()())$ vuol dire che è presente **un valore foglia** e altri figli vuoti, mentre $(...)$ raffigurano una **parentela in corso**.

Partendo dalla radice $8$ si avrà $(8 ...)$ e si aggiungerà una parentesi vuota poiché a sinistra non c'è nessun figlio, aggiungendo quindi $(8 () \dots)$ mentre a destra è presente un figlio che a sua volta è un sotto albero quindi si aprirà una nuova parentesi $(8 ()(5(\dots)))$.
Il $5$ ha sia figlio sinistro che destro che sono entrambi **foglie** quindi si rappresenterà il $27$ tramite $valore\ (\text{sinistro mancante})(\text{destro mancante})$, ovvero $(8()(5(27()())\dots))$, stessa cosa per il $16$, avendo come risultato finale:
$$(8()(5(27()())(16 ()())))$$
In questo modo potrà essere rappresentato **dinamicamente** la lista tramite un vettore e l'uso dei suoi indirizzi puntatori alle celle.
![[Pasted image 20251203145101.png]]
