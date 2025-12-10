```c
  
#include <stdio.h>  
#include <stdlib.h>  
#include <time.h>  
  
int main(void){  
    //Dichiarazione variabili per la generazione casuale  
    int seed=time(NULL);  
    srand(seed);  
  
    //Dichiarazione contatori e sentinella  
    int UnderQuaranta=0;  
    int OverQuaranta=0;  
    short sentinella=0;  
  
    //Dichiarazione variabili dati della persona  
    double Altezza=0.0;  
    int Eta=0, Peso=0;  
      
  
    //Dichiarazione dati calcolabili  
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
  
    //Ciclo generativo do-while con controlli e memorizzazioni  
    do{  
        printf("Inserire l'eta': ");  
        scanf("%i",&Eta);  
        printf("\nInserire altezza: ");  
        scanf("%lf",&Altezza);  
        printf("\nInserire peso: ");  
        scanf("%i",&Peso);  
        BMIPersona=Peso/(Altezza*Altezza);  
  
        //Output dati soggetto  
        printf("Soggetto  Eta:%d, Altezza: %.2f: Peso:%d, BMI:%.2f \n",Eta,Altezza,Peso,BMIPersona);  
  
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
  
        printf("Si vuole continuare a inserire? Inserire -1 per smettere\n");  
        scanf("%d",&sentinella);  
  
    }while(sentinella!=-1);  
      
    //Separatore  
    printf("\nMEDIA BMI:\n");  
    //Calcolo BMI medio per eta'  
    if (UnderQuaranta>0){  
        BMIMedioUnderQuaranta=BMITotaleUnderQuaranta/UnderQuaranta;  
        printf("BMI medio Under 40: %.2f", BMIMedioUnderQuaranta);  
        if(BMIMedioUnderQuaranta<18.50) {  
            printf(",campione generalmente sottopeso");  
        }  
        if(BMIMedioUnderQuaranta>18.50 && BMITotaleOverQuaranta<25.00) {  
            printf(",campione generalmente normopeso");  
        }  
        if(BMIMedioUnderQuaranta>25.00) {  
            printf(",campione generalmente sovrappeso");  
        }  
    }  
    if (OverQuaranta>0){  
        BMIMedioOverQuaranta=BMITotaleOverQuaranta/OverQuaranta;  
        printf("BMI medio Over 40: %.2f", BMIMedioOverQuaranta);  
        if(BMIMedioOverQuaranta<18.50) {  
            printf(",campione generalmente sottopeso");  
        }  
        if(BMIMedioOverQuaranta>18.50 && BMITotaleOverQuaranta<25.00) {  
            printf(",campione generalmente normopeso");  
        }  
        if(BMIMedioOverQuaranta>25.00) {  
            printf(",campione generalmente sovrappeso");  
        }  
    }  
  
    //Output dati  
    //separatore    printf("\nMAX ALTEZZE:\n");  
    printf("Massimo Altezza Globale: %.2f\n", MassimoAltezza);  
    printf("Massimo Altezza Under 40: %.2f\n", MassimoAltezzaUnderQuaranta);  
    printf("Massimo Altezza Over 40: %.2f\n",MassimoAltezzaOverQuaranta);  
    //Separatore  
    printf("\nMAX PESI\n:");  
    printf("Massimo Peso Globale: %d\n", MassimoPeso);  
    printf("Massimo Peso Under 40: %d\n", MassimoPesoUnderQuaranta);  
    printf("Massimo Peso Over 40: %d\n", MassimoPesoOverQuaranta);  
  
    return 0;  
}
```