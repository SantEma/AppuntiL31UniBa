```c
#include <stdio.h>  
#include <stdlib.h>  
#include <time.h>  
#define MAXCICLO 10  
#define MAXSTRINGA 100  
  
int main(void){  
    //Dichiarazione variabili per la generazione casuale  
    int seed=time(NULL);  
    srand(seed);  
  
    //Dichiarazione contatori  
    int UnderQuaranta=0;  
    int OverQuaranta=0;  
  
    //Dichiarazione variabili dati della persona  
    int Eta=0, Altezza=0, Peso=0;  
  
    //Dichiarazione dati calcolabili  
    double AltezzaInMetri=0.0;  
    double BMIPersona=0.0;  
  
    //Dichiarazione massimi globali  
    double MassimoAltezza=0.0;  
    int MassimoPeso=0;  
  
    //Dichiarazione dati massimi suddivisi per eta'  
    double MassimoAltezzaUnderQuaranta=0.0, MassimoAltezzaOverQuaranta=0.0;  
    int MassimoPesoUnderQuaranta=0,MassimoPesoOverQuaranta=0;  
  
    //Dichiarazione dati medi e totali del BMI suddivisi per eta'  
    double BMIMedioUnderQuaranta=0, BMIMedioOverQuaranta=0;  
    double BMITotaleUnderQuaranta=0, BMITotaleOverQuaranta=0;  
  
    //Ciclo generativo con controlli e memorizzazioni  
    for(int i=0;i<MAXCICLO;i++){  
        //Generazione dati persona e calcolo BMI soggetto  
        Eta=rand()%(146-1+1)+1; //MIN 1 anno, MAX 146 ANNI  
        Altezza=rand()%(240-100+1)+100; //MIN 1 metro, MAX 2.40 metri  
        Peso=rand()%(550-30+1)+30; //MIN 30KG, MAX 550KG  
        AltezzaInMetri=Altezza/100.0; //conversione da CM a M  
        BMIPersona=Peso/(AltezzaInMetri*AltezzaInMetri);  
          
        //Output dati soggetto  
        printf("Soggetto n.%d Eta:%d, Altezza: %.2f: Peso:%d, BMI:%.2f \n",i+1,Eta,AltezzaInMetri,Peso,BMIPersona);  
  
        //Assegnazione massimi globali  
        if(Altezza>MassimoAltezza) {  
            MassimoAltezza=Altezza;  
        }  
        if(Peso>MassimoPeso) {  
            MassimoPeso=Peso;  
        }  
  
        //Assegnazione massimi per eta'  
        if(Eta<=40) {  
            if(Peso>MassimoPesoUnderQuaranta) {  
                MassimoPesoUnderQuaranta=Peso;  
            }  
            if(Altezza>MassimoAltezzaUnderQuaranta) {  
                MassimoAltezzaUnderQuaranta=Altezza;  
            }  
            BMITotaleUnderQuaranta+=BMIPersona;  
            UnderQuaranta++;  
        }else {  
            if(Peso>MassimoPesoOverQuaranta) {  
                MassimoPesoOverQuaranta=Peso;  
            }  
            if(Altezza>MassimoAltezzaOverQuaranta) {  
                MassimoAltezzaOverQuaranta=Altezza;  
            }  
            BMITotaleOverQuaranta+=BMIPersona;  
            OverQuaranta++;  
        }  
    }  
  
    //Separatore  
    printf("\n");  
    //Calcolo BMI medio per eta'  
    if (UnderQuaranta>0){  
        BMIMedioUnderQuaranta=BMITotaleUnderQuaranta/UnderQuaranta;  
        printf("BMI medio Under 40: %.2f\n", BMIMedioUnderQuaranta);  
    }  
    if (OverQuaranta>0){  
        BMIMedioOverQuaranta=BMITotaleOverQuaranta/OverQuaranta;  
        printf("BMI medio Over 40: %.2f\n", BMIMedioOverQuaranta);  
    }  
  
    //Output dati  
    printf("\n");  
    printf("Massimo Altezza Globale: %.2f\n", MassimoAltezza/100.0);  
    printf("Massimo Altezza Under 40: %.2f\n", MassimoAltezzaUnderQuaranta/100.0);  
    printf("Massimo Altezza Over 40: %.2f\n",MassimoAltezzaOverQuaranta/100.0);  
    printf("Massimo Peso Globale: %d\n", MassimoPeso);  
    printf("Massimo Peso Under 40: %d\n", MassimoPesoUnderQuaranta);  
    printf("Massimo Peso Over 40: %d\n", MassimoPesoOverQuaranta);  
  
    return 0;  
}
```