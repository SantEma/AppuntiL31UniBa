Nell'utilizzare una base di dati, il dialogo diretto con l’interprete SQL è riservato a pochi utenti esperti. L'accesso di gran lunga più tipico a una base di dati avviene attraverso applicazioni integrate nel sistema informativo, le quali forniscono agli utenti un’interfaccia semplificata che favorisce l’interazione. 
SQL supporta le applicazioni in due modi:
- Incrementando le funzionalità del DBMS per mezzo della definizione di **procedure** e **trigger**;
- Integrando comandi SQL con istruzioni di un linguaggio di programmazione (procedurale/oggetti) utilizzando SQL Embedded per i linguaggi datati, oppure il Cal Level Interface per linguaggi di programmazione più recenti
## Procedure
Lo standard SQL-2 prevede la definizione di **Procedure**, ovvero dei brevi sottoprogrammi memorizzati nel database come parte dello schema (motivo per cui vengono dette anche **stored procedures**). Esse permettono di assegnare un nome a un’istruzione SQL ed eventuali parametri. Una volta definita, la procedura è utilizzabile come un qualunque comando SQL. La presenza di procedure memorizzate nella base di dati consente di azzerare il tempo che sarebbe stato necessario per scrivere ogni volta le stesse istruzioni in SQL e, di conseguenza, riducono il rischio di commettere errori involontari scrivendo istruzioni errate.

Consideriamo una procedura SQL che assegna lo sconto per ogni cliente in base al codice cliente:
```SQL
PROCEDURE AssegnaSconto (:Cod char(2), :Sc integer))
UPDATE Clienti
SET Sconto = :Sc
WHERE CodiceCliente = :Cod;
```
Quest'ultima può essere invocata all'interno del linguaggio ospite utilizzando il comando seguente:
```SQL
$ AssegnaSconto (:CodCli, :ScRid)
```

È bene sapere che lo standard SQL-2 non tratta la scrittura di procedure complesse, ma solo quelle composte da un singolo comando SQL. Questo è invece permesso in SQL-3, dove viene fornita una ricca sintassi per la definizione di procedure, integrando anche le strutture di controllo
## Trigger
### Definizione
I trigger, detti anche regole attive, seguono il paradigma Evento-Condizione-Azione (ECA), infatti ogni trigger si attiva al verificarsi di una condizione prestabilita. Un trigger attivato, avvia l’esecuzione di una specifica sequenza di istruzioni. Le basi di dati che contengono trigger sono dette basi di dati attive. 
Nello specifico il paradigma seguito da un trigger funziona come segue:
- **Evento**: Tipicamente una modifica dello stato del database consiste in una operazione di $\text{INSERT}$, $\text{DELETE}$, $\text{UPDATE}$. Se l’evento accade, il trigger si attiva.
- **Condizione**: Predicato booleano espresso in SQL che identifica se l’azione del trigger deve essere eseguita. Quando il trigger si attiva, viene valutata la condizione, quindi il trigger viene considerato.
- **Azione**: Consiste in una sequenza di update SQL o una procedura. Quando la condizione è verificata, allora l’azione viene eseguita, quindi il trigger viene eseguito.

Ogni trigger è caratterizzato da:
- Nome
- Target, ovvero la tabella controllata
- Modalità
	- Nella modalità $\text{BEFORE}$ il trigger è considerato e possibilmente eseguito prima dell’evento, ad esempio quando si vuole verificare una modifica prima di eseguirla.
	- Nella modalità $\text{AFTER}$ il trigger è considerato ed eseguito dopo l’evento, più comune.
- Evento: $\text{INSERT}$, $\text{DELETE}$, o $\text{UPDATE}$
- Granularità
	- Granularità statement-level: il trigger è considerato e possibilmente eseguito su un insieme di tuple, agendo in modalità set-oriented.
	- Granularità row-level: il trigger è considerato e possibilmente eseguito una volta per ogni tupla modificata
- Tabelle di transizione
	- Se la modalità è row-level, ci sono due variabili di transizione ($\text{old}$ e $\text{new}$) che rappresentano il valore precedente o successivo alla modifica di una tupla.
	- Se la modalità è statement-level, ci sono due tabelle di transizione ($\text{old table}$ e $\text{new table}$) che contengono i valori precedenti e successivi delle tuple modificate dallo statement.
- Azione
- Timestamp di creazione

È bene ricordare che l’azione eseguita al verificarsi della condizione di un trigger, può a sua volta attivare un ulteriore trigger, rendendoli quindi **a cascata**,  ma se non vi si è attenti si potrebbe generare una catena di attivazioni potenzialmente infinita, mandando in stallo l’intera applicazione. 
I trigger sono uno strumento molto potente che permette di gestire vincoli di integrità, calcolare dati derivati, gestire eccezioni e codificare regole aziendali. 
Un ulteriore vantaggio derivante dall'utilizzo dei trigger consiste nel riuscire a codificare la logica del sistema in maniera centralizzata e condivisa da tutte le applicazioni, con conseguenti vantaggi in fase di lettura e manutenzione del codice, infatti in caso di modifiche al comportamento del sistema è sufficiente intervenire nell'ambito della definizione dei trigger e non in più parti del codice. Lo svantaggio è che i trigger sono standardizzati sono il SQL-3, per cui potrebbero presentarsi casi (se pur sempre più rari) di non portabilità del codice.
## SQL Embedded
SQL Embedded prevede di introdurre direttamente nel programma sorgente scritto nel linguaggio di alto livello le istruzioni SQL, distinguendole dalle normali istruzioni tramite un opportuno separatore. Lo standard SQL prevede