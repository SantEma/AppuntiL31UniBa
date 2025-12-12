## Metodologie e modelli per il progetto
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
I requisiti di un sistema provengono da fonti diverse, in genere sono:
- **Gli utenti**, attraverso interviste oppure documentazione scritta
- **La documentazione esistente**, come moduli, regolamenti interni, procedure aziendali, normative etc...
- **Eventuali realizzazioni pre-esistenti**, ovvero applicazione che si devono rimpiazzare o che devono interagire in qualche maniera con il sistema da realizzare.

I requisiti di solito vengono raccolti in linguaggio naturale e, per questo motivo, spesso ambigue e disorganizzate, quindi successivamente alla raccolta arriva la fase dell'**analisi dei requisiti**, la quale prevede il chiarimento e l'organizzazione delle specifiche dei dati.
Generalmente, si possono stabilire le seguenti regole di analisi dei requisiti:
- **Scegliere il corretto livello di astrazione**, per evitare di scegliere termini troppo astratti o troppo specifici.
- **Standardizzare la struttura delle frasi**, usando sempre lo stesso stile semantico (a costo di essere ripetitivi).
- **Evitare frasi contorte**, ossia avere delle definizioni semplici e chiare
- **Individuare omonimi e sinonimi ed unificare i termini**
- **Rendere esplicito il riferimento fra termini**, può succedere infatti che l'assenza di un contesto renda alcuni concetti ambigui.
- **Costruire un glossario dei termini**, utile per la comprensione e precisazione dei termini usati

Un esempio di glossario dei termini può essere questo:
![[Pasted image 20251129121251.png]]

Alcune frasi di esempio per la strutturazione dei requisti possono essere tali:
![[Pasted image 20251129121420.png]]
### Rappresentazione concettuale dei dati
Prima di affrontare le metodologie di progetto, cerchiamo di stabilire alcune buone pratiche generali per una corretta rappresentazione concettuale dei dati.
#### Criteri generali di rappresentazione
Dal momento che spesso non esiste una rappresentazione univoca di un insieme di specifiche, è utile stabilire delle regole concettuali basate sul modello E-R:
- Se un concetto ha proprietà significative e/o descrive classi di oggetti con esistenza autonoma, è opportuno **rappresentarlo con una entità**.
- Se un concetto ha una struttura semplice e non possiede proprietà rilevanti associate, è opportuno rappresentarlo con un **attributo** di un altro concetto a cui si riferisce.
- Se sono state individuate due (o più) entità e nei requisiti compare un concetto che le associa, questo concetto può essere rappresentato da una **relazione**.
- Se uno o più concetti risultano essere casi particolari di un altro, è opportuno rappresentarli facendo uso di una **generalizzazione**.

I criteri appena elencati hanno validità generale, sono cioè indipendenti dalla strategia di progettazione adottata
#### Pattern di progetto
Riferirsi al PDF [[Pattern di progetto.pdf]], preso dal libro e presente negli Appunti Definitivi
(Essendo pieno di esempi vi è inutile riportarlo qui)
### Strategie di progetto
Lo sviluppo di uno schema concettuale a partire dalle sue specifiche può essere considerato un processo di ingegnerizzazione e, come tale, è possibile utilizzare strategie viste già in altre discipline
####  Top-Down
Schema concettuale dove si parte da uno schema iniziale che descrive tutte le specifiche con pochi concetti molto astratto e in pian piano viene raffinato mediante opportune trasformazioni.
Nel passaggio da un raffinamento ad un altro lo schema viene modificato facendo uso di alcune trasformazioni elementari, chiamate **primitive di trasformazione top-down**.
![[Pasted image 20251201104947.png]]
Il vantaggio di questa strategia è dalla parte del progettista, tale infatti può descrivere inizialmente tutte le specifiche dei dati trascurandone i dettagli, per poi entrare nel merito di un concetto per volta.
Tutto questo procedimento però vi è possibile solo se si ha una visione globale e astratta di tutte le componenti del sistema, molto difficile se quest'ultimo è complesso
#### Bottom-up
Questa strategia prevede l'inversione del metodo top-down, ossia si parte da componenti specifiche già dettagliate per poi astrarle per passi, rendendole descrittrici di un frammento elementare della realtà di interesse, e infine riunirle in uno schema concettuale finale molto più generale, composto da semplici concetti.
![[Pasted image 20251201110023.png]]
Nello schema possiamo notare le varie fasi:
1. Decomposizione delle specifiche
2. Rappresentazione delle componenti di base
3. Integrazione in schemi elementari

Le trasformazioni elementari che attuiamo vengono chiamate **primitive di trasformazione bottom-up**, che introducono in uno schema nuovi concetti non presenti precedentemente e in grado di descrivere aspetti della realtà di interesse non ancor stati rappresentati.
Il vantaggio di questa strategia è la stessa decomposizione di problemi in componenti più semplici e facilmente individuabili, rendendo possibile la collaborazione di progettisti diversi sullo stesso progetto. Uno svantaggio è la richiesta delle operazioni di integrazione degli schemi concettuali diversi che, nel caso di schemi complessi, presentano quasi sempre grosse difficoltà
#### Inside-out
In questa strategia si rappresentano prima i concetti in relazione con i concetti iniziali per poi muoversi verso quelli più lontani usando una "navigazione" tra specifiche.
![[Pasted image 20251201111618.png]]
Come si può vedere dall'esempio proposto in figura, si hanno i concetti principali internamente, per poi espandersi verso l'esterno.
Questo tipo di strategia è una particolare strategia di bottom-up.
Questa strategia ha come vantaggio il non richiedere passi integrazione, a discapito di esaminare sempre tutte le specifiche di volta in volta per individuare concetti ancora non rappresentati e descriverne di nuovi nel dettaglio, rendendo quindi impossibile avere livelli di astrazione come in top-down.
#### Mista (o ibrida)
La strategia mista prevede la combinazione delle strategia top-down e bottom-up:
Il progettista suddivide i requisiti in componenti separate (come in bottom-up) e allo stesso tempo definisce uno **schema scheletro** contenente, a livello astratto, i concetti principali dell'applicazione. Questo schema fornisce una visione astratta dell'intero progetto, favorendo l'integrazione degli schemi sviluppati separatamente.
Per ogni concetto principale completo nello schema è possibile proseguire per raffinamenti successivi (top-down) o per concetti ancora non rappresentati (bottom-up).
Questo tipo di strategia è la più flessibile tra tutte, poichè si adatta a tutte le esigenze, per questo è molte volte l'unica ad essere adottabile in progetti di certa complessità.
### Qualità di uno schema concettuale
Nella costruzione di uno schema concettuale vanno garantite alcune proprietà generali che uno schema di buona qualità deve possedere:
- **Correttezza**: Uno schema è corretto quando utilizza propriamente i costrutti messi a disposizione del modello concettuale di riferimento.
  Gli errori possibili possono essere semantici o sintattici
- **Completezza**: Uno schema è completo quando rappresenta tutti i dati di interesse e quando tutte le operazioni possono essere eseguite a partire da concetti descritti nello schema.
  Questa proprietà si può controllare verificando che tutte le specifiche sui dati siano rappresentate da qualche concetto presente nello schema che stiamo costruendo
- **Leggibilità**: Uno schema è leggibile quando rappresenta i requisiti in maniera naturale e facilmente comprensibile.
  Per questa proprietà è necessario rendere lo schema autoesplicativo, per esempio con la scelta dei nomi da dare ai concetti.
  Alcuni suggerimenti per renderlo più leggibili possono essere:
  - Riporre i costrutti su una griglia scegliendo come elementi centrali quelli con più legami
  - Tracciare solo linee perpendicolari e cercare di minimizzare le intersezioni
  - Disporre le entità che sono genitori di generalizzazioni sopra le relative entità figlie
- **Minimalità**: uno schema è minimale quando tutte le specifiche dei dati sono rappresentate una volta nello schema.
  Quindi non vi è minimale quando ci sono ridondanze, ma quest'ultima potrebbe non essere indesiderata ma voluta per scelte progettuali desiderate
### Metodologia generale
Descriviamo un metodo per la progettazione concettuale con strategia mista, da poter seguire:
1. **Analisi dei requisiti**
	- Costruire un glossario	
	- Analizzare i requisiti ed eliminare ambiguità
	- Raggruppare i requisiti in insiemi omogenei
2. **Passo base**
	- Individuare i concetti più rilevanti e rappresentarli in uno schema scheletro
3. **Passo di decomposizione (solo se necessario o appropriato**
	- Effettuare una decomposizione dei requisiti con riferimento ai concetti presenti nello schema scheletro 
4. **Passo iterativo**
	- Raffinare i concetti presenti sulla base delle loro specifiche
	- Aggiungere nuovi concetti allo schema per descrivere specifiche non ancora descritte
5. **Passo di integrazione**
	- Integrare vari sotto-schemi in uno schema generale facendo riferimento allo schema scheletro
6. **Analisi di qualità**
	- Verificare la correttezza dello schema ed eventualmente ristrutturare lo schema
	- Verificare la completezza dello schema ed eventualmente ristrutturare lo schema
	- Verificare la minimalità, documentare le ridondanze ed eventualmente ristrutturare lo schema
	- Verificare la leggibilità dello schema ed eventualmente ristrutturare lo schema

In questi passaggi è sempre utile per ogni passaggio la **documentazione degli schemi**
### Esempio di progettazione concettuale
Vi consiglio di andare a vedere il PDF [[Esempio di progettazione concettuale.pdf]], preso sempre dal libro e presente nella cartella degli Appunti Definitivi
## Progettazione logica
L'obbiettivo della progettazione logica è quello di costruire uno schema logico in grado di descrivere, in maniera corretta ed efficiente, tutte le informazioni contenute nello schema Entità-Relazione prodotto nella fase di progettazione concettuale,
### Fasi della progettazione logica
Le attività principali della progettazione logica sono la riorganizzazione dello schema concettuale e la traduzione in un modello logico. 
Questo processo prevede due fasi principali:
- **Ristrutturazione dello schema E-R**: fase indipendente dal modello logico scelto e si basa su criteri di ottimizzazione dello schema e di semplificazione della fase successiva.
- **Traduzione verso il modello logico**: fase che fa riferimento ad uno specifico modello logico e può includere un ulteriore ottimizzazione.

I dati di ingresso della prima fase sono lo schema concettuale prodotto nella fase precedente e il carico applicativo previsto, in termini di dimensione dei dati e caratteristiche delle operazioni. Il risultato che si ottiene è uno schema E-R che non è più strettamente concettuale, poiché tiene conto degli aspetti realizzativi. Questo schema e il modello logico scelto costituiscono i dati di input della seconda fase, che produce lo schema logico della base di dati.
Lo schema logico, i vincoli di integrità definiti su esso e la relativa documentazione, costituiscono i prodotti finali della progettazione logica
![[Pasted image 20251203151211.png]]
### Analisi delle prestazioni
Uno schema E-R può essere modificato per ottimizzare alcuni indici di prestazione del progetto, si parla di indici poiché non esiste una valutazione per le prestazioni della base di dati nella progettazione logica (non esistono semplicemente fattori possibili prevedibili).
Due parametri che generalmente regolano le prestazioni di un sistema software sono:
- **Costo di un'operazione**: valutato in termini di numero medio di occorrenze di entità e associazioni da visitare per rispondere a una operazione sul DB
- **Occupazione di memoria**: valutata in termini di spazio di memoria necessario per memorizzare i dati descritti dallo schema.

Per studiare questi parametri abbiamo bisogno di conoscere, oltre allo schema, le seguenti informazioni:
- **Volume dei dati**, ossia: 
	- Numero medio di occorrenze di ogni entità e associazione dello schema 
	- Dimensioni di ciascun attributo (di entità o associazione) •
- **Caratteristiche delle operazioni**, ossia:
	- Tipo dell’operazione (interattiva o batch)
	- Frequenza (numero medio di esecuzioni in un certo intervallo di tempo) 
	- Dati coinvolti (entità e associazioni)

Per fare un esempio pratico, prendiamo uno schema:
![[Pasted image 20251205101649.png]]
Trattandosi di uno schema riguardante dati sul personale di un'azienda, le operazioni possibili potrebbero essere quelle che seguono:
1. Assegna un impiegato ad un progetto
2. Trova i dati di un impiegato, del dipartimento in quale lavora e dei progetti a cui partecipa
3. Trova i dati di tutti gli impiegati di un certo dipartimento
4. Per ogni sede, trova i suoi dipartimenti con il cognome del direttore e l'elenco degli impiegati del dipartimento

Sebbene un'analisi delle prestazioni che fa riferimento ad un numero ristretto di operazioni può sembrare riduttiva rispetto al reale carico della base di dati, va notato che le operazioni sulle basi di dati seguono la cosidetta regola "ottanta-venti":
L'ottanta percento del carico è generato dal venti percento delle operazioni.
Questo ci permette di valutare adeguatamente il carico concentrandoci solo sulle operazioni previste

Il volume dei dati e le caratteristiche generali delle operazioni possono essere descritti facendo uso di tabelle, chiamate **tabelle dei volumi e delle operazioni** 
Nella tavola dei volumi vengono riportati tutti i concetti dello schema (entità e associazioni), un simbolo per ogni concetto che distingua le entità ($E$) dalle relazioni ($R$) e il volume a regime. 
Nella tavola delle operazioni riportiamo, per ogni operazione la frequenza prevista (su un arco di tempo) e un simbolo che indichi se l’operazione è interattiva ($I$) o batch ($B$). 
Nella tavola dei volumi, il numero delle occorrenze delle associazioni dipende da due parametri: il numero di occorrenze delle entità coinvolte nelle associazioni e il numero (medio) di partecipazioni di una occorrenza di entità alle occorrenze di associazioni. Il primo parametro è solitamente fornito dalla specifica dei requisiti, mentre il secondo parametro dipende a sua volta dalle cardinalità dell’associazione.
![[Pasted image 20251204153135.png]]
Ad esempio, considerando lo schema E-R precedente e le sue tabelle, per calcolare il numero di occorrenze per l’associazione $\text{PARTECIPAZIONE}$ si assume che un impiegato possa partecipare a 3 progetti contemporaneamente, ottenendo dunque 2000 × 3 = 6000 occorrenze.
Allo stesso modo, si sarebbe potuto assumere che ad uno stesso progetto possano partecipare 12 impiegati, ottenendo 500 × 12 = 6000 occorrenze della relazione partecipazione. 
È assolutamente errato moltiplicare tra loro il numero di occorrenze delle due entità che partecipano all'associazione, poiché così facendo si considera che, ricorrendo all'esempio precedente, ogni impiegato possa partecipare solo ad un progetto.

Per ogni operazione, possiamo inoltre descrivere graficamente i dati coinvolti con uno schema di operazione che consiste nel frammento dello schema E-R interessato dall'operazione, sul quale viene disegnato il cammino logico da percorrere per accedere alle informazioni di interesse.
![[Pasted image 20251205102711.png]]
Presa in considerazione l’operazione 2, per eseguirla si deve: accedere a una occorrenza dell’entità $\text{IMPIEGATO}$ per accedere poi ad una occorrenza dell’associazione $\text{AFFERENZA}$ e, attraverso questa, a una occorrenza dell’entità $\text{DIPARTIMENTO}$. Successivamente, per conoscere i dati dei progetti ai quali lavora, dobbiamo accedere a tre occorrenze dell’associazione $\text{PARTECIPAZIONE}$ e, attraverso queste, a tre occorrenze dell'entità progetto. 
Tutto questo viene riassunto in una tavola degli accessi in cui viene anche indicato se l’accesso avviene in lettura ($L$) o scrittura ($S$). Evidenziare questa differenza è fondamentale poiché si assume che le operazioni di scrittura abbiano un costo pari al doppio di una operazione di lettura.
![[Pasted image 20251205102913.png]]
### Ristrutturazione di schemi E-R
La prima fase di ristrutturazione di uno schema Entità-Relazione si può suddividere in una serie di passi in sequenza:
- **Analisi delle ridondanze**: si decide se eliminare o mantenere eventuali ridondanze presenti nel E-R.
- **Eliminazione delle generalizzazioni**: tutte le generalizzazioni presenti nello schema sono analizzate e sostituite da altri costrutti. Nella progettazione concettuale utilizziamo costrutti assenti nella progettazione logica per rappresentare nel miglior modo possibile i dati.
- **Partizionamento/accorpamento di entità e associazioni**: si decide se partizionare concetti dello schema in più concetti o, viceversa, accorpare concetti in un unico concetto\
- **Scelta degli identificatori primari**: si seleziona un identificatore per le entità che ne hanno più di uno.
![[Pasted image 20251205103837.png]]
#### Analisi delle ridondanze
Per ridondanza, in uno schema concettuale, si intende la presenza di un dato che può essere derivato da altri. Le forme di ridondanza in uno schema E-R sono:
- Attributi derivabili da altri attributi della stessa entità (o associazione).
  Per esempio nel primo schema in figura
- Attributi derivabili da attributi di altre entità (o associazioni), di solito attraverso funzioni di aggregazione (somma, media, conteggio...).
   Se ne vedono due esempi nel secondo e terzo schema in figura. Nel secondo schema, l’attributo importo totale dell’entità acquisto si può derivare, attraverso l’associazione composizione dall'attributo prezzo dell’entità prodotto, sommando i prezzi dei prodotti di cui un acquisto è composto
- Associazioni derivabili dalla composizione di altre associazioni in presenza di cicli. 
  Ad esempio nel quarto schema in figura l’associazione docenza tra studenti e professori può essere infatti derivata dalle associazioni frequenza e insegnamento. Va comunque precisato che la presenza di un ciclo non genera necessariamente ridondanze.

**Nota bene**: La presenza di cicli non genera necessariamente ridondanze.
![[Pasted image 20251208112702.png]]

La presenza di una ridondanza ha effetti positivi, semplificare le interrogazioni, ed effetti negativi, appesantisce gli aggiornamenti e comporta l'occupazione di più memoria. Per questo motivo, la decisione di mantenere o eliminare una ridondanza va presa in seguito ad una **analisi quantitativa** che confronti il costo di esecuzione delle operazioni che coinvolgono il dato ridondante e l’occupazione di memoria, sia in presenza e che in assenza di ridondanza.
##### Esempio di eliminazione di una ridondanza
Si consideri l’applicazione anagrafica di una regione rappresentata dal terzo schema nella figura precedente e si considerino le seguenti operazioni:
1. Memorizzare una nuova persona con la relativa città di residenza
2. Stampare tutti i dati di una città (incluso il numero di abitanti)

Supponiamo inoltre che per questa applicazione i dati di carico siano quelli riportati in figura:
![[Pasted image 20251208113020.png]]
Individuato l’attributo $\text{Numero abitanti}$ come dato ridondante, proviamo a valutare gli indici di prestazione in caso di presenza del dato ridondante. 
Assumendo che il numero degli abitanti di una città richieda 4 byte, abbiamo che il dato ridondante richiede $4 \times 200 = 800$ byte di memoria aggiuntiva. 
Passiamo ora alla stima del costo delle operazioni; per farlo si generano le tabelle degli accessi in figura, dalle quali si evince che l’operazione 1 ha un costo unitario pari a 7 in presenza di ridondanza e pari a 4 in assenza di ridondanza, l’operazione 2 ha costo unitario rispettivamente 1 e 5001. 
A questo punto, tenendo conto delle frequenze, si ottiene che il sistema presenta:
1. $7 \times 500 +1 \times 2 = 3502$ accessi totali in presenza di ridondanza
2. $4 \times 500 + 5001 \times 2 = 12002$ accessi assenti in presenza di ridondanza

Quindi, circa 8500 accessi giornalieri in più rispetto al caso di dato ridondante presente contro un risparmio di un solo kilobyte.
Possiamo dunque concludere che conviene, in questo caso, mantenere il dato ridondante.
Di seguito le tavole di accessi per caso
![[Pasted image 20251208113448.png]]
#### Eliminazione delle generalizzazioni
Dato che i sistemi tradizionali per la gestione dei dati non consentono di rappresentare direttamente una generalizzazione, risulta spesso necessario trasformare questo costrutto in altri costrutti del modello E-R per i quali esiste una implementazione naturale: le entità e le associazioni.
Prenderemo in esempio questo schema:
![[Pasted image 20251208115554.png]]

Per rappresentare una generalizzazione mediante entità e associazioni abbiamo essenzialmente tre alternative possibili:
1. **Accorpamento delle figlie nel genitore**. Le entità $E_{1}$ e $E_{2}$ sono eliminate e le loro proprietà (attributi, associazioni, generalizzazioni) sono aggiunte al padre $E_{0}$. All'entità padre viene aggiunto un attributo per distinguere il tipo di occorrenza di $E_{0}$, cioè se tale occorrenza apparteneva a $E_{1}$  o a $E_{2}$ o, nel caso di generalizzazione parziale, a nessuna delle due. In riferimento al primo schema, si osservi che gli attributi $A_{11}$ e $A_{12}$ possono assumere valori nulli per alcune occorrenze di $E_{0}$ e che la relazione $R_{2}$ avrà una cardinalità minima pari a 0 sull'entità $E_{0}$.![[Pasted image 20251208115904.png]]
2. **Accorpamento del genitore nelle figlie**. Si elimina l’entità genitore $E_{0}$. Per l’ereditarietà, gli attributi di $E_{0}$, il suo identificatore e le relazioni cui partecipava, sono aggiunti alle figlie $E_{1}$ e $E_{2}$. Le relazioni $R_{11}$ ed $R_{12}$ rappresentano la restrizione della relazione $R_{1}$ sulla occorrenze di $E_{1}$ ed $E_{2}$. La cardinalità delle associazioni presenti non vengono alterate.![[Pasted image 20251208120102.png]]
3. **Sostituzione delle generalizzazioni con associazioni**. La generalizzazione si trasforma in due associazioni uno a uno che legano rispettivamente l’entità genitore con le entità figlie $E_{1}$ e $E_{2}$. Non ci sono trasferimenti di attributi o associazioni e le entità $E_{1}$ ed $E_{2}$ sono identificate esternamente dall'entità $E_{0}$. Nello schema ottenuto vanno aggiunti però dei vincoli: ogni occorrenza di $E_{0}$ non può partecipare contemporaneamente a $R_{G1}$ e $R_{G2}$; inoltre, se la generalizzazione è totale, ogni occorrenza di $E_{0}$ deve partecipare o a un’occorrenza di $R_{G1}$ oppure ad un’occorrenza di $R_{G2}$.![[Pasted image 20251208120409.png]]

La scelta fra le alternative si può effettuare dopo un’analisi quantitativa, analogamente all'analisi delle ridondanze, oppure in seguito ad un’analisi qualitativa che tenga conto di semplici regole generali:
- La prima conviene quando le operazioni non fanno molta distinzione fra occorrenze e attributi di $E_{0}$, $E_{1}$ e $E_{2}$. Pur generando uno spreco di memoria per la presenza di valori nulli, induce un minor numero di accessi
- La seconda è possibile solo se la generalizzazione è totale, altrimenti le occorrenze di $E_{0}$ che non sono occorrenze nè di $E_{1}$ nè di $E_{2}$ non sarebbero rappresentate. Questa alternativa è conveniente quando ci sono operazioni che si riferiscono solo a occorrenze di $E_{1}$ o di $E_{2}$, e dunque fanno distinzione tra le entità figlie. In questo caso si ottiene un risparmio di memoria rispetto alla prima alternativa poiché in linea di principio non ci sono valori nulli, ed una riduzione di accessi rispetto alla terza alternativa perché non si deve visitare $E_{0}$ per accedere al alcuni attributi di $E_{1}$ ed $E_{2}$
- La terza alternativa conviene quando la generalizzazione non è totale e ci sono operatori che si riferiscono solo a $E_{1}$ o $E_{2}$ , oppure si riferiscono solo a $E_{0}$ , e dunque fanno distinzioni tra entità figlia ed entità genitore.

Le alternative presentate non sono le uniche ammesse, ma è possibile effettuare ristrutturazioni che sono combinazioni delle precedenti. Inoltre per la stessa generalizzazione è possibile applicare trasformazioni differenti sui diversi livelli
#### Partizionamento/Accorpamento di entità e/o associazioni
Entità e associazioni possono essere partizionati o accorpati per garantire maggiore efficienza delle operazioni in base a criteri simili a quelli usati per le generalizzazioni: 
Gli accessi si riducono separando attributi di uno stesso concetto che vengono acceduti da operazioni diverse e raggruppando attributi di concetti diversi che vengono acceduti dalle medesime operazioni.
##### Partizionamento (verticale e orizzontale) di entità
Si suddivide un concetto operando sui suoi attributi.
Questa ristrutturazione è conveniente se le operazioni che coinvolgono frequentemente l’entità originaria richiedono, ad esempio per un impiegato, o solo informazioni di carattere anagrafico o solo informazioni relative alla sua retribuzione. Un partizionamento di questo tipo è un esempio di partizionamento **verticale** di una entità. È possibile effettuare delle decomposizioni **orizzontali** nelle quali la suddivisione avviene sulle occorrenze dell’entità. 
Per esempio, per l’entità $\text{IMPIEGATO}$ ci potrebbero essere alcune operazioni che riguardano soltanto gli analisti e altre che operano solo sui venditori. In questo caso però le entità ottenute ($\text{ANALISTA e VENDITORE}$) hanno gli stessi attributi dell’entità di partenza. Si osservi che una decomposizione orizzontale corrisponde all'introduzione di una generalizzazione a livello logico. L’effetto collaterale dei partizionamenti orizzontali è la duplicazione di tutte le associazioni a cui l’entità originaria partecipa. D’altra parte i partizionamenti verticali ottimizzano il recupero delle informazioni.
![[Pasted image 20251209121342.png]]
##### Eliminazione di attributi multivalore
Questa ristrutturazione si rende necessaria perché, come per le generalizzazioni, il modello relazionale non permette di rappresentare questo tipo di attributo
![[Pasted image 20251209121453.png]]
Come si vede in figura, l'entità $\text{AGENZIA}$ avente l’attributo multivalore $\text{TELEFONO}$ viene partizionata in due entità: una entità con lo stesso nome e gli stessi attributi a meno dell’attributo multivalore, e l’entità $\text{TELEFONO}$, con il solo attributo numero, legata con una associazione uno a molti con l’entità $\text{AGENZIA}$. Se l’attributo multivalore fosse stato opzionale, allora la cardinalità minima dell’associazione sarebbe stata pari a zero.
##### Accorpamento di entità
È l’operazione inversa al partizionamento. Due entità connesse da relazione vengono accorpate in un’unica entità contenente gli attributi di entrambe. Si ottiene una riduzione degli accessi, ma con il rischio di uno spreco di memoria dovuto alla presenza di possibili valori nulli
![[Pasted image 20251209121700.png]]
Un ragionamento analogo si può estendere alle associazioni, come si vede nel prossimo esempio in cui vengono distinti i giocatori che compongono attualmente una squadra da quelli che ne facevano parte nel passato
![[Pasted image 20251209121741.png]]
#### Scelta degli identificatori principali
La scelta degli identificatori principali è indispensabile nelle traduzione verso il modello relazionale, perché in questo modello le chiavi sono usate per stabilire legami tra dati di relazioni diverse. Quindi, nei casi in cui esistono entità per le quali sono stati specificati più identificatori, bisogna decidere quale di questi identificatori verrà utilizzato come chiave primaria. I criteri per la scelta sono:
- Assenza di valori nulli, altrimenti non è garantito l’accesso a tutte le occorrenze dell’entità corrispondente.
- Semplicità: un identificatore composto da uno o pochi attributi è da preferire a identificatori costituiti da molti attributi.
- Utilizzo nelle operazioni più frequenti e/o importanti.
- Preferenza per gli identificatori interni
### Traduzione verso il modello relazionale
La seconda fase della progettazione logica corrisponde a una traduzione tra modelli di dati diversi: a partire da uno schema E-R ristrutturato si costruisce uno schema logico equivalente, in grado cioè di rappresentare le medesime informazioni. 
Facciamo riferimento a una versione semplificata del modello E-R, che non contiene generalizzazioni e attributi multivalore, e nella quale ogni entità ha un solo identificatore. 

Affrontiamo il problema della traduzione caso per caso, iniziando dal caso più generale (quello di entità legate da associazioni molti a molti) che ci suggerisce l’idea generale su cui si basa la metodologia di traduzione.
#### Associazioni molti a molti ($N:N$) 
Consideriamo lo schema in figura, la sua traduzione naturale nel modello relazionale prevede:
* per ogni entità, una relazione con lo stesso nome avente per attributi i medesimi attributi dell’entità e per chiave il suo identificatore;
* per l’associazione, una relazione con lo stesso nome avente per attributi gli attributi dell’associazione e gli identificatori delle entità coinvolte; tali identificatori formano la chiave della relazione.

Se gli attributi originali di entità o associazioni sono opzionali, i corrispondenti attributi di relazione possono assumere valori nulli.
![[Pasted image 20251209142951.png]]
Lo schema relazionale che si ottiene è il seguente:
$$\text{IMPIEGATO} (\underline{\text{Matricola}}, \text{Cognome}, \text{Stipendio})$$
$$\text{PROGETTO} (\underline{\text{Codice}}, \text{Nome}, \text{Budget})$$
$$\text{PARTECIPAZIONE} (\underline{\text{Matricola}}, \underline{\text{Codice}}, \text{DataInizio})$$
Vanno sempre specificati i **vincoli di integrità referenziale** presenti tra gli schemi creati. Nel caso in esempio esistono due vincoli tra gli attributi $\text{Matricola}$ e $\text{Codice}$ di $\text{PARTECIPAZIONE}$ e gli omonimi attributi delle entità $\text{IMPIEGATO}$ e $\text{PROGETTO}$.
Per rendere più comprensibile il significato dello schema è conveniente effettuare alcune ridenominazioni, ad esempio:
$$\text{PARTECIPAZIONE} (\underline{\text{Impiegato}}, \underline{\text{Progetto}}, \text{DataInizio})$$
La ridenominazione è essenziale in presenza di associazioni ricorsive
![[Pasted image 20251209143021.png]]
Questo schema si traduce nelle seguenti due relazioni: 
$$\text{PRODOTTO} (\underline{\text{Codice}}, \text{Nome}, \text{Costo})$$
$$\text{COMPOSIZIONE} (\underline{\text{Composto}}, \underline{\text{Componente}}, \text{Quantità})$$

Esistono vincoli di integrità referenziale tra gli attributi $\text{Composto}$ e $\text{Componente}$ di $\text{COMPOSIZIONE}$ e la chiave di $\text{PRODOTTO}$.

Le associazioni con più di due entità partecipanti si traducono in maniera analoga alle associazioni binarie.
![[Pasted image 20251209143056.png]]
Lo schema in figura si traduce nelle seguenti tre relazioni:
$$\text{FORNITORE} (\underline{\text{PartitaIVA}}, \text{NomeDitta})$$
$$\text{PRODOTTO} (\underline{\text{Codice}}, \text{Genere}) \ \text{DIPARTIMENTO}(\underline{\text{Nome}}, \text{Telefono})$$ 
$$\text{FORNITURA} (\underline{\text{Fornitore}}, \underline{\text{Prodotto}}, \underline{\text{Dipartimento}}, \text{Quantità})$$

#### Associazioni uno a molti $(1:N)$
Consideriamo lo schema in figura.
![[Pasted image 20251209144425.png]]
Secondo la regola vista per le associazioni molti a molti, la traduzione di questo schema dovrebbe essere la seguente:

$$\text{GIOCATORE} (\underline{\text{Cognome}}, \underline{\text{DataNascita}}, \text{Ruolo})$$
$$\text{CONTRATTO} (\underline{\text{CognGiocatore}}, \underline{\text{DataNascG}}, \underline{\text{Squadra}}, \text{Ingaggio})$$
$$\text{SQUADRA} (\underline{\text{Nome}}, \text{Città}, \text{ColoriSociali})$$
Ma la traduzione appena vista non è la più corretta. Infatti, essendo una relazione di tipo uno a molti, per identificare univocamente una squadra è sufficiente l'id di $\text{GIOCATORE}$.
Ottenendo in questo modo una nuova traduzione:

$$\text{GIOCATORE} (\underline{\text{Cognome}}, \underline{\text{DataNascita}}, \text{Ruolo})$$
$$\text{CONTRATTO} (\underline{\text{CognGiocatore}}, \underline{\text{DataNascG}}, \text{Squadra}, \text{Ingaggio})$$
$$\text{SQUADRA} (\underline{\text{Nome}}, \text{Città}, \text{ColoriSociali})$$
Ancora una volta, notiamo che nella relazione $\text{CONTRATTO}$, la chiave è costituita solo dall'identificatore di $\text{GIOCATORE}$ perché le cardinalità dell’associazione ci dicono che ogni giocatore ha un contratto con una sola squadra. A questo punto le relazioni $\text{GIOCATORE}$ e $\text{CONTRATTO}$ hanno la stessa chiave, quindi si possono fondere in un’unica relazione. 
Si preferisce, dunque, la traduzione che segue, nella quale la relazione $\text{GIOCATORE}$ rappresenta sia l’entità relativa sia l’associazione dello schema E-R originale:
$$\text{GIOCATORE} (\underline{\text{Cognome}}, \underline{\text{DataNascita}}, \text{Ruolo}, \text{Squadra}, \text{Ingaggio})$$
$$\text{SQUADRA} (\underline{\text{Nome}}, \text{Città}, \text{ColoriSociali})$$
con vincolo di integrità referenziale fra Squadra in $\text{GIOCATORE}$ e la chiave di $\text{SQUADRA}$.
Se la cardinalità minima dell'associazione è zero, allora $\text{SQUADRA}$ e $\text{INGAGGIO}$ in $\text{GIOCATORE}$ devono ammettere valore nullo.
#### Entità con identificatore esterno
Le entità con identificatori esterni danno luogo a relazioni con chiavi che includono gli identificatori delle entità identificanti.
![[Pasted image 20251209144538.png]]
Per esempio, una traduzione per lo schema in figura potrebbe essere il seguente:
$$\text{STUDENTE}(\underline{\text{Matricola}}, \underline{\text{NomeUniversità}}, \text{Cognome}, \text{AnnoIscrizione})$$
$$\text{UNIVERSITÀ} (\underline{\text{Nome}}, \text{Città}, \text{Indirizzo})$$
con vincolo di integrità referenziale tra l'attributo $\text{NomeUniversità}$ della relazione $\text{STUDENTE}$ e l'attributo $\text{Nome}$ dell'entità $\text{UNIVERSITÀ}$.
Come si può vedere, rappresentando l'identificatore esterno si rappresenta direttamente anche l'associazione tra le due entità. Ricordiamo, infatti, che le entità identificate esternamente partecipano all'associazione sempre con cardinalità minima e massima pari a uno.
#### Associazioni uno a uno $(1:1)$
Per le associazioni uno a uno esistono diverse possibilità di traduzione sulla base delle cardinalità minime.
Cominciamo a vedere le associazioni uno a uno con partecipazione obbligatoria per entrambe le entità, come in figura:
![[Pasted image 20251209153301.png]]
In questo caso abbiamo due tipi di traduzioni completamente simmetriche:
$$\text{DIRETTORE} (\underline{\text{Codice}}, \text{Cognome}, \text{Stipendio}, \text{DipartimentoDiretto}, \text{InizioDirezione})$$
$$\text{DIPARTIMENTO} (\underline{\text{Nome}}, \text{Telefono}, \text{Sede})$$
con vincolo di integrità referenziale tra l'attributo $\text{DipartimentoDiretto}$ di $\text{DIRETTORE}$ e $\text{Nome}$ di $\text{DIPARTIMENTO}$.
Oppure
$$\text{DIRETTORE} (\underline{\text{Codice}}, \text{Cognome}, \text{Stipendio})$$
$$\text{DIPARTIMENTO} (\underline{\text{Nome}}, \text{Telefono}, \text{Sede}, \text{Direttore}, \text{InizioDirezione})$$

con vincolo di integrità referenziale tra l'attributo $\text{Direttore}$ di $\text{DIPARTIMENTO}$ e l'attributo $\text{Codice}$ della relazione $\text{DIRETTORE}$.
Trattandosi di una relazione biunivoca, si potrebbe pensare di rappresentare tutti i concetti in un'unica relazione contenente tutti gli attributi in gioco. Questa alternativa è da escludere perché se ci fosse stato un valido motivo si sarebbe già ristrutturato lo schema E-R in modo da accorpare le due entità.

Consideriamo ora il caso di associazione uno a uno con **partecipazione opzional**e per una sola entità, come mostrato in figura.
![[Pasted image 20251210105819.png]]
In questo caso abbiamo una soluzione preferibile, ovvero la seguente:
$$\text{IMPIEGATO} (\underline{\text{Codice}}, \text{Cognome}, \text{Stipendio})$$
$$\text{DIPARTIMENTO} (\underline{\text{Nome}}, \text{Telefono}, \text{Sede}, \text{Direttore}, \text{InizioDirezione})$$
con vincolo di integrità referenziale tra l'attributo $\text{Direttore}$ di $\text{DIPARTIMENTO}$ e l'attributo $\text{Codice}$ di $\text{IMPIEGATO}$.

Consideriamo infine il caso in cui entrambe le entità hanno partecipazione opzionale, come nel caso in figura, dove però possono esistere dipartimenti senza direttori, e quindi la cardinalità dell'entità $\text{DIPARTIMENTO}$ diventa $(0,1);
In questo caso esiste un ulteriore possibilità che prevede tre relazioni separate:
$$\text{IMPIEGATO} (\underline{\text{Codice}}, \text{Cognome}, \text{Stipendio})$$
$$\text{DIPARTIMENTO} (\underline{\text{Nome}}, \text{Telefono}, \text{Sede})$$
$$\text{DIREZIONE} (\underline{\text{Direttore}}, \underline{\text{Dipartimento}}, \text{DataInizioDirezione})$$

con vincoli di integrità referenziale tra l'attributo $\text{Direttore}$ di $\text{DIREZIONE}$ e l'attributo $\text{Codice}$ di $\text{IMPIEGATO}$ e tra l'attributo $\text{Dipartimento}$ di $\text{DIREZIONE}$ e l'attributo $\text{Nome}$ di $\text{DIPARTIMENTO}$.
#### Documentazione di schemi logici
Il risultato della progettazione logica è costituito dallo schema di una base di dati e la documentazione ad esso associata.
Buona parte della documentazione dello schema concettuale in ingresso alla fase di progettazione può essere ereditata dallo schema logico ottenuto come risultato di questa fase, in particolare se i nomi dei concetti dello schema E-R son stati riutilizzati per costruire lo schema relazionale, le regole precedentemente definite possono essere utilizzate per documentare anche quest'ultimo. 

A questa documentazione bisogna aggiungere ulteriore documentazione per descrivere vincoli di integrità referenziale introdotti dalla traduzione (in aggiunta ai vincoli individuati durante la progettazione concettuale), quindi a tal fine si adotta un semplice formalismo grafico. 
Dato uno schema logico, i vincoli di integrità referenziale si documentano mediante un diagramma dove:
- Le chiavi delle relazioni sono rappresentate in grassetto
- Le frecce indicano vincoli di integrità referenziale
- Gli asterischi su attributi indicano possibili valori nulli

Prendendo in esempio lo schema logico sugli impiegati si otterà questo:
![[Pasted image 20251210115426.png]]
È interessante osservare come, con questo tipo di rappresentazione, sia possibile rappresentare esplicitamente anche le associazioni dello schema Entità-Relazione di partenza alle quali, nello schema relazionale equivalente, non corrisponde nessuna relazione.
### Extra
#### Tabella delle traduzioni dal modello E-R a quello relazionale
![[Pasted image 20251210115641.png]]
#### Esempio di progettazione logica
Guardare il file [[Esempio di Progettazione Logica.pdf]], estratto dal libro
## Normalizzazione
Esistono alcune proprietà, dette **forme normali**, che certificano la qualità dello schema di una base di dati relazionale tramite l'assenza di determinati difetti;
Quando una relazione non è normalizzata presenta ridondanze e si presta a comportamenti indesiderabili o anomali durante gli aggiornamenti. Dunque, per **normalizzazione** si intende la procedura che permette di trasformare schemi non normalizzati in schemi che
## Progettazione fisica 
