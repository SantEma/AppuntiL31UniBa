I linguaggi per interrogare un DB relazionale permettono di definire la relazione risultato a partire dalle relazioni che compongono la base di dati. 

Il modello relazionale è stato il primo modello ad introdurre linguaggi **set oriented**, in grado di operare su insiemi di dati con **operatori insiemistici**, nei modelli precedenti invece questo era dato tramite operatori simili a quelli dei linguaggi imperativi per la manipolazione dei file

I linguaggi si dividono in tre famiglie:
- **Linguaggi algebrici**: un interrogazione (query) è definita da un'espressione con operatori su relazioni che producono come risultato altre relazioni
- **Linguaggi basati sul calcolo dei predicati**: un'interrogazione è definita da una formula del calcolo dei predicati
- **Linguaggi logici**: sono linguaggi ispirati dal linguaggio logico Prolog in cui un’interrogazione è definita da un insieme di clausole di Horn.

I linguaggi algebrici sono **procedurali**, ossia le loro espressioni descrivono passo passo la computazione del risultato a partire da un istanza di base di dati.
I linguaggi basati sul calcolo dei predicati ed i linguaggi logici sono dichiarativi, ossia le loro espressioni specificano le proprietà del risultato, piuttosto della procedura per ottenerlo

I linguaggi relazionali esistenti offrono anche altri  operatori non previsti nell'algebra relazionale come le operazioni aritmetiche, la media, la somma, minimo o massimo, che consentono di agire su insiemi di valori per ottenere una singola quantità.
Un esempio di questo linguaggio logico è il **Datalog**, più espressivo del calcolo relazionale, riesce quindi ad esprimere interrogazioni ricorsive come la relazione transitiva

## Algebra relazionale
L'algebra è un linguaggio costituito da un insieme di operatori, definiti su relazioni e che producono ancora relazioni come risultati. 
Prima si definisce un insieme minimo e completo di operatori (**operatori primitivi**) per poi definire altri operatori derivati dai precedenti, utili per esprimere operazioni complesse in modo sintetico, molto frequente nell'uso dei DB relazionali

Gli operatori sono distinti in 3 gruppi:
- **Operatori insiemistici tradizionali**: unione, intersezione, differenza
- **Operatori specifici**: ridenominazione, selezione, proiezione
- **Operatori di join**: prodotto cartesiano, join naturale, theta-join, equi-join, join-esterno

Gli operatori **primitivi** (da cui si possono derivare tutti gli altri) per categoria sono:
- Unione e differenza
- Ridenominazione, selezione, proiezione
- Prodotto Cartesiano

### Operatori insiemistici
Gli operatori insiemistici sono possibili solo con **relazioni binarie**
#### Unione
L’unione di due relazioni $r_{1}(X)$ ed $r_{2}(X)$ definite sullo stesso insieme di attributi $X$, indicata con $r_{1} \bigcup r_{2}$, è una relazione ancora su $X$ contenente le tuple che appartengono ad $r_{1}$ oppure ad $r_{2}$, oppure ad entrambe.
$$r_{1} \bigcup r_{2}=\{t|t \in r_{1} \lor t \in r_{2}\}$$

![[Pasted image 20251007115556.png]]
#### Intersezione
L’intersezione di due relazioni e definite sullo stesso insieme di attributi , indicata con $r_{1} \bigcap r_{2}$, è una relazione su $X$ contenente le tuple che appartengono ad $r_{1}$ e $r_{2}$
$$r_{1} - r_{2}=\{t|t \in r_{1} \land t \in r_{2}\}$$


![[Pasted image 20251007120412.png]]
#### Differenza
La differenza di due relazioni $r_{1}(X)$ ed $r_{2}(X)$ definite sullo stesso insieme di attributi $X$, indicata con $r_{1} - r_{2}$, è una relazione su $X$ contenente le tuple che appartengono ad $r_{1}$ e non appartengono ad $r_{2}$.
$$r_{1} - r_{2}=\{t|t \in r_{1} \land t \notin r_{2}\}$$

![[Pasted image 20251007120004.png]]
### Operatori specifici
#### Ridenominazione
Consente di cambiare i nomi degli attributi, lasciando inalterato il contenuto delle relazioni e agendo soltanto sullo schema.

La possiamo definire con:
Sia $R(X)$ una relazione definita sull'insieme di attributi $X$ e sia $Y$ un altro insieme di attributi di stessa cardinalità. 
Siano $A_{1},A_{2} \dots A_{k}$ e $B_{1},B_{2} \dots B_{k}$ rispettivamente un ordinamento per gli attributi in $X$ e un ordinamento per quelli in $Y$, allora la ridenominazione:$$\rho B_{1},B_{2} \dots B_{k} \leftarrow A_{1},A_{2} \dots A_{k}(r)$$
contiene una tupla $t'$ su $Y$ per ciascuna tupla $t$ in $r$ (su $X$), definita come:
$$\rho B_{1},B_{2} \dots B_{k} \leftarrow A_{1},A_{2} \dots A_{k}(r)=\{t'|\exists t \in r \ \text{t.c} \ \forall i=1,\dots,k \ t'[B_{i}]=t[A_{i}]   \}$$

Nelle liste $A_{1},A_{2} \dots A_{k}$ e $B_{1},B_{2} \dots B_{k}$ si indicheranno solo gli attributi che vengono rinominati, cioè quelli per cui $A_{i} \not= B_{i}$
![[Pasted image 20251007121725.png]]
#### Selezione
Produce il sottoinsieme delle tuple di una relazione che soddisfano la “condizione di selezione” che possono prevedere: 
-  confronti fra attributi 
-  confronto fra attributi e costanti
- possono essere complesse (ossia ottenute combinando condizioni semplici con i connettivi logici $\land$, $\lor$, e $\neg$)
e viene indicato con $\sigma$ a pedice

Formalmente viene definito: 
Data una relazione $r(X)$, una formula proposizionale $F$ su $X$ è una formula ottenuta combinando, con i connettivi $\land$, $\lor$ e $\neg$, condizioni atomiche del tipo $A \theta B$ o $A \theta c$, dove:
- $\theta$ è un operatore di confronto ($=,\not=,>,<,\underline{>},\underline{<}$)
- $A \ \text{e} \ B$ sono attributi in $X$ dove il confronto $\theta$ abbia senso
- $c$ è una costante compatibile con il dominio di A

Date una formula $F$ e una tupla $t$, è definito un valore di verità (cioè vero o falso) per $F$ su $t$:
- $A \theta B$ è vera su $t$ se $t[A]$ è in relazione $\theta$ con $t[B]$, altrimenti è falsa; 
- $Aθc$ è vera su $t$ se $t[A]$ è in relazione $\theta$ con $c$, altrimenti è falsa; 
- $F_{1} \lor F_{2}, F_{1} \land F_{2} \ e \ \neg F_{1}$ hanno l’usuale significato

Date una relazione $r(X)$ ed una formula proposizionale $F$ su $X$, la selezione $\sigma_{F}(r)$  produce una relazione su $X$ che contiene le tuple $t$ di $r$ su cui $F$ è vera:$$\sigma_{F}(r)=\{t|t\in r \land F(t)\}$$![[Pasted image 20251007122410.png]]
#### Proiezione
Date una relazione $r(X)$ e un sottoinsieme $Y$ di $X$, la proiezione di $r$ su $Y$, $\pi_{Y}(r)$, è l’insieme di tuple su $Y$ ottenute dalle tuple di $r$ considerando solo i valori su $Y$:
![[Pasted image 20251007124323.png]]
### Operatori di Join
#### Prodotto cartesiano
Date due relazioni $r_{1}(X)$ ed $r_{2}(Y)$, con $X$ e $Y$ insiemi di attributi distinti, $X \bigcap Y = \varnothing$, il prodotto cartesiano è una relazione $r_{1} \times r_{2}(XY)$ così definita:$$r_{1} \times r_{2}=\{tt'|t \in r_{1} \land t' \in r_{2}\}$$
dove $tt'$ è una $k$ + $m$ $n$-upla ottenuta dalla concatenazione di due tuple $t$ e $t'$ dove $k$ è la cardinalità di $X$ ed $m$ è la cardinalità di $Y$. 
La relazione risultante ha:
- **grado** uguale alla somma dei gradi dei due operandi
- **cardinalità** uguale al prodotto delle cardinalità degli operandi

![[Pasted image 20251008115151.png]]
#### Join naturale
Il join naturale, denotato con , è un operatore che correla dati in relazioni diverse, sulla base di valori uguali in attributi con lo stesso nome.
Viene formalmente definito con:
Date due relazioni $r_{1}(X_{1})$ ed $r_{2}(X_{2})$, con attributi comuni a $r_{1}$ ed $r_{2}$ definiti sugli stessi domini, il join naturale è una relazione definita sull'unione degli insiemi degli attributi degli operandi ($X_{1}X_{2}$) e le cui tuple sono ottenute combinando le tuple degli operandi con valori uguali sugli attributi comuni:$$r_{1}\rhd\lhd r_{2}=\{t \ \text{su}\ X_{1} X_{2}| \exists \ t_{1} \in r_{1} \ \text{e} \ t_{2} \in r_{2} \ \text{con} \ t[X_{1}]=t_{1} \ \text{e} \ t[X_{2}] = t_{2} \} $$

![[Pasted image 20251008115203.png]]
Si parla di **join completo** se ogni tupla di ciascun operando contribuisce ad almeno una tupla del risultato. 
Se ciascuna tupla di un operando è compatibile con tutte le tuple dell’altro operando, il risultato conterrà un numero di tuple pari al prodotto delle cardinalità degli operandi. 
![[Pasted image 20251008124221.png]]
Può accadere che alcune tuple degli operandi non contribuiscano al risultato, perché l’altra relazione non contiene tuple con gli stessi valori sull'attributo comune, tali tuple si definiscono dangling (dondolanti). 
Come caso limite è possibile che nessuna delle tuple degli operandi sia combinabile e allora il risultato del join naturale è la relazione vuota.
![[Pasted image 20251008124234.png]]

Il join naturale presenta alcune proprietà:
- Il grado della relazione risultato di un join naturale è minore o uguale alla somma dei gradi degli operandi, poiché gli attributi omonimi degli operandi compaiono una sola volta nel risultato.
- **Commutatività**: $r_{1}\rhd\lhd r_{2} = r_{2}\rhd\lhd r_{1}$
- **Associatività**: $r_{1}\rhd\lhd (r_{2}\rhd\lhd r_{3}) = (r_{1}\rhd\lhd r_{2})\rhd\lhd r_{3}$ (quindi è possibile scrivere sequenze di join senza parentesi)
- Se $r_{1} \ \text{e} \ r_{2}$ non hanno attributi comuni allora $r_{1}\rhd\lhd r_{2} = r_{1} \times r_{2}$
- Se $r_{1} \ \text{e} \ r_{2}$ hanno lo stesso schema ($X_{1}=X_{2}$) allora $r_{1}\rhd\lhd r_{2} = r_{1} \bigcap r_{2}$ (il risultato sarà composto dagli elementi delle due istanze, quindi l'intersezione)
#### Join esterno
Il join esterno è una variante del join naturale, il quale restituisce il join naturale di $r_{1}$ ed $r_{2}$ esteso con le tuple di $r_{1}$ ed $r_{2}$ che non appartengono al join naturale, completate con valori nulli per gli attributi mancanti.
![[Pasted image 20251008115444.png]]
Esistono altri due tipi di join esterni:
- Join esterno **sinistro**
- Join esterno **destro**
Nel primo caso, solo le tuple dell’argomento sinistro $r_{1}$ che non appartengono al join naturale appaiono nel risultato, mentre nell'altro caso appaiono solo quelle dell’argomento destro $r_{2}$.
![[Pasted image 20251008115503.png]]
#### Theta-Join
Il prodotto cartesiano ha poca utilità nella pratica, poiché concatena tuple non necessariamente correlate dal punto di vista semantico, infatti viene spesso seguito da una selezione, che centra l’attenzione sulle tuple correlate secondo le esigenze. 
Per questo motivo si definisce l’operatore derivato theta-join come prodotto cartesiano seguito da una selezione.
Viene definito formalmente in:
Date due relazioni $R_{1}(X)$ e $R_{2}(Y)$, con $X \bigcup Y = \varnothing$, siano $A_{i} \in X \ \text{e} \ B_{j} \in Y$ e $\theta$ un operatore di confronto ($=,\not=,>,<,\underline{>},\underline{<}$) il theta-join è definito come:$$r_{1}\rhd\lhd_{A_{i} \ \theta\ B_{j}} r_{2} = \sigma_{A_{i} \ \theta\ B_{j}} (r_{1} \times r_{2})  $$
![[Pasted image 20251008115529.png]]
#### Equi-join
Un theta-join in cui la condizione di selezione sia una congiunzione di atomi di uguaglianza, con un attributo della prima relazione e uno della seconda, viene detto **equi-join**.
![[Pasted image 20251008115545.png]]Da un punto di vista pratico il theta-join e l’equi-join hanno una grande importanza, in quanto la maggior parte dei BDMS relazionali esistenti non utilizzano i nomi di attributo per correlare relazioni e quindi non ha senso per essi il join naturale. 
Peraltro il join naturale può essere simulato per mezzo della ridenominazione, dell’equi-join e della proiezione.
### Interrogazioni in algebra relazionale
Un interrogazione può essere definita come una funzione che, applicata a istanze di basi di dati, produce relazioni.

Formalmente:
Dato uno schema $R$ di basi di dati, un'interrogazione è una funzione che, per ogni istanza di $r$ di $R$, produce una reazione su un dato insieme di attributi $X$ 
In algebra relazionale, le interrogazioni su uno schema di base di dati $R$ vengono formulati con espressioni i cui atomi (nomi di) sono relazioni in $R$ (le variabili)
#### Efficienza del join
Il join è l’operazione più dispendiosa dell’algebra relazionale, il metodo più semplice per calcolare un join consiste nel confrontare tutte le coppie di tuple (La complessità è $O(n^{2})$ per relazioni di cardinalità $n$.)

Un alternativa per calcolare l'equi-join o il join naturale è l'ordinare entrambe le relazioni rispetto agli attributi coinvolti per poi fondere le liste, producendo le tuple del join.
Questo tipo di realizzazione ha una complessità $O(m+nlogn)$ per le relazioni di cardinalità $n$ ed $m$ numero di tuple risultanti dalla join.
**In generale si deve evitare join di grandi relazioni.**
#### Proprietà algebriche
L'algebra relazionale permette di formulare espressioni fra loro equivalenti sfruttando alcune proprietà degli operatori;
Queste trasformazioni sono utili perché possono ridurre di ordini di grandezza il costo di esecuzione delle espressioni (ottimizzazione algebrica). 
Una buona tecnica per ottimizzare un’espressione algebrica consiste nell'anticipare l’applicazione degli operatori di proiezione e di selezione rispetto al prodotto, in modo da ridurre la dimensione dei risultati intermedi, infatti l’operatore di selezione produce una relazione con un numero inferiore di tuple rispetto alla relazione a cui viene applicato e la proiezione riduce la dimensione delle tuple dell’operando ed elimina eventuali tuple dal risultato.

Definiamo le formule:
Siano $E$ un'espressione dell’algebra relazionale e $C_x$ una condizione sull'insieme di attributi $X$
![[Pasted image 20251009111902.png]]![[Pasted image 20251009111939.png]]
### Viste
Abbiamo visto che può risultare utile mettere a disposizione degli utenti rappresentazioni diverse per gli stessi dati, in una base di dati relazionale si ottiene distinguendo relazioni di base il cui contenuto è autonomo e relazioni derivate il cui contenuto e funzione del contenuto di altre relazioni, ed inoltre è possibile che una relazione derivata sia funzione di un’altra relazione derivata a patto di stabilire un ordinamento fra le relazioni derivate stesse

In linea di principio possono esistere due tipi di relazioni derivate:
- **Viste materializzate**: relazioni derivate effettivamente memorizzate nella base di dati
- **Relazioni virtuali (o viste):** relazioni definite per mezzo di espressioni del linguaggio di interrogazione non memorizzate nella base di dati, ma utilizzate nelle interrogazioni come se lo fossero.

Le viste materializzate hanno il vantaggio di essere immediatamente disponibili per le interrogazioni, ma è spesso oneroso mantenere il loro contenuto allineato con quello delle relazioni da cui derivano mentre al contrario le relazioni virtuali devono essere ricalcolate per ogni interrogazione ma non presentano problemi di allineamento. 
Per inciso, per mantenere costantemente allineate le viste materializzate occorre disporre di meccanismi di trigger per l’aggiornamento automatico (basi di dati attive), i DBMS attuali forniscono meccanismi per la loro gestione (poiché il loro allineamento è difficile manualmente).

Un’interrogazione su una relazione virtuale viene trasformata sostituendo ad ogni occorrenza della relazione virtuale l’espressione che la definisce.
Esempio:
[inserire esempio]

Mentre per quanto riguarda le interrogazioni, le viste possono essere trattate come relazioni di base, lo stesso non si può dire per le operazioni di aggiornamento, in molti casi non è possibile stabilire facilmente una semantica degli aggiornamenti sulle viste:
Ad esempio l’inserimento di una tupla nella vista non corrisponde univocamente ad un insieme di aggiornamenti sulle relazioni di base, per questo motivo i DBMS limitano aggiornamenti sulle viste.
#### Calcolo relazionale
Con il termine **calcolo relazionale** si fa riferimento ad una famiglia di linguaggi di interrogazione, basati sul calcolo dei predicati del primo ordine, che hanno la caratteristica di essere dichiarativi, cioè di specificare le proprietà del risultato delle interrogazioni, anziché la procedura seguita per generarlo. 
Nel calcolo relazionale, l’istanza di un DB relazionale è vista come una interpretazione di una logica del primo ordine, mentre un’interrogazione è espressa come una formula ben formata. Il risultato si ottiene, dunque, interpretando l’espressione rispetto all'istanza del DB disponibile.

Esistono diverse versioni del calcolo relazionale: 
- **il calcolo relazionale su domini** 
- **il calcolo relazionale su tuple**
##### Calcolo relazionale su domini
Le espressioni del calcolo relazionale su domini hanno la forma:
$$\{A_{1}:x_{1}\dots,A_{k}:x_{k}|f\}$$
dove:
- $A_{1}:x_{1}\dots,A_{k}$ sono attributi distinti (che possono anche non comparire nello schema del DB rispetto a cui viene formulata l'interrogazione)
- $x_{1},\dots,x_{k}$ sono variabili distinte quantificate universalmente
- $f$ è una formula logica dove le variabili locali sono da intendersi quantificate esistenzialmente, a meno di quantificazioni universali esplicite

I simboli che compaiono in una formula $f$ sono:
- **Costanti**: elementi di un dominio di interesse (Per semplicità si assume che tutti gli attributi siano definiti sullo stesso dominio $D$)
- **Variabili**: elementi di un insieme numerabile $V$ disgiunto dal dominio $D$
- **Nomi di relazione e attributi**: presi dallo schema di DB di interesse
- **Operatori di confronto**: $=,\not=,>,<,\underline{>},\underline{<}$
- **Connettori logici:** $\land$, $\lor$, e $\neg$
- **Quantificatori**: $\forall, \exists$
- **Simboli di punteggiatura e parentesi**

Questi simboli sono composti secondo le regole:
- **Formule atomiche**, di due tipi:
	- $R(A_{1}:x_{1}\dots,A_{p}:x_{p})$ dove $R(A_{1}\dots,A_{p})$ è uno schema di relazione e $x_{1}\dots x_{p}$ sono variabili distinte
	- $x \theta y \ \text{o} \ x\theta c$, con $x$ e $y$ variabili di $V$, $c$ costante e $\theta$ operatore di confronto
- Se $f_{1} \ \text{e} \ f_{2}$ sono formule allora $f_{1} \lor f_{2}, f_{1} \land f_{2},\neg f_{1}$ sono formule
- Se $f$ è una formula e $x$ una variabile allora $\exists x(f)$ e $\forall x(f)$ sono formule

La lista di coppie $A_{1}:x_{1}\dots,A_{p}:x_{p}$ nelle espressioni del calcolo relazionale su domini è detta **target list** in quanto definisce la struttura del risultato, ovvero una relazione con schema $\{A_{1}, \dots , A_{k}\}$ e tuple i cui valori sostituiti a $x_{1}, \dots , x_{k}$ rendono vera la formula $f$ rispetto ad una istanza di DB cui $f$ è applicata

Per definire la semantica di un'espressione bisogna definire la nozione di valore di verità di una formula rispetto ad una sostituzione, si deve inoltre fare riferimento ad uno schema $R = \{ R_1(X_1), \dots, R_m(X_m) \}$ ed una sua istanza $r = \{ r_1, \dots, r_m \}$

Interpretazione delle formule atomiche:
- $R_j(A_1 : x_1, \dots, A_p : x_p)$ è vera sui valori sui valori $a_{1}$ per $x_{1},\dots,a_{p}$ per $x_{p}$ se la relazione $r_{j}$ in $R$ contiene una tupla con valore $a_{1}$ per $A_{1}, \dots , a_{p}$ per $A_{p}$.
  In altre parole la formula è vera se nella base di dati esiste almeno una tupla tale per cui è possibile sostituire ad ogni variabile della formula un valore di attributo della tupla


##### Calcolo relazionale su tuple con dichiarazione di range
[da completare]
#### Algebra e calcolo con valori nulli
[da completare]