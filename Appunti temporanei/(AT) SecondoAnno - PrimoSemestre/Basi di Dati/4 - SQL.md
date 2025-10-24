SQL è il linguaggio di riferimento per le basi di dati relazionali.
Nel tempo la Structured Query Language ha subito diverse iterazioni e standardizzazioni, fino ad arrivare ad SQL-3 ma l'utilizzo di riferimento in questo caso sarà SQL-2
## Definizione dei dati in SQL
Prima di cominciare il capitolo, qualche accenno sulla sintassi che verrà utilizzata:
- Le **parentesi angolari** $\langle \rangle$ permettono di isolare un termine della sintassi
- Le **parentesi quadre** indicano che il termine all'interno è opzionale, ossia può non comparire oppure comparire una sola volta
- Le **parentesi graffe** indicano che il termine racchiuso può non comparire o essere ripetuto un numero arbitrario di volte
- Le **barre verticali** indicano che deve essere scelto uno tra i termini separati dalle barre (un elenco di termini in alternativa può essere racchiuso tra parentesi angolari)

Le parentesi tonde dovranno sempre essere intese come termini del linguaggio SQL, non come simboli della definizione della grammatica
### I domini elementari
SQL mette a disposizione alcune famiglie di domini elementari, da cui si possono poi definire i domini da associare agli attributi dello schema
#### Caratteri
Il dominio $\text{character}$ permette di rappresentare singoli caratteri oppure stringhe.
La lunghezza può essere fissa o variabile, in caso di quelle variabile si esplicita la lunghezza massima, oltre a poter prevedere una specifica della famiglia dei caratteri di default (latino, cirillico etc.).
La sintassi prevista è:$$\begin{align} \text{character [varying]} [(Lunghezza)] \\ [\text{character set}] NomeFamigliaCaratteri\end{align}$$
#### Tipi numerici esatti
[da completare]
#### Tipi numerici approssimativi
[da completare]
#### Istanti temporali
[da completare]
#### Intervalli temporali
[da completare]
### Definizioni di schema
[da completare]
### Definizioni di tabelle
[da completare]
### Definizione di domini
[da completare]
### Specifica valori di default
[da completare]
### Vincoli intrarelazionali
[da completare]
#### Not Null
[da completare]
#### Unique
[da completare]
#### Primary Key
[da completare]
### Vincoli interrelazionali
[da completare]
### Modifica degli schemi
[da completare]
#### Alter
[da completare]
#### Drop
[da completare]
### Cataloghi relazionali
[da completare]
## Interrogazioni in SQL
La parte di SQL dedicata alla formulazione di interrogazioni fa parte del DML;
D'altronde la separazione tra DML e DDL non è rigida e parte dei servizi di definizione di interrogazioni vengono riutilizzati nella specifica di alcuni aspetti avanzati dello schema
### Dichiaratività di SQL
SQL esprime le interrogazioni in modo **dichiarativo**, ossia si specifica l'obbiettivo dell'interrogazione e non il modo in cui ottenerlo, seguendo quindi i principi del calcolo relazionale (contrapponendosi a quelli procedurali come l'algebra relazionale).
Un interrogazione SQL per essere eseguita viene passata all'ottimizzatore di interrogazioni (**query optimizer**), un componente del DBMS che analizza interrogazione e formula a partire da quest'ultima un'interrogazione equivalente in calcolo relazionale.
In generale esistono diversi modi per effettuare la stessa interrogazione, il programmatore però deve effettuare la scelta non basandosi sull'efficienza ma sulla leggibilità e modificabilità.
### Interrogazioni semplici in SQL 
Le operazioni di interrogazione base in SQL vengono specificate per mezzo della struttura essenziale:
```sql
SELECT [DISTINCT] ListaAttributi <- Target list
FROM ListaTabelle <- clausola FROM
[WHERE Condizione] <- clausola WHERE
```




Questa interrogazione quindi seleziona, tra le righe che appartengono al prodotto cartesiano delle tabelle elencate nella clausola $\text{FROM}$, quelle che soddisfano le condizioni in $\text{WHERE}$, ottenendo come risultato una tabella con una riga per ogni riga prodotta dalla clausola $\text{FROM}$ e filtrata dalla clausola $\text{WHERE}$, le cui colonne si ottengono dalla valutazione delle espressioni 

Le operazioni più semplici si ottengono usando anche eventualmente le clausole:
- **Selezione**: 
```sql
  SELECT * 
  FROM Ordini 
  WHERE Ammontare > 10000
```
- **Proiezione**:
```sql
  SELECT CognomeENome, Città 
  FROM Clienti
```
- **Prodotto cartesiano**:
```sql
  SELECT * 
  FROM Clienti, Ordini
```


#### Clausola SELECT

#### Clausola FROM
ListaTabelle può essere un’unica tabella su cui fare una selezione e una proiezione, oppure una serie di tabelle separate da virgole, sul cui prodotto cartesiano viene fatta una selezione e/o una proiezione
La sintassi completa dell’argomento ListaTabelle della clausola $\text{FROM}$ è:
![[Pasted image 20251021151510.png]]
##### Interrogazioni con uso di join
Quando si desidera formulare un'interrogazione che coinvolge righe appartenenti a più di una tabella si pone come argomento della clausola $\text{FROM}$ l'insieme di tabelle le quali si vuole accedere.
Sul prodotto cartesiano delle tabelle elencate verranno applicate le condizioni della clausola $\text{WHERE}$, quindi un join può essere specificato indicando in modo esplicito le condizioni che esprimono il legame tra diverse tabelle.
La giunzione di default in questo caso è il **theta-join**, specificato mediante gli operatori.
Esempio:
```sql
SELECT Agenti.Supervisore, Ordini.Ammontare 
FROM Agenti JOIN Ordini ON 
	Agenti.Supervisore=Ordini.CodiceAgente
```
La giunzione esterna è comunque realizzata specificando vari operatori come:
$\text{LEFT|RIGHT|FULL[OUTER]JOIN}$
a seconda di quale giunzione si voglia effettuare:
```sql
SELECT Agenti.CodiceAgente, Ordini.Ammontare 
FROM Agenti NATURAL JOIN Ordini
```
#### Clausola WHERE
La clausola $\text{WHERE}$ ammette come argomento un espressione booleana costruita combinando predicati semplici con gli operatori $\text{AND,OR,NOT}$.
Ciascun predicato utilizza gli operatori $<>,<,>,\leq,\geq$ per confrontare un da un alto un'espressione costruita a partire dai valori degli attributi per riga e dall'altro lato un valore costante o un altra espressione.
Quando i predicati sono separati dall'operatore $\text{AND}$ saranno selezionate solo le righe per cui tutti i predicati sono veri;
Quando i predicati sono separati dall'operatore $\text{OR}$ saranno selezionate solo le righe per cui uno dei predicati risulti vero;
Il predicato $\text{NOT}$ è unario e inverte i valori di verità del predicato.

Il risultato di un predicato può essere $TRUE (T), FALSE (F)$ o $UNKNOWN (U)$.
Qui una tabella di esempio per quanto riguarda questi operatori booleani applicati ai predicati(con logica a tre valori):

| p   | q   | p AND q | p OR q | NOT p |
| --- | --- | ------- | ------ | ----- |
| T   | T   | T       | T      | F     |
| T   | F   | F       | T      | F     |
| T   | U   | U       | T      | F     |
| F   | F   | F       | F      | T     |
| F   | U   | F       | U      | T     |
| U   | U   | U       | U      | U     |
La sintassi della clausola $\text{WHERE}$ completa è la seguente:
![[Pasted image 20251022094906.png]]

#### Gestione dei valori nulli
Un valore nullo in un attributo può significare che tale non è applicabile, oppure che sia applicabile ma non conosciuto, oppure che non si sappia quale delle due situazioni valga.
Per selezionare i termini con valori nulli SQL fornisce il predicato $\text{IS NULL}$, con sintassi:
$$\text{Attributo is [not] NULL}$$
Questo predicato risulta vero soltanto se il valore è veramente null, altrimenti si ha la sua negazione.
Nell'SQL-2 ci si potrebbe aspettare
[da finire]

#### Interpretazione formale delle interrogazioni in SQL
[da completare]
#### Duplicati
[da completare]
#### Uso di variabili
[da completare]
#### Ordinamento
Se una relazione è un insieme non ha senso definire un ordinamento ma tuttavia, se si guarda al risultato di una interrogazione come ad una tabella, si può porre il problema di ordinare le righe. 
SQL permette di specificare un ordinamento mediante la clausola $\text{ORDER BY}$ riportata dopo la clausola $\text{WHERE}$:$$\text{ORDER BY} \langle \text{Attributo}\rangle [\text{DESC}] \{,\langle \text{Attributo}\rangle [\text{DESC}]\} $$
L’ordinamento è crescente, a meno che non sia specificato altrimenti ($\text{DESC}$)

