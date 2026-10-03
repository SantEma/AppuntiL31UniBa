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
- Una funzione di classificazione che restituisce un punteggio di rilevanza, calcolati prendendo in considerazione sia l'insieme dei documenti e query, indicato con $R(q_{i}, d_{j})$

Questo punteggio di rilevanza viene assortito generalmente in maniera decrescente 
### Tassonomia di modelli IR
[da rivedere]
## Classi di Retrivial Models
Esistono due classi principali dei Retrivial Models:
- I **modelli booleani**, basati sulla teoria degli insiemi
- **I modelli vettoriali**
- **I modelli probabilistici**
[da rivedere]
### Step di pre-processing
Prima di indicizzare i documenti, il testo viene "ripulito" e normalizzato seguendo una metodologia precisa:
- Si eliminano caratteri indesiderati e markup (come tag HTML, punteggiatura, numeri, etc.)
- Il testo ottenuto viene viene spezzato in token, usando gli spazi come separatori
- I token vengono ridotti alla loro "radice", così che il recupero non dipenda da tempo verbale, numero
  Questo passaggio può introdurre diversi errori come perdita di contesto