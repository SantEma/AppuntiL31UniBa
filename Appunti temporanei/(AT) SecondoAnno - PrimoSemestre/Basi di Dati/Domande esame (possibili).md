#### Si descriva brevemente cosa siano indipendenza logica e fisica dai dati e quale/i modello di basi di dati consente/ono di realizzarle

L'architettura a livelli di un DBMS garantisce l'indipendenza dei dati in due livelli:
- **Indipendenza fisica**: consente di interagire con il DBMS in modo indipendente dalla struttura fisica dei dati, senza influire sulle descrizioni e quindi sui programmi che usano i dati
- **Indipendenza logica**: consente di interagire con il livello esterno della base di dati in modo indipendente dal livello logico, per esempio come aggiungere un nuovo schema esterno senza modificare lo schema logico e perciò la sottostante organizzazione fisica dei dati

Il **modello relazionale** risponde perfettamente al requisito dell'indipendenza dei dati perché introduce una netta distinzione tra il livello fisico e quello logico. A differenza dei modelli precedenti (reticolare e gerarchico), che includevano riferimenti espliciti a puntatori fisici, il modello relazionale è basato su valori e non richiede la conoscenza delle strutture fisiche per accedere ai **dati**
#### "Descrivere brevemente cosa sono le ACID properties, a cosa servono e da dove deriva l'espressione 'ACID'" oppure "Illustrare brevemente cosa siano le proprietà ACID (spiegando anche l'acronimo) ed a quale entità/oggeto sono riferite"

Un DBMS prevede che le interazioni con la base di dati avvengano per mezzo di **transazioni**.
Una **transazione** è una sequenza di azioni di lettura e scrittura del DB e di elaborazioni di dati in memoria temporanea, che il DBMS esegue garantendo le seguenti proprietà (ACID properties):
- **Atomicity**: La transazione è eseguita nella sua interezza oppure non è eseguita affatto (le transazioni che terminano prematuramente sono abortite)
- **Consistency preservation**: una esecuzione corretta della transazione porta il DB da uno stato consistente all'altro (i vincoli di integrità devono essere rispettati).
- **Isolation**: L'esecuzione di una transazione deve essere indipendente da quella di altre transazioni concorrenti. Il risultato deve essere analogo a quello che si otterrebbe eseguendo le transazioni una alla volta (serialmente).
- **Durability** : le modifiche su DB di una transazione terminata normalmente sono permanenti, cioè non sono alterabili da malfunzionamenti successivi alla terminazione
#### Si definisca brevemente cosa siano i trigger, il paradigma su cui sono basati, vantaggi e svantaggi per essi
I trigger, detti anche regole attive, sono dei costrutti per rendere la base di dati in grado di reagire a eventi definiti dall'amministratore tramite l'esecuzione di opportune azioni. Seguono il paradigma Evento-Condizione-Azione (ECA), ossia:
- **Evento**: Tipicamente una modifica dello stato del database consiste in una operazione di $\text{INSERT}$, $\text{DELETE}$, $\text{UPDATE}$. Se l’evento accade, il trigger si attiva.
- **Condizione**: Predicato booleano espresso in SQL che identifica se l’azione del trigger deve essere eseguita. Quando il trigger si attiva, viene valutata la condizione, quindi il trigger viene considerato.
- **Azione**: Consiste in una sequenza di update SQL o una procedura. Quando la condizione è verificata, allora l’azione viene eseguita, quindi il trigger viene eseguito.

I trigger sono uno strumento molto potente che permette di gestire vincoli di integrità, calcolare dati derivati, gestire eccezioni e codificare regole aziendali. Un ulteriore vantaggio derivante dall'utilizzo dei trigger consiste nel riuscire a codificare la logica del sistema in maniera centralizzata e condivisa da tutte le applicazioni, con conseguenti vantaggi in fase di lettura e manutenzione del codice, infatti in caso di modifiche al comportamento del sistema è sufficiente intervenire nell'ambito della definizione dei trigger e non in più parti del codice. 
Lo svantaggio è che i trigger sono standardizzati solo per SQL-3, per cui potrebbero presentarsi casi (se pur sempre più rari) di non portabilità del codice.
#### Illustrare brevemente cosa sia il conflitto di impedenza e quali soluzioni esistano per gestirlo
Un importante problema che caratterizza l’integrazione tra SQL e i normali linguaggi di programmazione è il cosiddetto **conflitto di impedenza**. I linguaggi di programmazione accedono agli elementi di una tabella scandendone le righe una a una (tuple-oriented). Al contrario SQL è un linguaggio di tipo set-oriented, che opera su intere tabelle e restituisce come risultato di un’interrogazione un’intera tabella. Le soluzioni a questo problema si ottengono con l’utilizzo dei **cursori** e l’utilizzo di linguaggi con costruttori di tipo in grado di gestire una struttura del tipo “insieme di righe” (Call Level Interface).
#### Si definiscano brevemente le nozioni di sistema organizzativo, sistema informativo e sistema informatico e si delinei la differenza esistente tra di essi

Un **sistema organizzativo** è un insieme di risorse e regole che consentono il funzionamento di una qualunque struttura sociale per il raggiungimento dei suoi obbiettivi

Ogni sistema organizzativo è dotato di **sistema informativo**, ossia l'insieme delle risorse e delle procedure che un'organizzazione utilizza per gestire le informazioni necessarie al perseguimento dei propri scopi.
Un sistema informativo è molte volte indipendente dalle automatizzazioni che conosciamo.

Un **sistema informatico** non è altro che la parte automatizzata di un sistema informativo, quindi un sistema software orientato alla gestione dei dati, dove l’aspetto prevalente è rappresentato dai dati stessi (memorizzati, ricercati, modificati) che costituiscono il patrimonio informativo di un’**organizzazione**

#### Si descriva brevemente i/il criteri/o di ottimizzazione delle query implementato dal gestore di ottimizzazione delle query di un DBMS
Il gestore delle interrogazioni è un modulo cruciale dell’architettura di un DBMS, in quanto responsabile dell’esecuzione efficiente di operazioni che sono specificate a livello molto alto. Esso riceve in ingresso un’interrogazione scritta in SQL, controlla che non vi siano errori lessicali, sintattici o semantici, una volta accettata, l’interrogazione viene tradotta in una forma interna di tipo algebrico. A questo punto, l’ottimizzazione vera e propria ha inizio, dividendosi in: 
1.  **Ottimizzazione algebrica**: effettua trasformazioni sulle operazioni (come l'anticipazione di selezioni e proiezioni verso le foglie dell'albero) che sono sempre convenienti indipendentemente dai costi fisici
2. **Ottimizzazione basata sul modello dei costi**: Sceglie la strategia di esecuzione (es. quale indice usare, quale algoritmo di join tra nested loop, merge scan o hash join) basandosi su un modello di costo che stima il numero di accessi in memoria secondaria e l'uso della CPU, sfruttando i profili statistici delle tabelle memorizzati nel catalogo (come spiegato nei profili delle relazioni);
3. Generazione del codice.

#### Elencare ed illustrare brevemente le strutture usate da un DBMS per organizzare i file (e le stutture primarie dei file) a livello fisico
## Strutture primarie per l'organizzazione di file
La struttura primaria di un file stabilisce il criterio secondo il quale sono disposte le tuple nell'ambito del file. Le strutture possono essere divise in tre categorie principali: 
- Sequenziali;
- Ad accesso calcolato (hash);  
- Ad albero;

Nelle strutture sequenziali, un file è costituito da vari blocchi di memoria “logicamente” consecutivi, e le tuple vengono inserite nei blocchi rispettando una sequenza:
- **Seriale**: sequenza delle tuple indotta dall'ordine di immissione (organizzazione disordinata).
  Di solito viene chiamata anche **heap**, ossia mucchio
- **Array**: le tuple sono disposte come in un array, e la loro posizione dipende dal valore assunto in ciascuna tupla da un campo di indice.
  Possibile soltanto quando le tuple di una tabella sono di dimensione fissa.
- **Ordinata**: la sequenza delle tuple dipende dal valore assunto in ciascuna tupla da un campo (attributo) del file, ossia la chiave.

Una struttura con accesso ad **hash** garantisce un accesso associativo ai dati, ovvero un tipo di accesso in cui la locazione fisica dei dati dipende dal valore assunto da un campo chiave. Questo avviene tramite specifiche funzioni hash che consentono di trasformare un attributo chiave nell'indice di un array, e quindi associare ad ogni record una posizione specifica in una struttura sequenziale. 

Le strutture ad albero, denominate anche **indici**, favoriscono l'accesso in base al valore di uno o più campi, consentendo sia accessi puntuali che corrispondenti a valori con complessità logaritmica (sulla base della profondità dell’albero).
#### Definire cosa siano RDF ed RDFS e le differenze tra di essi" oppure "Descrivere brevemente RDF e quale sia il suo linguaggio di interrogazione
**RDF (Resource Description Framework):** È un modello per rappresentare informazioni sul Web sotto forma di **triple** (soggetto, predicato, oggetto), che formano un grafo orientato. Permette di descrivere risorse in modo semplice ma non impone uno schema rigido

**RDFS (RDF Schema):** È un'estensione di RDF che permette di definire **metadati** (uno schema). Introduce concetti come **classi**, **proprietà** e relazioni di ereditarietà (_subClassOf_), consentendo di descrivere la struttura e derivare nuova conoscenza (inferenza). Il linguaggio di interrogazione standard per RDF è **SPARQL**
#### Illustrare brevemente cosa siano B-tree e B+-Tree e le differenze tra essi" oppure "Illustrare brevemente cosa sia un B+-Tree..." o "Determinare quali azioni possono essere necessarie su una struttura dati di tipo B-Tree.