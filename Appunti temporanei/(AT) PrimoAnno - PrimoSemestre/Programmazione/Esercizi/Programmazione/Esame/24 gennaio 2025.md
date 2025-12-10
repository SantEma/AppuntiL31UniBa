## Chiarifica
### Analisi
Il programma prevede la gestione della comunicazione tra una catena di negozi di bricolage e un deposito da cui si riforniscono.
Bisogna creare un array di record per il negozio che contiene: codice del negozio, codice materiale nel negozio, descrizione del materiale nel negozio e il numero dei pezzi ordinati.
Creare un altro array di record per le informazioni del deposito, che sono: codice del materiale nel deposito, descrizione del materiale nel deposito, numero dei pezzi disponibili all'interno.
Realizzare funzioni e/o procedure che permettano di inserire quanti negozi sono presenti nella catena, inserire una richiesta di ordine tramite codice e sapere se questa viene accettata o no, nel caso negativo (dove non viene trovato materiale) notificare l'utente, nel caso positivo procedere, se il riscontro è positivo stampare il costo complessivo e rimuovere il materiale dal deposito; inserire il prodotto nel deposito con le sue informazioni; se richiesto, mandare in output la tabella della lista dei prodotti del deposito in ordine crescente riordinata tramite bubble sort con tutte le informazioni presenti.
Si ipotizza che ogni negozio possa anche occupare tutti gli ordini in una giornata, che il codice di ogni prodotto sia univoco per tutti i negozi e per il deposito, che la descrizione sia messa in input in modo giusto sia da tutti i negozi che dal deposito, che il numero dei prodotti massimi siano 100 per il magazzino
### Dati
#### Dati di input
<table><thead>
  <tr>
    <th>Dati di input</th>
    <th>Variabili</th>
    <th>Tipo e Vincoli</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Numero negozi</td>
    <td>Num_Ordini</td>
    <td>int&gt;0</td>
  </tr>
<tr>
    <td>Scelta</td>
    <td>scelta</td>
    <td></td>
  </tr>
  <tr>
    <td>Per ogni negozio:<br>- Codice del negozio<br>- Codice del materiale richiesto<br>- Descrizione del materiale<br>- Numero dei pezzi richiesti</td>
    <td>Array di record Negozio(Negozio_t):<br>- Negozio(i).CodNegozio<br>- Negozio(i).CodMatNeg<br>- Negozio(i).DescMatNeg<br>- Negozio(i).NumPezziOrdine</td>
    <td><br>int&gt;0<br>int&gt;0<br>string<br>int&gt;0</td>
  </tr>
  <tr>
    <td>Per ogni deposito:<br>- Codice del materiale nel magazzino<br>- Descrizione del materiale<br>- Numero dei pezzi presenti<br>- Costo singolo unità</td>
    <td>Array di record Deposito (Deposito_t)<br>- Deposito(i).CodiceMaterialeDep<br>- Deposito(i).DescMatDep<br>- Deposito(i).NumPezziDisponibili<br>- Deposito(i).CostoMateriale<br></td>
    <td><br>int&gt;0<br>string<br>int&gt;0<br>float&gt;0<br></td>
  </tr>
</tbody></table>
#### Dati di output
<table><thead>
  <tr>
    <th>Dati di output</th>
    <th>Variabili</th>
    <th>Tipo e Vincoli</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Per ogni richiesta aggiunta dal negozio:<br>- Messaggio di accettazione o rifiuto, costo complessivo</td>
    <td><br>"ordine effettuato"/"ordine fallito", CostoTotale<br></td>
    <td>double&gt;0<br></td>
  </tr>
  <tr>
    <td>Per ogni deposito<br> - Info ordinate <br></td>
    <td>- Deposito(i).CodiceMaterialeDep<br>- Deposito(i).DescMatDep<br>- Deposito(i).NumPezziDisponibili- Deposito(i).CostoMateriale</td>
    <td></td>
  </tr>
</tbody>
</table>
### Variabili di lavoro
I: contatore per array
J: contatore per array
N: contatore negozi
P: contatore prodotti
MAX: definizione statica per il massimo dei prodotti nel magazzino e la lunghezza del magazzino
CodiceTemp: codice temporaneano salvato dell'ordine del negozio
QuantitativoTemp: codice temporaneano salvato dell'ordine del negozio
### Sottoproblemi
![[Screenshot_20250117_184301_Nebo.jpg]]
### Flowchart
Non inserito per ragioni di lunghezza
### Codice
```c
#include <stdio.h>
#include <string.h>
#define MAX 100  
  
typedef struct {  
    int CodNegozio;  
    int CodMatNeg;  
    char DescMatNeg[MAX];  
    int NumPezziOrdine;  
} Negozio_t;  
  
typedef struct {  
    int CodiceMaterialeDep;  
    char DescMaterialeDep[MAX];  
    int NumPezziDisponibili;  
    float CostoMateriale;  
} Deposito_t;  
  
void InserimentoOrdine(Negozio_t *Negozio, int Num_Negozi);  
void InserimentoMateriali(Deposito_t *Deposito);  
void VerificaOrdine(Deposito_t *Deposito, Negozio_t *Negozio, int Num_Negozi);  
void ListaOrdinata(Deposito_t *Deposito);  
  
int main(void) {  
    int Num_Ordini=0, scelta=0;  
    printf("Inserire quanti negozi sono presenti:\n");  
    scanf("%i",&Num_Negozi);  
    while (Num_Negozi<0) {  
        printf("Re-inserire quanti negozi sono presenti:\n");  
        scanf("%i",&Num_Negozi);  
    }  
  
    Negozio_t Negozio[Num_Negozi];  
    Deposito_t Deposito[MAX]={0,"",0, 0.0};  
  
    do{  
        printf("Benvenuto! Opzioni disponbili:\n");  
        printf("1. Inserire ordine dal negozio\n");  
        printf("2. Inserire prodotto nel magazzino\n");  
        printf("3. Stampare prodotti disponibili per codice in ordine crescente\n");  
        printf("-1. Uscire dal programma\n");  
        scanf("%i",&scelta);  
        switch (scelta) {  
            case 1:  
                InserimentoOrdine(Negozio, Num_Negozi);  
                VerificaOrdine(Deposito, Negozio, Num_Negozi);  
                break;  
            case 2:  
                InserimentoMateriali(Deposito);  
                break;  
            case 3:  
                ListaOrdinata(Deposito);  
                break;  
            case -1:  
                break;  
            default:  
                printf("Opzione non valida\n");  
                break;  
        }  
    }while (scelta!=-1);  
    return 0;  
}  
  
void InserimentoOrdine(Negozio_t *Negozio, int Num_Negozi) {  
    for (int i=0;i<Num_Negozi;i++) {  
        printf("Inserire codice del negozio (inserire -1 per uscire):\n");  
        scanf("%i",&Negozio[i].CodNegozio);  
        if (Negozio[i].CodNegozio==-1) {  
            Negozio[i].CodNegozio=0;  
            break;  
        }  
        while (Negozio[i].CodNegozio<0){  
            printf("Codice errato, re-inserirlo:\n");  
            scanf("%i",&Negozio[i].CodNegozio);  
        }  
  
        printf("Inserire codice materiale:\n");  
        scanf("%i",&Negozio[i].CodMatNeg);  
        while (Negozio[i].CodMatNeg<0) {  
            printf("Codice errato, re-inserirlo:\n");  
            scanf("%i",&Negozio[i].CodMatNeg);  
        }  
  
        printf("Inserire descrizione materiale:\n");  
        scanf("%s",Negozio[i].DescMatNeg);  
  
        printf("Inserire numero di pezzi da ordinare:\n");  
        scanf("%i",&Negozio[i].NumPezziOrdine);  
    }  
}  
  
void InserimentoMateriali(Deposito_t *Deposito) {  
    for (int i=0;i<MAX;i++){  
        printf("Inserire codice materiale (inserire -1 per uscire):\n");  
        scanf("%i", &Deposito[i].CodiceMaterialeDep);  
        if (Deposito[i].CodiceMaterialeDep==-1) {  
            break;  
        }  
        while (Deposito[i].CodiceMaterialeDep<0) {  
            printf("Codice errato, re-inserirlo:\n");  
            scanf("%i", &Deposito[i].CodiceMaterialeDep);  
        }  
  
        printf("Inserire descrizione materiale\n");  
        scanf("%s",Deposito[i].DescMaterialeDep);  
  
        printf("Inserire numero pezzi disponibili:\n");  
        scanf("%i",&Deposito[i].NumPezziDisponibili);  
        while (Deposito[i].NumPezziDisponibili<0) {  
            printf("Numero pezzi impossibili, re-inserire:\n");  
            scanf("%i",&Deposito[i].NumPezziDisponibili);  
        }  
  
        printf("Inserire costo materiale:\n");  
        scanf("%f",&Deposito[i].CostoMateriale);  
        while (Deposito[i].CostoMateriale<0.0) {  
            printf("Re-inserire costo materiale:\n");  
            scanf("%f",&Deposito[i].CostoMateriale);  
        }  
    }  
}  
  
void VerificaOrdine(Deposito_t *Deposito, Negozio_t *Negozio,int Num_Negozi) {  
    int CodiceTemp=0, QuantitativoTemp=0;  
    float Totale=0;  
    for (int i=0;i<Num_Negozi;i++) {  
  
        if (Deposito[i].CodiceMaterialeDep==-1) {  
            break;  
        }  
  
        CodiceTemp=Negozio[i].CodMatNeg;  
        QuantitativoTemp=Negozio[i].NumPezziOrdine;  
  
        for (int j=0;j<MAX;j++) {  
            if (CodiceTemp==Deposito[j].CodiceMaterialeDep && Deposito[j].CodiceMaterialeDep>0) {  
            printf("Prodotto ordinato dal negozio %i (codice prodotto:%i) \n", Negozio[i].CodNegozio, CodiceTemp);  
  
            if (QuantitativoTemp <= Deposito[j].NumPezziDisponibili) {  
                Deposito[j].NumPezziDisponibili=Deposito[j].NumPezziDisponibili-QuantitativoTemp;  
                Totale=Deposito[j].CostoMateriale*QuantitativoTemp;  
                printf("Prodotto ordinato con successo a %.2f euro\n", Totale);  
            } else {  
                printf("Ordine fallito, scorte basse\n");  
            }  
        }  
    }  
  
    CodiceTemp=0;  
    QuantitativoTemp=0;  
    }  
}  
  
void ListaOrdinata(Deposito_t *Deposito) {  
    int sentinella = 0;  
  
    if (Deposito[0].CodiceMaterialeDep==0){  
        sentinella=1;  
    }  
  
    if (sentinella==0) {  
        for (int i=0; i<MAX-1;i++) {  
            for (int j=0; j<MAX-i-1; j++) {  
                if (Deposito[j].CodiceMaterialeDep>Deposito[j + 1].CodiceMaterialeDep) {  
                    Deposito_t temp=Deposito[j];  
                    Deposito[j]=Deposito[j + 1];  
                    Deposito[j + 1]=temp;  
                }  
            }  
        }  
  
        for (int i=0;i<MAX;i++) {  
            if (Deposito[i].CodiceMaterialeDep!=0 && Deposito[i].CodiceMaterialeDep>=0){  
                printf("Codice prodotto: %i\n", Deposito[i].CodiceMaterialeDep);  
                printf("Descrizione prodotto: %s\n", Deposito[i].DescMaterialeDep);  
                printf("Numero pezzi disponibili del prodotto: %i\n", Deposito[i].NumPezziDisponibili);  
                printf("Costo Materiale: %.2f\n", Deposito[i].CostoMateriale);  
            }  
        }  
    } else {  
        printf("Nessun prodotto presente\n");  
    }
}
```


