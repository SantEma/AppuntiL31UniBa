Prima di cominciare il capitolo, qualche accenno sulla sintassi che verrà utilizzata:
- Le **parentesi angolari** $\langle \rangle$ permettono di isolare un termine della sintassi
- Le **parentesi quadre** indicano che il termine all'interno è opzionale, ossia può non comparire oppure comparire una sola volta
- Le **parentesi graffe** indicano che il termine racchiuso può non comparire o essere ripetuto un numero arbitrario di volte
- Le **barre verticali** indicano che deve essere scelto uno tra i termini separati dalle barre (un elenco di termini in alternativa può essere racchiuso tra parentesi angolari)

Le parentesi tonde dovranno sempre essere intese come termini del linguaggio SQL, non come simboli della definizione della grammatica

Useremo poi questa tabella come esempio principale di tutto il capitolo:
![[Pasted image 20251016154754.png]]

---

SQL è il linguaggio di riferimento per le basi di dati relazionali.
Nel tempo la Structured Query Language ha subito diverse iterazioni e standardizzazioni, fino ad arrivare ad SQL-3 ma l'utilizzo di riferimento in questo caso sarà SQL-2

Occorre evidenziare una distinzione formale fra SQL e il modello dei dati relazionale: 
SQL permette ad una relazione (o tabella) di avere due o più tuple che sono identiche in tutti i valori di attributo (in generale, una tabella SQL non è un insieme di tuple, ma un multi-insieme)
Le ragioni fondamentali per cui SQL permette la presenza di duplicati sono: 
1. L’eliminazione di duplicati è una operazione costosa. 
2. Spesso l’operazione di eliminazione di duplicati non è necessaria, in quanto noto a priori che non ve ne saranno (è sufficiente che gli attributi che definiscono il risultato costituiscano una chiave). 
3. L’utente potrebbe voler vedere i duplicati delle tuple nel risultato di una interrogazione. 
4. Quando si applicano funzioni aggregate alle tuple, spesso non si vogliono eliminare i duplicati.
## Interrogazioni in SQL
La parte di SQL dedicata alla formulazione di interrogazioni fa parte del DML;
D'altronde la separazione tra DML e DDL non è rigida e parte dei servizi di definizione di interrogazioni vengono riutilizzati nella specificadi alcuni aspetti avanzati dello schema
### Dichiaratività di SQL
SQL esprime le interrogazioni in modo **dichiarativo**, ossia si specifica l'obbiettivo dell'interrogazione e non il modo in cui ottenerlo, seguendo quindi i principi del calcolo relazionale (contrapponendosi a quelli procedurali come l'algebra relazionale).
Un interrogazione SQL per essere eseguita viene passata all'ottimizzatore di interrogazioni (query optimizer), un componente del DBMS che analizza interrogazione e formula a partire da quest'ultima un'interrogazione equivalente in calcolo relazionale.
In generale esistono diversi modi per effettuare la stessa interrogazione, il programmatore però deve effettuare la scelta non basandosi sull'efficienza ma sulla leggibilità e modificabilità
### Interrogazioni semplici in SQL 
#### Clausola SELECT
Le operazioni di interrogazione base in SQL vengono specificate per mezzo di questa istruzione, la sua struttura essenziale è:
```sql
SELECT [DISTINCT] ListaAttributi <- Target list
FROM ListaTabelle <- clausola FROM
[WHERE Condizione] <- clausola WHERE
```

Di cui:
- **ListaAttributi** è l'elenco di nomi di attributi i cui valori devono essere ritrovati dall'interrogazione: $$ \langle ListaAttributi \rangle ::= * |\langle Attributi \rangle \{,\langle Attributo \rangle\} $$
- **ListaTabelle** è l'elenco di nomi di relazioni necessarie per elaborare l'interrogazione: $$\langle ListaTabelle \rangle ::= \langle Tabelle \rangle[\langle Ide \rangle] \{,\langle Tabelle \rangle [\langle Ide \rangle]\}$$$$\langle IDE \rangle ::=[as] \langle NuovoIdentificatore \rangle$$
- **Condizione** specifica la condizione che deve essere soddisfatta dalle tuple del risultato

Questa interrogazione quindi seleziona, tra le righe che appartengono al prodotto cartesiano delle tabelle elencate nella clausola $\text{FROM}$, quelle che soddisfano le condizioni in $\text{WHERE}$ 

Le operazioni più semplici si ottengono usando solo alcune clausole:
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

Per evitare ambiguità, quando si opera sul prodotto di tabelle con gli stessi attributi, si usa la notazione con il punto (sia clausola $\text{SELECT}$ che clausola $\text{FROM}$):
```sql
SELECT Clienti.CodiceCliente, Ordini.Ammontare 
FROM Clienti, Ordini 
WHERE Clienti.CodiceCliente = Ordini.CodiceCliente
```
N.B. Questo è un esempio di join in SQL, in particolare si tratta di un equi-join;
La condizione di selezione è una congiunzione di atomi di uguaglianza, con un attributo della prima relazione ed uno della seconda.

Una notazione alternativa prevede l’associazione di identificatori alle tabelle (alias), da usare per specificare gli attributi nella notazione, così l'esempio precedente può essere scritto come:
```sql
SELECT c.CodiceCliente, o.Ammontare 
FROM Clienti c, Ordini o 
WHERE c.CodiceCliente=o.CodiceCliente 
```
Questa seconda notazione è indispensabile quando si deve fare il prodotto di una tabella per se stessa.

Nella clausola $\text{SELECT}$ può apparire l'asterisco $*$ per indicare la selezione di tutti gli attributi.
A differenza dell’algebra relazionale è possibile costruire relazioni i cui attributi non corrispondono agli attributi delle relazioni selezionate, ma sono ottenuti come espressioni a partire da attributi e costanti
```sql
SELECT Stipendio/12 AS StipMensile 
FROM Impiegati
```
Se si vogliono eliminare i duplicati nel risultato di una interrogazione SQL, si può farlo mediante la parola chiave $\text{DISTINCT}$ nella clausola.
##### Target List
Il risultato dell’espressione $\text{SELECT ListaAttributi FROM}$ è una tabella, i cui nomi di colonna sono quelli indicati in $\text{ListaAttributi}$. 
La sintassi completa è:
![[Pasted image 20251021153740.png]]
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
#### Ordinamento
Se una relazione è un insieme non ha senso definire un ordinamento ma tuttavia, se si guarda al risultato di una interrogazione come ad una tabella, si può porre il problema di ordinare le righe. 
SQL permette di specificare un ordinamento mediante la clausola $\text{ORDER BY}$ riportata dopo la clausola $\text{WHERE}$:$$\text{ORDER BY} \langle \text{Attributo}\rangle [\text{DESC}] \{,\langle \text{Attributo}\rangle [\text{DESC}]\} $$
L’ordinamento è crescente, a meno che non sia specificato altrimenti ($\text{DESC}$)

