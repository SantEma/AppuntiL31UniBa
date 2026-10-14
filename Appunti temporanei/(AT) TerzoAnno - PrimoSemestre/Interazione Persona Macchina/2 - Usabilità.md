## Accetabilità di un sistema software
Secondo J. Nielsen, l'accettabilità di un sistema software dipende da più attributi: compatibilità, affidabilità, utilità, utilizzabilità (usabilità) e altri ancora.
Il modello di Nielsen scompone l'usabilità in cinque attributi quantificabili:
- L'apprendibilità (**learnability**), che misura la facilità con cui un utente novizio apprende il funzionamento base del sistema.
- L'efficienza (**efficiency**), che indica la velocità e la produttività che un utente esperto raggiunge a regime.
- La memorabilità (memorability), ossia la facilità con cui un utente occasionale ripristina la familiarità operativa dopo un periodo di inutilizzo.
- La gestione degli errori (**errors**), che considera la frequenza di errore, la gravità e la presenza di meccanismi di ripristino rapido.
- La soddisfazione (**satisfaction**), definita come la piacevolezza e il comfort soggettivo derivanti dall'utilizzo del prodotto.
### ISO su usabilità
La trattazione formale dell'usabilità trova il proprio fondamento normativo negli standard stilati dall'International Organization for Standardization.

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

L'usabilità varia inoltre in funzione del tempo e dell'evoluzione dell'utilizzatore, il cui profilo si modifica lungo la traiettoria che va dallo stadio di utente novizio, passando per utente principiante e competente, fino allo stadio di utente esperto.
L'usabilità varia inoltre in funzione del tempo e dell'evoluzione dell'utilizzatore, il cui profilo si modifica lungo la traiettoria che va dallo stadio di utente novizio, passando per utente principiante e competente, fino allo stadio di utente esperto.

In sede di progettazione, il rapporto tra la facilità di apprendimento (**learnability**) e la memorabilità (**memorability**) determina la forma delle curve di prestazione nel tempo:
- La *Curva A* rappresenta un sistema progettato con focus sull'utente novizio o occasionale. Tale curva evidenzia una rapida crescita di usabilità e prestazioni nelle fasi iniziali dell'interazione, consentendo all'utente di essere subito operativo, ma stabilizzandosi su un plateau asintotico a basso livello di efficienza massima.
- La *Curva B* rappresenta un sistema progettato con focus sull'utente continuativo ed esperto. La pendenza iniziale è più dolce e richiede un investimento di tempo significativo per l'apprendimento, ma consente a regime di raggiungere livelli di efficienza decisamente superiori.
![[Pasted image 20261014114725.png]]

Un sistema ideale deve permettere all'utente continuativo di evolvere da novizio ad esperto raggiungendo la massima efficienza, garantendo al contempo un'elevata memorabilità per consentire all'utente occasionale di riprendere l'uso a distanza di tempo senza dover consultare la documentazione. La memorabilità è fondamentale nei sistemi a uso sporadico ma critico, come i sistemi di gestione delle emergenze o degli allarmi.