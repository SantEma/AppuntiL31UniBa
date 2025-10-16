SQL è il linguaggio di riferimento per le basi di dati relazionali.
Nel tempo la Structured Query Language ha subito diverse iterazioni e standardizzazioni, fino ad arrivare ad SQL-3 ma l'utilizzo di riferimento in questo caso sarà SQL-2


Occorre evidenziare una distinzione formale fra SQL e il modello dei dati relazionale: 
SQL permette ad una relazione (o tabella) di avere due o più tuple che sono identiche in tutti i valori di attributo (in generale, una tabella SQL non è un insieme di tuple, ma un multi-insieme )
Le ragioni fondamentali per cui SQL permette la presenza di duplicati sono: 
1. L’eliminazione di duplicati è una operazione costosa. 
2. Spesso l’operazione di eliminazione di duplicati non è necessaria, in quanto noto a priori che non ve ne saranno (è sufficiente che gli attributi che definiscono il risultato costituiscano una chiave). 
3. L’utente potrebbe voler vedere i duplicati delle tuple nel risultato di una interrogazione. 
4. Quando si applicano funzioni aggregate alle tuple, spesso non si vogliono eliminare i duplicati.

Tuttavia se si vuole eliminare i duplicati nel risultato di una interrogazione SQL, si può farlo mediante la parola chiave distinct nella clausola select.
## Interrogazioni semplici in SQL
Le operazioni di interrogazione in SQL vengono specificate per mezzo dell'istruzione $\text{SELECT}$, la sua struttura essenziale è:
```
SELECT [DISTINCT] ListaAttributi
FROM ListaTabelle
[WHERE Condizione]
```

Le tre parti di cui si compone un'istruzione $\text{SELECT}$ vengono spesso chiamate **clausola** $\text{SELECT}$ (detta pure target list),**clausola** $\text{FROM}$ e **clausola** $\text{WHERE}$.
Il comando select è una combinazione di:
- Prodotto cartesiano (clausola $\text{SELECT}$)
- Selezione (clausola $\text{WHERE}$)
- Proiezione (target list)