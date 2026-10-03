# La conoscenza
Per gestire la **conoscenza** bisogna saper:
- **Raccogliere** la conoscenza
- Organizzarla
- Distribuirla
- Renderla **accessibile** a chi necessita di quel tipo di informazione e nel momento e luogo in cui la richiede

Queste prassi vengono eseguite al fine di **risparmiare tempo**, riducendo i tempi di accesso all'informazione e **migliorando** la qualità dei servizi che la distribuiscono.
## Dati - Informazione - Conoscenza
La **conoscenza** è un capitale difficile da gestire, poiché è:
- volatile
- intangibile
- difficile da conservare

Per questo la maggior parte dei database nel mondo possiede i dati in forma **non strutturata**.

L'obiettivo dell'**AKM** (Automatic Knowledge Management) è quello di costruire sistemi in grado di processare documenti in formato di **linguaggio naturale**.
### Acquisizione
L'**acquisizione** è il ritrovamento di conoscenza partendo da un database in forma testuale.
Per passare dal testo grezzo alla conoscenza formale (processo conosciuto come **Ontology Construction**) si utilizzano diverse tecniche come:
- Document Classification (classificazione dei documenti)
- Information Extraction (estrazione dell'informazione)
- Text Mining
### Paradigma Search vs Discover
![[Pasted image 20261003165350.png]]

Per comprendere dove collocare le varie tecnologie di recupero e analisi della conoscenza, vengono posti due **assi principali**:
- **La struttura del dato** (righe), ovvero se sono dati **strutturati** o **testuali**.
- **L'intento dell'utente** (colonne), ovvero se sono specifici ad un obiettivo (**Search**) o orientati alla scoperta di altre correlazioni (**Discover**).

Dall'incrocio di questi due assi, nascono quattro **paradigmi**:
- **Data Retrieval**:
  Questo paradigma si concentra sul trovare una corrispondenza di dati per risolvere un unico **obiettivo** mirato. Per questo il database è **strutturato** e l'utente troverà questa informazione tramite un **linguaggio formale** (come visto con SQL). Un esempio di questo paradigma è la ricerca di un ristorante in una città, dove sarà effettuata la sua query specifica. L'entità manipolata è il **record**.
  
- **Information Retrieval**:
  In questo caso l'utente deve cercare **documenti** in un DB **non strutturato**, quindi privo di schema relazionale, rendendo quindi l'SQL non praticabile come soluzione.
  Seguendo l'esempio precedente, si ottiene la medesima informazione tramite **navigazione per categorie** (prendendo ad esempio un ristorante giapponese a Boston, `Boston -> Restaurant -> Japanese`), ad albero o per **parole chiave**. L'entità manipolata è il **documento**.

- **Data Mining**:
  Nel Data Mining non si cerca più un dato noto, ma si vuole scoprire in un database **strutturato** una nuova **conoscenza** analizzando moli di dati strutturati. L'entità manipolata sono i valori **numerici**.
  
- **Text Mining**:
  Nel Text Mining andremo ad eseguire la stessa ricerca effettuata nel Data Mining, solo che la conoscenza nuova da scoprire **non è nota a priori** e non cerca su pattern numerici, ma puramente **linguistici**, analizzando direttamente **testi liberi**. L'entità manipolata è il **concetto linguistico**.
### Processo di KDD
Il **Data Mining** in realtà è un sottoprocesso del grande processo di ricerca dai Databases, chiamato KDD (**Knowledge Discovery from Databases**).

>[!NOTES] Definizione di Fayyad et al., 1996
> Il KDD è il processo non banale di identificare pattern validi, nuovi, potenzialmente utili e comprensibili all'interno dei dati.

Le fasi del ciclo del processo KDD sono 5:
1. **Selezione**: raccolta dei dati di partenza e isolamento del sottoinsieme su cui opera.
2. **Pre-processing**: pulizia del dato eliminando file corrotti o sporchi.
3. **Trasformazione**: riduzione delle dimensioni dei dati e conversione in formati adatti all'algoritmo.
4. **Data Mining**: applicazione di **algoritmi per estrarre** e scoprire i pattern nascosti nei dati.
5. **Valutazione**: analisi critica dei pattern estratti dall'essere umano per tradurli in conoscenza utilizzabile, veritiera e concreta.

![[Pasted image 20261003170140.png]]

> [!example] Esempio di Data Mining
> ![[Pasted image 20261003170615.png]]
> Come si evince da questo esempio non c'è mai una garanzia del 100% [ma di cosa?] ma da piccole conoscenze già presenti possiamo notare dei pattern tra diversi dati, creando correlazioni utili per l'azienda.
### Dal Data Mining al Text Mining
Il **Text Mining** utilizza i principi del KDD sul testo a **linguaggio naturale**.
L'obiettivo è quello di scavare tra mille testi diversi fino alla ricerca di pattern, trend e associazioni nascoste, così da costruire nuova conoscenza.

>[!NOTES] Definizione Feldman e Dagan 1995
>Il **Text Data Mining** è l'estrazione non banale di informazioni implicite, precedentemente sconosciute e potenzialmente utili a partire da grandi quantità di dati testuali.

Espressa tramite la formula concettuale: $$\text{Text Mining}=\text{Data Mining (applicato al testo)}+\text{Linguistica di base}$$
#### Processo di Text Mining
![[Pasted image 20261003170819.png]]

L'architettura tipica di un sistema Text Mining, come si evince dall'immagine è basato su una **pipeline sequenziale**.
Partendo da un **testo grezzo** si va all'**analisi sintattica**, per poi andare nella fase di **Feature Generation** dove vengono create le **Bag of Words**.
Dopodiche si eseguono i **filtri statistici**, estraendo dei **pattern**, la loro estrazione avviene tramite tecniche di **classificazione** o di **clustering**. Dopo questa operazione vengono eseguiti i controlli dei pattern e per concludere vengono poi **valutati i risultati**.
### Text Mining nell'Impresa
In ambito aziendale è diventata una delle pratiche più importanti, utilizzata sopratutto per comprendere opinioni, reclami, feedback, etc. dei clienti in una società dove questi espongono le loro preferenze tramite **fonti testuali eterogenee** (email, ticket, siti web, social, comunicati stampa, etc.).

L'obiettivo chiave quindi del Text Mining aziendale è quello di poter analizzare documenti testuali in modo **rapido**, **raggruppandoli automaticamente** in base al loro contenuto, così da permetterne l'estrazione (come cercare le criticità maggiormente segnalate) con **velocità**.
#### Aree di ricerca correlate al Text Mining
Il Text Mining non è una disciplina isolata mirata solamente alla ricerca di dati da formati di testo generici, ma unisce diverse branche dell'informatica e dell'IA, come:
- **Information Retrieval (IR)**: per indicizzare collezioni di testi.
- **Text Categorization**: per l'assegnazione automatica di documenti a determinate classi tematiche.
- **Information Extraction (IE)**: per identificare entità e relazioni.
- **Natural Language Processing (NLP)**: per comprendere la sintassi, la grammatica e il significato del linguaggio naturale umano per introdurlo alla macchina.
- **Data Mining**: per la scoperta di ulteriori pattern.
## IR System 
![[Pasted image 20261003171918.png]]
Nei modelli classici di Information Retrieval, il processo di ricerca funziona esattamente come mostrato nello schema: l'utente invia una stringa di testo (la **query**), il sistema consulta la propria collezione di documenti e, calcolando un punteggio di pertinenza, restituisce una lista ordinata di documenti (**ranking**) dal più utile al meno utile.

Il punto centrale di tutto il sistema è comprendere cosa sia la **rilevanza**. A differenza di quanto si possa pensare, la rilevanza è un concetto prettamente **soggettivo**: non esiste una pertinenza assoluta, ma dipende sempre dal bisogno specifico di chi cerca, dal contesto e dal momento temporale (spesso serve un'informazione recente e aggiornata).
A rendere la rilevanza un problema davvero complesso è la natura stessa del linguaggio naturale. 


> [!example] Esempio di rilevanza di informazione
> Se ad esempio un utente cerca la parola `Mosca`, il motore di ricerca si trova davanti a un campo vastissimo di possibilità:
> - La capitale della Russia
> - L'insetto
> - Il cognome di una persona
> 
> Capire quale di questi risultati sia effettivamente rilevante per chi ha digitato la query è una delle sfide principali dell'IR.
### La ricerca per parole chiave e i suoi limiti
Il metodo più immediato per rappresentare i documenti e cercare al loro interno si basa sull'approccio **Bag of Words** (a "sacco di parole"). In questa modalità, la ricerca avviene tramite **parole chiave**, ossia si verifica semplicemente la presenza e la frequenza delle parole della query all'interno del documento, a prescindere dal loro ordine sintattico. 
Questo approccio è molto comodo e permette di trovare risposte anche se l'utente formula una frase sgrammaticata, anche se per query più articolate l'ordine dei termini farebbe la differenza.

Tuttavia, affidarsi alla pura presenza delle parole chiave porta con sé due grandi problemi linguistici:
1. **La polisemia (o ambiguità)**: accade quando una stessa parola possiede più significati diversi. È proprio il caso dell'esempio di `Mosca`, oppure della parola `Apple`, che può riferirsi all'azienda di informatica o al frutto. In questi casi il motore rischia di restituire documenti che contengono la parola giusta ma con il significato sbagliato.
2. **La sinonimia**: accade quando termini diversi indicano lo stesso concetto (ad esempio cercare `ristorante` quando un testo parla di `café` o `trattoria`, oppure cercare `Cina`quando nel documento è scritto `Repubblica Popolare Cinese`). Se il sistema cerca solo la parola esatta digitata, finirà per ignorare documenti perfettamente pertinenti solo perché usano un sinonimo.
## IR Intelligente
Per superare questi limiti, un motore di ricerca per definirsi "intelligente" deve andare oltre la semplice corrispondenza letterale dei termini: deve iniziare a considerare il significato semantico delle parole e tenere conto del loro ordine.
Un elemento chiave dell'IR intelligente è la capacità di adattarsi all'utente sfruttando il **Relevance Feedback** (feedback di rilevanza): il sistema raccoglie i segnali lasciati dall'utente durante le ricerche (quali link ha aperto, quali ha ignorato, su quali si è soffermato) e usa queste informazioni per correggere o arricchire la query, ricalcolando il ranking nelle ricerche successive per mettere in primo piano i risultati più graditi.
### Architettura di un sistema IR
![[Pasted image 20261003172510.png]]
Guardando lo schema dell'architettura completa, possiamo distinguere due grandi percorsi che si incontrano: da una parte la gestione dei documenti archiviati, dall'altra l'interazione con l'utente.

- **Text Operations**: prima di poter essere cercati, i testi devono essere processati (allo stesso modo dell'analisi di un compilatore su del codice). In questa fase il testo viene ripulito e ridotto ai minimi termini: si fa la **stopword removal** (eliminando parole grammaticalmente necessarie ma prive di valore informativo, come articoli e preposizioni) e lo **stemming** (riducendo le parole alla loro radice comune, rimuovendo prefissi e desinenze).
- **Indexing**: è il processo con cui si costruisce la struttura dati per le ricerche veloci, chiamata **Inverted Index** (indice invertito). Funziona in modo analogo all'indice analitico in fondo a un libro: invece di scorrere tutti i documenti da cima a fondo a ogni query, l'indice associa a ogni singola parola l'elenco di tutti i documenti in cui compare. In questo modo il motore cerca direttamente dentro l'indice (rendendo la ricerca molto più veloce per gli archivi enormi).
- **Searching e Ranking**: quando l'utente digita una query, il motore consulta l'indice invertito, recupera i documenti candidati che contengono quei termini e infine applica gli algoritmi di **ranking**, assegnando un punteggio a ciascuno e ordinandoli prima di mostrarli nell'interfaccia finale.

Per rendere un sistema di Information Retrieval realmente capace di comprendere i testi e le intenzioni dell'utente, la ricerca fa affidamento su due grandi discipline dell'Intelligenza Artificiale: il **Natural Language Processing (NLP)** e il **Machine Learning (ML)**.
### Natural Language Processing (NLP)
Il **Natural Language Processing** (Elaborazione del Linguaggio Naturale) si concentra sull'analisi computazionale del testo e del discorso umano a tre livelli progressivi:
- **Sintattico**: studio della struttura grammaticale e della disposizione delle parole nella frase
- **Semantico**: comprensione del significato letterale dei termini e delle relazioni concettuali
- **Pragmatico**: interpretazione del significato del messaggio in relazione al contesto d'uso

L'obiettivo fondamentale dell'integrazione dell'NLP nei motori di ricerca è superare il vecchio modello basato sulla semplice corrispondenza letterale delle parole chiave, permettendo al sistema di recuperare i documenti in base al loro **reale significato** (meaning-based retrieval).
#### Le direzioni dell'NLP applicate all'Information Retrieval
In ambito IR, le tecniche di NLP si sviluppano principalmente lungo tre direzioni applicative:
1. **Disambiguazione del significato delle parole (Word Sense Disambiguation - WSD)**: algoritmi capaci di determinare il significato corretto di un termine polisemico o ambiguo analizzando il contesto della frase (ad esempio capire se *Apple* nella query si riferisce all'azienda informatica o al frutto).
2. **Estrazione dell'informazione (Information Extraction - IE)**: tecniche per individuare ed estrarre automaticamente fatti, relazioni ed entità specifiche dal testo non strutturato per popolare schemi strutturati.
3. **Risposta a domande (Question Answering)**: sistemi evoluti in grado di fornire risposte puntuali ed esaustive a domande formulate dall'utente in linguaggio naturale, estraendo la risposta direttamente dall'analisi dell'intero corpus di documenti.
### Machine Learning
Il **Machine Learning** (conosciuto in italiano come Apprendimento Automatico) si focalizza sullo sviluppo di sistemi computazionali capaci di **migliorare automaticamente le proprie prestazioni con l'esperienza** (in sostanza aumentando la quantità dei propri risultati nel tempo e imparando dai propri errori).

Nel contesto dell'Information Retrieval, il Machine Learning interviene attraverso due paradigmi fondamentali:
- **Supervised Learning**: viene impiegato per la **classificazione automatica**.  Il sistema apprende modelli concettuali a partire da un insieme di esempi pre-etichettati (training set, come email già marchiate come "spam" o "non spam"), imparando ad assegnare autonomamente i nuovi documenti alla classe corretta.
- **Unsupervised Learning**: viene impiegato per il **clustering**. Il sistema analizza dati ed esempi privi di etichetta (unlabeled), raggruppandoli spontaneamente in cluster omogenei e scoprendo temi, affinità e correlazioni nascoste senza bisogno di una guida umana preventiva.
