Prima di cominciare il capitolo, qualche accenno sulla sintassi che verrà utilizzata:
- Le **parentesi angolari** permettono di isolare un termine della sintassi
- Le **parentesi quadre** indicano che il termine all'interno è opzionale, ossia può non comparire oppure comparire una sola volta
- Le **parentesi graffe** indicano che il termine racchiuso può non comparire o essere ripetuto un numero arbitrario di volte
- Le **barre verticali** indicano che deve essere scelto uno tra i termini separati dalle barre (un elenco di termini in alternativa può essere racchiuso tra parentesi angolari)

Le parentesi tonde dovranno sempre essere intese come termini del linguaggio SQL, non come simboli della definizione della grammatica

Useremo poi questa tabella come esempio principale di tutto il capitolo:
![[Pasted image 20251016154754.png]]

---

SQL è il linguaggio di riferimento per le basi di dati relazionali.
Nel tempo la Structured Query Language ha subito diverse iterazioni e standardizzazioni, fino ad arrivare ad SQL-3 ma l'utilizzo di riferimento in questo caso sarà SQL-2

Occorre evidenziare una distinzione formale fra SQL e il modello dei dati relazionale: 
SQL permette ad una relazione (o tabella) di avere due o più tuple che sono identiche in tutti i valori di attributo (in generale, una tabella SQL non è un insieme di tuple, ma un multi-insieme )
Le ragioni fondamentali per cui SQL permette la presenza di duplicati sono: 
1. L’eliminazione di duplicati è una operazione costosa. 
2. Spesso l’operazione di eliminazione di duplicati non è necessaria, in quanto noto a priori che non ve ne saranno (è sufficiente che gli attributi che definiscono il risultato costituiscano una chiave). 
3. L’utente potrebbe voler vedere i duplicati delle tuple nel risultato di una interrogazione. 
4. Quando si applicano funzioni aggregate alle tuple, spesso non si vogliono eliminare i duplicati.

Tuttavia se si vuole eliminare i duplicati nel risultato di una interrogazione SQL, si può farlo mediante la parola chiave $\text{DISTINCT}$ nella clausola $\text{SELECT}$.
## Interrogazioni in SQL
### Interrogazioni semplici in SQL
Le operazioni di interrogazione in SQL vengono specificate per mezzo dell'istruzione $\text{SELECT}$, la sua struttura essenziale è:
```
SELECT [DISTINCT] ListaAttributi <- target list
FROM ListaTabelle <- clausola FROM
[WHERE Condizione] <- clausola WHERE
```

Di cui:
- **ListaAttributi** è l'elenco di nomi di attributi i cui valori devono essere ritrovati dall'interrogazione: $$ \langle ListaAttributi \rangle ::= * |\langle Attributi \rangle \{,\langle Attributo \rangle\} $$
- **ListaTabelle** è l'elenco di nomi di relazioni necessarie per elaborare l'interrogazione: $$\langle ListaTabelle \rangle ::= \langle Tabelle \rangle[\langle Ide \rangle] \{,\langle Tabelle \rangle [\langle Ide \rangle]\}$$
  $$\langle IDE \rangle ::=[as] \langle NuovoIdentificatore \rangle$$
- **Condizione** specifica la condizione che deve essere soddisfatta dalle tuple del risultato

Il significato di $\text{SELECT}$ può essere dato con le seguenti equivalenze:
![[Pasted image 20251016160923.png]]
Le operazioni più semplici si ottengono usando solo alcune clausole:
- **Selezione**: 
  ```
  SELECT * 
  FROM Ordini 
  WHERE Ammontare > 10000
  ```
- **Proiezione**:
  ```
  SELECT CognomeENome, Città 
  FROM Clienti
  ```
- **Prodotto cartesiano**:
  ```
  SELECT * 
  FROM Clienti, Ordini
  ```

Per evitare ambiguità, quando si opera sul prodotto di tabelle con gli stessi attributi, si usa la notazione con il punto:
```
SELECT Clienti.CodiceCliente, Ordini.Ammontare 
FROM Clienti, Ordini 
WHERE Clienti.CodiceCliente = Ordini.CodiceCliente
```
N.B. Questo è un esempio di join in SQL, in particolare si tratta di un equi-join;
La condizione di selezione è una congiunzione di atomi di uguaglianza, con un attributo della prima relazione ed uno della seconda.

Una notazione alternativa prevede l’associazione di identificatori alle tabelle (alias), da usare per specificare gli attributi nella notazione, così l'esempio precedente può essere scritto come:
```
SELECT c.CodiceCliente, o.Ammontare 
FROM Clienti c, Ordini o 
WHERE c.CodiceCliente=o.CodiceCliente 
```
Questa seconda notazione è indispensabile quando si deve fare il prodotto di una tabella per se stessa.




