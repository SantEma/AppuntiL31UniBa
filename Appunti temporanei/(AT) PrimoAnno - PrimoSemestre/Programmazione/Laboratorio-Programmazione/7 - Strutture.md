## Tipi definiti dall'utente: i record
Un record è una collezione di informazioni riguardanti uno specifico oggetto. La struttura del record è determinata dalla natura dell'oggetto che si vuole rappresentare. Per definire una struttura, è necessario specificare tutti i singoli elementi che la compongono.
### Il tipo struttura in C
In C, un record si definisce utilizzando la `structure type definition`, che permette di creare variabili con una determinata struttura. La sintassi è la seguente:
```c
typedef struct {
    tipo1 comp1;
    tipo2 comp2;
    ...
} nome_struttura;
```
### Riferimento a componente
Per manipolare ogni singola componente della struttura, si utilizza l'operatore di selezione `.` che divide il nome della struttura dal nome della componente. Ad esempio:
```c
studente_t stud;
stud.nome; // si riferisce al nome
stud.cognome; // si riferisce al cognome
stud.matricola; // si riferisce alla matricola

complesso_t numero;
numero.parte_reale; // si riferisce alla parte reale
numero.parte_immaginaria; // si riferisce alla parte immaginaria
```
### Riferimento alla struttura
È possibile riferirsi all’intera struttura utilizzando il nome della variabile dichiarata del tipo struttura. Le istruzioni di assegnazione creano una copia della struttura che può essere manipolata indipendentemente dalla struttura originaria:
```c
studente_t stud, stud2;
stud2 = stud;

complesso_t numero, numero2;
numero2 = numero;
```
### Array di strutture
È possibile combinare la definizione di un array con la definizione di una struttura per utilizzare collezioni di dati che contengano elementi simili e a loro volta composti da componenti differenti:
```c
typedef struct {
    char cognome[20];
    char nome[20];
} studente_t;

studente_t studente[MAX]; // definizione array di studente
```
Ciò consente di memorizzare molteplici informazioni relative a più studenti, accedendo a ogni record come un elemento dell’array.