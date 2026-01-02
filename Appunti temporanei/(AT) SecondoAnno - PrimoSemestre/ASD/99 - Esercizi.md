Offerti gentilmente da Giuseppe Sifanno
# Estensione di una struttura dati esistente, con nuovi operatori

*Es n.1 primo esonero 12/11/2024*
*Traccia:*
![[Pasted image 20251105090935.png]]
1. **Specifica sintattica**
	* *Operatori*:
		* numero_volte: (lista, stringa) -> intero
		* duplicati: (lista) -> insieme
2. **Specifica semantica**
	* *Operatori*: 
		* numero_volte(L, S) = i
			* pre:  $L  = <a1, a2, ..., an> ,  n >= 0$
			* post: $\sum_j^n \text{uguale}(a_j, s)$  
		* duplicati(L) = i
			* pre:  $L  = <a1, a2, ..., an> ,  n >= 0$
			* post: $i = \{x | \text{numero\_volte(L,x)} \geq 2\}$ 

Definiamo altri operatori per il supporto alla risoluzione:

uguale(stringa, stringa) -> intero
uguale(s1, s2) -> i
PRE: -
POST: i = 1 se s1 = s2, i = 0 altrimenti

*Es n.1, Esonero I 2022/11/16*
*Traccia:*
![[Pasted image 20251108175137.png]]

1. **Specifica sintattica**
	* *Operatori*:
		* insieme_in_lista(insieme, lista) -> booleano
		* trasforma_lista(lista) -> lista
2. **Specifica semantica**:
	* *Operatori*: 
		* insieme_in_lista(I, L) = b
			* pre: 
				  $I = \{a1, a2, ..., an\} \text{ } n>= 1$
				  $L = <b1, b2, ..., bm> m >= 1$	
			* post: b = true se per qualunque elemento $a \in I$ e se scandendo tutta la lista, $an$ appartiene cioè è presente in $L$ ($$a_{i}$$ = bj) altrimenti *false*
		* trasforma_lista(L) = L'
			* pre: $L=<b1,b2,...,bj> j >= 1$
			* post: 
				  invertiamo l'ordine $L' = <bj, b_{j-1}, b_{j-2},...,b1>$
				  rimuoviamo l'elemento centrale se $j$ dispari quindi l'elemento $b_{j/2}$, $L' = L' - \{b_[j/2]\}$

*Es n.1 Esonero I 2023/11/13*
*Traccia:*
![[Pasted image 20251109101051.png]]
1. **Specifica sintattica**
	* *Operatori*:
		* almeno_k_volte(lista, elem, intero) -> boolean
		* multipli(insieme_interi, intero) -> insieme
2. **Specifica semantica**
	* *Operatori*:
		* almeno_k_volte(L, P, K) -> b
			* pre: 
				  L = $<a_{1},a_{2},\dots a_{n}>$ n >= 1
				  K >= 0
			* post:
				  b = true se $\sum_{i}^{n} uguale(a_i, P) >= K$ altrimenti false
		* multipli(I, Q) -> I'
			* pre: $I = \{x1,x2,...,xn | x \in N\}$  
			* post: $I' = \{x1, x2,...,xn | x \in I, x \% Q = 0\}$

*Nuovi operatori*
uguale(elem, elem) -> intero
uguale(P1, P) -> i
pre: -
post: i = 1 se P1 = P altrimenti 0


*Tracce prese da Telegram*
*Date due liste L e Q, restituire una lista contenente gli elementi presenti in entrambe le liste di partenza*

1. **Specifica sintattica**
	* *Operatori*:
		* unisci_liste(lista, lista) -> lista
2. **Specifica semantica**
	* *Operatori*:
		* unisci_lista(L, Q) = L'
			* pre: L = $<a_{1},a_{2},\dots a_{n}>$ n>=0 , Q = <b1,b2,...,bm> m >= 0
			* post: L' = <a1,a2,...an, b1,b2,...bm>

*Data una lista L di interi e un intero N, restituire il numero di volte in cui N compare in L*

1. **Specifica sintattica**
	* *Operatori*:
		* numero_volte(lista, intero) -> intero
2. **Specifica semantica**
	* *Operatori*:
		* numero_volte(L, N) = n
			* pre: L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
			* post: n = $\sum_{i=0}^{n} uguale($$a_{i}$$,n)$

*Altri operatori*
uguale(intero,intero) -> intero
uguale(a,n) -> n'
pre: - 
post: 1 se a = n, 0 altrimenti

*Data una lista L di interi e un intero N, restituire una nuova lista dalla quale sono stati rimossi tutti gli elementi con valore N*

1. **Specifica sintattica**
	* *Operatori*: 
		* rimuovi_numero(lista, intero) -> lista
2. **Specifica semantica**
	* *Operatori*:
		* rimuovi_numero(L, N) -> L'
			* pre: L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
			* post: 
				  L' = $<a_{1},a_{2},\dots a_{n}>$ per qualunque $a_i \in L$ che non rispetta la condizione uguale($a_i$,N)

*Altri operatori*
uguale(intero,intero) -> booleano
uguale(a,n) -> b
pre: - 
post: 1 se a = n, 0 altrimenti

*Data una lista L, rimuovere tutti gli elementi in posizione pari*

1. **Specifica sintattica**
	* *Operatori*:
		* rimuovi_pari(lista) -> lista
2. **Specifica semantica**
	* *Operatori*:
		* rimuovi_pari(L) = L'
			* pre: L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
			* post: L' = $<a_{1},a_{3},\dots a_{n}>$  con *n dispari* 


*Dati due insiemi A e B, restituire gli elementi pari presenti sia in A che in B*
1. **Specifica sintattica**
	* *Operatori*:
		* insiemi_pari(insieme, insieme) -> insieme
2. **Specifica semantica**
	* *Operatori*:
		* insiemi_pari(A, B) = C
			* pre: 
				  A = ${a_{1},a_{2},a \dots a_{n}}$ n>=0
				  B = ${b_{1},b_{2},...b_{m}}$ m>=0
			* post: $C = \{ x \in A \cap B \mid x \text{ è pari} \}$

*Data una lista L, un insieme I e un intero K, restituire vero se tutti gli elementi di I sono presenti in L almeno K volte; false altrimenti.*

1. **Specifica sintattica**
	* *Operatori*:
		* presente_k_volte(lista, insieme, intero) -> boolean
2. **Specifica semantica**
	* *Operatori*:
		* presente_k_volte(L, I, K) = b
			* pre: 
				  L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
				  I = ${b_{1},b_{2},...b_{m}}$  m >=0
			* post: 
			  b = 1 se $\{x \in I | \text{numero\_volte} (x, L) >= K\}$
			  b = 0 altrimenti

*Altri operatorie*
numero_volte(intero, lista) -> intero
numero_volte(x, L) = n
pre: - 
post: $n = \sum_i^n uguale(x,L_i) =$ 

uguale(intero, elem) -> intero
uguale(x, i) -> n
pre: -
post: n = 1 se x = i altrimenti 0


*Data una lista L e un insieme I, restituire vero se hanno lo stesso numero di elementi distinti*
1. **Specifica sintattica**
	* *Operatori*:
		* numeri_distinti(lista, insieme) -> boolean
2. **Specifica semantica**
	* *Operatori*:
		* numeri_distinit(L, I) = b
			* pre: 
				  L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
				  I = ${b_{1},b_{2},...b_{m}}$  m >=0
			* post: 
			  b = 1 se  |\{per qualunque x $\in I$ | 1<=i<=n, 1<=j<=n,  $i \not= j$,  $x_i \not= x_j$\}| = |\{per qualunque x $\in I$ | 1<=i<=n, 1<=j<=n,  $y_i \not= y_j, j\not=i$ \}| 
			  b = 0 altrimenti

*Data una lista L, rimuoverne i duplicati*

1. **Specifica sintattica**
	* *Operatori*:
		* rimuovi_duplicati(lista) -> lista
2. **Specifica semantica**
	* *Operatori*:
		* rimuovi_duplicati(L) = L'
			* pre: L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
			* post: L' = $<a_{1},a_{2},\dots a_{n}>$ se per qualunque $$a_{i}$$ e $a_{j}$ con i != j, $a_{i}$ = $a_{j}$ 

*Data una lista L e un insieme I, restituire vero se tutti gli elementi di L sono multipli di tutti gli elementi dell'insieme I*

1. **Specifica sintattica**
	* *Operatori*:
		* tutti_multipli(lista, insieme) -> booleano
2. **Specifica semantica**
	* *Operatori*:
		* tutti_multipli(L, I) = b
			* pre: 
				  L = $<a_{1},a_{2},\dots a_{n}>$ n>=0
				  I = {b1,b2,...bm} m >= 0
			* post: 
				  b = *true* se per ogni $a_i$ e per ogni $b_j \in I$, $a_i$ multiplo($a_i$, $b_j$) = *true*; 
				  b = *false* altrimenti

multiplo(elem, elem) -> booleano
multiplo(x,y) = b
pre : -
post: b = *true* se x / y da resto 0 altrimenti *false*

---
# Domande di teoria con esercizio

## Definizione di pila

La pila basata su un algoritmo di tipo LIFO è un caso particolare di lista, infatti possiamo definire la corrispondenza tra gli operatori:
* creapila() -> crealista()
* pilavuote(p) -> listavuota(p)
* leggipila(p) -> leggilista(primolista(p), p)
* fuoripila(p) -> canclista(primolista(p), p)
* inpila(a,p) -> inslista(a, primolista(p), p)
Possiamo distinguere due diverse implementazioni tramite la pila:
1. ***Realizzazione con vettore***
	* Vanno memorizzati gli *n* elementi della pila in ordine inverso nelle prime *n* posizioni del vettore mantenendo un cursore alla testa della pila.
		* **Svantaggi**: richiede di determinare a priori un limite di elementi della pila, inoltre lo spezio di memoria utilizzato è indipendente dal numero effettivo di elementi, ciò significa che a determinare la sua dimensione è la sua capienza massima e non il numero di elementi contenuti in un istante. Ogni operatore richiede tempo costante per essere seguito
		* **Vantaggi**: al contrario delle lista, gli inserimenti e le cancellazioni non richiedono spostamenti perché effettuati ad una estremità dell'array
2. ***Realizzazioni con puntatore***
	* Ci si riferisce alla pila con un puntatore alla cella che si trova in cima, una delle applicazioni della pila è l'esecuzione di programmi ricorsivi, l'esecuzione di una procedura ricorsiva prevede il salvataggio dei dati della procedura al momento della chiamata ricorsiva, quest'ultimi verranno poi ripristinati con meccanismo LIFO quando la computazione interna termina. La chiamata più recente è quella che termina per prima. Nella pila vanno salvati i parametri, il punto di ritorno ovvero l'istruzione da cui ripartire non appena termina una computazione interna. Infine è possibile trasformare un programma ricorsivo ad uno iterativo.
		* **Vantaggi**: presenta una dimensione **dinamica** senza nessun limite fisso, non serve riallocare memoria poichè è necessario solo una modifica ai puntatori
		* **Svantaggi**: Maggior uso di memoria per memorizzare i puntatori, accesso agli elementi interni più lento poichè non indicizzato. (Overhead per la gestione dinamica della memoria.)

## Definizione di coda

Le possibile rappresentazioni della coda sono analoghe a quella della pila ma è più conveniente consentire l'accesso sia all'elemento inserito per primo sia all'elemento inserito per ultimo, la rappresentazione sequenziale con vettore non è accettabile per la coda a causa degli shift necessari per le operazioni da effettuare ad uno degli estremi, dunque abbiamo: 
1. Realizzazione con puntatori
	* La coda è realizzata con *n* cella, la prima delle quali è indirizzata da un puntatore "testa" e l'ultima da un puntatore "fondo". La coda vuota è individuata dal valore nullo *null* del puntatore di testa
2. Realizzazione con vettore circolare
	* Per le code la rappresentazione sequenziale è sostituita con una circolare dove consideriamo l'elemento di indice 0 come successore dell'ultima cella con indice *maxlung-1*. Si utilizzano quindi due variabili, *primo* e *ultimo* che indicano la posizione in cui è inserito il primo elemento e l'ultimo elemento (oppure la lunghezza della coda) rispettivamente
### Descrizione di trasformazione da infissa a postfissa

Procedendo da sinistra a destra nell'espressione, se incontriamo un numero lo inseriamo (push) nella pila, mentre se incontriamo un operatore, inseriamo nella pila il risultato dell'applicazione dell'operatore ai due operandi che si trovano sulla cima della pila.

### Descrizione di trasformazione da postfissa a infissa

Procediamo da sinistra a destra nell'espressione:
* se incontriamo un numero lo scriviamo in output
* se incontriamo una parentesi aperta la ignoriamo;
* se incontriamo un operatore lo inseriamo nella pila;
* se incontriamo una parentesi chiusa scriviamo l'operatore che sta in cima alla pila in output.
## Esercizi

*Es n.2 primo esonero 12/11/2024*
*Traccia:*
![[Pasted image 20251105100018.png]]
*Infissa*: $((6/2) + (((4-1)*(4+3))-1))$
*Postfissa*: 62/ 41- 43+ \* 1-+

*Es n.2 Esonero I 2022/11/16*
*Traccia:*
![[Pasted image 20251108183450.png]]
*Postfissa:* $32 + 7515 / +3+-*$
*Infissa:* $((3+2)∗(7−((5+(1/5))+3)))$

*Es n.2 Esonero I 2023/11/13*
*Traccia:*
![[Pasted image 20251109103409.png]]
*Infissa*: $((((5-3) * (10/5)) + 8) / 2)$ 
*Postfissa*: 5 3 - 10 5 / * 8 + 2 /

---
# Complessità algoritmi

*Es n.3 primo esonero 12/11/2024*
*Traccia:*
![[Pasted image 20251105090801.png]]

Consideriamo il **caso migliore** dove *i* nel primo ciclo diventa 0 e fa interrompere il *for*
Istruzioni:
1. test condizione *i > 0* costo=1 + 1 volta alla fine
2. test condizione *if* eseguito 1 volta + accesso a variabile = 1
3. assegnazione eseguita 1 volta
4. test else mai eseguito
5. incremento mai eseguito
6. eseguito 1 volta

Costo di esecuzione: $1 + 1 + 1 + 1 + 1 + 1 = 6$
Complessità data dall'iterazione del ciclo: $O(1)$ poichè nel caso migliore viene eseguito una sola volta il ciclo.

Consideriamo il **caso peggiore** dove la condizione *A\[i\] < A\[0\]* non è mai vera
Istruzioni:
1. test confronto *i > 0* costo 1
2. accesso a A\[i\] costo 1
3. confronto condizione if costo 1
4. assegnazione i = 0 mai eseguita 
5. confronto seconda condizione if costo 1
6. incremento 1
7. aggiornamento i = i / 3 costo 1

Costo di esecuzione = 6 + $\log_3 n$ (poichè *i* viene divisa per 3 ogni volta)
Complessità = $O(\log n)$ 

*Es n.3, Esonero I 2022/11/16*
*Traccia:*
![[Pasted image 20251108185429.png]]

L'algoritmo presenta due cicli, il caso ottimo si verifica quando ogni elemento del vettore *V* non è pari quindi il ciclo più interno verrà eseguito solamente 5 volte.
Il caso pessimo si presenta quando ogni elemento di V è pari quindi

Caso ottimo: $O(n)$
Caso pessimo: $O(n^2)$ 

*Es n.3 Esonero I 2023/11/13*
*Traccia:*
![[Pasted image 20251109111628.png]]

Caso ottimo: si verifica quando al primo passo del ciclo la condizione *if* è verificata quindi *stop = true* che è una condizione per la terminazione del ciclo, complessità: $O(1)$

Caso pessimo: si verifica quando per ogni iterazione del ciclo, non si verifica mai la condizione all'interno dell'*if*, continuando fino a *n-1* ripetizioni. Complessità: $O(n)$

# Progettare una nuova struttura dati
*Es n.4 primo esonero 12/11/2024*
*Traccia:*
![[Pasted image 20251105113232.png]]
*Assomiglia ad una pila*

history = insieme delle sequenze $<a1, a2, ..., an>, n >= 0$ elementi di tipo stringa gestito con politica LIFO

creaHistory(s) -> h
PRE: -
POST: h = \<s\> 

nuovoStato(h,s) -> h'
PRE: h = \<a1, a2,...,an\>, n >= 1
POST: h' = \<s, a1, a2,...,an>

undo(h) -> h'
PRE: h = \<a1, a2,...,an\>, n >= 2
POST: se n = 2 allora h' = \<a2\> altrimenti h' = \<a2,...,an\> (caso estremo di correttezza)
h' = \<a2,a3...,an\> (accettato ma non corretto al 100%)

stati_duplicati(h) -> b
PRE: h = \<a1, a2,...,an\>, n >= 1
POST: b = true se esiste *i*, 1 <= i <= n ed esiste *j* != *i*, 1 <= j <= n t.c. a_{i = $a_{j}$; b = false altrimenti

stato_più_frequente(h) -> s
PRE: h = \<a1, a2,...,an\>, n >= 1
POST: 
* CON ASSENZA DI PARIMERITO -> s = $a_{i}$, 1 <= i <= n, t.c. per ogni j != i, 1<= j <= n, numero_volte(h$$a_{i}$${i}$) > numero_volte(h, $a_{j}$)
* CON PARIMERITO -> s = $$a_{i}$$, 1 <= i <= n, t.c. per ogni j != i, 1<= j <= n, numero_volte(h, $$a_{i}$$) > numero_volte(h, $a_{j}$) OR numero_volte(h, $$a_{i}$$) = numero_volte(h, $a_{j}$) AND i < j  

Possibili soluzioni per trovare il massimo:
* x nell'history h t.c. numero_volte(h,x) è massimo
* x nell'history h t.c. numero_volte(h,x) > numero_volte(h,y) per tutti gli altri y nell'history
* x nell'history h t.c. non esiste alcun altro y nell'history t.c. numero_volte(h,y) > numero_volte(h,x)

*definiamo altri operatori per il supporto*: 
numero_volte: (lista, stringa) -> intero
numero_volte(L, S) = i
pre:  L  = \<a1, a2, ..., an\> ,  n >= 1
post: $\sum_j^n \text{uguale}(a_j, s)$  

uguale(stringa, stringa) -> intero
uguale(s1, s2) -> i
PRE: -
POST: i = 1 se s1 = s2, i = 0 altrimenti


*Es n.4, Esonero I 2022/11/16*
*Traccia*:
![[Pasted image 20251110110442.png]]
*Assomiglia ad una pila*
mensola = insieme delle sequenze M = <a1,a2,...,an> gestita in modo LIFO
libro = struttura dati $L = \{ \text{titolo: stringa, numero\_autori: intero}\}$ 

creaMensola() -> M
pre: -
post: M = <>

aggiungiLibro(M, L) -> M'
pre: M = $<a_{1},a_{2},\dots a_{n}>$ n>= 0
post: M' = <L, a1,a2,...an>

scambiaLibri(M) -> M'
pre: M = $<a_{1},a_{2},\dots a_{n}>$ n>= 2
post M' = <a2,a1,...an>

pulisciMensola(M, I) -> M'
pre: M = $<a_{1},a_{2},\dots a_{n}>$ n>= 0
post: $\sum_{i}^{j = I} \text {cancellaLibro(M)}$

piùAutori(M) -> L
pre: M = $<a_{1},a_{2},\dots a_{n}>$ n>= 1
post: 
	L = $a_i , 1 \leq i \leq n$ t.c. per ogni $i,j \text{con} j \not=i, 1 \leq j \leq n$, (L\[i\].numero_autori > L\[j\].numero_autori) OR (L\[i\].numero_autori = L\[j\].numero_autori AND i < j) 

*Altri operatori a supporto:*
cancellaLibro(M) -> M'
pre: M = $<a_{1},a_{2},\dots a_{n}>$ n>= 1
post: M' = <$\not {a1}$, a2, an>  

*Es n.4 Esonero I 2023/11/13*
*Traccia:*
![[Pasted image 20251109112803.png]]
*Assomiglia ad una coda*

persona = struttura dati 'P' contenente informazioni sulle persone come la loro matricola;
segreteria = insieme delle sequenze S=$<a_{1},a_{2},\dots a_{n}>$ gestita in modo FIFO

creaSegreteria() -> S
pre: -
post: S = <a1,a2,...,an>

aggiungiPersona(S,P) -> S'
pre: S = $<a_{1},a_{2},\dots a_{n}>$ n >= 0
post: S' = $<a_{1},a_{2},\dots a_{n}>, P$ se $a_n$ è studente altrimenti $S' = <a1,a2,...an, a_{n+1}$> se $P = a_{n+1}$ e il suo $a_{n-1}$ è docente

serviPersona(S) -> S'
pre: S = $<a_{1},a_{2},\dots a_{n}>$ n >= 0
post: S' = $<a_{2},a_{3},\dots a_{n}>$

chiusuraSegreteria(S) -> S'
pre: S = $<a_{1},a_{2},\dots a_{n}>$ n >= 0
post: S' = <$a_{1},a_{2},\dots a_{i}$> dove $i <= n$ e tutti gli $a_{i + 1}$ sono studenti e non vengono serviti

piùAnziano(S) -> P
pre: S = $<a_{1},a_{2},\dots a_{n}>$ n >= 0
post: P = per ogni elemento di S applico l'operatore $min(a_i, a_{i+1})$ per trovare la persona con la matricola più piccola ovvero più "vecchia"

*Nuovi operatori*
min(persona, persona) -> persona
min(P, P1) -> P'
pre: -
post: P' = P se P.matricola < P1.matricola altrimenti P' = P1
 

*Es n.3 Appello I 2022-01-17*
*Traccia*:
![[Pasted image 20251112103537.png]]
*Assomiglia ad una lista*

socialnet = insieme delle sequenze $<a_{1},a_{2},\dots a_{n}>$ 
utente = struttura dati contente nome, relazioni di amicizia, data inizio della relazione
lista = insieme delle sequenze di elementi omogenei ordinati tra loro

creaSocial() = S
pre: -
post: S = <>

aggiungiUtente(S, U, s) = S'
pre: S = $<a_{1},a_{2},\dots a_{n}>$ n>=0
post: S' = $<U, a_{1},a_{2},...a_{n}>$ con U.nome = s

amici(S, U, U1 d) = S'
pre:  S = $<a_{1},a_{2},\dots a_{n}>$ n>=2
post: S' = $<a_{1}=U, a_{2}=U1,...a_{n} = U_{n}>$ U.relazioni = U1 e U.data_amicizia = d



