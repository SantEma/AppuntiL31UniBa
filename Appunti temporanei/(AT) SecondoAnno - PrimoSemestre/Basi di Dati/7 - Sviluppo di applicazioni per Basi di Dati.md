Nell'utilizzare una base di dati, il dialogo diretto con l’interprete SQL è riservato a pochi utenti esperti. L'accesso di gran lunga più tipico a una base di dati avviene attraverso applicazioni integrate nel sistema informativo, le quali forniscono agli utenti un’interfaccia semplificata che favorisce l’interazione. 
SQL supporta le applicazioni in due modi:
- Incrementando le funzionalità del DBMS per mezzo della definizione di **procedure** e **trigger** (visti nel capitolo [[4 - SQL]]);
- Integrando comandi SQL con istruzioni di un linguaggio di programmazione (procedurale/oggetti) utilizzando SQL Embedded per i linguaggi datati, oppure il Cal Level Interface per linguaggi di programmazione più recenti
## SQL Embedded
SQL Embedded prevede di introdurre direttamente nel programma sorgente scritto nel linguaggio di alto livello le istruzioni SQL, distinguendole dalle normali istruzioni tramite un opportuno separatore. Lo standard SQL prevede che il codice SQL sia preceduto dalla stringa $\text{exec sql}$ e termini con il carattere ‘;’. Dal punto di vista dell’implementazione, è necessario far precedere la compilazione del linguaggio di alto livello all'esecuzione di un preprocessore che riconosca le istruzioni SQL e sostituisce a esse un insieme di chiamate ai servizi del DBMS, tramite una libreria specifica per ogni sistema.

Tutte le variabili usate per scambiare dati fra il programma e il DBMS sono dichiarate in un blocco di istruzioni compreso tra i due comandi:
$$\begin{aligned}
&\text{exec sql begin declare section} \\
&\text{exec sql end declare section}
\end{aligned}$$Ogni volta che si dichiara una variabile $(x)$ in questo blocco nel linguaggio ospite, viene creata una variabile copia identica in SQL il cui nome è lo stesso ma preceduto dal carattere $\textquoteleft:\textquoteright (:x)$. Ogni modifica effettuata $x$ viene effettuata anche se $:x$ e viceversa. 

Per lavorare con un database da linguaggio ospite:
1. Si stabilisce una connessione con il sistema specificando il DB su sui lavorare tramite il comando seguente:$$\text{CONNECTED TO} <IdUtente> \text{IDENTIFIED BY} <password> \text{USING} <Database>$$
2. Il preprocessore introduce implicitamente la dichiarazione di una struttura SQLCA (SQL communication area) per gestire la comunicazione tra programma e DBMS
3. Si può accedere al campo SQLCODE della struttura SQLCA, il cui valore è un intero che codifica l’effetto dell’ultima operazione SQL effettuata. Se il valore è uguale a zero indica la corretta esecuzione del comando, se è diverso da zero indica che si è verificata una anomalia ed il comando non è andato a buon fine.
4. In caso di query scalary (unico risultato) il comando select è esteso con la clausola $$\text{into} <Variabile> \{,<Variabile>\}$$per assegnare a delle variabili del programma il valore degli attributi dell’unica tupla del risultato. Se il comando select può assegnare ad una variabile il valore nullo (non previsto nel linguaggio ospite) la variabile va dichiarata come: $$:Variabile \ \text{INDICATOR} <\text{IndVariabile}>$$Se dopo l’esecuzione IndVariabile ha valore minore di zero allora Variabile ha valore significativo (quindi non null).
 
### Cursori
[da finire]