```c
#include <stdio.h>  
#include <stdlib.h>  
#include <time.h>  
  
#define STUDENTIMAX 10  
#define VOTIMAX 20  
  
typedef struct {  
    short int anno;  
    short int mese;  
    short int giorno;  
} Data_T;  
  
typedef struct {  
    char nome[50];  
    char cognome[50];  
    Data_T nascita;  
    short int libretto[VOTIMAX];  
    int matricola;  
    float media;  
}Studenti_T;  
  
  
int main() {  
    srand(time(NULL));  
  
    Studenti_T stud[STUDENTIMAX]={0};  
  
    static int GiorniValidi[12]={31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};  
    float MaxMedia=0;  
    float SommaVoti=0;  
    short int FrequenzaVoti[14]={0};  
    short int VotoMinimo=18;  
    int StudenteModello=0;  
      
    for(int i=0;i<STUDENTIMAX;i++) {  
  
        printf("Inserire nome studente n.%i\n",i+1);  
        scanf("%s",stud[i].nome);  
  
        printf("Inserire cognome studente n.%i\n",i+1);  
        scanf("%s",stud[i].cognome);  
  
        printf("Inserire anno di nascita dello studente n.%i\n",i+1);  
        scanf("%i",&stud[i].nascita.anno);  
        while (stud[i].nascita.anno<1970 || stud[i].nascita.anno>2005){  
            printf("Anno errato, reinserire:\n");  
            scanf("%i",&stud[i].nascita.anno);  
        }  
  
        printf("Inserire mese di nascita dello studente n.%i\n",i+1);  
        scanf("%i",&stud[i].nascita.mese);  
        while(stud[i].nascita.mese<1 || stud[i].nascita.mese>12) {  
            printf("Mese errato, reinserire:\n");  
            scanf("%i",&stud[i].nascita.mese);  
        }  
  
        printf("Inserire giorno di nascita dello studente n.%i\n",i+1);  
        scanf("%i",&stud[i].nascita.giorno);  
        while (stud[i].nascita.giorno<1 || stud[i].nascita.giorno>GiorniValidi[(stud[i].nascita.mese)-1]) {  
            printf("Giorno errato, reinserire:\n");  
            scanf("%i",&stud[i].nascita.giorno);  
        }  
  
        printf("Inserire matricola dello studente n.%i\n",i+1);;  
        scanf("%i",&stud[i].matricola);  
        while(stud[i].matricola<1) {  
            printf("Matricola errata, reinserire:\n");  
            scanf("%i",&stud[i].matricola);  
        }  
  
        for(int j=0;j<VOTIMAX;j++) {  
            stud[i].libretto[j]=rand()%(31-18+1)+18;  
            SommaVoti+=stud[i].libretto[j];  
        }  
  
        stud[i].media=SommaVoti/VOTIMAX;  
        SommaVoti=0;  
  
        if(stud[i].media>MaxMedia) {  
            StudenteModello=i;  
            MaxMedia=stud[i].media;  
        }  
    }  
  
    do {  
        for(int i=0;i<14;i++) {  
            for(int j=0;j<STUDENTIMAX;j++) {  
                for(int k=0;k<VOTIMAX;k++) {  
                    if(stud[j].libretto[k]==VotoMinimo){  
                        FrequenzaVoti[i]++;  
                    }  
                }  
            }  
            VotoMinimo++;  
        }  
    }while(VotoMinimo<31);  
  
    for(int i=0;i<STUDENTIMAX;i++) {  
        printf("Dati Studente n.%i:\nNome:%s, Cognome:%s, Data di nascita:%i/%i/%i,Matricola:%i, Mediavoti:%.2f \n", i+1,stud[i].nome,stud[i].cognome,stud[i].nascita.giorno,stud[i].nascita.mese,stud[i].nascita.anno,stud[i].matricola,stud[i].media);  
        printf("\n---\n");  
    }  
  
    printf("\n---\n");  
    printf("Lo studente modello e' %s,%s con la media di %.2f", stud[StudenteModello].nome,stud[StudenteModello].cognome,MaxMedia);  
  
    printf("\n---\n");  
    printf("Frequenza voti:\n");  
    for(int i=0;i<14;i++) {  
        printf("Voto %i: %i\n", i+18, FrequenzaVoti[i]);  
    }  
  
    return 0;  
}
```