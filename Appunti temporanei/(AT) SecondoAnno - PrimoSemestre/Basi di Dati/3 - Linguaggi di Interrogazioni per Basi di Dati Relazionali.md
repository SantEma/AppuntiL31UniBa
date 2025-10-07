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
Consente di cambiare i nomi degli attributi, lasciando  inalterato il contenuto delle relazioni, agendo solo sullo schema.

La possiamo definire con:
[inserire def.]


![[Pasted image 20251007121725.png]]
Nelle liste $A_{1},A_{2} \dots A_{k}$ e $B_{1},B_{2} \dots B_{k}$ si indicheranno solo gli attributi che vengono rinominati, cioè quelli per cui $A_{i} \not=  B_{i}$

#### Selezione
Produce il sottoinsieme delle tuple di una relazione che soddisfano la “condizione di selezione” che possono prevedere: 
-  confronti fra attributi 
-  confronto fra attributi e costanti
- possono essere complesse (ossia ottenute combinando condizioni semplici con i connettivi logici $\land$, $\lor$, e $\neg$)

![[Pasted image 20251007122410.png]]
#### Proiezione


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