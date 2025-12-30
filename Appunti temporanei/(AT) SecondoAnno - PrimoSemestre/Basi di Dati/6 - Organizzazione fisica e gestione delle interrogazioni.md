Per illustrare i concetti principali dell'organizzazione fisica della basi di dati e gestione delle interrogazioni facciamo riferimento a questo schema, che illustra schematicamente le componenti di un DBMS coinvolte nel processo:
![[Pasted image 20251229152404.png]]
Nelle applicazioni di per basi di dati, le operazioni vengono affidate a un modulo detto **gestore delle interrogazioni**, che traduce le interrogazioni in una forma interna, le trasforma al fine di renderle più efficienti (tramite il modulo chiamato **ottimizzatore delle interrogazioni**) e le realizza in termini di operazioni a livello più basso (scansione, ordinamento, accesso diretto), che fanno riferimento alla struttura fisica dei file e sono eseguite da un modulo sottostante chiamato **gestore dei buffer**, che ha la responsabilità di mantenere temporaneamente porzioni della basi di dati in memoria centrale, per favorire l'efficienza garantendo allo stesso tempo affidabilità. Il gestore dei buffer invia poi al **gestore della memoria secondaria** le richieste effettive di lettura e scrittura fisica
## Memoria principale, memoria secondaria e gestione dei buffer
Ci sono due motivi fondamentali per cui le basi di dati hanno necessità di gestire dati in memoria secondaria:
- La dimensione della memoria principale non risulta quasi mai sufficiente per contenere una base di dati
- Una delle caratteristiche fondamentali per le base di dati è la persistenza, cosa che la memoria principale non può offrire poiché limitata a contenere informazioni nel lasso di tempo di esecuzione dei programmi e non oltre (quindi spegnimento della macchina o guasto).
### Gestione dei buffer
L'interazione fra memoria centrale e memoria secondaria è realizzata nei DBMS attraverso l'utilizzo di un'apposita grande zona di memoria detta **buffer**, gestita dal DBMS in modo condiviso per tutte le applicazioni.
Il buffer è organizzato in **pagine**, che hanno dimensioni pari a un numero intero di blocchi di memoria secondaria (assumeremo che ogni blocco corrisponda a esattamente un blocco di memoria secondaria).
Il gestore dei buffer si occupa del caricamento e del salvataggio delle pagine dalla memoria centrale alla memoria di massa.
L'interfaccia del gestore offerta dal gestore del buffer è relativamente più complessa di quanto accennato, poichè non può limitarsi alle richieste di lettura e scrittura ma ha bisogno di informazioni sul prevedibile riutilizzo delle pagine (per evitarne la lettura ripetuta) e sull'eventuale necessità di effettuare immediatamente gli aggiornamenti (per garantirne l'effettuazione ed evitare che vadano persi in modo irrecuperabile in caso di guasto).

Il gestore del buffer gestisce, oltre al buffer appunto, un direttorio che per ogni pagina mantiene:
- File fisico e numero blocco corrispondete alla pagina
- Due variabili di stato: un contatore che indica quanti programmi utilizzano la pagina, un bit che indica se la pagina è “sporca”, cioè se è stata modificata

La conoscenza di questi dati è fondamentale nel momento in cui diventa necessario introdurre nuove pagine in un buffer saturo, per capire quali pagine andare a sostituire. Il buffer comunica con il sistema mediante delle operazioni primitive:
- $\text{fix}$: consiste nella richiesta di accesso ad una pagina, restituisce il riferimento alla pagina richiesta, richiede una lettura se la pagina non è nel buffer, incrementa il contatore per l’utilizzo della pagina
- $\text{setDirty}$: comunica al buffer manager che la pagina è stata modificata
- $\text{unfix}$: indica che la transazione ha concluso l’utilizzo della pagina, quindi decrementa il contatore di utilizzo di pagina
- $\text{force}$: trasferisce in modo sincrono una pagina in memoria secondaria su richiesta del gestore dell’affidabilità.

Il buffer può richiedere scritture in modo sincrono, cioè esplicitamente richiesto con una primitiva $\text{force}$, oppure in modo asincrono, quindi le scritture delle pagine modificate e conservate nel buffer vengono scritte quando il buffer lo ritiene opportuno.
### DBMS e file system
Il file system è un modulo messo a disposizione dal sistema operativo che gestisce la memoria secondaria, utilizzato dai DBMS oggi, seppur in maniera limitata, per:
- Creare ed eliminare file
- Leggere e scrivere blocchi o sequenze di blocchi contigui

In molti casi, il DBMS crea file di grandi dimensioni che utilizza per memorizzare diverse relazioni (o l'intera basi di dati), in altri invece vengono creati file in tempi successivi e può succedere che un file contenga i dati di più relazioni e che varie tuple di una relazione siano in file diversi. In sostanza, il DBMS 

























## Progettazione fisica
La fase finale nel processo di progettazione di una base di dati è quella della progettazione fisica, che, ricevendo in ingresso lo schema logico della base dei dati, le caratteristiche del sistema scelto e le previsioni sul carico applicativo, produce in uscita lo schema fisico della base di dati, costituito da effettive definizioni delle relazioni (le istruzioni $\text{CREATE TABLE}$ in SQL) e soprattutto delle strutture fisiche utilizzate con i relativi parametri.
La maggior parte delle scelte da effettuare nel corso della progettazione fisica dipende dal specifico DBMS utilizzato, quindi risulta difficile fornire una panoramica completa e di validità generale, ma esistono delle linee generali per delle basi di dati non enormi o con carichi non particolarmente complessi.

Le scelte fondamentali nella progettazione sono da ricondurre a due:
- Scelta della **struttura primaria** per ciascuna relazione, fra quelle disponibili dal DBMS
- Definizione di eventuali **indici secondari**