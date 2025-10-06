La programmazione ad oggetti cerca di creare una rappresentazione della realtà.
Nel mondo reale qualsiasi cosa è un oggetto, e ogni oggetto ha uno **stato** e un **funzionamento**
## Oggetti
Alcuni esempi che troviamo nel mondo reale sono:
![[Pasted image 20251006084708.png]]Come si può vedere non ci sono superficialità, ma **dettagli utili soltanto allo scopo che si vuole raggiungere**
### Oggetti software
Sostanzialmente gli **oggetti software** sono una mappatura di quello che si trova nel mondo reale.
Lo stato viene memorizzato negli **attributi** o **variabili**, mentre il loro funzionamento si ritrova nei **metodi** o **funzioni**
![[Pasted image 20251006085025.png]]
Lo stato è dato dai **valori che assumono i suoi attributi** (Es. la bicicletta ha marcia corrente=5, velocità=30 km/h)
Il funzionamento è dato dai suoi metodi (es. bicicletta -> cambia marcia), gli stessi metodi **possono modificare** lo stato dell'oggetto agendo sui suoi attributi
#### Vantaggi
Gli oggetti hanno diversi vantaggi:
- **Modularità**: ogni oggetto è indipendente dagli altri, il suo codice può essere gestito separatamente (avendo vita propria)
- **Information hiding**: i dettagli implementativi di ogni oggetto sono nascosti dagli altri oggetti, che interagiscono solo tramite i suoi metodi
- **Riutilizzo**: oggetti scritti da altri possono facilmente essere ri-utilizzati
- **Sostituzione**: ogni oggetto può essere facilmente essere sostituito

### Classe
Nel mondo reale molti oggetti condividono delle caratteristiche simili, per questo si raggruppano sia per queste sia per funzionamento simile, le riconosciamo come **classi** (che abbiamo visto nei vari linguaggi)
(Es. tutte le biciclette che abbiamo posseduto sono delle biciclette)
![[Pasted image 20251006085830.png]]
Riconosciamo gli **oggetti** o **istanze** come specifiche di classi 
#### Ereditarietà
Alcune classi di oggetti hanno delle caratteristiche in comune, ma si possono avere caratteristiche in più o comportamenti specifici diversi dalla classe originale, **l’ereditarietà** permette quindi ad una classe di ereditare attributi e metodi di un’altra classe
Riprendendo l'esempio delle biciclette:
![[Pasted image 20251006090115.png]]
L’ereditarietà è un meccanismo potente per rappresentare una gerarchia tra classi
### Programmazione Object Oriented
Nella programmazione imperativa è possibile accedere alle variabili globali da qualsiasi parte del programma;
In questa maniera andiamo a perdere **l’information-hiding**,nei programmi molto grandi diventa praticamente ingestibile, i moduli non possono essere sviluppati indipendentemente (alcune funzioni potrebbero leggere o scrivere una variabile globale in contemporanea ad altre)

La soluzione sarebbe quella di incapsulare ogni variabile globale in un modulo insieme ad un gruppo di operatori che possono accedere a tali variabili in modo che gli altri moduli possono accedere indirettamente alle variabili solo per mezzo di questi operatori
### I metodi
Gli oggetti sono costituiti da **dati privati** e **operazioni permesse su tali dati**, tra di loro comunicano tramite passaggio di **messaggi** (chiamata a procedura, detta metodo, che appartiene all'oggetto)

Nei linguaggi ad oggetti come C++ nelle definizioni di classi si avranno metodi accessibili da qualunque punto nel programma e altri che potranno essere accessibili solo alla classe stessa (in C questo non è possibile)
![[Pasted image 20251006091231.png]]
