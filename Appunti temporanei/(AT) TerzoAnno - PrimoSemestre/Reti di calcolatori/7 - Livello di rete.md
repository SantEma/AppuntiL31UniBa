Il ruolo principale del livello di rete è quello di trasferire pacchetti da un host a un altro. Per fare questo è possibile identificare due importanti funzioni:
- **Inoltro (forwarding)**: quando un router riceve un pacchetto, lo deve trasferire sull’appropriato collegamento di uscita.
- **Instradamento (routing)**: il livello di rete deve determinare il percorso che i pacchetti devono seguire tramite algoritmi di instradamento (algoritmi di routing).

Con inoltro quindi faremo riferimento all'azione locale con cui il router trasferisce i pacchetti da un’interfaccia di ingresso a quella di uscita. Poiché l’inoltro avviene su scala temporale molto piccola, dell’ordine di pochi nanosecondi, è usualmente implementato in hardware. Con instradamento indicheremo, invece, il processo globale di rete che determina i percorsi dei pacchetti nel loro viaggio dalla sorgente alla destinazione.

Per inoltrare i pacchetti, i router estraggono da uno o più campi dell’intestazione (per esempio il campo Indirizzo di destinazione) i loro valori che utilizzano come indice nella **tabella di inoltro** (tabella di forwarding o forwarding table), un elemento chiave di qualsiasi router. Il risultato indica a quale interfaccia di uscita il pacchetto debba essere diretto.

