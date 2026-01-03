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
1. Si stabilisce una connessione con il sistema specificando il database su sui lavorare tramite il comando seguente:$$\text{CONNECTED TO} <IdUtente> \text{IDENTIFIED BY} <password> \text{USING} <Database>$$
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

Il vantaggio di utilizzare linguaggi che ospitano SQL consiste nella facilità con cui un programmatore può accedere ad un database utilizzando linguaggi già conosciuti. Lo svantaggio consiste nel curare la conversazione dei dati fra i tipi del linguaggio host e quelli relazionali (conflitto di impedenza).
Quanto visto fin’ora è definibile come SQL statico, perché le interrogazione effettuate hanno una struttura predefinita e ciò che varia è solamente il valore dei parametri usati in ingresso. È però possibile che l’applicazione richieda di effettuare interrogazioni non note a priori ma create a run time, sulla base dell’evoluzione delle informazioni contenute del database stesso o sulla base dell’interazione dell’utente con il database.
### SQL dinamico
Nel caso di SQL statico, i comandi SQL sono noti a tempo di compilazione e vengono gestiti dal preprocessore, venendo ottimizzati solo una volta, e non ogni volta che il comando deve essere eseguito. Questo comporta grossi vantaggi in termini di prestazioni. L'SQL dinamico non può avvalersi della fase di preprocessamento, non essendo noti a priori i comandi da ottimizzare. Per questo motivo SQL dinamico mette a disposizione due modalità di interazione:
1. Esecuzione immediata: si esegue immediatamente l’interrogazione/istruzione specificata direttamente o in un parametro di tipo stringa $$\text{execute immediate:}<VariabileConIstruzioneSQL>$$Questo è possibile solo quando si hanno una o più istruzioni conosciute a priori nella sua interezza, ma non si sa quando o quali verranno eseguite (perché magari dipende dalla scelta dell’utente). Come si può vedere il comando non richiede parametri né in ingresso né in uscita (variabile di tipo stringa).
2. La seconda modalità permette di eseguire istruzioni dipendenti non solo dalla dinamicità del sistema, ma prevede anche dei parametri in ingresso o in uscita. Tale modalità si compone di due fasi:
	- **Fase di preparazione**: il comando $\text{prepare}$ analizza l’istruzione SQL e la traduce nel linguaggio interno al DBMS. Inoltre il comando $\text{prepare}$ associa alla traduzione dell’istruzione un nome, che può essere usato dagli altri comandi: $$\text{PREPARE} \  NomeComando \ \text{FROM} \ IstruzioneSQL$$L’istruzione SQL può contenere dei parametri in ingresso, rappresentati dal carattere di punto interrogativo, per esempio:$$ \begin{aligned} &\text{PREPARE } :comando \\ &\text{FROM } \text{ SELECT } Città \text{ FROM } Dipartimento \text{ WHERE } Nome = \text{ ? }\end{aligned} $$In questo modo alla variabile comando del programma corrisponde la traduzione dell’istruzione, con un parametro di ingresso che rappresenta il nome del dipartimento che deve essere selezionato dall'interrogazione.
	- **Fase di esecuzione**: per eseguire un comando che è stato preelaborato da una istruzione prepare si una l’istruzione execute, che ha la seguente sintassi: $$\begin{aligned} &\text{EXECUTE } NomeComando \ [ \ \text{INTO } ListaTarget \ ] \ [ \ \text{USING } ListaParametri \ ] \end{aligned} $$La lista dei target contiene l’elenco dei parametri in cui deve essere scritto il risultato dell’esecuzione del comando, la lista dei parametri specifica i valori che devono essere assunti dai parametri variabili. Un esempio vi è:

Se l’istruzione SQL restituisce più di un risultato (un insieme di tuple) è necessario usare i cursori. L’uso dei cursori con SQL dinamico è molto simile all’uso dei cursori in SQL statico. Le uniche due differenze consistono nel fatto che si associa al cursore l’identificativo dell’interrogazione invece che l’interrogazione stessa, e che i comandi d’uso del cursore ammettono le clausole into e using che permettono la specifica degli eventuali parametri di ingresso e uscita.
$$
\begin{aligned}
&\text{PREPARE } :comando \ \text{FROM } :istruzioneSQL \\
&\text{DECLARE } Cursore \ \text{CURSOR FOR } :comando \\
&\text{OPEN } Cursore \ \text{USING } :nome1
\end{aligned}
$$
Quando un’istruzione SQL che era stata preparata non serve più, è possibile rilasciare la memoria occupata dalla traduzione dell’istruzione utilizzando il comando:$$\text{DEALLOCATE PREPARE} \ NomeComando$$
### Call Level Interface (CLI)
I meccanismi descritti finora ricadono nella famiglia delle soluzioni SQL Embedded, in cui si prevede di disporre di un preprocessore, utilizzato a tempo di compilazione o di esecuzione.
È possibile anche interfacciarsi direttamente con un DBMS, da un programma usando apposite primitiva (librerie per realizzare il dialogo con il database). Quello offerto dal CLI è uno strumento meglio integrato con il linguaggio di programmazione. Lo svantaggio consiste nel dover gestire esplicitamente aspetti che in SQL Embedded vengono risolti automaticamente dal preprocessore.
Diversi sistemi offrono proprie CLI sulla base del sistema operativo e del linguaggio di programmazione. Il modo generale d’uso di questi strumenti è il seguente:
- Si utilizza un servizio CLI per creare una connessione con il DBMS
- Si invia sulla connessione un comando SQL che rappresenta la richiesta
- Si riceve come risposta del comando una struttura relazionale in un opportuno formato
- Al termine della sessione di lavoro:
	- Si chiude la connessione
	- Si rilasciano le strutture dati usate per la gestione del dialogo
### Object Relational Mapping (ORM)
Le soluzioni viste fino a questo momento sono basate sul trasferimento di dati dagli oggetti/variabili temporanei dell’applicazione alla fonte persistente e viceversa. I sistemi di mappatura relazionale (ORM), sviluppati in ambito Object Oriented, offrono e gestiscono automaticamente la corrispondenza tra oggetti temporanei e tuple.
## Gestione delle transazioni
Come detto nel capitolo [[Appunti temporanei/(AT) SecondoAnno - PrimoSemestre/Basi di Dati/1 - Introduzione|1 - Introduzione]], una transazione è un'operazione che il DBMS esegue garantendone:
- **Atomicità**: una transazione non può essere incompleta, se una transazione fallisce i suoi effetti vengono annullati ed il database torna alla fase di consistenza precedente.
- **Seralizzabilità**: garantita con il meccanismo del bloccaggio dei dati (record and table locking). È una gestione ottenuta mediante tecniche di programmazione concorrente in cui prima di leggere/modificare un dato, una transazione deve bloccare questo dato il lettura/scrittura.

Idealmente per assicurare che accessi concorrenti non provochino stati di inconsistenza nel database, ogni transazione dovrebbe essere effettuata in serie. Questo comporterebbe una estrema lentezza del DB in caso di accessi concorrenti. Con la tecnica del record and table locking, invece, si riesce a serializzare le letture e scritture su un dato, bloccando solo il dato interessato dall'operazione. Si interviene quindi con la programmazione delle transazioni, ovvero bloccare, a livello di codice, il dato di interesse per il tempo strettamente necessario all'esecuzione della transazione. Questo permette di prevedere due condizioni:
- Quando il programma rileva un’anomalia, può essere opportuno disfare solo una parte delle operazioni fatte
- Quando un programma richiede un tempo lungo per terminare le proprie operazioni, può essere opportuno spezzare il programma in più transizioni.

I DBMS relazionali permettono di spezzare un programma in più transazioni usando le operazioni di $\text{COMMIT}$ e $\text{ROLLBACK}$. Una transazione è considerata iniziata dal sistema quando un programma esegue un’operazione su una tabella e termina quando:
- Viene eseguito il comando$\text{exec sql commit work}$, cioè la transazione termina esplicitamente, rilasciando i blocchi sui dati usati, rendendoli disponibili ad altre transazioni (es. attesa di un input utente).