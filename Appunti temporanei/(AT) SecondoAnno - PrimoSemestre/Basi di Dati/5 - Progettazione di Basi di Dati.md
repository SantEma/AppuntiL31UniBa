## Introduzione
### Ciclo di vita dei sistemi informatici
La progettazione di una base di dati costituisce solo una delle componenti del processo di sviluppo di un sistema informativo complesso e va quindi inquadrata in un contesto più ampio, quello del ciclo di vita dei sistemi informativi. 
Tale ciclo di vita, generalmente, comprende:
- **Studio di fattibilità**: serve a definire se il progetto deve essere realizzato ed eventualmente i costi delle varie alternative possibili.
- **Raccolta e analisi dei requisiti**: consiste nel definire precisamente il problema da risolvere e quali proprietà e funzionalità il sistema deve avere. Occorre specificare l’ambiente di realizzazione sia hardware che software.
- **Progettazione**: in questa fase si concepisce la soluzione.
  Si divide in:
  - Progettazione dei dati: si individua la struttura e l’organizzazione che i dati dovranno avere
  - Progettazione delle applicazioni: si definiscono le caratteristiche dei programmi applicativi
- **Implementazione**: consiste nella realizzazione del sistema informatico secondo la struttura e le caratteristiche definite nella fase di progettazione
- **Validazione e collaudo**: serve a verificare il corretto funzionamento e la qualità del sistema informativo. Il collaudo è condotto mediante prove su dati di test
- **Funzionamento**: in questa fase il sistema informativo diventa operativo ed esegue i compiti per i quali era stato originariamente progettato. Questa fase prevede anche attività di gestione e manutenzione
![[Pasted image 20251118173625.png]]

Le fasi appena descritte non sono sequenziali, spesso durante l’esecuzione di una delle attività citate, bisogna rivedere decisioni prese nell'attività precedente, ottenendo un ciclo di operazioni. Le due fasi che saranno principalmente trattare durante questo corso sono la **fase di raccolta ed analisi dei requisiti** e **la fase di progettazione della base di dati**.
### Metodologie di progettazione
La fase di progettazione di una base di dati è un’attività tanto complessa e delicata da essere considerata la più critica dell’intero ciclo, per questo motivo richiede l’applicazione di una vera e propria metodologia di progettazione e quest'ultima consiste in:
- Una **decomposizione** dell’intera attività di progetto in fasi successive indipendenti, con input e prodotti
- Una serie di **strategie** da seguire nei vari passi e alcuni criteri per la scelta in caso di alternative
- L’utilizzo di **modelli di riferimento** per descrivere i dati di ingresso e uscita delle varie fasi.

Le proprietà che una metodologia deve garantire sono principalmente: 
- La **generalità** rispetto alle applicazioni e ai sistemi in gioco e quindi la possibilità di utilizzo indipendentemente dal problema e dagli strumenti a disposizione 
- La **qualità** del prodotto in termini di correttezza, completezza ed efficienza 
- La **facilità d'uso** sia delle strategie che dei modelli di riferimento. 
  
Nell'ambito delle basi di dati, si è consolidata negli anni una metodologia di progetto che ha dato prova di soddisfare pienamente le proprietà descritte. 
Questa metodologia è costituita da tre fasi principali da effettuare in cascata e si fonda sul principio dell’astrazione, separando le decisioni relative a cosa rappresentare in una base di dati (prima fase) dalle decisioni relative al come farlo (seconda e terza fase):
- **Progettazione concettuale**: Traduce le specifiche dei requisiti in uno schema concettuale dei dati, dei vincoli e delle operazioni sui dati. Lo schema prodotto è descritto in modo formale e completo, inoltre fa riferimento a un modello concettuale dei dati che consente di descrivere l’organizzazione dei dati senza tener conto degli aspetti implementativi. Il modello concettuale utilizzato nell'ambito di questo corso è il **modello entità-relazione esteso**.
- **Progettazione logica**: Consiste nella traduzione dello schema concettuale in uno schema logico della base di dati che fa riferimento ad un modello logico dei dati. In questa fase le scelte progettuali si basano su criteri di ottimizzazione delle operazioni effettuabili sui dati. 
  La qualità dello schema logico può essere verificata con tecniche formali, ad esempio la normalizzazione per i DB basati su modello relazionale.
- **Progettazione fisica**: produce lo schema fisico che arricchisce lo schema logico con informazioni relative all'organizzazione fisica dei dati. Il modello fisico di riferimento dipende dallo specifico DBMS scelto.

Il risultato della progettazione di una base non sarà solo uno schema fisico, ma sarà costituito anche da uno schema concettuale ed uno schema logico. Lo schema concettuale fornisce infatti una rappresentazione della base di dati di alto livello che può essere utile a livello documentativo, mentre lo schema logico fornisce una descrizione concreta del contenuto della base di dati che, prescindendo dagli aspetti implementativi, è il riferimento per le operazioni di interrogazione e aggiornamento.
![[Pasted image 20251118184446.png]]
## Il modello Entità-Relazione
Il modello **Entità-Relazione (ER)** è un modello concettuale di dati e, come tale, fornisce una serie di strutture, dette **costrutti**, atte a descrivere la realtà di interesse. 
Questi costrutti vengono utilizzati per definire schemi che descrivono l’organizzazione e la struttura delle occorrenze (o istanze) dei dati, ovvero dei valori assunti dai dati al variare del tempo. 
Tra i costrutti forniti dal modello E-R vi sono:
- **Entità**
- **Relazioni o Associazioni**
- **Attributi**
- **Identificatori**
- **Generalizzazioni e sottoinsiemi**
![[Pasted image 20251119142513.png]]
### Costrutti principali del modello E-R
#### Entità
Rappresentano classi di oggetti che hanno proprietà comuni ed esistenza autonoma ai fini dell’applicazione di interesse (per esempio città, dipartimento, impiegato, acquisto, vendita sono esempi di entità di un’applicazione aziendale). 
Una **occorrenza** di un’entità è un oggetto della classe che l'entità rappresenta, a tutti gli effetti le entità sono la descrizione intensionale del modello concettuale, le occorrenze sono la descrizione estensionale. 
Ogni entità ha un nome che la identifica univocamente nello schema. La scelta del nome deve soddisfare due criteri: essere espressivi ed essere al singolare