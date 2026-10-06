>[!warning] ATTENZIONE
> Questo documento è un infarinatura generale degli argomenti che il professore ha trattato all'inizio del corso, non è da prendersi come lezione vera e propria ma come "pre"-corso

La rete è vista come una fornitrice di **canali logici** attraverso cui i processi applicativi comunicano, con l'obiettivo di **nascondere la complessità della rete** al progettista di un'applicazione.

Il vero Internet quindi, quello formato da tanti diversi apparati che sono interconnessi tra loro, scompaiono dietro le interfacce semplici da utilizzare.
## Rete di calcolatori
Internet è una rete di calcolatori che interconnette miliardi di dispositivi di calcolo in tutto il mondo.
Questa rete è formata da due principali parti:
- **I nodi** (host), ossia i calcolatori di uso generale (PC, TV, smarthphone etc.). 
- **I collegamenti** (link), ossia il mezzo trasmissivo che si utilizza per mettere in comunicazione i vari nodi. Esistono vari mezzi trasmissivi guidati e non (doppino telefonico, cavo coassiale, fibra ottica,canali radio terrestri, canali radio satellitari, WiFi, reti cellulari (3G, 4G, 5G) etc.)

Per i collegamenti, esistono diverse tipologie (a stella, bus, anello) ma si contraddistinguono in **diretto** e **indiretto**
## Tipi di collegamenti tra reti
### Diretto
Esistono due tipologie di collegamenti diretti:
- **Reti PtP (Point To Point, punto a punto)**: ogni coppia di calcolatori è connessa da un link dedicato. Per $N$ calcolatori da interconnettere, servono $\frac{(N^{2} - N)}2$ collegamenti, rendendola una soluzione che non scala. 
    ![[Pasted image 20261006171035.png]]
- **Reti ad accesso multiplo**: un solo canale condiviso da tutti che costringe a gestire la contesa su chi trasmette