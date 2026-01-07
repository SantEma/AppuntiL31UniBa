Le basi di dati semantiche sono collezioni di dati che possono essere arricchite con metadati che possono avere delle definizioni formali definite in vocabolari condivisi chiamati ontologie. Nascono dall'evoluzione dei linguaggi a oggetti e dei linguaggi logici e dall'evoluzione del Semantic Web. 
L’obbiettivo è fornire una descrizione dei dati (annotazioni) con conformazione irregolare attraverso linguaggi con capacità espressiva crescente in grado di fornire informazioni dalle annotazioni. L’innovazione consiste nella costruzione di collezioni di dati aperte e connesse (linked open data).
## Modello dei dati RDF
L'RDF È un modello per rappresentare informazioni sul Web sotto forma di **triple** (soggetto, predicato, oggetto), che formano un grafo orientato. Un esempio di tripla RDF è la frase: $<Verdi HaComposto Traviata>$.
Le triple possono essere rappresentate graficamente mediante un grafo nel quale, per ogni tripla $(s, p, o)$, esiste un arco orientato con etichetta $p$ tra un nodo con etichetta $s$ e un nodo con etichetta $o$. In genere, le etichette usate in RDF possono appartenere a un vocabolario o un’ontologia predefinita, e ciò può consentire di derivare ulteriore conoscenza. Ogni vocabolario (namespace) utilizzato in un’stanza di RDF è introdotto da un’istruzione $\text{prefix}$ che consente di estrarre le etichette presenti nel vocabolario. Per esempio con il comando $\text{@prefix o: <http://www.polimi.it/ceri/opera>}$ si assegna al prefisso ‘o:’ l’indirizzo web dell’ontologia che si trova appunto tramite quel link.
La differenza con le basi di dati relazionali consiste nel fatto che, per queste ultime i dati possono essere rappresentati esclusivamente con uno schema corrispondente, mentre con le basi semantiche i dati hanno significato a prescindere dallo schema corrispondente, il quale può anche non essere definito.

I dati rappresentabili in RDF sono di tre categorie:
1. **IRI (Internationalized Resource Identifier)**: sono stringhe che identificano univocamente tutte le risorse disponibili sul Web in base a una notazione standard.
2. **Letterali**: sono valori che descrivono le risorse presenti nell’istanza RDF.
3. Blank nodes: sono identificatori usati per rappresentare risorse anonime, sintatticamente preceduti dal carattere underscore 

Nelle triple RDF il soggetto può essere un IRI o un blank node, il predicato è un IRI e l’oggetto può essere IRI, letterale o blank node.
Esempio:
![[Pasted image 20260107165218.png]]
L’istanza precedente poteva essere scritta come sequenza di triple in conformità con la notazione RDF/XML, mentre con la notazione N3 quando due tuple condividono il soggetto si usa il separatore ‘;’ (punto e virgola), quando condividono sia soggetto sia predicato, si usa ‘,’ (virgola), ottenendo in questo modo una notazione più compatta
## RDF Schema
Il modello di dati fin qui utilizzato è un modello utile a esprimere conoscenza elementare in forma di triple, ma non possiede di per se la capacità di classificare risorse sulla base delle loro caratteristiche semantiche. L’aggiunta della semantica è possibile con l’estensione RDFS di RDF, che aggiunge a RDF costrutti per descrivere **metadati**, ovvero struttura e proprietà di istanze RDF. Per ciò, RDFS costituisce lo strumento per definire lo schema di un’istanza RDF. I principali costrutti messi a disposizione da RDFS sono le **classi** e le **proprietà**. Una classe RDFS consente di definire insiemi di risorse di un’istanza RDF che hanno caratteristiche comuni. Una proprietà RDFS consente di definire il tipo di soggetto e oggetto dei predicati in un’istanza RDF (in termini matematici significa definire dominio e range di una funzione).

**Esempio**: 
Il RDFS è inoltre possibile definire relazioni di generalizzazione tra classi e tra proprietà
![[Pasted image 20260107165407.png]]
La semantica di RDFS definisce l’ereditarietà, infatti se i compositori sono artisti e gli artisti sono persone, allora i compositori sono persone. Similmente si può definire come casi particolari di altre proprietà utilizzando il predicato$\text{rdfs:SubPropertyOf}$.
## SPARQL 1.0
SPARQL (Simple Protocol and RDF Query Language) è un linguaggio di interrogazione con caratteristiche molto simili a SQL, proposto dal W3C per interrogare dati descritti in RDF. In questo linguaggio, alle classiche forme di interrogazione si affiancano altre forme:
- $\text{SELECT}$ per interrogazioni classiche
- $\text{DESCRIBE}$ per ottenere descrizione di risorse in un istanza RDF interrogabile in SPARQL (detto endpoint)
- $\text{ASK}$ per sapere se specifici termini sono disponibili nell'endpoint
- $\text{CONSTRUCT}$ per costruire un nuovo grafo RDF a partire da una interrogazione
### Sintassi
Una query in SPARQL 1.0 è composta da cinque parti:
1. Clausola opzionale $\text{PREFIX}$ per introdurre vocabolari
2. Il risultato prodotto dalla query
3. Gli endpoin consultati, tramite le clausole $\text{FROM}$ e $\text{FROM NAMED}$, indicano una IRI da cui leggere l’istanza RDF
4. La parte centrale della query costituita dalla clausola $\text{WHERE}$, consente di esprimere condizioni di pattern matching tra la query stessa (triple pattern) e l’istanza RDF su cui opera