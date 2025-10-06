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