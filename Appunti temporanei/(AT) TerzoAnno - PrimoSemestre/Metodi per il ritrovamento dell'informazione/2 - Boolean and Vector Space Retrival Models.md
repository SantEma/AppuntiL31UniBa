# Retrivial Models
Un **retrivial model** è un modello che definisce come un sistema di Information Retrieval decide quali documenti sono rilevanti per una query.
Ogni modello specifica:
- La rappresentazione del documento, ossia come viene descritto un documento (ad esempio come insieme di parole, oppure come vettore di pesi dei termini
- La rappresentazione della query, ossia come viene espressa la richiesta dell'utente
- Le funzioni di "retrivial"
## Modello per Information Retrival formale
Un modello IR è una quadrupla $[D,Q,F,R(q_{i}, d_{j})]$ composta da:
- Un insieme di documenti, indicato con $D$
- Un insieme di query, espresse con $Q$
- Un framework per modellarli i due precedenti, indicato con $F$
- Una funzione di ritrovamento che restituisce un punteggio di rilevanza, calcolati prendendo in considerazione sia l'insieme dei documenti e query, indicato con $R(q_{i}, d_{j})$

Questo punteggio di rilevanza viene assortito generalmente in maniera decrescente 

I modelli classici di ritrovamento dell’informazione considerano ogni documento
come descritto da un insieme di keyword rappresentative chiamate **index terms**.

Un index term è semplicemente una parola (documento) che aiuta semanticamente a ricordare il tema principale del documento.
### Tassonomia di modelli IR
Accanto ai modelli puramente testuali, esistono modelli che integrano l'analisi della struttura dei collegamenti (**link analysis**), essenziali per il Web Retrieval, in questo scenario le pagine Web formano un grafo connesso da collegamenti ipertestuali e il ranking calcola l'autorevolezza e l'importanza relativa delle pagine connesse.

I modelli di reperimento differiscono per architettura e algoritmi, ma condividono tutti il medesimo ciclo di vita: una fase preliminare di elaborazione e indicizzazione della collezione, seguita dalla fase di ricerca e ranking a fronte della query dell'utente
![[Pasted image 20261005161158.png]]

### Modello booleano
Nel modello booleano, un documento è rappresentato come un **insieme di parole chiave**.
Le sue **query** sono espresse tramite i vari connettori logici che si conoscono della teoria degli insiemi, quindi intersezione (AND), unione (OR), complementare (NOT), incluse le parentesi per indicare l'ambito.
L'**output** che questo modello produce è un peso binario, ossia se il documento è rilevante oppure no, non includendo quindi match parziali o ranking (frequenza di quel termine). Questo output viene indicato con $w_{i,j} \in \{0,1\}$.
#### Matrice di incidenza
> [!example] Esempio di matrice di incidenza
> ![[Pasted image 20261003190722.png]]
> In questo esempio si costruisce una **matrice termine-documento** in cui ogni cella vale 1 se l'opera contiene la parola, altrimenti vale 0 . 
> Ogni termine ha così un **vettore di incidenza** 0/1.
> Con questa rappresentazione si perde comunque il numero di occorrenze all'interno dei documenti e l'ordine delle parole (posizioni)

I principali vantaggi di questo modello sono il formalismo dell’algebra booleana e la sua semplicità.
Nelle collezioni reali di grandi dimensioni la matrice è estremamente **sparsa** (la quasi totalità delle celle ha valore 0, poiché nessun documento contiene più di una minima frazione dell'intero vocabolario). memorizzandoli si ha un enorme spreco di spazio.
#### Efficienza delle query
In un motore di ricerca, le query troppo lunghe o cariche di termini **non sono efficienti**. Una ricerca con troppe parole complica le operazioni di intersezione, allunga i tempi di risposta e spesso **peggiora** la qualità del risultato finale anziché migliorarlo.

Nel modello di reperimento booleano la sequenza operativa è quindi:
1. **Processare i documenti** (pulizia e normalizzazione iniziale del testo)
2. **Creare le posting list**
3. **Costruire l'indice invertito**
4. **Lavorare ed eseguire le query direttamente sull'indice**
#### Problemi del modello booleano
L'estrema semplicità di questo modello, però, porta dei problemi:
- **L'operatore di intersezione è fin troppo restrittivo**, basta che manchi una sola parola chiave per escludere un documento, con il rischio di restituire zero risultati.
- **È difficile utilizzare gli operatori booleani per esprimere richieste complesse**, infatti l'operatore di unione allarga eccessivamente la ricerca includendo qualsiasi documento contenga anche solo uno dei termini.
- **È difficile sapere con certezza che la query formulata dall'utente sia corretta sintatticamente**
- **I documenti restituiti dalle query non hanno un ordine di pertinenza**. Se l'utente vuole consultare i primi 10 risultati, il sistema non può indicare quali siano i migliori, poiché tutti i documenti estratti hanno esattamente lo stesso peso
- **Il modello fatica ad adattarsi al comportamento dell'utente**, non essendoci pesi o punteggi ma solo risposte **binarie** (vero/falso), il motore non può correggere o riordinare facilmente i risultati in base a quali documenti sono stati aperti o preferiti nelle ricerche precedenti.
### Indice inverso
L'indice  invertito memorizza **esclusivamente le presenze effettive**, associando a ciascun termine solo l'elenco dei documenti in cui esso compare.

Si compone di due elementi:
1. **Vocabolario / Dictionary**: l'elenco ordinato di tutti i termini unici estratti dalla collezione.
2. **Posting List**: per ciascun termine $t$, la lista ordinata dei documenti identificati tramite `docID` in cui il termine compare. Ciascun elemento della lista è detto **posting**.

> [!example] Esempio dei due elementi precedentemente citati
> ![[Pasted image 20261005163721.png]]
> Per inciso i termini sulla sinistra (Brutus, Caesar e Calpurnia) costituiscono il dizionario, mentre i vettori sulla sinistra costituiscono le posting lists corrispondenti.

Le interrogazioni booleane si risolvono eseguendo operazioni insiemistiche direttamente sulle posting list. Quando nuovi documenti vengono aggiunti o modificati, non è necessario riprocessare l'intero corpus, ma è sufficiente aggiornare le posting list corrispondenti ai documenti coinvolti.

#### Costruzione dell'indice inverso
La costruzione di un indice invertito prevede le seguenti fasi:
1. I documenti da indicizzare vengono tokenizzati;
2. Le stream di token vengono sottoposte a operazioni di modellazione sintattica (lemmatizzazione e stemming);
3. Ai token modiﬁcati vengono ora associate le posting list e quindi viene creato l’indice invertito.
##### Token stream

I documenti da indicizzare vengono scansionati uno alla volta e tokenizzati. Per ciascun token viene generata una coppia formata dal termine normalizzato e dal `docID` del documento in cui si trova. ![[Pasted image 20261005170111.png]]

Successivamente, la lista globale di tutte le coppie estratte viene **ordinata alfabeticamente per termine** e, a parità di termine, in ordine crescente di `docID`. ![[Pasted image 20261005170446.png]]

I termini duplicati vengono raggruppati: per ciascun termine unico del vocabolario viene generata la corrispondente **posting list** con i relativi `docID`, memorizzando anche la **document frequency** (df), ovvero la cardinalità della lista (il numero totale di documenti che contengono quel termine). Vengono salvati in **Lemma**, poiché molte parole possono essere singolari-plurali / maschile-femminile.
![[Pasted image 20261005170510.png]]
##### Vantaggi dell'ordinamento per la ricerca
- **Ricerca binaria nel vocabolario**: poiché i termini nel dizionario sono ordinati in modo **lessicografico**, la localizzazione di una parola non richiede una scansione lineare, ma può essere effettuata con una ricerca binaria o tramite alberi in tempo logaritmico.
- **Intersezione efficiente (Posting Merge)**: poiché le posting list sono mantenute rigorosamente ordinate per `docID` crescente, l'intersezione tra due liste per una query `AND` (aventi rispettivamente lunghezza $x$ e $y$) viene risolta tramite un algoritmo di scansione a due puntatori (merge) in tempo lineare, senza scorrere la collezione.
###### Algoritmo di merge delle posting list
![[Pasted image 20261006162409.png|354]]

L'algoritmo confronta due liste muovendo due puntatori, $p_1$ e $p_2$:
- **Corrispondenza**: se i due puntatori puntano allo stesso `docID`, abbiamo trovato una corrispondenza. L'ID viene salvato nella lista dei risultati (`answer`) e si fanno avanzare entrambi i puntatori per passare ai documenti successivi.
- **Differenza**: se i valori non coincidono, si fa avanzare il puntatore che ha il valore minore:
  - se $\text{docID}(p_1) < \text{docID}(p_2)$, si incrementa $p_1$;
  - se $\text{docID}(p_1) > \text{docID}(p_2)$, si incrementa $p_2$.

Possiamo riprendere l'esempio delle liste di Bruto e Cesare: se per Bruto il valore iniziale è $2$ e per Cesare è $1$, si incrementa il puntatore di Cesare ($p_2$) perché è più piccolo, allineandolo così al valore successivo. 
Il ciclo continua finché uno dei due puntatori arriva alla fine della propria lista (`null`): a quel punto l'algoritmo si ferma, poiché è impossibile trovare altri documenti in comune.
## Pre-processing steps
### Step generali
Prima di indicizzare i documenti (o in generale dati di tipo testuale), il testo viene "ripulito" e normalizzato seguendo una metodologia precisa:
- Si eliminano caratteri indesiderati e markup (come tag HTML, punteggiatura, numeri, etc.)
- Il testo ottenuto viene viene spezzato in token, usando gli spazi come separatori
- Si effettua lo **stemming**, ossia i token vengono ridotti alla loro "radice "(ad esempio $\text{computational}\to \text{compute}$). Questo passaggio può introdurre diversi errori come perdita di contesto, errori di punteggiatura, perdita del significato della radice o parole che vanno in conflitto con la radice stessa (ad esempio $\text{policies e police} → \text{polic}$)
- Si tolgono le parole molto comuni e poco informative (chiamate **stepword**)
- Si rilevano delle frasi comuni, eventualmente con un **dizionario specifico del dominio**
- Si costruisce un **indice invertito**, dove ogni parola chiave viene associata alla lista dei documenti che la contengono
Durante la fase di **pre-processing** non esistono regole universali prefissate: spetta infatti al progettista definire strategie consapevoli in base allo scopo dell'applicazione e alla natura dei dati da trattare, gestendo con attenzione i diversi casi limite.

Una delle prime decisioni riguarda la definizione stessa dell'unità di documento (**document unit**). 

La prima fase durante il processing di un documento è il **parsing** dello stesso, ma è una fase con tante difficoltà, infatti la questione si complica ulteriormente in presenza di **collezioni multi-lingua**, dove il messaggio principale potrebbe essere redatto in una lingua e l'allegato in un'altra.
Inoltre, in lingue che hanno ideogrammi (o comunque alfabeti diverso a quello latino) bisogna processarli in un altra maniera.
> [!example] Esempio di un e-mail
>Nel caso emblematico di un'email con allegati, ad esempio, bisogna scegliere se indicizzare l'intero messaggio come un unico blocco oppure trattare il corpo del testo e i vari allegati come documenti distinti. 
#### Tokenizzazione
Nella fase di **tokenizzazione**, il flusso di caratteri viene suddiviso in unità elementari (token).
Il token è una **sequenza di caratteri** che possono essere analizzati nell'indice invertito e compone una parte significativa nel documento che merita considerazione.
Molti motori di ricerca permettono di sbagliare alcuni caratteri di un token, poiché grazie alla **distanza di Levenstain** (la distanza possibile tra una parola e un altra in base al cambiamento dei caratteri), si va a ricercare le **chiavi di ricerca** più simili a quella parola ,con la stessa distanza di Levenstain calcolata, memorizzate nell'indice, trovando comunque una corrispondenza e suggerendo la parola corretta.

Anche in questo passaggio emergono ambiguità legate ai **separatori**, come apostrofi e trattini: davanti a forme come `"state-of-the-art"`, è il progettista a dover stabilire se scartare i simboli come semplice punteggiatura, mantenere i termini uniti in una sola parola oppure spezzarli in token separati.

Infine, non tutti i token individuati entrano a far parte dell'indice, poiché rappresentano solo candidati provvisori.
##### Stop words
Il primo filtro consiste nell'eliminazione delle **stopword**, ovvero le parole puramente grammaticali e di congiunzione che non apportano valore informativo utile alla ricerca. 
Nei testi esistono parole che sono quasi sempre utilizzate nei testi (articoli, congiunzioni etc.), togliere tutte le piccolezze semantiche dal dizionario rende il contenuto molto più facile da scorrere nell'indice, ma questa "compressione" in diversi casi presenta dei problemi:
- In un contenuto che magari ha molte stop-words porta a perdere il contesto
- In domande più complesse (dove va questo volo?) si perde il contesto generale
#### Normalizzazione dei termini
Alcune parole possono avere lo stesso significato ma  diversi modi di essere espressa.
Per questa ragione, si **normalizzano** le parole, ossia si crea una classe di equivalenze di termini (nell'indice quindi cercare una parole in una maniera o in un altra diventa indifferente, il sistema riconosce lo stesso).

> [!example] Esempio di classe di equivalenza
> ![[Pasted image 20261007164045.png]]
Successivamente si può pensare di trasformare tutte le parole in lower case
e di includere nella classe di equivalenza tutte le varianti più rilevanti presenti
nel dizionario della lingua considerata.

In una classe di equivalenza possono essere presente anche sinonimi, date etc.
#### Lemmatizzazione
I token selezionati vengono ricondotti al loro **lemma**, ossia la forma base di dizionario, permettendo di non disperdere le varianti flesse di una parola tra singolari, plurali, maschili e femminili: invece di registrare fino a quattro voci distinte che occuperebbero spazio inutile nell'indice invertito, si conserva un unico token di riferimento.
In questo modo si risolvono anche i problemi di disallineamento durante un interrogazione..
#### Stemming
Lo stemming, invece, consiste nel ridurre un termine alla sua radice prima di indicizzarlo semplicemente tagliandone il suﬃsso. Ad esempio “automate”, “automates”, “automatic”, “automation” diventano “automat”.