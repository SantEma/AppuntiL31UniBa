# Esercizio 6
## Chiarifica
L'utente inserisce una data, partendo da giorno, mese e anno, il programma effettuerà poi dei controlli specifici, in base all'anno determina se è bisestile o no tramite una divisione con modulo, poi controlla il mese, se febbraio o no, se il mese è da 31 giorni o no e se il giorno è corretto in base ai parametri precedenti, se tutti corretti e infine da output positivo se quest'ultimo è corretto, nel caso sia sbagliato la data verrà visualizzata come non corretta.
Ipotizziamo che l'utente usi il normale calendario e non altri presenti
## Input
1. Giorno
2. Mese
3. Anno
## Output
1. Esito
## Sotto problemi
1. Inserimento dei dati
2. Controllo variabili e operazioni
	1. Controllo anno (se divisibile per 4 è bisestile, se divisibile per 400 bisestile secolare)
	2. Controllo mese
	3. Controllo giorno
3. Output finale all'utente
## Trace table
<table><thead>
  <tr>
    <th>Giorno</th>
    <th>Mese</th>
    <th>Anno</th>
    <th>Bisestile?</th>
    <th>Output</th>
  </tr></thead>
<tbody>
  <tr>
    <td>4</td>
    <td>12</td>
    <td>2005</td>
    <td>No</td>
    <td>Vero</td>
  </tr>
  <tr>
  </tr>
<tr>
    <td>29</td>
    <td>2</td>
    <td>2003</td>
    <td>No</td>
    <td>Falso</td>
  </tr>
<tr>
    <td>29</td>
    <td>2</td>
    <td>2008</td>
    <td>Sì</td>
    <td>Vero</td>
  </tr>
</tbody>
</table>
## Flowchart
![[Es6.png]]

# Esercizio 7
## Chiarifica
L'utente inserisce la data di nascita in giorno, mese e anno, si verificano che siano tutti i dati siano maggiori di 0 e veritieri, si effettua l'operazione sottraendo dalla data attuale (nel caso il mese di nascita sia minore di quello attuale si sottrae un anno) e si da un output all'utente con l'età calcolata
## Input
Giorno
Mese
Anno
## Output
Età
## Sotto problemi
1. Dichiarazione e inizializzazione variabili
2. Controllo valore data
3. Operazione mese e anno
4. Restituzione età
## Trace table
<table><thead>
  <tr>
    <th>Giorno</th>
    <th>Mese</th>
    <th>Anno</th>
    <th>Output</th>
  </tr></thead>
<tbody>
  <tr>
    <td>04</td>
    <td>12</td>
    <td>2005</td>
    <td>18 anni</td>
  </tr>
  <tr>
  </tr>
<tr>
    <td>20</td>
    <td>10</td>
    <td>2005</td>
    <td>19</td>
  </tr>
</tbody>
</table>
## Flowchart
![[ES7.png]]
