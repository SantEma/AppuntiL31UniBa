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
La sintassi prevista è:$$\begin{align}  \\
&\text{character [varying]} [(Lunghezza)] \\
&[\text{character set}] NomeFamigliaCaratteri
\end{align}$$
#### Tipi numerici esatti
Questa famiglia contiene i domini che permettono di rappresentare valori esatti, interi o con una parte decimale di lunghezza prefissata.
La sintassi prevista è:
$$\begin{aligned}
&\text{numeric}[(Precisione[,Scala])] \\
&\text{decimal}[(Precisione[,Scala])] \\
&\text{integer}\ \\
&\text{smallint}
\end{aligned}$$
I numeri $\text{numeric}$ e $\text{decimal}$ rappresentano tutti i numeri in base decimale, mentre il parametro $Precisione$ specifica il numero di cifre significative (con un dominio $\text{decimal}(4)$ si possono rappresentare per esempio valori da -9999 a +9999).
Il parametro $\text{Scala}$ si specifica la scala di rappresentazione (ossia quante cifre compaiono dopo la virgola nella scala di rappresentazione, tutto ciò sempre specificando la precisione), se non specificata è sempre 0.
La differenza tra $\text{decimal}$ e $\text{numeric}$ consiste nella precisione, per il primo ci deve essere un requisito minimo, per il secondo invece si rappresenta un valore esatto.
Nel caso in cui non interessi la parte frazionaria e quindi la precisione della rappresentazione decimale, allora si possono usare i domini $\text{integer}$ e $\text{smallint}$, che si basano sulla rappresentazione interna binaria del calcolatore (la precisione è lasciata all'implementazione)
Qualora comunque la precisione non sia specificata, il sistema usa un valore caratteristico della implementazione.
#### Tipi numerici approssimativi
Per la rappresentazione di valori reali approssimativi SQL fornisce i seguenti tipi:
$$\begin{aligned}
&\text{float}[(Precisione)] \\
&\text{real} \\
&\text{double precision} \\
\end{aligned}$$
Tutti questi domini ovviamente permettono di descrivere numeri approssimati mediante rappresentazione con virgola mobile, in cui ciascun numero corrisponde ad una coppia di valori: mantissa e esponente.
Per ottenere un valore approssimativo del numero reale si moltiplica la mantissa per la potenza di 10 con il grado dell'esponente (esempio: $1,7 \times 10^{15}$ rappresenta $0.17E16$, oppure $-4\times 10^{-7}$ rappresenta $-0.4E-6$).
Al dominio $\text{float}$ può essere associata una precisione che rappresenta il numero delle cifre dedicate alla mantissa, la precisione invece dell'esponente è dipendente dall'implementazione.
La precisione del dominio $\text{real}$ è fissa, quella del dominio $\text{double precision}$ è di dimensione doppia rispetto al dominio precedente
#### Istanti temporali
Per la rappresentazione di istanti di tempo in SQL usiamo questa famiglia di domini con i seguenti tipi:
$$\begin{aligned}
&\text{date} \\
&\text{time}[(Precisione)] [with\  time \ zone] \\
&\text{timestamp}[(Precisione)] [with\  time \ zone] \\
\end{aligned}$$
Ciascuno di questi domini è strutturato e decomponibile con un insieme di campi:
- Il dominio $\text{date}$ ammette i campi $\text{year, month, day}$
- Il dominio $\text{time}$ ammette i campi $\text{hour,minute,seconds}$
- Il dominio $\text{timestamps}$ ammette tutti i campi precedentemente descritti
Se l'opzione $with \ time \ zone$ è specificata allora risulta possibile accedere a due campi, $\text{timezone \_\ hour}$ e $\text{timezone \_\ minute}$, che rappresentano la differenza tra il fuso orario locale e quello standard (standard UTC)
#### Intervalli temporali
Questa famiglia di domini permette di rappresentare intervalli di tempo (come la durata di un evento), la sintassi è:
$$\text{interval} \  PrimaUnitàDiTempo [\text{to} \ UltimaUnitàdiTempo]$$
PrimaUnitàDiTempo e UltimaUnitàDiTempo definiscono le unità di misure, dalla più precisa alla meno precisa, in questo modo si possono definire domini come $\text{interval year to month}$ per indicare per indicare che la durata di intervallo deve essere misurata in numero di anni e di mesi.
Notiamo che comunque ci sono due insieme distintivi nelle unità di misura: $\text{year to month}$ e $\text{day to seconds}$, questo perchè non si possono paragonare per esempio giorni e mesi in modo esatto (un mese potrebbe avere dai 28 ai 31 giorni), rendendo difficile eventuali operazioni aritmetiche.
### Definizioni di schema
SQL consente la definizione di uno schema di base di dati come collezione di oggetti (tabelle, domini, viste etc.).
Uno schema viene definito dalla seguente sintassi:
$$\begin{aligned}
&\text{create schema}[NomeSchema][[authorization]\text{Autorizzazione}] \\
&\text{\{DefinizioneElementoSchema\}}
\end{aligned}$$
$Autorizzazione$ rappresenta il nome dell'utente proprietario dello schema, se omesso si assume che chi abbia lanciato il comando sia il proprietario, $NomeSchema$ viene poi rinominato con lo stesso nome del proprietario.
Dopo il comando $\text{create schema}$ compaiono le definizioni dei suoi componenti, ma non è necessario che questa definizione avvenga contemporaneamente alla crazione dello schema.
### Definizioni di tabelle
Una tabella SQL è costituita da una collezione ordinata di attributi e da un insieme (eventualmente vuoto) di vincoli.
Lo schema della tabella $\text{DIPARTIMENTO}$ viene definita per esempio tramite la seguente istruzione SQL:
```sql
create table Dipartimento
(
	Nome varchar(20) primary key,
	Indirizzo varchar(50),
	Città varchar(20)
)
```
Definendolo quindi con uno schema più generale:
$$\begin{aligned}
&\text{create table}NomeTabella \\
&(Nomeattributo \ Dominio [ValoreDiDefault][Vincoli]) \\
& \quad \{,NomeAttributo Dominio[ValoreDiDefault][Vincoli]\} \\
&AltriVincoli
\end{aligned}
$$

Ogni tabella viene quindi definita associandole un nome ed elencando gli attributi che compongono lo schema.
Per ogni attributo abbiamo un nome, dominio ed eventuali insiemi di vincoli che devono essere rispettati dai valori dell'attributo.
Dopo la definizione degli attributi si possono definire i vincoli che coinvolgono più attributi della tabella.
### Definizione di domini
Nella definizione delle tabelle si può fare riferimento ai domini predefiniti del linguaggio o a domini definiti dall'utente a partire da quelli predefiniti, infatti proprio da questi è possibile definirli in questa maniera:
$$ \begin{aligned}
&\text{create domain}NomeDominio \ as \ TipoDiDato \\
&\quad\quad\quad\quad\quad\quad \ [ValoreDiDefault] \\
&\quad\quad\quad\quad\quad\quad \ [Vincolo] \\
\end{aligned}
$$
Un dominio quindi è caratterizzato dal proprio nome, un dominio elementare (predefinito o definito dall'utente in precedenza), da un eventuale valore di default e da un insieme di vincoli (eventualmente vuoti) per gli eventuali valori del dominio.
Definendo un dominio si può rendere più facile la modifica della definizione, infatti se si vuole modificare la definizione di attributi in uno stesso dominio sarà necessario soltanto modificare quest'ultimo e si applicherà a tutte le tabelle del dominio.
### Specifica valori di default
Il termine $ValoreDiDefault$ nei domini e nelle tabelle permette di specificare un valore predefinito quando viene inserito un attributo in una riga della tabella senza specificare un valore.
In modo predefinito il valore di default risulta sempre nullo.
La sintassi specifica è la seguente:
$$default \langle GenericoValore | user| null \rangle$$
- $GenericoValore$ rappresenta un valore compatibile con il dominio
- $user$ impone come valore di default l'identificativo dell/ utente che esegue il comando
- $null$ corrisponde al valore di default base

Quando un attributo o un dominio è definito a a partire da un altro a cui è stato già specificato un valore di default automaticamente quello ha la maggiore priorità, diventando valore effettivo. 
### Vincoli intrarelazionali
Sia nella definizione di domini che di tabelle è possibile definire dei vincoli, ovvero delle proprietà che devono essere verificate da ogni istanza della base di dati.
Ricordiamo che i vincoli intrarelazionali coinvolgono una sola relazione.
#### Not Null
Il valore nullo come sappiamo è un particolare valore che indica l'assenza di informazioni, ma SQL non permette la distinzione dei diversi casi, per questo bisogna avere delle soluzioni ad-hoc, come l'introduzione di altri attributi o l'uso di particolare codifica.
Il vincolo $\text{not null}$ indica che il valore nullo non è ammesso come valore dell'attributo e deve essere necessariamente specificato in fase di inserimento (ma anche successivamente), ma nel caso sia presente un valore di default non è necessario l'inserimento forzato.
Un esempio può essere:
```sql
Cognome varchar(20) not null 
```
#### Unique
Il vincolo $\text{unique}$ si applica ad un attributo o a un insieme di attributi di una tabella e impone che i valori (o le n-uple dei valori sull'insieme degli attributi) siano una superchiave, ossia per tutte le righe differenti della tabella non ci siano gli stessi valori.
Un'eccezione viene fatta per il valore nullo, in quanto si assume che siano tutti diversi tra loro.
La definizione del vincolo può avvenire in due modi:
1. Quando si vuole specificare questo vincolo su un unico attributo, in quel caso viene dichiarato nella specifica di quell'attributo:
```sql 
Matricola character(6) unique 
```
2. Quando avviene su un insieme di attributi in una tabella, usando la sintassi seguente: $$unique(Attributo,\{, Attributo\})$$
   Un esempio di sintassi è il seguente:
```sql
Nome varchar(20) not null,
Cognome varchar(20) not null,
unique (Cognome,Nome)   
```

Si noti che si potrebbe pensare che la definizione:
```sql
Nome varchar(20) not null unique
Cognome varchar(20) not null unique
```
sia uguale in modo logico, ma in non lo è, nel primo caso si presuppone che non ci siano righe uguali con nome e cognome uguale, nel secondo caso invece si presuppone che non esistano o lo stesso nome o lo stesso cognome ripetuto più di una volta
#### Primary Key
SQL permette di specificare il vincolo $\text{primary key}$ soltanto una volta per tabella e per singolo attributo o più attributi che costituiscono l'identificatore.
Gli attributi che fanno parte della chiave primaria non possono essere nulli, quindi si implica che ci sia una definizione $\text{not null}$ omessa.
Un esempio di dichiarazione può essere:
```sql
Nome varchar(20),
Cognome varchar(20),
primary key (Cognome, Nome)
```
### Vincoli interrelazionali
I vincoli interrelazionali più diffusi e significativi sono i **vincoli di integrità referenziale**, in SQL per loro definizione viene usato il vincolo di $\text{foreign key}$, chiamato anche **chiave esterna**;
Questa chiave esterna crea un legame tra i valori di un attributo della tabella su cui è definito (chiamata **interna**) e i valori di attributo di un altra tabella (chiamata **esterna**).
Il vincolo impone che per ogni riga della tabella interna il valore dell'attributi specificato (se diverso da nullo) sia presente nelle righe della tabella esterna tra i valori del corrispondente attributo;
Questo vincolo ha come unico requisito che la sintassi dell'attributo a cui si fa riferimento alla tabella esterna sia soggetto ad $\text{unique}$, ossia che questo sia un identificatore, infatti tipicamente la chiave esterna fa riferimento la chiave primaria della tabella.
Nel vincolo possono essere coinvolti più attributi, in tal caso l'unica differenza è che bisognerà confrontare n-uple di valori piuttosto di singoli valori.

Possiamo definirlo in due modi:
1. Nel caso sia un unico attributo coinvolto si può usare il costrutto sintattico $\text{references}$, con il quale si specificano la tabella esterna e l'attributo della tabella esterna:
```sql
create table Impiegato
(
Matricola character(6) primary key
Nome varchar(20) not null,
Cognome varchar(20) not null,
Dipart varchar(15)
	   references Dipartimento(NomeDip)
Ufficio numeric(9) default 0,
unique(Cognome,Nome)
)   
```
2. Nel caso ci sia un insieme di attributi si utilizza $\text{foreing key}$, posto al termine della definizione degli attributi
```sql
foreign key (Nome,Cognome)
			references Anagrafica(Nome,Cognome)
```

La corrispondenza tra gli attributi locali e quelli esterni avviene in base all'ordine, infatti il primo attributo corrispondente a $\text{foreign key}$ corrisponde al primo argomento di $\text{references}$ e via via gli altri attributi.

Per tutti i vincoli visti finora quando il sistema rileva una violazione il comando di aggiornamento viene rifiutato segnalando l'errore all'utente, invece per quelli referenziali SQL permette di scegliere altre reazioni da adottare quando viene rilevata una violazione, per esempio una violazione può essere la modifica del contenuto della tabella interna in due modi:
- Inserire una nuova riga
- Modificare il valore dell'attributo referente
In questo caso non viene offerto particolare supporto e l'operazione viene semplicemente rifiutata.

Per le modifiche sulla tabella esterna le alternative esistono, dato dal particolare significato della tabella esterna che sul piano applicativo rappresenta la tabella principale (**master**) alle cui variazioni la tabella interna (**slave**) deve adeguarsi.
Per poter agire con le operazioni di modifica possiamo usare uno dei seguenti modi:
- $\text{cascade}$: il nuovo valore dell'attributo della tabella esterna viene riportato su tutte le corrispondenti righe della tabella interna
- $\text{set null}$: all'attributo referente viene assegnato il valore nullo al posto del valore modificato nella tabella esterna
- $\text{set default}$: all'attributo referente viene assegnato il valore di default al posto del valore modificato nella tabella esterna
- $\text{no action}$: l'azione di modifica non viene consentita e il sistema quindi non ha bisogno di riparare la violazione

Per le violazioni da cancellazione di un elemento della tabella esterna si ha a disposizione lo stesso insieme di reazioni:
- $\text{cascade}$: tutte le righe della tabella interna corrispondenti alla riga cancellata vengono cancellate
- $\text{set null}$: all'attributo referente viene assegnato il valore nullo al posto del valore cancellato nella tabella esterna
- $\text{set default}$: all'attributo referente viene assegnato il valore di default al posto del valore modificato nella tabella esterna
 - $\text{no action}$: l'azione di cancellazione non viene consentita

In generale per ogni evento è possibile associare una politica diversa in base a come la si vuole gestire.
Nel caso della politica $\text{cascade}$ si assume che le righe della tabella interna siano strettamente legate alle corrispondenti righe della tabella esterna, per cui se si apporta una modifica alla tabella esterna si devono modificare in modo conseguente tutte le righe della tabella interna, per quanto riguarda le altre politiche ci sono dipendenze meno strette tra prima e seconda tabella.
Le violazioni possono generare una reazione a catena qualora la tabella interna compaia a sua volta come tabella esterna in un altro vincolo di integrità.

La politica di reazione viene specificata immediatamente dopo il vincolo di integrità secondo la seguente sintassi:
$$
\begin{aligned}
&\text{on \langle delete | update\rangle}\\
&\quad \  \text{\langle cascade | set null | set default | no action\rangle}
\end{aligned}
$$
### Modifica degli schemi
SQL fornisce primitive per la manipolazione degli schemi della base di dati che permettono di modificare le definizioni di tabelle precedentemente introdotte.
I comandi che vengono utilizzati a questo fine sono $\text{alter}$ e $\text{drop}$ 
#### Alter
Il comando $\text{alter}$ permette di modificare domini e schemi di tabelle, le forme che troviamo sono:
Per i domini:
$$\begin{aligned}
&\text{alter domain} \ NomeDominio \langle \text{set default} \ ValoreDiDefault \| \\
&\quad \quad \quad\quad \quad \ \ \  \text{drop default} \ | \\
&\quad \quad \quad\quad \quad \ \ \  \text{add constraint} \  DefVincolo \ | \\
&\quad \quad \quad\quad \quad \ \ \  \text{drop constraint} \ NomeVincolo \rangle
\end{aligned}$$
Per le tabelle:
$$\begin{aligned}
&\text{alter table} \ NomeTabella \langle \\ 
&\text{alter column} \ NomeAttributo \langle \text{set default} \ NuovoDefault \ | \\
&\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad  \ \ \  \text{drop default} \rangle \ | \\ 
&\text{add constraint} \  DefVincolo \ | \\
&\text{drop constraint} \ NomeVincolo \ |\\ 
&\text{add column} \ DefAttributo\ |\\
&\text{drop column} \ NomeAttributo \rangle
\end{aligned}$$
Tramite $\text{alter domain}$ e $\text{alter table}$ è possibile aggiungere e rimuovere vincoli e modificare i valori di default associati ai domini e agli attributi, inoltre è possibile aggiungere ed eliminare attributi e vincoli sullo schema di una tabella.
Quando si definisce un nuovo vincolo questo deve essere soddisfatto dai dati già presenti, altrimenti l'inserimento viene rifiutato
#### Drop
Mentre il comando $\text{alter}$ effettua delle modifiche sui domini o sullo schema delle tabelle il comando $\text{drop}$ permette di rimuovere dei componenti come schemi, domini, tabelle, viste o asserzioni.
Il comando usa la seguente sintassi:
$$\begin{aligned}
&\text{drop} \langle \text{schema | domain| table | view | assertion}\rangle \ NomeElemento \\
& \quad \quad [\text{ restrict | cascade }]
\end{aligned}$$
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

