Per illustrare i concetti principali dell'organizzazione fisica della basi di dati e gestione delle interrogazioni facciamo riferimento a questo schema, che illustra schematicamente le componenti di un DBMS coinvolte nel processo:
![[Pasted image 20251229152404.png]]
Nelle applicazioni di per basi di dati, le operazioni vengono affidate a un modulo detto **gestore delle interrogazioni**, che traduce le interrogazioni in una forma interna, le trasforma al fine di renderle più efficienti (tramite il modulo chiamato **ottimizzatore delle interrogazioni**) e le realizza in termini di operazioni a livello più basso (scansione, ordinamento, accesso diretto), che fanno riferimento alla struttura fisica dei file e sono eseguite da un modulo sottostante chiamato **gestore dei buffer**, che ha la responsabilità di mantenere temporaneamente porzioni della basi di dati in memoria centrale, per favorire l'efficienza garantendo allo stesso tempo affidabilità. Il gestore dei buffer invia poi al **gestore della memoria secondaria** le richieste effettive di lettura e scrittura fisica
## Memoria principale, memoria secondaria e gestione dei buffer
Ci sono due motivi fondamentali per cui le basi di dati hanno necessità di gestire dati in memoria secondaria:
- La dimensione della memoria principale non risulta quasi mai sufficiente per contenere una base di dati
- Una delle caratteristiche fondamentali per le base di dati è la persistenza, cosa che la memoria principale non può offrire poiché limitata a contenere informazioni nel lasso di tempo di esecuzione dei programmi e non oltre (quindi spegnimento della macchina o guasto).
[da finire]






























## Progettazione fisica
La fase finale nel processo di progettazione di una base di dati è quella della progettazione fisica, che, ricevendo in ingresso lo schema logico della base dei dati, le caratteristiche del sistema scelto e le previsioni sul carico applicativo, produce in uscita lo schema fisico della base di dati, costituito da effettive definizioni delle relazioni (le istruzioni $\text{CREATE TABLE}$ in SQL) e soprattutto delle strutture fisiche utilizzate con i relativi parametri.
La maggior parte delle scelte da effettuare nel corso della progettazione fisica dipende dal specifico DBMS utilizzato, quindi risulta difficile fornire una panoramica completa e di validità generale, ma esistono delle linee generali per delle basi di dati non enormi o con carichi non particolarmente complessi.

Le scelte fondamentali nella progettazione sono da ricondurre a due:
- Scelta della **struttura primaria** per ciascuna relazione, fra quelle disponibili dal DBMS
- Definizione di eventuali **indici secondari**