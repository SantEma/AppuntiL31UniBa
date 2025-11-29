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

Alcune simbologie che vedremo: 
![[Pasted image 20251119142513.png]]
### Costrutti principali del modello E-R
#### Entità
Rappresentano classi di oggetti che hanno proprietà comuni ed esistenza autonoma ai fini dell’applicazione di interesse (per esempio città, dipartimento, impiegato, acquisto, vendita sono esempi di entità di un’applicazione aziendale). 
Una **occorrenza** di un’entità è un oggetto della classe che l'entità rappresenta, a tutti gli effetti le entità sono la descrizione intensionale (lo schema del database) del modello concettuale, le occorrenze sono la descrizione estensionale (l'istanza, l'insieme di dati). 
Ogni entità ha un nome che la identifica univocamente nello schema. La scelta del nome deve soddisfare due criteri: essere espressivi ed essere al singolare
#### Relazioni (o associazioni)
Rappresentano legami logici, significativi per l’applicazione, tra due o più entità.
Una **occorrenza** è una n-upla (coppia nel caso di relazione binaria) costituita da occorrenze di entità, una per ciascuna delle entità coinvolte. 
In uno schema E-R ogni relazione ha un nome che la identifica univocamente e viene rappresentata graficamente mediante un rombo con il nome della relazione all'interno e da linee che collegano la relazione con ciascuna delle sue componenti.
È importante osservare che due entità possono essere coinvolte in più relazioni e che l’insieme delle occorrenze di una relazione è a tutti gli effetti una relazione matematica tra le occorrenze delle entità coinvolte, ossia, è un sottoinsieme del loro prodotto cartesiano, implicando che tra le occorrenze di una relazione non ci possono essere ennuple ripetute
È anche possibile avere **relazioni ricorsive**, ovvero relazioni tra un entità e se stessa, e **relazioni n-arie**, cioè relazioni che coinvolgono più di due entità.
Infine, quando una relazione non è simmetrica può essere necessario distinguere i ruoli che l’entità coinvolta gioca nella relazione, ciò è ottenuto associando degli identificatori alle linee uscenti dalla relazione ricorsiva.

**Esempio**:
![[Pasted image 20251119143512.png]]
$\text{ESAME}$ è un esempio di relazione tra le entità $\text{STUDENTE}$ e $\text{CORSO}$
**Esempio ricorsivo**:
![[Pasted image 20251119143742.png]]
**Esempio n-ario (ternario)**:
![[Pasted image 20251119143817.png]]
#### Attributi
Descrivono le proprietà elementari di entità o relazioni che sono di interesse ai fini dell’applicazione.
Un attributo associa a ciascuna occorrenza di entità (o di relazione) un valore appartenente a un dominio che contiene i valori ammissibili per l’attributo. 
Gli attributi atomici possono essere raggruppati in un attributo composto.
**Esempio schema E-R con relazioni, entità e attributi**:
![[Pasted image 20251119144017.png]]
### Altri costrutti del modello E-R
#### Cardinalità delle relazioni
Vengono specificate per ciascuna partecipazione di entità a una relazione e descrivono il numero minimo e massimo di occorrenze di relazione a cui una occorrenza dell’entità può (o deve) partecipare.
![[Pasted image 20251119144154.png]]
Nell'esempio in figura , ad un impiegato deve essere assegnato almeno un incarico, ma non più di cinque. Uno stesso incarico può non essere attribuito affatto, oppure può essere attributo al più a cinquanta impiegati diversi. 

Per la cardinalità minima (primo elemento nella coppia delle cardinalità) si usa:
- $0$ per indicare che la partecipazione dell’entità alla relazione è **opzionale**
- $1$ per indicare che la partecipazione dell’entità alla relazione è **obbligatoria**.

Per la cardinalità massima (secondo elemento nella coppia delle cardinalità) si usa:
- $1$ per indicare che la relazione può essere espressa mediante una funzione che associa a una occorrenza dell’entità una sola occorrenza (o nessuna) dell’altra entità che partecipa alla relazione
- $N$ per indicare un’associazione con un numero arbitrario di occorrenze dell’altra entità

Con riferimento alle cardinalità massime, si classificano le relazioni binarie come:
- **Uno-A-Uno**: relazioni aventi cardinalità massima pari a uno per entrambe le entità coinvolte
  ![[Pasted image 20251119160643.png]]
- **Uno-A-Molti**: relazioni aventi un'entità con cardinalità massima pari a uno e l'altra cardinalità massima pari ad $N$
  ![[Pasted image 20251119160652.png]]
- **Molti-A-Molti**: relazioni aventi cardinalità massima pari ad $N$ per entrambe le entità coinvolte
  ![[Pasted image 20251119160707.png]]

Nelle relazioni n-arie, le entità coinvolte partecipano quasi sempre con cardinalità massima pari ad $N$. Se una entità partecipa ad una relazione n-aria con cardinalità massima pari a uno, significa che ogni sua occorrenza può essere legata ad un’unica ennupla delle altre entità coinvolte, si può allora legare direttamente tale entità con altre entità, mediante relazioni binarie di tipo uno-a-molti.
#### Cardinalità degli attributi
Possono essere specificate per attributi di entità o relazioni e descrivono il numero minimo e massimo di valori dell’attributo associati a ogni occorrenza di entità o relazione.
Se la cardinalità di un attributo è uno, può essere omessa e l’attributo rappresenta una funzione che associa ad ogni occorrenza di entità un solo valore dell’attributo. 
Il valore di un certo attributo può essere però nullo, oppure possono esistere diversi valori di un certo attributo per una occorrenza di entità (multivalore), in quest'ultimo caso bisogna fare attenzione, poiché possono rappresentare situazioni talvolta modellabili con entità a sé, legate da relazioni uno-a-molti all'entità cui si riferiscono.
![[Pasted image 20251119160857.png]]
#### Identificatori delle entità
Vengono specificati per ciascuna entità di uno schema e sono lo strumento che permette di identificare univocamente una determinata occorrenza di una entità. In molti casi, uno o più attributi di una entità sono sufficienti ad individuare un identificatore, in questo caso si parla di **identificatore interno (o chiave)**.
![[Pasted image 20251119161008.png]]
Un identificatore interno può essere anche un insieme di attributi, come si vede nello schema $\text{PERSONA}$
Alcune volte però gli attributi di un’entità non sono sufficienti a identificare univocamente le sue occorrenze. 
Guardiamo un esempio:
![[Pasted image 20251120160354.png]]Si consideri per esempio l’entità $\text{STUDENTE}$ nello schema in figura, l’attributo $\text{Matricola}$ non può essere un identificatore interno poiché lo schema descrive studenti iscritti a varie università e due studenti iscritti a università diverse possono avere lo stesso numero di matricola.
In questo caso per identificare univocamente uno studente serve, oltre al numero di matricola, anche la relativa università, quindi, un identificatore corretto per l’entità studente in questo schema è costituito dall'attributo $\text{Matricola}$ e dall'entità $\text{UNIVERSITÀ}$.
Va osservato che questa identificazione è resa possibile alla relazione uno a molti tra le entità $\text{UNIVERSITÀ}$ e $\text{STUDENTE}$, che associa a ogni studente una e una sola università. Se questa relazione non esistesse, l’identificazione univoca attraverso un’altra entità non sarebbe possibile. 
In generale un'entità $E$ può essere identificata da altre entità solo se tali entità sono coinvolte in una relazione a cui $E$ partecipa con cardinalità (1,1), in casi come questi, ovvero nei casi in cui l’identificazione è ottenuta utilizzando altre entità si parla di **identificatore esterno**.
#### Generalizzazioni
Rappresentano legami logici tra un'entità $E$ (detta **entità genitore**) e una o più entità $E_{1}\dots E_{n}$ (dette **entità figlie**), di cui $E$ è la più generale, nel senso che le comprende come caso particolare.
Le entità figlie $E_{1}\dots E_{n}$ quindi sono **specializzazioni** (rispetto all'entità genitore), mentre quella genitore vi è una **generalizzazione** (rispetto all'entità dei figli).

Tra le entità coinvolte in una generalizzazione valgono le seguenti proprietà:
- Ogni occorrenza di un entità figlia è anche un occorrenza dell'entità genitore
- Ogni proprietà dell'entità genitore viene ereditata da tutte le entità figlie

Le generalizzazioni vengono rappresentate graficamente mediante delle frecce che congiungono le entità figlie con l’entità genitore. 
Per le entità figlie le proprietà ereditate non vanno rappresentate esplicitamente.
![[Pasted image 20251127142117.png]]
Le generalizzazioni possono essere classificate sulla base di due proprietà tra loro ortogonali:
- Una generalizzazione è **totale** se ogni occorrenza dell’entità genitore è una occorrenza di almeno una delle entità figlie, altrimenti è **parziale**.
  La generalizzazione totale si rappresenta graficamente con una freccia chiusa, mentre la generalizzazione parziale si rappresenta con una freccia aperta.
- Una generalizzazione è **esclusiva** se ogni occorrenza dell’entità genitore è al più un’occorrenza di una delle entità figlie, altrimenti è **sovrapposta**.

Guardiamo questo schema:
![[Pasted image 20251127142713.png]]
La generalizzazione tra $\text{PERSONA}$, $\text{UOMO}$ e $\text{DONNA}$ in figura è totale, poiché gli uomini e le donne costituiscono tutte le persone (una persona deve essere o un uomo o una donna), e esclusiva, poiché una persona o è uomo o è donna (una persona non può essere sia uomo che donna). 
La generalizzazione tra l’entità $\text{PROFESSIONISTA}$ e le entità $\text{INGEGNERE}$ e $\text{DOTTORE}$ è invece parziale ed esclusiva, perché assumiamo che ciascun professionista abbia una sola professione principale e che vi siano altre professioni oltre a queste tre. 
Tra l’entità $\text{PERSONA}$ e le entità $\text{STUDENTE}$ e $\text{LAVORATORE}$ esiste infine una generalizzazione parziale e sovrapposta, perché esistono studenti che sono anche lavoratori.

Quest'ultimo esempio ci suggerisce che le generalizzazioni sovrapposte possono essere facilmente trasformate in generalizzazioni esclusive aggiungendo una o più entità figlie per rappresentare i concetti che costituiscono le intersezioni delle entità che si sovrappongono;
nel caso degli studenti e dei lavoratori è sufficiente aggiungere l’entità $\text{STUDENTELAVORATORE}$ per ottenere una generalizzazione esclusiva. 
Una stessa entità può essere coinvolta in più generalizzazioni diverse e possono esserci inoltre generalizzazioni su più livelli, in questo caso si parla di **gerarchia di generalizzazioni.** 
Nel caso in cui una generalizzazione ha una sola entità figlia si parla di **sottoinsieme**. 
È necessario tener conto del fatto che le generalizzazioni non possiedono nomi, quindi per identificarle assumiamo che siano numerate. 
Esistono infine altri vincoli sull'uso dei costrutti che non si possono esprimere sullo schema, per esempio il fatto che le gerarchie di generalizzazione non possono contenere cicli, oppure il fatto che una cardinalità minima non può essere maggiore della corrispondente cardinalità massima.
### Documentazione di schemi E-R
Uno schema E-R non è quasi mai sufficiente, da solo, a rappresentare nel dettaglio tutti gli aspetti di un’applicazione;
Nel caso di schemi particolarmente complessi può accadere di non riuscire a rappresentare in maniera comprensibile ed esaustiva i vari concetti, dunque è buona norma corredare uno schema con una documentazione di supporto utile a facilitare l’interpretazione dello schema stesso e a descrivere vincoli non esprimibili nel modello E-R.
La documentazione di supporto è costituita da un dizionario dei dati e dai vincoli di integrità sui dati.

Il dizionario dei dati è composto dalle due tabelle:
1. Entità delle tabelle, che comprende:
   - Nome
   - Descrizione (informale)
   - Attributi (con eventuali descrizioni associate)
   - Identificatori
2. Relazioni delle tabelle, che comprende:
   - Descrizione
   - Entità coinvolte
   - Attributi

Il dizionario dei dati è utile soprattutto quando lo schema è complesso e risulta pesante aggiungere allo schema tutti gli attributi di entità e relazioni. 
Le regole che descrivono i vincoli di integrità possono essere espresse sotto forma di **asserzioni**, ovvero affermazioni che devono sempre essere verificate nella base di dati che si sta progettando;
Le asserzioni vanno enunciate in maniera dichiarativa, in una forma quindi che non suggerisca un metodo per soddisfarle, per esempio notazioni del tipo “se allora ” non sono adatte ad esprimere regole aziendali, quando queste documentano uno schema E-R. 
Una struttura per enunciare regole aziendali sotto forma di asserzioni potrebbe essere invece la seguente:
$$\text{<concetto> deve/non deve <espressione sui concetti>}$$
dove i concetti citati corrispondono a concetti che compaio nello schema E-R a cui si fa riferimento.

Prendiamo in esempio questo schema:
![[Pasted image 20251127144602.png]]
Da questo possiamo trarre il dizionario dei dati e i vincoli di integrità dei dati:
![[Pasted image 20251127144709.png]]
![[Pasted image 20251127144746.png]]
## Progettazione concettuale
### La raccolta e l'analisi dei requisiti
Per **raccolta dei requisiti** si intende la completa individuazione sia dei problemi che il sistema da realizzare deve risolvere, sia delle caratteristiche che tale sistema dovrà assumere.
Per caratteristiche di sistema si intendono sia gli aspetti statici (dati) sia gli aspetti dinamici (operazioni sui suddetti dati).
I requisiti di solito vengono raccolti in linguaggio naturale e, per questo motivo, spesso ambigue e disorganizzate, quindi successivamente alla raccolta arriva la fase dell'**analisi dei requisiti**;
Questa fase prevede il chiari

