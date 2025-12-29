Come sappiamo, un database è un sistema (prodotto software) in grado di gestire collezioni di dati che siano grandi, condivise e persistenti e garantendo affidabilità, privatezza, efficienza ed efficacia. Questo 
## Progettazione fisica
La fase finale nel processo di progettazione di una base di dati è quella della progettazione fisica, che, ricevendo in ingresso lo schema logico della base dei dati, le caratteristiche del sistema scelto e le previsioni sul carico applicativo, produce in uscita lo schema fisico della base di dati, costituito da effettive definizioni delle relazioni (le istruzioni $\text{CREATE TABLE}$ in SQL) e soprattutto delle strutture fisiche utilizzate con i relativi parametri.
La maggior parte delle scelte da effettuare nel corso della progettazione fisica dipende dal specifico DBMS utilizzato, quindi risulta difficile fornire una panoramica completa e di validità generale, ma esistono delle linee generali per delle basi di dati non enormi o con carichi non particolarmente complessi.

Le scelte fondamentali nella progettazione sono da ricondurre a due:
- Scelta della **struttura primaria** per ciascuna relazione, fra quelle disponibili dal DBMS
- Definizione di eventuali **indici secondari**