Il modello relazionale si basa su due concetti:
- **Relazione** (formalmente in matematica)
- **Tabella** (intuitivo per tutti gli utenti)

Ricordiamo in matematica che:
Dati due insiemi $A$ e $B$ (detti domini della relazione), dicesi relazione tra $A$ e $B$ un qualsiasi sottoinsieme del loro prodotto cartesiano $(A \times B)$.

Una relazione matematica sui domini $D_{1}, D_{2}, . . . , D_{n}$ è un sottoinsieme del prodotto cartesiano $D_{1} × D_{2} × . . . \times D_{n}$. 
Il numero $n$ delle componenti del prodotto cartesiano rappresenta il grado della relazione, il numero di elementi (cioè n-uple) della relazione rappresenta la **cardinalità** della relazione.
È bene, comunque, sottolineare le differenze che ci sono tra il concetto di relazione ed il concetto di tabella; 
Infatti, poiché una relazione è un insieme di $n$-uple, si ha che:
- fra le $n$-uple non è definito alcun ordinamento, quindi l’ordine presente in una tabella è occasionale: due tabelle con le stesse righe ma in ordine diverso rappresentano la stessa relazione;
- le $n$-uple di una relazione sono distinte l’una dall'altra, in quanto tra gli elementi di un insieme non sono ammessi duplicati, quindi una tabella rappresenta una relazione solo se le sue righe sono l’una diversa dall'altra.

Al tempo stesso però, ciascuna $n$-upla è internamente ordinata: l’$i$-esimo valore di ciascuna proviene dall'$i$-esimo dominio:
$$(v_{1}, v_{2}, \dots , v_{n}) \quad \text{con} \quad v_{1} \in D_{1}, v_{2} \in D_{2},\dots,v_{n} \in D_{n}$$
Questo implica che ci sia un ordinamento tra i domini (tra le colonne della tabella), che è significativo ai fini dell’interpretazione dei dati nelle relazioni. 
Risulta evidente come le informazioni che siamo interessati ad organizzare nelle relazioni dei DB abbiano una struttura riconducibile a quella dei record: una relazione è sostanzialmente un insieme di record omogenei, cioè definiti sugli stessi campi, di conseguenza attribuendo ad ogni dominio un nome identificativo, detto attributo, l’ordinamento dei domini (quindi la posizione delle colonne nella tabella) diventa irrilevante.
![[Pasted image 20251006112024.png]]
Rispetto al modello basato su record e puntatori (gerarchico e reticolare), il modello relazionale, basato su valori, presenta diversi vantaggi:
1. **Astrazione**: Richiede di rappresentare solo ciò che è rilevante dal punto di vista di applicazione/utente.
2. **Portabilità**: essendo tutta l’informazione contenuta nei valori, è relativamente semplice trasferire i dati da un contesto ad un altro.
	Al contrario, i puntatori hanno un significato locale al singolo sistema, che non sempre è immediato esportare
3. **Indipendenza fisica dei dati**: la rappresentazione logica dei dati non fa alcun riferimento a quella fisica, che può anche cambiare nel tempo;

Possiamo a questo punto riassumere le definizioni relative al modello relazionale distinguendo il livello degli schemi da quello delle istanze:
- Uno **schema di relazione** è costituito da un simbolo $R$, detto nome della relazione e da un insieme di attributi $X = \{ A_{1}, A_{2}, \dots , A_{n} \}$, il tutto di solito indicato con $R(X)$. 
  A ciascun attributo è associato un dominio, come già visto.
- Uno **schema di basi** di dati è un insieme di schemi di relazione con nomi diversi: $$R = \{R_{1}(X_{1}), R_{2}(X_{2}), \dots , R_{n}(X_{n})\}$$
- **Una istanza di relazione (o relazione)** su uno schema $R(X)$ è un insieme di $r$ tuple su $X$.
- **Una istanza di base di dati (o base di dati)** su uno schema $R = \{R_{1}(X_{1}), R_{2}(X_{2}), \dots , R_{n}(X_{n}\}$ è un insieme di relazioni $r = \{r_{1}, r_{2},\dots , r_{n}\}$, dove ogni $r_{i}$, per $1 \leq i \leq n$, è una relazione sullo schema $R_{i}(X_{i})$.
![[Pasted image 20251006120215.png]]

## Valori nulli o incompleti
Quando si aggiunge una tupla ad una relazione può non essere possibile specificare il valore di un attributo per alcune ragioni come: 
- Valore non applicabile
- Valore sconosciuto
- Valore senza informazioni
Questa assenza di valore si potrebbe ovviare usando un valore del dominio per rappresentare l'assenza (0 per esempio), ma non va bene per 2 ragioni specifiche:
- **Si richiede un valore mai utilizzato per valori significativi** (generalmente impossibile a meno di condizioni specifiche)
- **L'uso di valori del dominio genera confusione**: la distinzione tra valori veri e fittizi è nascosta, costringendo i programmi che accedono alla base di dati di tenerne conto, distinguendo opportunamente e tenendo conto di quali sono in ciascun caso

Per poter rappresentare in modo semplice e comodo si estende la possibilità di inserire un **valore nullo**, ben distinto dai valori del dominio e aggiunto proprio per lo scopo
Qualche esempio:
![[IMG_0010.jpg]]

Alcune considerazioni da fare sono:
- Il valore nullo sulla data di nascita è ammissibile, poiché si può pensare che non sia un dato essenziale in questo contesto
- Il valore nullo sul numero di matricola impedisce di stabilire correlazioni fra tuple di relazioni diverse.
-  La presenza di più valori nulli in una tupla può rendere inutilizzabili le altre informazioni nella tupla.
- La presenza di più valori nulli in una relazione può causare problemi sull'identitá delle tuple
I sistemi relazionali permettono di specificare per ciascun attributo di una relazione se esso può assumere il valore nullo oppure se per esso vale il vincolo not-null e quindi non può assumere il valore nullo.
## Vincoli di integrità
Non è corretto dire che qualsiasi insieme di tuple (insieme di dati) sullo schema rappresenti informazioni corrette per l'applicazione, considerando per esempio l'esempio precedente:
![[IMG_0010.jpg]]
- Nella tupla ESAMI non si potrebbe avere un voto pari a 36
- Nella tupla STUDENTI non si potrebbero avere due studenti con stessa matricola 
ed etc.

Per poter evitare situazioni come queste viene introdotto il concetto di **vincolo di integrità** come proprietà che deve essere soddisfatta dalle istanze che rappresentano informazioni corrette per l'applicazione.
Ogni vincolo può essere visto come un predicato che assegna valori vero o falso se queste soddisfano le condizioni o no.

È possibile classificare i vincoli a seconda degli elementi di una base di dati che ne sono coinvolti, in particolare ci sono due categorie:
- **Vincolo intrarelazionale**: se il suo soddisfacimento è definito rispetto alle singole relazioni della base di dati (per esempio i casi descritti sopra), talvolta il coinvolgimento riprende direttamente le tuple separatamente chiamato vincolo di **tupla**
- **Vincolo interreleazionale**: se coinvolge più relazioni 
## Chiavi
[da completare]
### Vincoli di integrità referenziale
[da completare]