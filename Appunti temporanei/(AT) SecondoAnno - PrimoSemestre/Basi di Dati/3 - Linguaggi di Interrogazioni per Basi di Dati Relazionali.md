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
[da completare]
![[Pasted image 20251007124323.png]]
### Operatori di Join
#### Prodotto cartesiano
[da completare]
#### Join neutrale
[da completare]
#### Theta-Join
[da completare]
#### Equi-join
[da completare]
#### Join esterno
[da completare]