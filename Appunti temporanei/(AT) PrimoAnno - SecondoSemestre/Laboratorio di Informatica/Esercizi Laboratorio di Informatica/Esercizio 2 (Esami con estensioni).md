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
    char *esame;  
    short int voto;  
}Libretto_T;  
  
typedef struct {  
    char *nome;  
    char *cognome;  
    Data_T nascita;  
    Libretto_T libretto[VOTIMAX];  
    int matricola;  
    float media;  
}Studenti_T;  
  
  
int main() {  
    srand(time(NULL));  
  
    Studenti_T stud[STUDENTIMAX]={0};  
  
    static int GiorniValidi[12]={31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};  
    short int FrequenzaVoti[14]={0};  
    short int VotoContatore=18;  
    float MaxMedia=0;  
    float SommaVoti=0;  
    int StudenteModello=0;  
  
    char *nomi[]={"Luca", "Marco", "Giulia", "Francesca", "Alessandro", "Matteo", "Sara", "Elena", "Davide", "Simone"};  
    char *cognomi[]={"Rossi", "Bianchi", "Esposito", "Russo", "Colombo", "Romano", "Ricci", "Gallo", "Conti", "Ferrari"};  
    char *esami[]={ "Analisi Matematica 1", "Algebra Lineare", "Fisica 1", "Programmazione 1", "Matematica Discreta", "Architettura degli Elaboratori", "Programmazione 2", "Algoritmi e Strutture Dati", "Basi di Dati", "Sistemi Operativi", "Reti di Calcolatori", "Ingegneria del Software", "Intelligenza Artificiale", "Calcolo Numerico", "Sicurezza Informatica", "Interazione Uomo-Macchina", "Elaborazione delle Immagini", "Big Data e Data Science", "Cloud Computing", "Machine Learning" };  
    for(int i=0;i<STUDENTIMAX;i++) {  
  
        stud[i].nome=nomi[rand()%(9-1+1)+1];  
        stud[i].cognome=cognomi[rand()%(9-1+1)+1];  
        stud[i].nascita.anno=rand()%(2005-1970+1)+1970;  
        stud[i].nascita.mese=rand()%(12-1+1)+1;  
  
        stud[i].nascita.giorno=rand()%(31-1+1)+1;  
        while (stud[i].nascita.giorno>GiorniValidi[(stud[i].nascita.mese)-1]) {  
            stud[i].nascita.giorno=rand()%(31-1+1)+1;  
        }  
  
        stud[i].matricola=rand()%(1000000-1+1)+1;  
  
        for(int j=0;j<VOTIMAX;j++) {  
            stud[i].libretto[j].esame=esami[j];  
            stud[i].libretto[j].voto=rand()%(31-18+1)+18;  
            SommaVoti+=stud[i].libretto[j].voto;  
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
                    if(stud[j].libretto[k].voto==VotoContatore){  
                        FrequenzaVoti[i]++;  
                    }  
                }  
            }  
            VotoContatore++;  
        }  
    }while(VotoContatore<31);  
  
    for(int i=0;i<STUDENTIMAX;i++) {  
        printf("--DATI STUDENTE---\n");  
        short int MinVotoStudente=30,MaxVotoStudente=0;  
        short int pos_min_temp=0,pos_max_temp=0;  
        printf("Dati Studente n.%i: Nome e Cognome: %s-%s, Data di nascita:%i/%i/%i, Matricola:%i, Media:%.2f \n", i+1,stud[i].nome,stud[i].cognome,stud[i].nascita.giorno,stud[i].nascita.mese,stud[i].nascita.anno,stud[i].matricola,stud[i].media);  
        printf("---VOTI ESAMI---\n");  
        for(int j=0;j<VOTIMAX;j++) {  
            printf("Nome esame:%s|", stud[i].libretto[j].esame);  
            printf("Voto esame:%i\n", stud[i].libretto[j].voto);  
            if(stud[i].libretto[j].voto<MinVotoStudente) {  
                MinVotoStudente=stud[i].libretto[j].voto;  
                 pos_min_temp=j;  
            }  
            if(stud[i].libretto[j].voto>MaxVotoStudente) {  
                MaxVotoStudente=stud[i].libretto[j].voto;  
               pos_max_temp=j;  
            }  
  
        }  
        printf("---ALTI E BASSI STUDENTE---\n");  
        printf("Esame con voto piu' alto (primo esame preso in considerazione): %s\n" , stud[i].libretto[pos_max_temp].esame);  
        printf("Esame con voto piu' basso (primo esame preso in considerazione): %s\n" , stud[i].libretto[pos_min_temp].esame);  
        printf("\n");  
    }  
  
        printf("\n---STUDENTE MODELLO---\n");  
        printf("Lo studente modello e' %s,%s con la media di %.2f", stud[StudenteModello].nome,stud[StudenteModello].cognome,MaxMedia);  
        printf("\n");  
        printf("\n---FREQUENZA VOTI---\n");  
        printf("Frequenza voti:\n");  
        for(int i=0;i<14;i++) {  
            printf("Voto %i: %i\n", i+18, FrequenzaVoti[i]);  
        }  
  
        return 0;  
  
}
```