Il modello relazionale si basa su due concetti:
- **Relazione** (formalmente in matematica)
- **Tabella** (intuitivo per tutti gli utenti)

Ricordiamo in matematica che:
Dati due insiemi A e B (detti domini della relazione) dicesi relazione tra A e B un qualsiasi sottoinsieme del loro prodotto cartesiano $(A \times B)$.

Una relazione matematica sui domini $D_{1}, D_{2}, . . . , D_{n}$ é un sottinsieme del
prodotto cartesiano $D_{1} × D_{2} × . . . \times D_{n}$. Il numero n delle componenti del
prodotto cartesiano rappresenta il grado della relazione. Il numero di elementi
(cioé n-uple) della relazione rappresenta la cardinalitá della relazione.
É bene, comunque, sottolineare le differenze che ci sono tra il concetto di
relazione ed il concetto di tabella. Infatti, poiché una relazione é un insieme di
n-uple, si ha che:
• fra le n-uple non é definito alcun ordinamento; quindi l’ordine presente
in una tabella é occasionale: due tabelle con le stesse righe ma in ordine
diverso rappresentano la stessa relazione;
• le n-uple di una relazione sono distinte l’una dall’altra, in quanto tra gli
elementi di un insieme non sono ammessi duplicati; quindi una tabella
rappresenta una relazione solo se le sue righe sono l’una diversa dall’altra.

Al tempo stesso peró, ciascuna n-upla é internamente ordinata: l’i-esimo va-
lore di ciascuna proviene dall’i-esimo dominio: (v1, v2, . . . , vn) con v1 ∈ D1, v2 ∈

D2, . . . , vn ∈ Dn. Questo implica che ci sia un ordinamento tra i domini (tra
le colonne della tabella), che é significativo ai fini dell’interpretazione dei dati
nelle relazioni. Risulta evidente come le informazioni che siamo interessati ad
![[Pasted image 20251006112024.png]]
Rispetto al modello basato su record e puntatori (gerarchico e reticolare), il modello relazionale, basato su valori, presenta diversi vantaggi:
1. **Astrazione**: Richiede di rappresentare solo ciò che è rilevante dal punto di vista di applicazione/utente.
2. **Portabilità**: essendo tutta l’informazione contenuta nei valori, è relativamente semplice trasferire i dati da un contesto ad un altro.
	Al contrario, i puntatori hanno un significato locale al singolo sistema, che non sempre è immediato esportare
3. **Indipendenza fisica dei dat**i: la rappresentazione logica dei dati non fa alcun riferimento a quella fisica, che può anche cambiare nel tempo;