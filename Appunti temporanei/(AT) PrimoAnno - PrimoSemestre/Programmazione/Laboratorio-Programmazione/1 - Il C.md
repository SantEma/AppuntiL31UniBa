È un linguaggio di programmazione di tipo imperativo (un programma viene inteso come un insieme di istruzioni con un "ordine") e di natura procedurale (creare blocchi di codice sorgente come sottoprogrammi, procedure o funzioni);
I programmi sono composti da espressioni matematiche e istruzioni imperative raggruppate in procedure parametrizzate in grado di manipolare i vari tipi di dati.
Il linguaggio integra caratteristiche di linguaggio di basso livello (caratteri, numeri e indirizzi) che possono essere indicati tramite operatori aritmetici e logici di cui si servono le macchine reali.
## Strumenti per programmare in C
Per programmare in C serve:
- Un codice sorgente
- Un compilatore

Per poter facilitare questa operazione si utilizzano gli **IDE** (Integrated Development Enviroment), ambienti che aiutano il programmatore offrendo strumenti come:
- Editor di codice sorgente
- Compilatore e/o interprete
- Tool di building automatico
- Debugger
- Strumenti collaborativi (come GIT)
## Struttura del programma C
![[Pasted image 20241210142127.png]]
## Compilazione
Il programma in C viene salvato in una estensione `.c` per poi poterlo compilare;
La **compilazione** è il processo di traduzione dal linguaggio sorgente a linguaggio oggetto, comprensibile dalla macchina.
Il risultato del processo di compilazione è un file eseguibile (varia da sistema operativo).

La compilazione avviene tramite diverse fasi, il codice sorgente viene controllato dal preprocessore che rimuove eventuali commenti nel codice sorgente, interpreta speciali direttive (come # per \#include) e rileva errori sintattici per produrre alla fine un codice sorgente "espanso" pronto per essere tradotto in assembly.

Il compilatore individua eventuali errori di sintassi ma mai quelli logici, alcune volte può dare dei warning che segnalano parti di codice strane per porre attenzione ad eventuali errori logici
## Cenni sulla memoria
**Perchè è importante inizializzare una variabile quando dichiarata?** 
Il nome di una variabile è un alias di una locazione di memoria, inizializzandola si sovrascrive il valore memorizzato in quella locazione di memoria

**Perchè durante uno `scanf` è importante specificare l'operatore di indirizzo (&) e non nel `printf`?**
Durante l'acquisizione di variabili, vogliamo che queste ultime vengano salvate, perchè questo accada bisogna indicare l'indirizzo della locazione di memoria (RAM) dove è memorizzata la variabile.
Con la visualizzazione della variabile questo non è richiesto poiché usa il valore delle variabili per poterli stampare.
## Tipo di dato
Il tipo di dato in C determina il range e la tipologia di valori che una variabile può assumere, questa scelta deve essere fatta durante la progettazione considerando anche il dominio della variabile.
Una variabile più piccola consumerà meno memoria, ma sempre con la sicurezza che i valori da memorizzare nella variabile siano adeguati al tipo di dato scelto
![[Pasted image 20241210144409.png]]
