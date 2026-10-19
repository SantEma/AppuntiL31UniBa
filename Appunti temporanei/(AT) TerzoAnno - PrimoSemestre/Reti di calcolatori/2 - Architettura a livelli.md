L'**architettura a livelli** è il paradigma strutturale fondamentale con cui vengono progettati protocolli, hardware e software di rete. Consiste nel decomporre un sistema di comunicazione complesso in una pila ordinata di livelli di astrazione.

Ogni livello del sistema offre dei **servizi**, che effettuano determinate azioni all'interno di se stesso e utilizzando i servizi del livello immediatamente inferiore.

> [!example] Esempio di servizi forniti da un livello e da quello immediatamente sotto
> Un servizio a livello $n$ può implementare la consegna affidabile dei messaggi da un lato della rete all'altro. Ciò può essere implementato utilizzando un servizio di consegna dei messaggi non affidabile da lato a lato, fornito dal livello $n-1$ e aggiungendo a livello $n$ la funzionalità di determinare e ritrasmettere i messaggi persi

Un livello di protocolli può sempre essere implementato via software, hardware o un misto dei due precedenti.

La **stratificazione** dei protocolli rende la gestione delle componenti molto più semplice, poichè se ne può aggiornare e/o implementarne una nuova senza impattare i livelli inferiori, ma c'è sempre una concreta possibilità di introdurre funzionalità duplicate in un livello e/o di richiedere informazioni presenti solo in un altro livello (violando lo scopo della separazione dei livelli)