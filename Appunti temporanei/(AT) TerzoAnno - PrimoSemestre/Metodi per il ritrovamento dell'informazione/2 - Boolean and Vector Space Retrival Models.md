# Retrivial Models
Un **retrivial model** è un modello che definisce come un sistema di Information Retrieval decide quali documenti sono rilevanti per una query.
Ogni modello specifica:
- La rappresentazione del documento, ossia come viene descritto un documento (ad esempio come insieme di parole, oppure come vettore di pesi dei termini
- La rappresentazione della query, ossia come viene espressa la richiesta dell'utente
- Le funzioni di "retrivial"
## Modello per Information Retrival formale
Un modello IR è una quadrupla $[D,Q,F,R(q_{i}, d_{j})]$ composta da:
- Un insieme di documenti, indicato con $D$
- Un insieme di query, espresse con $Q$
- Un framework per modellarli i due precedenti, indicato con $F$
- Una funzione di ritrovamento che restituisce un punteggio di rilevanza, calcolati prendendo in considerazione sia l'insieme dei documenti e query, indicato con $R(q_{i}, d_{j})$

Questo punteggio di rilevanza viene assortito generalmente in maniera decrescente 

I modelli classici di ritrovamento dell’informazione considerano ogni documento
come descritto da un insieme di keyword rappresentative chiamate **index terms**.

Un index term è semplicemente una parola (documento) che aiuta semanticamente a ricordare il tema principale del documento.
### Tassonomia di modelli IR
Accanto ai modelli puramente testuali, esistono modelli che integrano l'analisi della struttura dei collegamenti (**link analysis**), essenziali per il Web Retrieval, in questo scenario le pagine Web formano un grafo connesso da collegamenti ipertestuali e il ranking calcola l'autorevolezza e l'importanza relativa delle pagine connesse.

I modelli di reperimento differiscono per architettura e algoritmi, ma condividono tutti il medesimo ciclo di vita: una fase preliminare di elaborazione e indicizzazione della collezione, seguita dalla fase di ricerca e ranking a fronte della query dell'utente
![[Pasted image 20261005161158.png]]

### Modello booleano
Nel modello booleano, un documento è rappresentato come un **insieme di parole chiave**.
Le sue **query** sono espresse tramite i vari connettori logici che si conoscono della teoria degli insiemi, quindi intersezione (AND), unione (OR), complementare (NOT), incluse le parentesi per indicare l'ambito.
L'**output** che questo modello produce è un peso binario, ossia se il documento è rilevante oppure no, non includendo quindi match parziali o ranking (frequenza di quel termine). Questo output viene indicato con $w_{i,j} \in \{0,1\}$.
#### Matrice di incidenza
> [!example] Esempio di matrice di incidenza
> ![[Pasted image 20261003190722.png]]
> In questo esempio si costruisce una **matrice termine-documento** in cui ogni cella vale 1 se l'opera contiene la parola, altrimenti vale 0 . 
> Ogni termine ha così un **vettore di incidenza** 0/1.
> Con questa rappresentazione si perde comunque il numero di occorrenze all'interno dei documenti e l'ordine delle parole (posizioni)

I principali vantaggi di questo modello sono il formalismo dell’algebra booleana e la sua semplicità.
Nelle collezioni reali di grandi dimensioni la matrice è estremamente **sparsa** (la quasi totalità delle celle ha valore 0, poiché nessun documento contiene più di una minima frazione dell'intero vocabolario). memorizzandoli si ha un enorme spreco di spazio.
