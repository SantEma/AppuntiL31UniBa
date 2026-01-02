Nell'utilizzare una base di dati, il dialogo diretto con l’interprete SQL è riservato a pochi utenti esperti. L'accesso di gran lunga più tipico a una base di dati avviene attraverso applicazioni integrate nel sistema informativo, le quali forniscono agli utenti un’interfaccia semplificata che favorisce l’interazione. 
SQL supporta le applicazioni in due modi:
- Incrementando le funzionalità del DBMS per mezzo della definizione di **procedure** e **trigger**;
- Integrando comandi SQL con istruzioni di un linguaggio di programmazione (procedurale/oggetti) utilizzando SQL Embedded per i linguaggi datati, oppure il Cal Level Interface per linguaggi di programmazione più recenti
## Procedure 
Lo standard SQL-2 prevede la definizione di **Procedure**, ovvero dei brevi sottoprogrammi memorizzati nel database come parte dello schema (motivo per cui vengono dette anche **stored procedures**). Esse permettono di assegnare un nome a un’istruzione SQL ed eventuali parametri. Una volta definita, la procedura è utilizzabile come un qualunque comando SQL. La presenza di procedure memorizzate nella base di dati consente di azzerare il tempo che sarebbe stato necessario per scrivere ogni volta le stesse istruzioni in SQL e, di conseguenza, riducono il rischio di commettere errori involontari scrivendo istruzioni errate.

Consideriamo una procedura SQL che aggiorna il nome della città del dipartimento:
```SQL
PROCEDURE AssegnaCitta(:Dip varchar(20),
                       :Citta varchar(20))
UPDATE Dipartimento
SET Città=: Citta
WHERE Nome =: Dip;
```

È bene sapere che lo standard SQL-2 non tratta la scrittura di procedure complesse, ma solo quelle composte da un singolo comando SQL. Questo è invece permesso in SQL-3, dove viene fornita una ricca sintassi per la definizione di procedure, integrando anche le strutture di controllo
## Trigger