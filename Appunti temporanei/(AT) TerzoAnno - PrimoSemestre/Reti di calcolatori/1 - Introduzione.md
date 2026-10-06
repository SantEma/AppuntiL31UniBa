>[!warning] ATTENZIONE
> Questo documento è un infarinatura generale degli argomenti che il professore ha trattato all'inizio del corso, non è da prendersi come lezione vera e propria ma come "pre"-corso

La rete è vista come una fornitrice di **canali logici** attraverso cui i processi applicativi comunicano, con l'obiettivo di **nascondere la complessità della rete** al progettista di un'applicazione.

Il vero Internet quindi, quello formato da tanti diversi apparati che sono interconnessi tra loro, scompaiono dietro le interfacce semplici da utilizzare.
## Rete di calcolatori
Internet è una rete di calcolatori che interconnette miliardi di dispositivi di calcolo in tutto il mondo.
Questa rete è formata da due principali parti:
- **I nodi** (host), ossia i calcolatori di uso generale (PC, TV, smarthphone, server etc.). 
- **I collegamenti** (link), ossia il mezzo trasmissivo che si utilizza per mettere in comunicazione i vari nodi. Esistono vari mezzi trasmissivi guidati e non (doppino telefonico, cavo coassiale, fibra ottica,canali radio terrestri, canali radio satellitari, WiFi, reti cellulari (3G, 4G, 5G) etc.)

Per i collegamenti, esistono diverse tipologie (a stella, bus, anello) ma si contraddistinguono in **diretto** e **indiretto**

Una rete di calcolatori può essere vista anche come un infrastruttura che fornisce servizi alle applicazioni (posta elettronica, navigazione sul Web etc.).
Ognuna di queste applicazioni è chiamata **applicazione distribuita**, in quanto coinvolgono più sistemi periferici che scambiano reciprocramente dati
## Tipi di collegamenti tra reti
### Diretto
Esistono due tipologie di collegamenti diretti:
- **Reti PtP (Point To Point, punto a punto)**: ogni coppia di calcolatori è connessa da un link dedicato. Per $N$ calcolatori da interconnettere, servono $\frac{(N^{2} - N)}2$ collegamenti, rendendola una soluzione che non scala. 
    ![[Pasted image 20261006171035.png]]
- **Reti ad accesso multiplo**: un solo canale condiviso da tutti che costringe a gestire la contesa su chi trasmette
#### Indiretto
Una soluzione veramente scalabile è il **collegamento indiretto**: i nodi periferici parlano solo con i nodi intermedi (**router, switch**) che inoltrano i messaggi da un calcolatrore all'altro.
#### Reti commutate
In una rete commutata, è necessario quindi che un unico collegamento fisico possa supportare la condivisione di più flussi di informazioni.

Ogni rete commutata trasporta, da ogni applicazione distribuita, dei messaggi definiti tramite dei **protocolli** (la maniera quindi in cui ci si comunica), spezzati poi in diversi **pacchetti**

Tra la sorgente e la destinazione, questi pacchetti viaggiano attraverso collegamenti e commutatori di pacchetto (di cui, ricordiamo, esistono due tipi principali: i router e i commutatori a livello di collegamento). I pacchetti vengono trasmessi su ciascun collegamento a una velocità pari alla velocità totale di trasmissione del collegamento stesso. Quindi, se un sistema periferico o un commutatore invia un pacchetto di L bit su un canale con velocità di R bps, il tempo di trasmissione risulta pari a L/R secondi.