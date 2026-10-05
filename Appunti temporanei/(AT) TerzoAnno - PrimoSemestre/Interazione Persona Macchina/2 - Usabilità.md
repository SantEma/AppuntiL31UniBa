## Accetabilità di un sistema software
Secondo J. Nielsen, l'accettabilità di un sistema software dipende da più attributi: compatibilità, affidabilità, utilità, utilizzabilità (usabilità) e altri ancora. 
L'**usabilità** si articola a sua volta in cinque componenti: 
- Facilità di apprendimento; 
- Facilità d'uso;
- Facilità di memorizzazione; 
- Numero ridotto di errori; 
- Soddisfazione nell'uso;
### ISO su usabilità
Esistono diversi standard ISO sull'usabilità:
- **ISO 9126**: qualità del prodotto software.
- **ISO 9241-11**: definizioni e concetti di usabilità (sostituisce la versione del 1988).
- **ISO 9241-210**: processi di progettazione centrata sull'uomo per sistemi interattivi.
- **ISO 9241-220**: processi per abilitare, eseguire e valutare la progettazione centrata sull'uomo nelle organizzazioni (sostituisce ISO TR 18529).
- **ISO/IEC 25066**: formato comune di settore per i rapporti di valutazione dell'usabilità.
- **ISO/IEC 25022**: misura della qualità in uso (efficacia, efficienza, soddisfazione), in sostituzione di ISO TR 9126-4.
- **ISO/IEC 25023**: misura della qualità del sistema e del prodotto software (include le misure degli attributi di usabilità), in sostituzione di ISO/IEC TR 9126-2 e 9126-3.

Quello con la definizione più interessante per noi è l'ISO 9241-11:
> [!info] Definizione di usabilità secondo 9241-11
>L'usabilità è la misura in cui un prodotto può essere usato da specifici utenti per raggiungere specifici obiettivi con efficacia, efficienza e soddisfazione in uno specifico contesto d'uso.

Ma esistono anche altri standard ISO attualmente importanti
#### ISO 9126
Lo standard ISO 9126-1 (_Information Technology, Software Product Quality_) sottolinea l'importanza di progettare per la qualità, intesa come capacità interna ed esterna del prodotto di supportare il raggiungimento degli obiettivi degli utenti e delle loro organizzazioni. L'usabilità è definita come la capacità del prodotto software di essere compreso, appreso, usato e di risultare attraente per l'utente, in condizioni specificate.
> [!example] Schema dell'ISO 9126
> ![[Pasted image 20261005142147.png]]

Oggi, questo standard è considerato obsoleto dallo ISO/IEC 25000
#### ISO 25000
Sviluppato dal gruppo di lavoro ISO/SC7 WG6, lo standard **SQuaRE** (_Systems and Software Quality Requirements and Evaluation_) è composto da 21 sotto-progetti che definiscono modelli di qualità del prodotto (software, dati, servizi, qualità in uso) e funge da **strumento di raccordo** tra le diverse visioni in gioco (committente, fornitore, progettista e utente finale) promuovendo un approccio sinergico tra tutte le parti.
##### ISO/IEC 25000
Pubblicato nel 2011, definisce le caratteristiche di qualità del software e del sistema. 
Non entra nel merito di **"cosa" fa** il software (proprietà funzionali), ma di **"come" opera** (proprietà qualitative). Come detto precedentemente, sostituisce l'ISO/IEC 9126-1 e riprende alcune definizioni degli standard ISO 9241-11 (usabilità), 14 (dialoghi a menu) e 110 (ergonomia dell'interazione uomo-sistema).

Questo standard prevede 3 tipi di qualità:
- **Qualità interna**: verificabile con ispezioni o strumenti di analisi statica sul codice;
- **Qualità esterna**: verificabile da tecnici con test dinamici, in ambienti simulati;
- **Qualità in uso**: verificabile in ambienti reali (o simulati) **con la partecipazione degli utenti finali**, per individuare in modo affidabile e concreto le difficoltà che incontrano interagendo con il sistema.

### Le tre dimensioni sull'usabilità
L'usabilità si misura sempre rispetto a 3 parametri:
- **Utenti specifici**; 
- **Obiettivi specifici**; 
- **Contesto d'uso specifico**,

Non esiste quindi un'usabilità "assoluta", è sempre relativa a chi usa il prodotto, per fare cosa e dove.
#### Comprendere gli utenti
Comprendere le persone nei contesti in cui vivono, lavorano e apprendono è fondamentale per aiutare i designer a progettare prodotti interattivi che offrano buone esperienze e rispondano ai bisogni reali. 

> [!example] Esempio di comprensione degli utenti
> Uno strumento di pianificazione collaborativa per una **missione spaziale** avrà esigenze completamente diverse da uno strumento per clienti e agenti di vendita in un **negozio di arredamento**, pur essendo entrambi "strumenti collaborativi".

Ciò che funziona per un gruppo di utenti può essere del tutto inappropriato per un altro: i prodotti interattivi vanno progettati **diversamente per tipi diversi di utenti**.
