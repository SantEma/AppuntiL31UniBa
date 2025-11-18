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
- **Progettazione fisica**: produce lo schema fisico che arricchisce lo schema logico con informazioni relative all’organizzazione fisica dei dati. Il modello fisico di riferimento dipende dallo specifico DBMS scelto.