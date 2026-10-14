La progettazione di sistemi usabili richiede una discontinuità concettuale e metodologica profonda rispetto all'impostazione ingegneristica tradizionale. Nella progettazione tradizionale, definita **orientata al sistema** (System-Centred Design), il punto di partenza del processo è rappresentato dalla specifica formale dei requisiti funzionali. In tale contesto, l'oggetto primario di attenzione del progettista è il sistema in sé e la sua architettura hardware/software. L'utente finale riveste un ruolo marginale e passivo; gli sforzi del team di sviluppo si concentrano sulla realizzazione tecnica delle funzionalità descritte nel documento di specifica, nell'intento di raggiungere un rapporto costo/qualità ritenuto accettabile dall'organizzazione.

Al contrario, **la progettazione dell'interazione** (Interaction Design) **e la progettazione centrata sull'essere umano** (Human-Centred Design, HCD) pongono l'utente al centro dell'intero ciclo di vita del prodotto. Il progettista non si limita a implementare funzioni, ma studia preventivamente e con estremo rigore le caratteristiche psico-fisiche, le abitudini, le limitazioni cognitive e le necessità dell'utente. L'obiettivo primario si sposta dalla semplice creazione di funzionalità alla modellazione dell'interazione che intercorre tra il sistema e la persona.
### Limiti dell'approccio funzionale ed esemplificazioni pratiche
L'esperienza applicativa dimostra che progettare unicamente sulla base delle funzionalità tecniche produce sistemi rigidi e difficilmente usabili. 

> [!example] Esempio di limite di approccio funzionale
> Un esempio emblematico di questo fallimento è rappresentato dai sistemi audio-video domestici. Tali sistemi vengono tradizionalmente progettati secondo una logica modulare perfetta dal punto di vista ingegneristico: ogni componente (amplificatore, lettore DVD, televisore, decoder) offre un insieme articolato di funzioni governabili da un proprio pannello e da un telecomando dedicato. Sebbene questo garantisca un'elevata flessibilità di configurazione tecnica, l'usabilità complessiva per l'utente risulta drammaticamente compromessa, costringendolo a gestire una pluralità di interfacce complesse e non coordinate.

Analizzando il caso specifico del telecomando televisivo, emerge una profonda discrepanza tra la prospettiva del progettista e la realtà d'uso. Mentre il progettista tende a rendere accessibili sul dispositivo tutte le combinazioni di comandi potenzialmente disponibili (dando origine a dispositivi con decine di pulsanti inutilizzati), l'utente medio desidera compiere sequenze di azioni semplici, come accendere l'impianto e selezionare un canale. Un approccio orientato all'HCD avrebbe condotto a un dispositivo essenziale con pochi comandi visibili legati ai casi d'uso frequenti, relegando i comandi di configurazione avanzata a livelli di accesso secondari.
## Analisi fondamentali dell'HCD
### Le quattro analisi preliminari
Per strutturare un sistema interattivo realmente rispondente alle esigenze umane, l'Human-Centred Design stabilisce che la progettazione delle funzionalità debba essere sempre preceduta da quattro analisi fondamentali:
1. **Analisi dell'utente**: esplorazione delle caratteristiche fisiche, cognitive, psicologiche e socio-culturali del pubblico di destinazione.
2. **Analisi dei bisogni**: esame dettagliato delle necessità esplicite e implicite, nonché delle aspettative dell'utente nei confronti del sistema.
3. **Analisi del contesto**: studio dell'ambiente specifico (fisico, organizzativo, sociale e tecnologico) all'interno del quale avverrà l'interazione.
4. **Analisi dei casi d'uso**: identificazione e formalizzazione dei compiti specifici e delle sequenze di interazione utili a raggiungere scopi concreti per l'utente.
### I quattro livelli di maturità della progettazione
La qualità e la maturità di un processo di progettazione possono essere modellate secondo quattro livelli gerarchici progressivi:
- **Primo livello**: il prodotto funziona da un punto di vista strettamente tecnico e computazionale.
- **Secondo livello**: il prodotto fornisce le funzioni richieste e risponde alle specifiche, ma rimane ancorato a una prospettiva orientata al sistema (system-centred design).
- **Terzo livello**: il prodotto è facile da imparare e da usare, incarnando appieno la filosofia dell'Human-Centred Design.
- **Quarto livello**: il prodotto diventa "invisibile" durante l'uso.

Un artefatto interattivo raggiunge il massimo grado di usabilità quando assume la qualifica di prodotto "invisibile". In questa condizione, l'utente concentra le proprie risorse cognitive esclusivamente sul compito che sta svolgendo e non sullo strumento impiegato. L'interfaccia si dissolve nella percezione del soggetto; l'artefatto riacquista visibilità unicamente quando si verifica un malfunzionamento, un errore imprevisto o un'interruzione inattesa del flusso operativo.
## Ingegneria dell'usabilità e modelli del ciclo di vita del software
L'ingegneria dell'usabilità comprende, nella sua accezione più ampia, l'insieme integrato delle pratiche, dei metodi e dei processi adottati nello sviluppo dei sistemi interattivi a partire dalla fase di rilevazione dei requisiti. La disciplina si fonda su tre principi cardine imprescindibili:
1. **Focalizzazione precoce e continua sull'utente**: comprensione diretta e costante delle caratteristiche cognitive e comportamentali degli utenti lungo tutto il progetto.
2. **Misurazione empirica**: effettuazione costante di prove d'uso con l'utente mediante l'uso di metriche quantitative e analisi qualitative.
3. **Sviluppo iterativo per prototipi successivi**: affinamento progressivo della soluzione progettuale attraverso cicli ripetuti di prototipazione, test e revisione.
### Il modello "a cascata" 
Il modello di sviluppo tradizionale adottato dall'ingegneria del software è il modello "a cascata", articolato in una sequenza rigida di fasi: definizione dei requisiti, analisi e progettazione dell'architettura, specifica di progetto, codifica, integrazione e test, rilascio e manutenzione. 
![[Pasted image 20261014160901.png]]
Sebbene il modello a cascata presenti vantaggi teorici legati alla chiara definizione logica delle fasi e alla presenza di checkpoint gestionali, esso risulta strutturalmente inadeguato per la realizzazione di sistemi interattivi usabili. Il problema risiede nella presunzione di poter "congelare" i requisiti iniziali prima della realizzazione. Poiché il sistema nella sua interezza viene mostrato all'utente soltanto al termine della catena produttiva, eventuali difetti di impostazione o incomprensioni dei bisogni emergono quando il costo economico e temporale della modifica è diventato ormai proibitivo.

L'impossibilità di definire requisiti perfetti a priori trova una spiegazione teorica nel principio del ciclo compito-artefatto (Task-Artifact Cycle). Il principio stabilisce che:
$$\text{I bisogni dell'utente} \longrightarrow \text{fanno nascere lo strumento} \longrightarrow \text{lo strumento modifica i bisogni dell'utente}$$
L'introduzione di ogni nuovo manufatto tecnologico modifica inevitabilmente l'ambiente operativo dell'utente e il suo modo di agire, facendo emergere nuovi bisogni prima non ipotizzabili. Si innesca così un processo evolutivo continuo e circolare tra la tecnologia e l'essere umano e di conseguenza, la bontà di uno strumento non può mai essere valutata "sulla carta" a prescindere dall'osservazione empirica dell'uso reale.
### Il modello iterativo e la distribuzione dinamica delle risorse
Per ovviare ai limiti della rigidità sequenziale, l'ingegneria dell'usabilità adotta modelli di sviluppo iterativi per prototipi successivi. In questo framework, la progettazione avanza per approssimazioni successive: si realizza un primo prototipo preliminare, lo si sottopone a test empirico con l'utente, si analizzano le reazioni e si modificano i requisiti e l'architettura per la produzione della versione successiva.
![[Pasted image 20261014161012.png]]
Come evidenziato nel modello dello **Unified Software Development Process**, all'interno di un ciclo iterativo le diverse attività (requisiti, progettazione, prototipazione, test, rilascio) non si susseguono in modo monolitico, ma procedono in parallelo per tutta la durata del progetto. Ciò che varia da un'iterazione all'altra è il peso relativo e l'allocazione delle risorse dedicate a ciascuna attività:
- Nelle **prime iterazioni**, lo sforzo maggiore si concentra sulla definizione dei requisiti e sulla progettazione di massima, impiegando prototipi a bassa fedeltà e mock-up.
- Nelle **iterazioni intermedie e avanzate**, il carico di lavoro si sposta progressivamente verso la prototipazione ad alta fedeltà, i test di usabilità approfonditi e le attività di codifica e rilascio.

I principali **vantaggi** del modello iterativo includono:
- Visibilità precocemente garantita del prodotto (seppur parziale).
- Possibilità di sperimentare empiricamente le scelte progettuali e scartare tempestivamente quelle errate.
- Correttezza concettuale nella gestione di sistemi complessi sottoposti a co-evoluzione.

Di contro, i principali **svantaggi e criticità** comprendono:
- Difficoltà nella stima preventiva dei costi e dei tempi di sviluppo complessivi.
- Rischio di divergenza del progetto qualora le modifiche non siano gestite da un controllo rigoroso.
- Complessità di comunicazione tra committente, dirigenti e team di progetto dovuta alla fluidità delle specifiche.
### Integrazione nel ciclo a cascata e il modello "a stella"
Nel tentativo di conciliare la flessibilità dell'usabilità con i processi industriali tradizionali, sono state proposte soluzioni che integrano attività centrate sull'utente (interviste, scenari, prototipi e test) come input diretti all'interno delle tradizionali fasi a cascata.

Un approccio radicalmente non sequenziale è rappresentato dal **ciclo di vita a stella** (Star Life Cycle). In tale modello, la fase di **valutazione** occupa una posizione centrale e radiale rispetto a tutte le altre attività:
- Analisi dei compiti e analisi funzionale.
- Requisiti e specifiche.
- Progettazione concettuale e formale.
- Prototipazione.
- Implementazione.
![[Pasted image 20261014161837.png]]

Il modello a stella non impone alcun ordine di esecuzione prestabilito: il team di progettazione può avviare il processo da una qualsiasi delle punte della stella, a condizione che ogni transizione da un'attività all'altra passi obbligatoriamente attraverso un ciclo di valutazione empirica.
## Standard ISO 9241-210 per i processi HCD
Lo standard internazionale di riferimento per la progettazione dei sistemi interattivi usabili è l'**ISO 9241-210** (che ha sostituito ed evoluto lo storico ISO 13407). La norma definisce l'Human-Centred Design come:
> [!info] Definizione HCD secondo ISO 9241-210
> Un approccio allo sviluppo dei sistemi interattivi specificamente orientato alla creazione di sistemi usabili. È un'attività multidisciplinare che incorpora la conoscenza e le tecniche dei fattori umani e dell'ergonomia.

L'applicazione dell'ergonomia richiede di considerare espressamente le capacità, i limiti, le abilità e le necessità dell'essere umano. I benefici derivanti dall'adozione dello standard includono un incremento della produttività, il miglioramento della qualità del lavoro, la riduzione dei costi di formazione e supporto tecnico, e un netto aumento della soddisfazione dell'utente.
### I quattro principi caratterizzanti
Qualunque sia la metodologia specifica o il framework organizzativo adottato, lo standard ISO 9241-210 stabilisce che un processo si definisce Human-Centred solo se soddisfa quattro requisiti fondamentali:
1. **Coinvolgimento attivo degli utenti** e comprensione chiara dei requisiti dei compiti e del contesto.
2. **Assegnazione appropriata delle funzioni** tra le persone e la tecnologia.
3. **Iterazione sistematica delle soluzioni di progetto**.
4. **Progettazione multidisciplinare**, con il coinvolgimento di competenze diversificate.
### Le quattro fasi operative dello standard ISO 9241-210
Preceduto da una fase preliminare di pianificazione del processo HCD, il ciclo di vita operativo dello standard si articola in quattro attività cicliche interconnesse:
1. **Comprendere e specificare il contesto d'uso**:
	- Definire le caratteristiche psicofisiche, cognitive e culturali degli utenti.
	- Identificare le mansioni e i compiti che gli utenti dovranno eseguire.
	- Specificare l'ambiente fisico, sociale, organizzativo e tecnologico in cui avverrà l'uso.
2. **Specificare i requisiti dell'utente e di business**:
	- Formalizzare i requisiti operativi dell'utente e gli obiettivi dell'organizzazione.
	- Stabilire le metriche e gli obiettivi misurabili di usabilità.
	- Identificare eventuali vincoli legislativi, tecnici o di mercato.
3. **Produrre soluzioni di progettazione**:
	- Sviluppare proposte concettuali e di dettaglio avvalendosi di competenze multidisciplinari.
	- Rendere concrete le soluzioni realizzando simulazioni, storyboard e prototipi a fedeltà crescente.
	- Sottoporre le soluzioni a test preliminari e gestire l'iterazione delle proposte.
4. **Valutare il progetto rispetto ai requisiti**:
	- Predisporre il piano di valutazione e condurre test di usabilità empirici.
	- Fornire feedback diretto al team di sviluppo per guidare i cicli di riprogettazione.
	- Effettuare validazioni sul campo, monitoraggio a lungo termine e documentazione sistematica dei risultati.

Qualora la valutazione evidenzi che la soluzione non soddisfa integralmente i requisiti stabiliti, il processo riparte iterativamente rientrando nella fase 1, 2 o 3. Il rilascio avviene unicamente quando la soluzione soddisfa tutti i requisiti prefissati.
## Ruolo dell'utente e professioni dell'usabilità
Sebbene il modello ISO 9241-210 richieda la presenza dell'utente, le modalità del suo coinvolgimento possono variare in base alle scelte organizzative. Nel classico User-Centred Design (UCD), l'utente partecipa attivamente nella fase iniziale di rilevazione dei requisiti e nella fase di valutazione dei prototipi, rimanendo escluso dall'attività di progettazione e codifica svolta dai tecnici.

Tuttavia, si è evidenziato come l'ascolto passivo e incondizionato delle **richieste individuali degli utenti possa rivelarsi dannoso** (Human-Centred Design Considered Harmful). Accogliere ogni singola richiesta di modifica formulata dai clienti rischia di compromettere l'architettura del software, rendendolo inutilmente complesso e incoerente a ogni nuova release. 

Per superare questa criticità, si propone l'**Activity-Centred Design** (ACD). L'ACD sposta il focus dall'utente come individuo alle attività strutturate che le persone devono svolgere. Concentrandosi sulla logica e sulle regole dell'attività, l'ACD fornisce un modello concettuale coerente; i suggerimenti o le richieste degli utenti che non si armonizzano con la struttura dell'attività vengono rigorosamente scartati dal progettista.
### Le professioni dell'usabilità e i team multidisciplinari
L'adozione dei processi dell'ingegneria dell'usabilità e dell'HCD richiede competenze diversificate che superano la figura del singolo sviluppatore software. Come formalizzato dalla Usability Professional Association (UPA), i team di sviluppo multidisciplinari integrano diverse figure specialistiche:
- **User Experience Practitioner**: responsabile dell'impostazione globale dell'esperienza utente.
- **Interface Designer**: specializzato nella modellazione visiva e d'interazione delle schermate.
- **Usability Practitioner / Specialist**: esperto nella conduzione dei test d'usabilità e delle valutazioni empiriche.
- **Information Architect**: responsabile dell'organizzazione logica e strutturale delle informazioni e dei percorsi di navigazione.
- **Usability Manager**: coordinatore dei processi e del budget legato alle attività di usabilità all'interno dell'organizzazione.
### Valutazione costi e benefici
L'integrazione delle attività di usabilità richiede un investimento economico iniziale legato alla conduzione di test e alla produzione di prototipi. Tuttavia, l'analisi costi-benefici dimostra che le attività di usabilità generano un ritorno sull'investimento (ROI) ampiamente positivo:
- Riducono drasticamente i costi di riprogettazione tardiva (modificare un'idea su un prototipo cartaceo costa una frazione trascurabile rispetto alla modifica del codice sorgente già rilasciato).
- Riducono i tempi e i costi di formazione e addestramento del personale.
- Abbattono le richieste di assistenza tecnica post-vendita e i costi di help-desk.
## Istanze pratiche dell'HCD: UCD Sprint e Design Thinking
L'Human-Centred Design descritto dallo standard ISO 9241-210 costituisce un modello generale e astratto. Nello sviluppo applicativo reale, tale modello astratto viene istanziato ed eseguito attraverso metodologie concrete, tra le quali assumono primaria rilevanza lo **User-Centred Design Sprint (UCD Sprint)** e il **Design Thinking**.
### User-Centred Design Sprint (UCD Sprint)
Lo UCD Sprint è una metodologia procedurale passo-dopo-passo sviluppata per favorire la generazione di idee e la progettazione guidata all'interno dei team software. Derivato dal Design Sprint, l'UCD Sprint fornisce passaggi prescrittivi definiti che lo rendono particolarmente adatto per contesti didattici o per team poco esperti.

La metodologia si suddivide in tre macro-fasi operative:
1. **Discovery**:
	- Initial mapping: comprensione della visione d'insieme e dell'idea generale.
	- User group analysis & Interviewing: studio dei gruppi di utenti e interviste sul campo.
	- Defining UX goals: formalizzazione degli obiettivi di esperienza utente.
	- Mapping and selecting target: identificazione e selezione del bersaglio progettuale.
	- Defining design brief: documentazione formale delle specifiche di sintesi.
2. **Design**:
	- Exploring webs & Defining focus: esplorazione dei riferimenti e selezione dei compiti utente da testare.
	- Brainstorming designs & Selecting good design: produzione di schizzi preliminari e selezione della soluzione migliore.
	- Happy paths & Low-fi prototypes: modellazione dei flussi d'interazione e realizzazione di prototipi a bassa fedeltà.
3. **Reality check**:
	- User testing & Prototype review: test d'uso su prototipi cartacei e revisione con esperti.
	- Realistic clickable prototype: sviluppo di prototipi interattivi e cliccabili.
	- 5 users tests & Analysing results: esecuzione di test d'usabilità con un campione di 5 utenti e analisi dei dati.
	- Decide next steps: decisione motivata sull'avanzamento o sul riciclo del processo.
### Il Design Thinking
Il Design Thinking viene formalmente definito come un'ideologia supportata da un processo operativo correlato:
- **L'ideologia del Design Thinking**: afferma che un approccio pratico, empatico e centrato sull'utente alla risoluzione dei problemi è in grado di sbloccare l'innovazione, conducendo a elementi di differenziazione e vantaggio competitivo.
- **Il processo del Design Thinking**: definisce la sequenza di sei fasi operative attraverso le quali l'ideologia viene concretizzata.
#### Le sei fasi del processo di Design Thinking
Le sei fasi del Design Thinking comprendono:
1. **Empatizzare**: condurre ricerche sul campo per acquisire una comprensione profonda di ciò che gli utenti fanno, dicono, pensano e provano.
2. **Definire**: combinare le informazioni raccolte nell'analisi per individuare i problemi reali degli utenti e far emergere le opportunità di innovazione.
3. **Ideare**: condurre sessioni di brainstorming ad ampio spettro per generare una vasta gamma di idee creative capaci di rispondere ai bisogni insoddisfatti.
4. **Prototipare**: costruire rappresentazioni tangibili e verificabili per un sottoinsieme delle idee generate, al fine di comprendere quali componenti funzionano e quali no.
5. **Testare**: ritornare dagli utenti reali per sottoporre a verifica i prototipi, verificando se la soluzione risponde alle loro esigenze.
6. **Implementare**: mettere in pratica la visione progettuale, assicurandosi che la soluzione sia resa operativa e impatti positivamente sul contesto d'uso.

Le sei fasi si raggruppano in tre macro-aree di attività cognitiva:
- **Understand** (Empatizzare, Definire): esplorazione dello spazio del problema.
- **Explore** (Ideare, Prototipare): esplorazione dello spazio delle soluzioni.
- **Materialize** (Testare, Implementare): concretizzazione e rilascio dell'artefatto.
![[Pasted image 20261014162433.png]]
### Sintesi comparativa tra UCD Sprint, Design Thinking e ISO 9241-210
Le evidenti affinità tra UCD Sprint e Design Thinking derivano dal fatto che entrambe le metodologie condividono la medesima radice concettuale sancita dallo standard ISO 9241-210. La scelta tra l'uno o l'altro approccio dipende principalmente dalle caratteristiche del team e dagli obiettivi di lavoro:
- Lo **UCD Sprint** si rivela particolarmente indicato per contesti nei quali si necessita di una guida fortemente strutturata e prescrittiva (utile per team giovani o novizi).
- Il **Design Thinking** offre un quadro ideale per contesti orientati all'innovazione aperta, alla riflessione strategica e al coinvolgimento di stakeholder aziendali eterogenei.

In entrambi i casi, l'elemento discriminante risiede nel rispetto indiscutibile dei principi dell'Human-Centred Design: focalizzazione sull'utente, misurazione empirica e iterazione costante.

