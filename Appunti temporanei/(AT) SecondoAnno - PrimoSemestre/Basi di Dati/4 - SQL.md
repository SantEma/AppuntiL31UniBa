SQL è il linguaggio di riferimento per le basi di dati relazionali.
Nel tempo la Structured Query Language ha subito diverse iterazioni e standardizzazioni, fino ad arrivare ad SQL-3.
L'utilizzo di riferimento in questo caso sarà SQL-2
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