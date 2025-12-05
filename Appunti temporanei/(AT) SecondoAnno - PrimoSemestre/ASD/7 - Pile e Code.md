## Pile

La pila è una sequenza di elementi di uno stesso tipo da cui è possibile estrarre o aggiungere un elemento solo da un estremo, chiamato testa.

Viene definita come una sequenza $P = <a_{1}, a_{2}, \dots, a_{n}>$ di elementi di uno stesso tipo, con accesso LIFO (Last In First Out).

Operatori:
- creapila : () $\to$ pila
	- POST: `p= <>`
- pilavuota : (pila) $\to$ boolean
	- POST: `b = true se p = <> altrimenti b = false`
- leggipila: (pila) $\to$ tipoelem
	- PRE: `p = <a1, a2, ...,an>`
	- POST: `a = a1`
- inpila: (tipoelem, pila) $\to$ pila
	- PRE: `p = <a1, a2, ..., an>`
	- POST: `p' = <a, a1, a2, ..., an>`
- fuoripila: (pila) $\to$ pila
	- PRE: `p = <a1, a2, ..., an>`
	- POST: `p' = <a2, a3, ..., an>`

E' possibile realizzare la pila con due possibili rappresentazioni:
- con vettore
- con puntatori
#### Realizzazione con vettore
Vanno memorizzati gli n elementi della pila, in ordine inverso, nelle prime n posizioni del vettore, mantenendo un cursore alla testa della pila.
![[Pasted image 20251110110903.png]]

Con questa realizzazione, ogni operatore richiede tempo costante per essere eseguito, inoltre gli inserimenti e le cancellazioni non richiedono spostamenti poiché' vengono effettuate ad una estremità dell'array. Tuttavia questa rappresentazione presenta due svantaggi: la presenza di un limite al numero massimo di elementi della pila e **l**o spazio di memoria utilizzato è indipendente dal numero effettivo di elementi.

#### Realizzazione con puntatori
Ci riferiamo alla pila con un puntatore alla cella che si trova in cima, con ogni elemento che punta all'elemento successivo.
![[Pasted image 20251110111252.png]]

Una delle applicazioni più interessanti delle pile riguarda l'esecuzione di programmi ricorsivi, difatti l'esecuzione di una procedura ricorsiva prevede il salvataggio dei dati su cui lavora la procedura al momento della chiamata ricorsiva. Le diverse chiamate attive sono organizzate in una pila in cui la chiamata più recente è quella che si conclude per prima. Nella pila vanno salvati i parametri (e le eventuali variabili locali) e il punto di ritorno, cioè l’etichetta della istruzione da cui ripartire al termine della esecuzione della procedura chiamata.

**ATTENZIONE**: imparare bene i prossimi algoritmi delle valutazioni aritmetiche poiché il professore ha detto che molte volte le chiede all'esame.

La pila è la struttura ideale per valutare delle operazioni aritmetiche. Una espressione aritmetica viene convertita in forma postfissa (in cui l'operatore viene inserito successivamente ai due operandi) e poi risolta. 
#### Conversione di un'espressione infissa in postfissa
Data un'espressione infissa, procediamo da sinistra a destra nell'espressione:
- se incontriamo un numero lo scriviamo in output;
- se incontriamo una parentesi aperta la ignoriamo;
- se incontriamo un operatore lo inseriamo nella pila;
- se incontriamo una parentesi chiusa scriviamo l'operatore che sta in cima alla pila in output.

Consideriamo l'espressione: (5*(((9+8)*(4*6))+7))

Applicando i passaggi scritti sopra avremo:
![[Pasted image 20251110112512.png]]

#### Valutazione di un'espressione postfissa
Con l'uso di una pila possiamo valutare una qualsiasi espressione postfissa. Data un'espressione postfissa, procedendo da sinistra verso destra dovremo:
- inserire nella pila ogni operando che incontriamo;
- se incontriamo un operatore estraiamo i due operandi presenti in cima alla pila, eseguiamo l'operazione e inseriamo il risultato nella pila.

Partendo dall'espressione precedentemente ottenuta: 5 9 8 + 4 6 * * 7 + * avremo:
![[Pasted image 20251110113229.png]]
## Code
La coda è una struttura astratta simile alla pila, dalla quale si differisce per l'accesso. Difatti, a differenza della pila in cui si inserisce e si toglie l'elemento situato "in testa", nella coda gli elementi vengono aggiunti da un estremo "fondo", ed estratti dalla testa, secondo l'algoritmo FIFO.

È particolarmente adatta a rappresentare sequenze nelle quali l'elemento viene elaborato secondo l'ordine di arrivo.
#### Operatori:
- creacoda (): $\to$ coda
	- POST: `q' = <>`
- codavuota (coda): $\to$ boolean
	- POST: `b = vero se q = <>, b = falso altrimenti`
- leggicoda (coda): $\to$ tipoelem
	- PRE: `q = <a1 , a2,...,an > e n ≥ 1`
	- POST: `a = a1`
- fuoricoda (coda): $\to$ coda
	- PRE: `q = <a1 , a2, ..., an> e n ≥ 1`
	- POST: `q’ = <a2, a3, ..., an> se n > 1, q’ = <> se n = 1`
- incoda (tipoelem, coda): $\to$ coda
	-  PRE: `q = <a1, a2, ..., an> e n ≥ 0 `
	- POST: `q’ = <a1, a2, ..., an, a>`

Per rappresentare la coda, le rappresentazioni sono simili a quelle della pila, con la differenza, però, che la rappresentazione sequenziale con il vettore diventa dispendiosa a causa degli shift da effettuare. E' possibile tuttavia usare i vettori (anche se comunque è poco usata) in modo circolare.

Quando si usa il vettore circolare, gli elementi non vengono salvati a partire dalla prima posizione, ma piuttosto la coda viene inserita nel vettore nel mezzo (non per forza nel centro letterale), lasciando dello spazio all'inizio e alla fine, permettendo la scrittura degli elementi da un lato o dall'altro, qualora si sfori.

La realizzazione più usata è comunque la rappresentazione collegata, in cui la sequenza è rappresentate da delle celle, legate da dei puntatori che puntano alla cella successiva, con due puntatori "testa" e "fondo" che puntano ai due estremi.
![[Pasted image 20251103092610.png]]