SQL è il linguaggio di riferimento per le basi di dati relazionali.
Nel tempo la Structured Query Language ha subito diverse iterazioni e standardizzazioni, fino ad arrivare ad SQL-3 ma l'utilizzo di riferimento in questo caso sarà SQL-2
## Definizione dei dati in SQL (Creazione di una base di dati)
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
Notiamo che comunque ci sono due insieme distintivi nelle unità di misura: $\text{year to month}$ e $\text{day to seconds}$, questo perché non si possono paragonare per esempio giorni e mesi in modo esatto (un mese potrebbe avere dai 28 ai 31 giorni), rendendo difficile eventuali operazioni aritmetiche.
### Definizioni di schema
SQL consente la definizione di uno schema di base di dati come collezione di oggetti (tabelle, domini, viste etc.).
Uno schema viene definito dalla seguente sintassi:
$$\begin{aligned}
&\text{create schema} \ [NomeSchema] \ [[authorization]\text{Autorizzazione}] \\
&\text{\{DefinizioneElementoSchema\}}
\end{aligned}$$
$\text{Autorizzazione}$ rappresenta il nome dell'utente proprietario dello schema, se omesso si assume che chi abbia lanciato il comando sia il proprietario, $NomeSchema$ viene poi rinominato con lo stesso nome del proprietario.
Dopo il comando $\text{create schema}$ compaiono le definizioni dei suoi componenti, ma non è necessario che questa definizione avvenga contemporaneamente alla creazione dello schema.
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
&\text{create table} \ NomeTabella \\
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
&\text{create domain} \ NomeDominio \ as \ TipoDiDato \\
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
Ricordiamo che i vincoli intrarelazionali coinvolgono una sola relazione su un unico attributo.
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
&\text{drop} \ \langle \text{schema | domain| table | view | assertion}\rangle \ NomeElemento \\
& \quad \quad [\text{ restrict | cascade }]
\end{aligned}$$
L'operazione $\text{restrict}$ specifica che il comando non deve essere eseguito in presenza di oggetti **non vuoti**, nei diversi casi:
- Uno **schema** non è rimosso se contiene tabelle o altri oggetti
- Un **dominio** non è rimosso se appare in qualche definizione di tabella
- Una **tabella** non è rimossa se possiede delle righe o se è presenta qualche definizione di tabella o vista 
- Una **vista** non è rimossa se è utilizzata nella definizione di altra tabelle o viste.
L'opzione $\text{restrict}$ è un opzione di default.

Nel caso si specifichi l'opzione $\text{cascade}$ tutti gli oggetti specificati devono essere rimossi.
Nei diversi casi:
- Quando si rimuove uno **schema** non vuoto anche tutti gli oggetti che fanno parte dello schema vengono eliminati
- Rimuovendo un **dominio** che compare nelle definizioni di qualche attributo l'opzione fa si che il nome del dominio venga rimosso, ma gli attributi che sono stati definiti utilizzando quel dominio rimangano associati al dominio elementare
- Quando si rimuove una **tabella** tutte le righe vengono perse, se la tabella poi compariva in qualche definizione di tabella o vista viene rimossa anche questa
- Eliminando una **vista** che compare nella altre definizioni di altre tabelle o viste viene anche queste tabelle e viste vengono rimosse
Quindi in generale l'opzione $\text{cascade}$ attiva una reazione a catena per cui tutti gli elementi che dipendono da un elemento vengono rimossi fin quando non si giunge ad una situazione dove non esistono dipendenze non risolte
## Interrogazioni in SQL
Useremo per tutto questo capitolo le seguenti tabelle per gli esempi:
![[Pasted image 20251030105223.png]]

---
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

Una descrizione più precisa della sintassi è tale:
$$
\begin{aligned}
&\text{SELECT} \ AttrEspr[[\text{as}]Alias]\{,AttrEspr [[\text{as}]Alias]\} \\
&\text{FROM} \ Tabella [[\text{as}]Alias] \{,Tabella [[\text{as}]Alias]\} \\
&[\text{WHERE} \ Condizione]
\end{aligned}
$$

Questa interrogazione quindi seleziona, tra le righe che appartengono al prodotto cartesiano delle tabelle elencate nella clausola $\text{FROM}$, quelle che soddisfano le condizioni in $\text{WHERE}$, ottenendo come risultato una tabella con una riga per ogni riga prodotta dalla clausola $\text{FROM}$ e filtrata dalla clausola $\text{WHERE}$, le cui colonne si ottengono dalla valutazione delle espressioni 

Le operazioni più semplici si ottengono usando anche le clausole:
- **Singola tabella**:
```sql
SELECT * 
FROM Agenti
```
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
La clausola $\text{SELECT}$ specifica gli elementi dello schema della tabella risultato.
Come argomento della clausola $\text{SELECT}$ può comparire il carattere speciale asterisco ($*$) che rappresenta la selezione di tutti gli attributi delle tabelle elencate nella clausola $\text{from}$
**Esempio**:
```sql
SELECT *
FROM Impiegato
WHERE Cognome="Rossi"
```

Possono anche comparire generiche espressioni sul valore degli attributi di ciascuna riga selezionata
**Esempio**:
```sql
SELECT Stipendio/12 as Stipendio mensile
FROM Impiegato
WHERE Cognome="Bianchi"
```
Risultato:
![[Pasted image 20251103094158.png]]
#### Clausola FROM
Quando si desidera formulare un'interrogazione che coinvolge righe appartenenti a più di una tabella si pone come argomento della clausola $\text{FROM}$ l'insieme di tabelle che si vuole accedere.
Sul prodotto cartesiano delle tabelle elencate verranno applicate le condizioni contenute nella clausola $\text{WHERE}$, quindi un join può essere specificato in modo esplicito indicando le condizioni che esprimono il legame tra le diverse tabelle

**Esempio**:
```sql
SELECT Impiegato.Nome, Impiegato.Cognome, Dipartimento.Città
FROM Impiegato, Dipartimento
WHERE Impiegato.Dipart = Dipartimento.Nome
```

È necessario utilizzare l'operatore punto "$.$" per specificare il nome della tabella quando le tabelle presenti possiedono più attributi con lo stesso nome, qualora poi non ci siano ambiguità è un ridondante se utilizzato.
#### Clausola WHERE
La clausola $\text{WHERE}$ ammette come argomento un'espressione booleana costruita combinando predicati semplici con gli operatori $\text{AND,OR,NOT}$;
Ciascun predicato semplice usa gli operatori classici come $<,>,<>,=,\leq,\geq$ per confrontare da un lato un'espressione costruita a partire dai valori degli attributi per la riga, e dall'altro lato un valore costante o un'altra espressione.

**Esempio:**
```sql
SELECT Nome, Cognome
FROM Impiegato
WHERE Ufficio=20 AND Dipart='Amministrazione'
```

Oltre ai normali predicati di confronto relazionali, SQL mette a disposizione un operatore $\text{LIKE}$ per il confronto delle stringhe, che permette di effettuare confronti con stringhe in cui compaiono caratteri speciali come $\_$ e $\%$, il primo rappresenta nel confronto un carattere arbitrario, il secondo una stringa di un numero arbitrario (anche eventualmente nullo).
Un confronto come $\text{LIKE 'ab\%ba\_'}$ sarà perciò soddisfatto da una qualsiasi stringa di caratteri che inizia con $\text{ab}$ e che ha la coppia di caratteri $\text{ba}$ prima dell'ultima posizione (come $\text{abcdedcbac}$)
#### Gestione dei valori nulli
Un valore nullo in un attributo può significare che un certo attributo non è applicabile, o che il valore è applicabile ma non conosciuto, oppure che non si conosca quale delle due situazioni sia applicabile.
Per selezionare i termini con i valori nulli SQL fornisce il predicato $\text{IS NULL}$, la cui sintassi è:
$$Attributo \text{ is [not] null}$$
Il predicato risulta vero solo se l'attributo ha valore nullo, mentre $\text{not null}$ è la sua negazione.
Ricordiamo che da SQL-2 viene utilizzata la logica a tre valori che prevede il valore $\text{unknown}$
#### Interpretazione formale delle interrogazioni in SQL
È possibile costruire una corrispondenza tra interrogazioni SQL ed equivalenti interrogazioni espresse in algebra relazionale (ricordiamo cosa viene esplicitato nel paragrafo [[#Dichiaratività di SQL]]).
Data un interrogazione SQL nella sua formula più semplice:
$$\begin{aligned}
&\text{SELECT} \ T_{1}.Attributo_{11},\dots, T_{h}Attributo_{hm} \\
&\text{FROM} \ Tabella_{1} \ T_{1}, \dots , Tabella_{n} \ T_{n} \\
&\text{WHERE} \ Condizione
\end{aligned}$$
si può costruire un'interrogazione equivalente in algebra relazionale usando la seguente traduzione (in cui per semplicità omettiamo le ridenominazioni che ci permettono di considerare tutti i join come prodotti cartesiani):
$$\Pi_{T_{1}.Attributo_{11},\dots, T_{h}Attributo_{hm}} (\sigma_{Condizione}(TABELLA_{1}\rhd\lhd \dots \rhd\lhd TABELLA_{n}))$$
Per interrogazioni più complicate la forma di conversione non è più applicabile.
Una condizione essenziale per l'esecuzione di queste traduzioni è però che l'interrogazioni di partenza non usi funzionalità di SQL non presenti nell'algebra e calcolo relazionale come la valutazione degli [[#Operatori Aggregati]].
I risultati delle interrogazioni SQL differiscono anche dalle espressioni dell'algebra e del calcolo relazionale nella gestione dei duplicati
#### Duplicati
Una differenza significativa tra SQL e algebra relazionale è data dalla gestione di duplicati:
Mentre in algebra relazionale una tabella viene vista come una relazione dal punto di vista matematico e quindi come un insieme di elementi (tuple) diversi tra loro, in SQL si possono avere in una tabella più righe uguali (dette **duplicati**).
Per emulare il comportamento dell'algebra relazionale sarebbe necessario effettuare l'eliminazione dei duplicati tutte le volte che si eseguono operazioni di proiezione, ma la rimozione è costosa e spesso non necessaria, per questo in SQL si è stabilito di permettere la presenza di duplicati all'interno delle tabelle, lasciando che sia l'interrogazione ad escluderli esplicitamente.
L'eliminazione dei duplicati viene esplicitata con la parola chiave $\text{DISTINCT}$ posta dopo $\text{SELECT}$
#### Join interni ed esterni
Una sintassi alternativa per la specifica dei join permette di distinguere, tra le condizioni che compaiono nell'interrogazione, quelle che rappresentano condizioni di join e quelle che rappresentano condizioni di selezioni sulle righe.
La sintassi proposta è la seguente:
$$
\begin{aligned}
&\text{SELECT} \ AttrEspr[[\text{as}]Alias]\{,AttrEspr [[\text{as}]Alias]\} \\
&\text{FROM} \ Tabella [[\text{as}]Alias] \\
&\{[TipoJoin]\ \text{JOIN} \ Tabella [[\text{as}]Alias] \text{on} CondizioneDiJoin\} \\
&[\text{WHERE} \  AltraCondizione]
\end{aligned}
$$
Mediante questa sintassi la condizione di join non compare come argomento della clausola $\text{WHERE}$, ma viene spostata nella clasuola $\text{FROM}$, associata alle tabelle che vengono coinvolte nel join.
Il parametro $TipoJoin$ specifica quale tipo di join usare, tra cui:
- $\text{inner}$, di default
- $\text{right outer}$
- $\text{left outer}$
- $\text{full outer}$
L'inner join rappresenta il tradizionale tetha-join dell'algebra relazionale

Con il join interno le righe che vengono coinvolte nel join sono in generale un sottoinsieme delle righe di ciascuna tabella, può infatti capitare che alcune righe non vengano considerate in quanto non vengano rispettate le condizioni;
Questo comportamento non è desiderabile solitamente, si preferisce infatti utilizzare il join esterno per mantenere le righe che fanno parte di una o entrambe le tabelle coinvolte.
Esistono poi tre variante dei join esterni:
- $\text{left}$, fornisce come risultato il join esteso con le righe della tabella che compare a sinistra per le quali non esiste una corrispondente riga nella tabella di destra
- $\text{right}$, che si comporta nella maniera opposta del $\text{left}$
- $\text{full}$, che restituisce il join interno esteso con le righe escluse di entrambe le tabelle

Normalmente il join naturale non è consigliato, dato che può introdurre dei rischi nelle applicazioni in quanto il suo comportamento può maturare profondamente al variare dello schema delle tabelle
#### Uso di variabili
Nelle interrogazioni SQL è possibile associare un nome alternativo, detto **alias**, alle tabelle che compaiono come argomento della clausola $\text{FROM}$. 
Il nome viene usato per fare riferimento alla tabella nel contesto dell'interrogazione. 
Questa funzionalità può essere sfruttata per fare riferimento a una tabella in modo compatto, ricorrendo a brevi alias ed evitando così di scrivere per esteso il nome della tabella tutte le volte che ne viene richiesto l'uso, ma ci sono però altre ragioni per usare gli alias;
Come prima cosa, utilizzando gli alias è possibile fare accesso più volte alla stessa tabella, come avviene nel calcolo relazionale quando si usano più variabili associate alla stessa tabella e in modo simile all'uso dell'operatore di ridenominazione $\rho$ dell'algebra relazionale. Tutte le volte che si introduce un alias per una tabella si dichiara in effetti una variabile che rappresenta le righe della tabella di cui è alias. 
Quando la tabella compare una sola volta in un'interrogazione, non c'è differenza tra l'interpretare l'alias come uno pseudonimo o come una nuova variabile, quando compare invece più volte è necessario considerare l'alias come una nuova variabile.

**Esempio:**
```sql
select I1.Cognome, I1.Nome
from Impiegato I1, Impiegato I2
where I1.Cognome = I2.Cognome and
      I1.Nome <> I2.Nome and
      I2.Dipart = 'Produzione'
```
#### Ordinamento
SQL permette di specificare un ordinamento delle righe del risultato di un'interrogazione tramite la clausola $\text{ORDER BY}$, con la quale si chiude l'interrogazione.
La sintassi è la seguente:
$$\begin{aligned}
&\text{order by} AttrDiOrdinamento \ \text{[asc|desc]} \\
&\quad\quad\quad\quad \{,AttrDiOrdinamento \ \text{[asc|desc]}\}
\end{aligned}$$
Le righe vengono ordinate in base al primo attributo nell'elenco, per le righe che hanno lo stesso valore del primo attributo si considerano i valori degli attributi successivi in sequenza.
L'ordine su ciascun attributo può essere ascendente o discendente in base al qualificatore utilizzato, se omesso viene usato un ordinamento ascendente

**Esempio**:
```sql
SELECT *
FROM Automobile
ORDER BY Marca desc, Modello
```
### Operatori Aggregati
Nell'algebra relazionale ogni condizione viene valutata su una signola tupla alla volta in modo indipendente, SQL invece permette di valutare delle proprietà che dipendono da un insieme di tuple.
SQL mette a disposizione 5 operatori:
$$\text{COUNT,SUM,MAX,MIN,AVG}$$
Gli operatori aggregati vengono gestiti come un'estensione delle normali interrogazioni, considerando prima le parti $\text{FROM}$ e $\text{WHERE}$, per poi applicare l'operatore alla tabella contenente il risultato dell'interrogazione.

L'operatore $\text{COUNT}$ usa la seguente sintassi:
$$\begin{aligned}
&\text{count} (\langle *|[\text{distinct}|\text{|all}] \ ListaAttributi)
\end{aligned}
$$
La prima opzione (\*) restituisce il numero di righe, $\text{distinct}$ restituisce il numero di diversi valori degli attributi in $ListaAttributi$ (elimina i duplicati), $\text{all}$ restituisce il numero di righe che possiedono valori diversi dal valore nullo in $ListaAttributi$, nel caso di omissione di qualunque opzione quest'ultima è quella di default.
**Esempio:**
```sql
SELECT COUNT(DISTINCT Stipendio)
FROM Impiegato
```

Gli altri 4 operatori operatori aggregati invece ammettono come argomento un attributo o un'espressione, eventualmente preceduta dalle parole chiave $\text{distinct}$ e $\text{all}$.
Le funzioni aggregate $\text{SUM}$ e $\text{AVG}$ ammettono come argomento solo espressioni che rappresentano valori numerici o valori di tempo.
Le funzioni $\text{MAX}$ e $\text{MIN}$ richiedono solo sull'espressione sia definito un ordinamento per cui si possano applicare anche su stringhe di caratteri o su istanti di tempo
La sintassi è la seguente:
$$\langle \text{sum | max | min | avg} \ ([\text{distinct| all}]AttrEspr)$$
Gli operatori si applicano sulle righe che soddisfano la condizione presente nella clausola $\text{WHERE}$.
### Interrogazioni con raggruppamento
SQL mette a disposizione la clausola $\text{GROUP BY}$ che permette di specificare come dividere le tabelle in sottoinsiemi.
La clausola ammette come argomento un insieme di attributi e l'interrogazione raggrupperà le righe che possiedono gli stessi valori per questo insieme di attributi.
**Esempio**:
```sql
SELECT Dipart, SUM(Stipendio)
FROM (Impiegato)
GROUP BY Dipart
```

L'interrogazione viene eseguita prima come se la clausola $\text{GROUP BY}$ non esistesse, selezionando gli attributi che compaiono nella clausola o all'interno dell'espressione argomento dell'operatore aggregato;
La tabella ottenuta poi viene analizzata, dividendo le righe in insiemi caratterizzati dallo stesso valore degli attributi che compaiono come argomento della clausola.
Dopo che le righe son state raggruppate in sottoinsiemi, l'operatore aggregato viene applicato separatamente su ogni sottoinsieme;
Il risultato dell'interrogazione è costituito da una tabella con righe che contengono l'esito della valutazione dell'operatore aggregato affiancato al valore dell'attributo che è stato usato per l'aggregazione

La sintassi SQL prevede che la clausola $\text{GROUP BY}$ in un interrogazione possa comparire come argomento di la clausola $\text{SELECT}$ soltanto se viene utilizzato lo stesso sottoinsieme degli attributi
#### Predicati sui gruppi
Si potrebbe voler prendere in considerazione solo i sottoinsiemi che soddisfano determinate condizioni, nel caso questo sia verificabile per singole righe allora basta porre gli opportuni predicati come argomento della clausola $\text{WHERE}$, altrimenti sarà necessario usare il costrutto $\text{HAVING}$:
La clausola $\text{HAVING}$ descrive le condizioni che si devono applicare al termine dell'esecuzione di un'interrogazione che fa uso della clausola $\text{GROUP BY}$, e ogni sottoinsieme di righe costruito dalla $\text{GROUP BY}$ fa parte del risultato dell'interrogazione solo se il predicato argomento di $\text{HAVING}$ risulta soddisfatto
**Esempio**:
```sql
SELECT Dipart, SUM(Stipendio) AS SommaStipendi
FROM Impiegato
GROUP BY Dipart
HAVING SUM(Stipendio)>100
```

La sintassi permette anche la definizione che presentano la clausola $\text{HAVING}$ senza una corrispondente clausola $\text{GROUP BY}$, in questo caso l'intero insieme di righe è trattato come unico raggruppamento, ma si ha un campo limitato di applicabilità, poiché se la condizione non vien soddisfatta il risultato sarà vuoto.
Questa clausola permette l'utilizzo di espressioni booleane su predicati semplici (cioè i confronti tra il risultato della valutazione di un operatore aggregato e una generica espressione).
Sintatticamente nella clausola è ammessa anche la presenza diretta degli attributi argomento di $\text{GROUP BY}$, ma è preferibile raccogliere tutte le condizioni su questi attributi nell'ambito della clausola $\text{WHERE}$.

Per sapere quali predicati di un interrogazione che fa uso di raggruppamento vanno dati come argomento a $\text{WHERE}$ rispetto ad $\text{HAVING}$ basta rispettare il seguente criterio
### Interrogazioni di tipo insiemistico
[da completare]
### Interrogazioni nidificate
[da completare]
#### Interrogazioni nidificate complesse
[da completare]
## Modifica dei dati in SQL
[da completare]
### Inserimento
[da completare]
### Cancellazione
[da completare]
### Modifica
[da completare]