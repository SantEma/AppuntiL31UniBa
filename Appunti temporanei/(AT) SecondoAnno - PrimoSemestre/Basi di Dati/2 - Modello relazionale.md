Il modello relazionale si basa su due concetti:
- **Relazione** (formalmente in matematica)
- **Tabella** (intuitivo per tutti gli utenti)

Ricordiamo in matematica che:
Dati $n$ domini $D_{1}, D_{2} \dots D_{n}$, si chiama prodotto cartesiano $D_{1} \times D_{2}\dots \times D_{n}$ l'insieme delle coppie ordinate ($v_{1},v_{2},\dots v_{n}$) tali che $v_{1}$ è un elemento di $D_{1}$, $v_{2}$ è elemento di $D_{2}$ e così via.
Il numero $n$ rappresenta il **grado** del prodotto cartesiano e quindi della relazione, mentre il **numero di n–uple** (cioè di tuple) che compongono la relazione rappresenta la sua **cardinalità**.

Una **relazione matematica** su due insiemi $D_{1}$ e $D_{2}$ è il sottoinsieme del prodotto cartesiano $D_{1} \times D_{2}$.
Una relazione può essere **rappresentata in forma tabellare**, dove ogni riga corrisponde a una tupla e ogni colonna a un attributo.  

Poiché una relazione è un **insieme**, essa possiede due proprietà fondamentali:  
1. **Assenza di ordinamento**: non è definito alcun ordinamento tra le n–uple di una relazione. L’ordine con cui le tuple appaiono nelle tabelle è puramente casuale o derivato da esigenze di rappresentazione, ma non ha valore semantico.  
2. **Assenza di duplicati**: poiché gli elementi di un insieme sono tutti distinti, anche le tuple di una relazione devono esserlo. Pertanto, una tabella rappresenta correttamente una relazione solo se le sue righe sono tutte diverse tra loro.

Ogni tupla di una relazione è **ordinata**, e il suo $i$–esimo valore proviene dal dominio $D_i$.  
Le informazioni che vogliamo organizzare all'interno delle relazioni di un database hanno una struttura analoga a quella dei **record**: di conseguenza, una relazione può essere vista come un insieme di record omogenei.  
Per rendere irrilevante l’ordinamento dei domini, si assegna un **nome**, detto **attributo** ,a ciascun dominio. 
In questo modo, le informazioni sono descritte in base ai loro attributi e non alla posizione che occupano nella tupla.

La corrispondenza tra l’insieme degli attributi $X$ e quello dei domini $D$ è stabilita da una funzione:
$$DOM: X \to D$$
che associa a ogni attributo $A \in X$ un dominio $DOM(A) \in D$.  
Una **tupla** su un insieme di attributi $X$ è definita come una **funzione** $t$ che associa a ciascun attributo $A \in X$ un valore appartenente al dominio $DOM(A)$.  
Il valore di una tupla $t$ sull'attributo $A$ si indica come $t[A]$ oppure $t.A$.  
Analogamente, $t[AB]$ indica il valore di $t$ sull'unione degli attributi $A$ e $B$.  

Una **relazione su X** è un insieme di tuple su $X$ ed è rappresentata mediante una **tabella dotata di intestazione**, che riporta i nomi degli attributi.  
Una **base di dati relazionale** è infine costituita da **più relazioni** tra loro coerenti.