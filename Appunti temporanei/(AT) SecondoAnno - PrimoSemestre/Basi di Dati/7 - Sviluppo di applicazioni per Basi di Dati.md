Nell'utilizzare una base di dati, il dialogo diretto con l’interprete SQL è riservato a pochi utenti esperti. L'accesso di gran lunga più tipico a una base di dati avviene attraverso applicazioni integrate nel sistema informativo, le quali forniscono agli utenti un’interfaccia semplificata che favorisce l’interazione. 
SQL supporta le applicazioni in due modi:
- Incrementando le funzionalità del DBMS per mezzo della definizione di **procedure** e **trigger** (visti nel capitolo [[4 - SQL]], più precisamente nelle caratteristiche evolute);
- Integrando comandi SQL con istruzioni di un linguaggio di programmazione (procedurale/oggetti) utilizzando SQL Embedded per i linguaggi datati, oppure il Cal Level Interface per linguaggi di programmazione più recenti
## SQL Embedded
SQL Embedded prevede di introdurre direttamente nel programma sorgente scritto nel linguaggio di alto livello le istruzioni SQL, distinguendole dalle normali istruzioni tramite un opportuno separatore. Lo standard SQL prevede che il codice SQL sia preceduto dalla stringa $\text{exec sql}$ e termini con il carattere ‘;’. Dal punto di vista dell’implementazione, è necessario far precedere la compilazione del linguaggio di alto livello all'esecuzione di un preprocessore che riconosca le istruzioni SQL e sostituisce a esse un insieme di chiamate ai servizi del DBMS, tramite una libreria specifica per ogni sistema.

Tutte le variabili usate per scambiare dati fra il programma e il DBMS sono dichiarate in un blocco di istruzioni compreso tra i due comandi:$$\begin{aligned}
&\text{exec sql begin declare section} \\
&\text{exec sql end declare section}
\end{aligned}$$Ogni volta che si dichiara una variabile $(x)$ in questo blocco nel linguaggio ospite, viene creata una variabile copia identica in SQL il cui nome è lo stesso ma preceduto dal carattere $\textquoteleft:\textquoteright (:x)$. Ogni modifica effettuata $x$ viene effettuata anche se $:x$ e viceversa. 

Per lavorare con un database da linguaggio ospite:
1. Si stabilisce una connessione con il sistema specificando il DB su sui lavorare tramite il comando seguente:$$\text{CONNECTED TO} <IdUtente> \text{IDENTIFIED BY} <password> \text{USING} <Database>$$
2. Il preprocessore introduce implicitamente la dichiarazione di una struttura SQLCA (SQL communication area) per gestire la comunicazione tra programma e DBMS
3. Si può accedere al campo SQLCODE della struttura SQLCA, il cui valore è un intero che codifica l’effetto dell’ultima operazione SQL effettuata. Se il valore è uguale a zero indica la corretta esecuzione del comando, se è diverso da zero indica che si è verificata una anomalia ed il comando non è andato a buon fine.
4. In caso di query scalary (unico risultato) il comando $\text{SELECT}$ è esteso con la clausola $$\text{into} <Variabile> \{,<Variabile>\}$$per assegnare a delle variabili del programma il valore degli attributi dell’unica tupla del risultato. Se il comando $\text{SELECT}$ può assegnare ad una variabile il valore nullo (non previsto nel linguaggio ospite) la variabile va dichiarata come: $$:Variabile \ \text{INDICATOR} <\text{IndVariabile}>$$Se dopo l’esecuzione $IndVariabile$ ha valore minore di zero allora $Variabile$ ha valore significativo (quindi non null).

Un esempio di implementazione di SQL Embedded tramite C è questo:
```c
#include <stdlib.h>
#include <stdio.h>

main()
{
    exec sql begin declare section;
        char *NomeDip = "Manutenzione";
        char *CittaDip = "Pisa";
        int NumeroDip = 20;
    exec sql end declare section;

    exec sql connect to utente@librobd;

    if (sqlca.sqlcode != 0) {
        printf("Connessione al DB non riuscita\n"); 
    }
    else {
        exec sql insert into Dipartimento 
            values(:NomeDip, :CittaDip, :NumeroDip);
        
        exec sql disconnect all;
    }
}
```

Un importante problema che caratterizza l’integrazione tra SQL e i normali linguaggi di programmazione è il cosiddetto **conflitto di impedenza**. I linguaggi di programmazione accedono agli elementi di una tabella scandendone le righe una a una (tuple-oriented). Al contrario SQL è un linguaggio di tipo set-oriented, che opera su intere tabelle e restituisce come risultato di un’interrogazione un’intera tabella. Le soluzioni a questo problema si ottengono con l’utilizzo dei **cursori** e l’utilizzo di linguaggi con costruttori di tipo in grado di gestire una struttura del tipo “insieme di righe” (Call Level Interface).
### Cursori
Un cursore è una variabile speciale che permette ad un programma di accedere alle righe di una tabella una alla volta; il cursore viene definito su una generica interrogazione, con la seguente sintassi:$$
\begin{aligned}
&\text{DECLARE}\  NomeCursore \ [ \ \text{SCROLL} \ ] \ \text{CURSOR FOR} \ SelectSQL \\
&\quad [ \ \text{FOR} \langle \text{READ ONLY} \ | \ \text{UPDATE} [ \ \text{OF} Attributo \ \{\ , Attributo \ \} \ ] \ \rangle \ ]
\end{aligned}
$$

Al momento della sua dichiarazione, il cursore si riferisce ad una struttura vuota. L’avvaloramento del cursore avviene mediante il comando $\text{OPEN} \ Nomecursore$ e determina l'esecuzione dell'interrogazione associata al cursore, il suo risultato poi diventa accessibile tramite il comando $\text{FETCH}$:$$
\begin{aligned}
&\text{FETCH } [ \ Posizione \ \text{FROM } ] \ NomeCursore \ \text{INTO } ListaDiFetch
\end{aligned}
$$Il comando copia il contenuto di una riga dal cursore nelle variabili del linguaggio ospite enumerate in $ListaDiFetch$. In particolare, $ListaDiFetch$ contiene una variabile per ogni elemento della target list dell'interrogazione, con una corrispondenza tra colonne della tabelle e variabili del linguaggio ospite dettata dalla posizione della variabile nella lista; ciascuna variabile dalla lista di fetch deve avere un tipo compatibile con i domini degli elementi della target list dell'interrogazione SQL.

Il cursore è una variabile speciale dotata di un proprio stato, infatti il parametro $Posizione$ permette di specificare quale riga deve essere oggetto dell'operazione di fetch; il parametro può assumere i valori:
- $\text{NEXT}$ sposta il cursore alla riga successiva alla corrente
- $\text{PRIOR}$ alla precedente alla corrente
- $\text{FIRST}$ alla prima riga del risultato
- $\text{LAST}$ all'ultima riga del risultato
- $\text{ABSOLUTE} <Espressione Intera>$, ammesso che espressione intera sia uguale a $i$, posiziona il cursore alla $i$-esima posizione a partire dalla prima riga del risultato
- $\text{RELATIVE} <Espressione Intera>$, ammesso che espressione intera sia uguale a $i$, posiziona il cursore alla $i$-esima posizione a partire dalla riga corrente in cui si trova il cursore
Di default il cursore va usa il comando $\text{NEXT}$

I comandi di $\text{UPDATE}$ e $\text{DELETE}$ permettono di apportare modifiche alla base di dati tramite l'uso di cursori tramite seguente sintassi:$$
\begin{aligned}
&\text{UPDATE } NomeTabella \\
&\quad \text{SET } Attributo = \langle \ Espressione \ | \ \text{NULL} \ | \ \text{DEFAULT} \ \rangle \\
&\quad \{ \ , Attributo = \langle \ Espressione \ | \ \text{NULL} \ | \ \text{DEFAULT} \ \rangle \ \} \\
&\quad \text{WHERE CURRENT OF } NomeCursore
\end{aligned}
$$$$
\begin{aligned}
&\text{DELETE FROM } NomeTabella \ \text{WHERE CURRENT OF } NomeCursore
\end{aligned}
$$
Infine, esiste il comando $\text{CLOSE}$ che comunica al sistema che il risultato dell'interrogazione non serve più, chiudendo il cursore e rilasciando l'area di buffer occupata da tale, si definisce come $\text{CLOSE} \ NomeCursore$

Il vantaggio di utilizzare linguaggi che ospitano SQL consiste nella facilità con cui un programmatore può accedere ad un DB utilizzando linguaggi già conosciuti. Lo svantaggio consiste nel curare la conversazione dei dati fra i tipi del linguaggio host e quelli relazionali (conflitto di impedenza).
Quanto visto fin’ora è definibile come SQL statico, perché le interrogazione effettuate hanno una struttura predefinita e ciò che varia è solamente il valore dei parametri usati in ingresso. È però possibile che l’applicazione richieda di effettuare interrogazioni non note a priori ma create a run time, sulla base dell’evoluzione delle informazioni contenute del DB stesso o sulla base dell’interazione dell’utente con il DB.
### SQL dinamico
Nel caso di SQL statico, i comandi SQL sono noti a tempo di compilazione e vengono gestiti dal preprocessore, venendo ottimizzati solo una volta, e non ogni volta che il comando deve essere eseguito. Questo comporta grossi vantaggi in termini di prestazioni. L'SQL dinamico non può avvalersi della fase di preprocessamento, non essendo noti a priori i comandi da ottimizzare. Per questo motivo SQL dinamico mette a disposizione due modalità di interazione:
1. Esecuzione immediata: si esegue immediatamente l’interrogazione/istruzione specificata direttamente o in un parametro di tipo stringa $$\text{execute immediate:}<VariabileConIstruzioneSQL>$$Questo è possibile solo quando si hanno una o più istruzioni conosciute a priori nella sua interezza, ma non si sa quando o quali verranno eseguite (perché magari dipende dalla scelta dell’utente). Come si può vedere il comando non richiede parametri né in ingresso né in uscita (variabile di tipo stringa).
2. La seconda modalità permette di eseguire istruzioni dipendenti non solo dalla dinamicità del sistema, ma prevede anche dei parametri in ingresso o in uscita. Tale modalità si compone di due fasi:
	- **Fase di preparazione**: il comando $\text{prepare}$ analizza l’istruzione SQL e la traduce nel linguaggio interno al DBMS. Inoltre il comando $\text{prepare}$ associa alla traduzione dell’istruzione un nome, che può essere usato dagli altri comandi: $$\text{PREPARE} \  NomeComando \ \text{FROM} \ IstruzioneSQL$$L’istruzione SQL può contenere dei parametri in ingresso, rappresentati dal carattere di punto interrogativo, per esempio:$$
\begin{aligned}
&\text{PREPARE } :comando \\
&\text{FROM } \text{ SELECT } Città \text{ FROM } Dipartimento \text{ WHERE } Nome = \text{\textquoteright ?\textquoteright \textquotedbl}
\end{aligned}
$$