## Accetabilità di un sistema software
Secondo J. Nielsen, l'accettabilità di un sistema software dipende da più attributi: compatibilità, affidabilità, utilità, utilizzabilità (usabilità) e altri ancora.
Il modello di Nielsen scompone l'usabilità in cinque attributi quantificabili:
- L'apprendibilità (**learnability**), che misura la facilità con cui un utente novizio apprende il funzionamento base del sistema.
- L'efficienza (**efficiency**), che indica la velocità e la produttività che un utente esperto raggiunge a regime.
- La memorabilità (memorability), ossia la facilità con cui un utente occasionale ripristina la familiarità operativa dopo un periodo di inutilizzo.
- La gestione degli errori (**errors**), che considera la frequenza di errore, la gravità e la presenza di meccanismi di ripristino rapido.
- La soddisfazione (**satisfaction**), definita come la piacevolezza e il comfort soggettivo derivanti dall'utilizzo del prodotto.
### Standard ISO sull'usabilità
La trattazione formale dell'usabilità trova il proprio fondamento normativo negli standard stilati dall'International Organization for Standardization.

Esistono diversi standard ISO sull'usabilità:
- **ISO 9126**: qualità del prodotto software.
- **ISO 9241-11**: definizioni e concetti di usabilità (sostituisce la versione del 1988).
- **ISO 9241-210**: processi di progettazione centrata sull'uomo per sistemi interattivi.
- **ISO 9241-220**: processi per abilitare, eseguire e valutare la progettazione centrata sull'uomo nelle organizzazioni (sostituisce ISO TR 18529).
- **ISO/IEC 25066**: formato comune di settore per i rapporti di valutazione dell'usabilità.
- **ISO/IEC 25022**: misura della qualità in uso (efficacia, efficienza, soddisfazione), in sostituzione di ISO TR 9126-4.
- **ISO/IEC 25023**: misura della qualità del sistema e del prodotto software (include le misure degli attributi di usabilità), in sostituzione di ISO/IEC TR 9126-2 e 9126-3.
#### Standard ISO 9241-11

> [!info] Definizione Operativa di Usabilità (ISO 9241-11)
> L'usabilità è la misura in cui un prodotto può essere usato da **specifici utenti** per raggiungere **specifici obiettivi** con **efficacia**, **efficienza** e **soddisfazione** in uno **specifico contesto d'uso**.

Le tre metriche prestazionali fondamentali sono:
- **Efficacia**: l'accuratezza e la completezza con cui gli utenti raggiungono gli obiettivi prefissati.
- **Efficienza**: la quantità di risorse spese (tempo impiegato, sforzo cognitivo, numero di tasti premuti) in relazione all'accuratezza e completezza dei risultati.
- **Soddisfazione**: l'assenza di disagio e la presenza di un'attitudine positiva dell'utente verso il prodotto.
Nello spazio concettuale, l'usabilità costituisce un modello cartesiano tridimensionale i cui assi sono definiti dalle metriche di efficacia, efficienza e soddisfazione.
#### Standard ISO 9126-1 e ISO/IEC 25000 (SQuaRE)

##### ISO 9126-1
Inquadra l'usabilità all'interno della qualità software, distinguendo tra:
- **Qualità Interna ed Esterna**: definita dai sotto-attributi di *comprensibilità*, *apprendibilità*, *operabilità* ed *attrattività*.
- **Qualità in Uso**: valutata tramite *efficacia*, *produttività* (equivalente all'efficienza), *soddisfazione* e *sicurezza nell'uso*.
> [!example] Schema dell'ISO 9126
> ![[Pasted image 20261005142147.png]]
##### ISO/IEC 25000 (SQuaRE) e ISO/IEC 25010
Evoluzione della ISO 9126, valuta le caratteristiche qualitative del software distinguendo tra tre livelli di verifica:
- **Qualità Interna**: verificabile staticamente tramite ispezione del codice sorgente e della documentazione;
- **Qualità Esterna**: verificabile tramite test dinamici condotti da tecnici in ambienti di simulazione;
- **Qualità in Uso**: verificabile sul campo mediante test condotti con utenti reali in contesti d'uso operativi.

## Usabilità
L'usabilità non è una proprietà assoluta o intrinseca di un artefatto, bensì una grandezza relativa definita dalla relazione quantificabile$$Usabilità = f(Utenti, Obiettivi, Contesto)$$Un medesimo artefatto interattivo può dimostrare un'elevata usabilità per un dato gruppo di utenti in un preciso compito e risultare inusabile per utenti differenti o contesti diversi. Ciò dimostra che i prodotti interattivi devono essere progettati in modo differenziato per rispondere alla diversità degli utenti.

L'usabilità varia inoltre in funzione del tempo e dell'evoluzione dell'utilizzatore, il cui profilo si modifica lungo la traiettoria che va dallo stadio di utente novizio, passando per utente principiante e competente, fino allo stadio di utente esperto.