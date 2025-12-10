## Chiarifica
In questo programma l'utente inserisce il consumo giornaliero della macchina, con diverse condizioni:
- Intanto che il consumo è minore di 0 ma diverso da -1, si chiede di reinserire il valore
- Se il consumo è uguale a -1, l'inserimento si interrompe (durante l'interruzione dell'inserimento la cella attuale del vettore viene portata a 0 per segnare che quel giorno), altrimenti si somma il consumo di quel giorno al totale
Si utilizzerà il vettore per salvare e sottoporre i dati a diverse operazioni di controllo:
- Si stabilisce se la macchina è stata utilizzata controllando se, in tutti i giorni del mese, il valore del carburante sia diverso da 0 (con una variabile di controllo chiamata Inutilizzo)
- Si controlla se nel giorno i+1, diverso da 0, sia minore del valore stabilito nella variabile MinC(che è uguale a 999 inizialmente), se la condizione è affermativa viene assegnato il consumo del carburante giornaliero di quel giorno al valore minore di tutto il mese, oltre a salvare il giorno in una variabile apposita
- Si controlla se nel giorno i+1, diverso da 0, sia maggiore del valore stabilito nella variabile MaxC (che è uguale a 0 inizialmente), se la condizione è affermativa, viene assegnato il consumo del carburante giornaliero di quel giorno al valore maggiore di tutto il mese, oltre a salvare il giorno in una variabile apposita
Se la variabile di controllo Inutilizzo è uguale a 0 significa che per tutti i giorni non è stata usata la macchina,  dando un output dove si scrive che la macchina non è stata usata, altrimenti si da il consumo massimo e il consumo minimo di un giorno nel mese insieme al consumo totale.

Ipotizziamo che il valore che l'utente inserisca non sia negativo (fatta eccezione per -1) e se fosse negativo di reinserirlo; 
Che il mese sia di 31 giorni (a meno che in base alle necessità dell'utente quest'ultimo non decida di interrompere prima il flusso, l'interruzione avverà come specificato sopra)
### Variabili di input
- Consumo Giornaliero
### Variabili di output
- TotaleC
- MaxC
- MinC
- GIornoMax
- GiornoMin
### Variabili di lavoro
- i (vari contatori per scorrimento di array)
- Inutilizzo (variabile che controlla l'utilizzo della macchina nel mese in base al suo valore)
## Sottoproblemi
1. Inserimento dei dati in input
2. Controllo del dato
	- Input corretto
	- Dati che non siano tutti uguali a 0
3. Lavorazione sui dati 
	- Calcolo del consumo totale
	- Calcolo del consumo minimo e il suo giorno
	- Calcolo del consumo massimo e il suo giorno
4. Output dei dati
## Flowchart
![[Pasted image 20241112151339.png]]
![[Pasted image 20241112151429.png]]
![[Pasted image 20241112151457.png]]
## Trace table
Ipotizziamo per semplicità che l'utente finisca l'input a 5 giorni e che non ci siano giorni di inutilizzo.
<table><thead>
  <tr>
    <th>Giorno</th>
    <th>ConsumoGiornaliero</th>
    <th>TotaleC</th>
    <th>MaxC</th>
    <th>MinC</th>
    <th>GiornoMax</th>
	<th>GiornoMin</th>
	<th>Inutilizzo</th>
  </tr></thead>
<tbody>
  <tr>
    <td>1</td>
    <td>10.10</td>
    <td>10.10</td>
    <td>10.10</td>
    <td>10.10</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>2</td>
    <td>15.20</td>
    <td>25.30</td>
    <td>15.20</td>
    <td>10.10</td>
    <td>2</td>
    <td>1</td>
    <td>2</td>
  </tr>
  <tr>
    <td>3</td>
    <td>37.30</td>
    <td>62.60</td>
    <td>37.30</td>
    <td>10.10</td>
    <td>3</td>
    <td>1</td>
    <td>3</td>
  </tr>
  <tr>
    <td>4</td>
    <td>28.40</td>
    <td>91.00</td>
    <td>37.30</td>
    <td>10.10</td>
    <td>3</td>
    <td>1</td>
    <td>4</td>
  </tr>
<tr>
    <td>5</td>
    <td>50.50</td>
    <td>141.50</td>
    <td>50.50</td>
    <td>10.10</td>
    <td>5</td>
    <td>1</td>
    <td>5</td>
  </tr>
</tbody>
</table>
