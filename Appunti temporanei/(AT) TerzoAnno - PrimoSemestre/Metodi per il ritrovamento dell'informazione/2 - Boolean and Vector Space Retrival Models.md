# Retrivial Models
Un modello di recupero (chiamiato **Retrivial model**) è un modello che definisce come un sistema di Information Retrieval decide quali documenti sono rilevanti per una query.
Ogni modello specifica:
- La rappresentazione del documento, ossia come viene descritto un documento (ad esempio come insieme di parole, oppure come vettore di pesi dei termini
- La rappresentazione della query, ossia come viene espressa la richiesta dell'utente
- Le funzioni di "retrivial"
## Modello per Information Retrival
Un modello IR è una quadrupla $[D,Q,F,R(q_{i}, d_{j})]$ composta da:
- Un insieme di documenti, indicato con $D$
- Un insieme di query, espresse con $Q$
- Un framework per modellarli i due precedenti, indicato con $F$
- Una funzione di ritrovamento che restituisce un punteggio di rilevanza, calcolati prendendo in considerazione sia l'insieme dei documenti e query, indicato con $R(q_{i}, d_{j})$

Questo punteggio di rilevanza viene assortito generalmente in maniera decrescente 
### Tassonomia di modelli IR
Accanto ai modelli puramente testuali, esistono modelli che integrano l'analisi della struttura dei collegamenti (**link analysis**), essenziali per il Web Retrieval, in questo scenario le pagine web formano un grafo connesso da collegamenti ipertestuali e il ranking calcola l'autorevolezza e l'importanza relativa delle pagine connesse.

I modelli di reperimento differiscono per architettura e algoritmi, ma condividono tutti il medesimo ciclo di vita: una fase preliminare di elaborazione e indicizzazione della collezione, seguita dalla fase di ricerca e ranking a fronte della query dell'utente
![[Pasted image 20261005161158.png]]
### Step di pre-processing
Prima di indicizzare i documenti (o in generale dati di tipo testuale), il testo viene "ripulito" e normalizzato seguendo una metodologia precisa:
- Si eliminano caratteri indesiderati e markup (come tag HTML, punteggiatura, numeri, etc.)
- Il testo ottenuto viene viene spezzato in token, usando gli spazi come separatori
- Si effettua lo **stemming**, ossia i token vengono ridotti alla loro "radice "(ad esempio $\text{computational}\to \text{compute}$). Questo passaggio può introdurre diversi errori come perdita di contesto, errori di punteggiatura, perdita del significato della radice o parole che vanno in conflitto con la radice stessa (ad esempio $\text{policies e police} → \text{polic}$)
- Si tolgono le parole molto comuni e poco informative (chiamate **stepword**)
- Si rilevano delle frasi comuni, eventualmente con un **dizionario specifico del dominio**
- Si costruisce un **indice invertito**, dove ogni parola chiave viene associata alla lista dei documenti che la contengono
### Modello booleano
Nel modello booleano, un documento è rappresentato come un **insieme di parole chiave**.
Le sue **query** sono espresse tramite i vari connettori logici che si conoscono della teoria degli insiemi, quindi intersezione (AND), unione (OR), complementare (NOT), incluse le parentesi per indicare l'ambito.
L'**output** che questo modello produce è binario, ossia il documento è rilevante oppure no, non includendo quindi match parziali o ranking (frequenza di quel termine).
#### Matrice di incidenza
> [!example] Esempio di matrice di incidenza
> ![[Pasted image 20261003190722.png]]
> In questo esempio si costruisce una **matrice termine-documento** in cui ogni cella vale 1 se l'opera contiene la parola, altrimenti vale 0 . 
> Ogni termine ha così un **vettore di incidenza** 0/1.
> Con questa rappresentazione si perde comunque il numero di occorrenze all'interno dei documenti e l'ordine delle parole (posizioni)

Nelle collezioni reali di grandi dimensioni la matrice è estremamente **sparsa** (la quasi totalità delle celle ha valore 0, poiché nessun documento contiene più di una minima frazione dell'intero vocabolario). Memorizzare tutti gli zeri comporterebbe un enorme spreco di spazio.
#### Indice inverso
L'indice è invertito memorizza **esclusivamente le presenze effettive**, associando a ciascun termine solo l'elenco dei documenti in cui esso compare.

Si compone di due elementi:
1. **Vocabolario / Dictionary**: l'elenco ordinato di tutti i termini unici estratti dalla collezione.
2. **Posting List**: per ciascun termine, la lista ordinata dei documenti identificati tramite `docID` in cui il termine compare. Ciascun elemento della lista è detto **posting**.
![[Pasted image 20261005163721.png]]

Le interrogazioni booleane si risolvono eseguendo operazioni insiemistiche direttamente sulle posting list. Quando nuovi documenti vengono aggiunti o modificati, non è necessario riprocessare l'intero corpus, ma è sufficiente aggiornare le posting list corrispondenti ai documenti coinvolti.
##### Costruzione dell'indice inverso
I documenti vengono scansionati uno alla volta e tokenizzati. Per ciascun token viene generata una coppia formata dal termine normalizzato e dal `docID` del documento in cui si trova. ![[Pasted image 20261005170111.png]]

Successivamente, la lista globale di tutte le coppie estratte viene **ordinata alfabeticamente per termine** e, a parità di termine, in ordine crescente di `docID`. ![[Pasted image 20261005170446.png]]

I termini duplicati vengono raggruppati: per ciascun termine unico del vocabolario viene generata la corrispondente **posting list** con i relativi `docID`, memorizzando anche la **document frequency** (df), ovvero la cardinalità della lista (il numero totale di documenti che contengono quel termine). Vengono salvati in **Lemma**, poiché molte parole possono essere singolari-plurali / maschile-femminile.
![[Pasted image 20261005170510.png]]

##### Step dell'indexer
[da finire]
#### Match esatto
Il modello booleano è un modello che funziona secondo diverse funzioni
[riguardare questa parte]
#### Problemi del modello booleano
L'estrema semplicità di questo modello porta dei problemi:
- **È rigido**: l'operazione potrebbe avere pochi o troppi risultati
- È difficile utilizzare gli operatori booleani per esprimere richieste complesse
- È difficile il controllo del numero dei documenti [da rivedere]
- [da rivedere]
- [da rivedere]

## Pre-processing steps
Durante la fase di **pre-processing** non esistono regole universali prefissate: spetta infatti al progettista definire strategie consapevoli in base allo scopo dell'applicazione e alla natura dei dati da trattare, gestendo con attenzione i diversi casi limite.

Una delle prime decisioni riguarda la definizione stessa dell'unità di documento (**document unit**). Nel caso emblematico di un'email con allegati, ad esempio, bisogna scegliere se indicizzare l'intero messaggio come un unico blocco oppure trattare il corpo del testo e i vari allegati come documenti distinti. 
La questione si complica ulteriormente in presenza di **collezioni multi-lingua**, dove il messaggio principale potrebbe essere redatto in una lingua e l'allegato in un'altra.

Per rilevare automaticamente l'**idioma** di un testo, una tecnica **euristica** diffusa consiste nell'analizzare la **frequenza degli articoli**.
Poiché ogni lingua possiede il proprio insieme caratteristico di articoli, il sistema può dedurre la lingua prevalente esaminandone la presenza. Trattandosi tuttavia di una semplice euristica, **non garantisce un'accuratezza assoluta** e può fallire facilmente in presenza di testi brevi, poco strutturati o eterogenei.
### Token
Un'analoga discrezionalità si riscontra nella fase di **tokenizzazione**, in cui il flusso di caratteri viene suddiviso in unità elementari. 
Anche in questo passaggio emergono ambiguità legate ai **separatori**, come apostrofi e trattini: davanti a forme come l'albero o state-of-the-art, è il progettista a dover stabilire se scartare i simboli come semplice punteggiatura, mantenere i termini uniti in una sola parola oppure spezzarli in token separati.

Infine, non tutti i token individuati entrano a far parte dell'indice, poiché rappresentano solo candidati provvisori.
Il primo filtro consiste nell'eliminazione delle **stopword**, ovvero le parole puramente grammaticali e di congiunzione che non apportano valore informativo utile alla ricerca. Successivamente, i token selezionati vengono ricondotti al loro **lemma**, ossia la forma base di dizionario, permettendo di non disperdere le varianti flesse di una parola tra singolari, plurali, maschili e femminili: invece di registrare fino a quattro voci distinte che occuperebbero spazio inutile nell'indice invertito, si conserva un unico token di riferimento.
In questo modo si risolvono anche i problemi di disallineamento durante l'interrogazione, consentendo al motore di recuperare i documenti rilevanti anche quando l'utente cerca un termine in una forma grammaticale diversa rispetto a quella presente nel testo originale.

Il token quindi è una **sequenza di caratteri** che possono essere analizzati nell'indice invertito che compone una parte significativa nel documento che merita considerazione.
Molti motori di ricerca permettono di sbagliare alcuni caratteri di un token, poiché grazie alla **distanza di Levenstain** (la distanza possibile tra una parola e un altra in base al cambiamento dei caratteri), cerco le **chiavi di ricerca** più simili a quella parola ,con la stessa distanza di Levenstain calcolata, memorizzate nell'indice e si trova comunque una corrispondenza e quindi viene suggerita la parola corretta.
### Numeri
Un'ulteriore criticità nella fase di pre-processing riguarda il trattamento delle stringhe contenenti entità numeriche, in particolare le **date**. 
Se il sistema tratta i numeri semplicemente come token slegati tra loro, perde del tutto il valore informativo del dato temporale. A ciò si aggiunge l'ambiguità dei formati: memorizzare una data nella forma generica `n1/n2/n3` crea disallineamenti, poiché `n1` può rappresentare il **giorno** nello standard europeo o il **mese** in quello anglosassone. 

I motori di ricerca più moderni e definiti **intelligenti**, superano questo limite interpretando il contesto circostante, riuscendo a riconoscere e normalizzare la data corretta anche quando viene formulata in formati particolari o notazioni storiche (come ad esempio *55 B.C.*).

Una problematica del tutto analoga si riscontra con i **numeri di telefono** e la gestione dei prefissi. Come ad esempio `+39 333...`, `(080) 23343` o `(080)23-323`.
Se l'algoritmo si limitasse a trattare i separatori come normale punteggiatura da eliminare o se frammentasse i numeri in elementi distinti, diventerebbe impossibile far corrispondere la query al documento corretto. Anche in questo caso è compito del progettista introdurre procedure di normalizzazione specifiche che convertano queste sequenze in un formato standard univoco prima di registrarle nell'indice.

[da finire]