# La conoscenza
La conoscenza è la consapevolezza e la comprensione di verità, fatti o informazioni ottenute attraverso l’esperienza o l’apprendimento (a posteriori), ovvero tramite l’introspezione (a priori). La conoscenza è l’autocoscienza del possesso di informazioni connesse tra di loro, le quali, prese singolarmente, hanno un valore e un’utilità inferiori.

Per gestire la **conoscenza** bisogna saper:
- **Raccoglierla**
- Organizzarla
- Distribuirla
- Renderla **accessibile** a chi necessita di quel tipo di informazione e nel momento e luogo in cui la richiede

Queste prassi vengono eseguite al fine di **risparmiare tempo**, riducendo i tempi di accesso all'informazione e **migliorando** la qualità dei servizi che la distribuiscono.
## Dati - Informazione - Conoscenza
La **conoscenza** è un capitale difficile da gestire, poiché è:
- Volatile
- Intangibile
- Difficile da conservare

Per questo la maggior parte dei database nel mondo possiede i dati in forma **non strutturata**.

Alla luce di ciò, gli obiettivi dell'**AKM** (Automatic Knowledge Management) è quello di costruire sistemi in grado di processare documenti in formato di **linguaggio naturale**.
### Acquisizione
Come sappiamo, l’unità fondamentale dell’informazione è il dato. Esso può essere strutturato, quindi organizzato secondo tabelle, oppure non strutturato, come lo sono i documenti. Inoltre un informazione può essere cercata, se si ha un obiettivo preciso del risultato della ricerca, oppure scoperta, nel caso in cui si vogliano cercare informazioni che non sono evidenti nella sorgente.
Per passare dal testo grezzo alla vera conoscenza formale (processo conosciuto come **Ontology Construction**) si utilizzano diverse tecniche come:
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
  La ricerca di informazione su dati strutturati.
  Questo paradigma si concentra sul trovare una corrispondenza di dati per risolvere un unico **obiettivo** mirato. Per questo il database è **strutturato** e l'utente troverà questa informazione tramite un **linguaggio formale** (come visto con SQL). Un esempio di questo paradigma è la ricerca di un ristorante in una città, dove sarà effettuata la sua query specifica. L'entità manipolata è il **record**.
  
- **Information Retrieval**:
  La ricerca di informazione su dati non strutturati.
  In questo caso l'utente deve cercare **documenti** in un DB **non strutturato**, quindi privo di schema relazionale, rendendo quindi l'SQL non praticabile come soluzione.
  Seguendo l'esempio precedente, si ottiene la medesima informazione tramite **navigazione per categorie** (prendendo ad esempio un ristorante giapponese a Boston, `Boston -> Restaurant -> Japanese`), ad albero o per **parole chiave**. L'entità manipolata è il **documento**.

- **Data Mining**:
  La scoperta di informazione su dati strutturati, attraverso l’analisi di dati.
  Nel Data Mining non si cerca più un dato noto, ma si vuole scoprire in un database **strutturato** una nuova **conoscenza** analizzando moli di dati strutturati. L'entità manipolata sono i valori **numerici**.
  
- **Text Mining**:
  La scoperta di informazione su dati non strutturati, attraverso il riconoscimento di pattern o informazioni potenzialmente utili e non banali.
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
### Text Mining
Il **Text Mining** utilizza i principi del KDD sul testo a **linguaggio naturale**.
L'obiettivo è quello di scavare tra mille testi diversi fino alla ricerca di pattern, trend e associazioni nascoste, così da costruire nuova conoscenza.

>[!NOTES] Definizione Feldman e Dagan 1995
>Il **Text Data Mining** è l'estrazione non banale di informazioni implicite, precedentemente sconosciute e potenzialmente utili a partire da grandi quantità di dati testuali.

Espressa tramite la formula concettuale: $$\text{Text Mining}=\text{Data Mining (applicato al testo)}+\text{Linguistica di base}$$
#### Processo di Text Mining
![[Pasted image 20261003170819.png]]

L'architettura tipica di un sistema Text Mining, come si evince dall'immagine è basato su una **pipeline sequenziale**.
Il processo di Text Mining prende in input documenti in linguaggio naturale e
un template specificato e restituisce in output una specifica parte di documenti
che corrispondono al template. Tale processo prevede il susseguirsi di diverse
fasi:
- **Text processing**: partendo dal **testo grezzo**, si processa l'informazione 
- **Feature generation**: qui vengono create le **Bag Of Words**
- **Features Selection**: si eseguono i **filtri statistici**, in questa fase centrale vengono estratti i pattern
- **Text Mining**: estrazione tramite tecniche di **classificazione** o di **clustering** e controlli dei pattern
- **Analyzing results**: valutazione del risultato

In ambito aziendale è diventata una delle pratiche più importanti, utilizzata sopratutto per comprendere opinioni, reclami, feedback, etc. dei clienti in una società dove questi espongono le loro preferenze tramite **fonti testuali eterogenee** (email, ticket, siti web, social, comunicati stampa, etc.).

L'obiettivo chiave quindi del Text Mining aziendale è quello di poter analizzare documenti testuali in modo **rapido**, **raggruppandoli automaticamente** in base al loro contenuto, così da permetterne l'estrazione (come cercare le criticità maggiormente segnalate) con **velocità**.
#### Aree di ricerca correlate al Text Mining
Il Text Mining non è una disciplina isolata mirata solamente alla ricerca di dati da formati di testo generici, ma unisce diverse branche dell'informatica e dell'IA, come:
- **Information Retrieval (IR)**: per indicizzare collezioni di testi.
- **Text Categorization**: per l'assegnazione automatica di documenti a determinate classi tematiche.
- **Information Extraction (IE)**: per identificare entità e relazioni.
- **Natural Language Processing (NLP)**: per comprendere la sintassi, la grammatica e il significato del linguaggio naturale umano per introdurlo alla macchina.
- **Data Mining**: per la scoperta di ulteriori pattern.
[da ricopiare]