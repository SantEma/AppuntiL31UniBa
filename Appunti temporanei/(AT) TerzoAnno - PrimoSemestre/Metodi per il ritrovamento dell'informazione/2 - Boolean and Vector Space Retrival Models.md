# Retrivial Models
Un modello di recupero (chiamiato **Retrivial model**) è un modello che definisce come un sistema di Information Retrieval decide quali documenti sono rilevanti per una query.
Ogni modello specifica:
- La rappresentazione del documento, ossia come viene descritto un documento (ad esempio come insieme di parole, oppure come vettore di pesi dei termini
- La rappresentazione della query, ossia come viene espressa la richiesta dell'utente
- Le funzioni di "retrivial"
## Modello per IR
Un modello IR è una quadrupla $[D,Q,F,R(q_{i}, d_{j})]$ composta da:
- Un insieme di documenti, indicato con $D$
- Un insieme di query, espresse con $Q$
- Un framework per modellarli i due precedenti, indicato con $F$
- Una funzione di ritrovamento che restituisce un punteggio di rilevanza, calcolati prendendo in considerazione sia l'insieme dei documenti e query, indicato con $R(q_{i}, d_{j})$

Questo punteggio di rilevanza viene assortito generalmente in maniera decrescente 
### Tassonomia di modelli IR
Ci sono diversi modelli di ritrovamento, fondamentalmente il corso verrà basato su dati non strutturati (come i normali documenti).
Alcuni modelli di ritrovamento lavorano anche sui link (documenti legati ad altri documenti)
Altri lavorano anche su immagini, video, audio...
![[Pasted image 20261005161158.png]]
## Classi di Retrivial Models
Esistono due classi principali dei Retrivial Models:
- I **modelli booleani**, basati sulla teoria degli insiemi
- **I modelli vettoriali**
- **I modelli probabilistici** (Language Models)
### Step di pre-processing
Prima di indicizzare i documenti (o in generale dati di tipo testuale), il testo viene "ripulito" e normalizzato seguendo una metodologia precisa:
- Si eliminano caratteri indesiderati e markup (come tag HTML, punteggiatura, numeri, etc.)
- Il testo ottenuto viene viene spezzato in token, usando gli spazi come separatori
- Si effettua lo **stemming**, ossia i token vengono ridotti alla loro "radice "(ad esempio $\text{computational}\to \text{compute}$). Questo passaggio può introdurre diversi errori come perdita di contesto, errori di punteggiatura, perdita del significato della radice o parole che vanno in conflitto con la radice stessa (ad esempio $\text{policies e police} → \text{polic}$)
- Si tolgono le parole molto comuni e poco informative (chiamate **stepword**)
- Si rilevano delle frasi comuni, eventualmente con un **dizionario specifico del dominio**
- Si costruisce un **indice invertito**, dove ogni parola chiave viene associata alla lista dei documenti che la contengono
### Modello booleano
Nel modello booleano, un documento è rappresentato come un **insieme di parole chiave**.
Le sue **query** sono espresse tramite i vari connettori logici che si conoscono della teoria degli insiemi, quindi intersezione (AND), unione (OR), complementare (NOT), incluse le parentesi per indicare l'ambito
L'**output** che questo modello produce è binario, ossia il documento è rilevante oppure no, non includendo quindi match parziali o ranking (frequenza di quel termine).
#### Matrice di incidenza
> [!example] Esempio di matrice di incidenza
> ![[Pasted image 20261003190722.png]]
> In questo esempio si costruisce una **matrice termine-documento** in cui ogni cella vale 1 se l'opera contiene la parola, altrimenti vale 0 . 
> Ogni termine ha così un **vettore di incidenza** 0/1.
> Con questa rappresentazione si perde comunque il numero di occorrenze all'interno dei documenti e l'ordine delle parole (posizioni)

Questa rappresentazione spreca molto spazio in generale
#### Indice inverso

L'indice è invertito perchè in primis si inizia la ricerca dalla parola, non dal documento. Per ogni termine andiamo a memorizzare in una lista i documenti che lo contengono.

![[Pasted image 20261005163721.png]]

La query, essendo sull'indice, diviene molto più leggera rispetto al contrario. 
Il processo di analisi della collezione per la creazione dell'indice avviene soltanto una volta ma è molto laborioso.
[da finire]