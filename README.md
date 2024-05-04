
//gestion de stock 

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <conio.h>
#include <windows.h>
#define MAX_PR 100
typedef struct{
    int ID,quan,alert;
    float prix;
    char nom[20],desc[100], uti[20];
    char date_ent[11], date_der[11];
}Pro;
char uti[20];
void lon(int color);
void gotoxy(int x, int y);
void window1();
void highlight( int count);
int stockMenu();
void login();
void signup(const char* uti, const char* pas);
void choof(Pro pr[], int *n);
void debut();
void affi(Pro pr[], int n);
void ajou(Pro pr[], int *n);
void supp(Pro pr[], int *n);
void rechr(Pro pr[], int n);
void tri_n(Pro pr[], int n);
void tri_p(Pro pr[], int n);
void done(Pro pr[], int n);

 main() { 
 int choice,n=0;
    Pro pr[MAX_PR];
    login();
    do { 
	    choof(pr, &n);
        system("cls"); 
        window1(); 

        choice=stockMenu();
        system("cls");
        switch (choice) {
            case 1:
                ajou(pr, &n);
                break;
            case 2:
                supp(pr, &n);
                break;
            case 3:
                affi(pr, n);
                break;
            case 4:
                rechr(pr, n);
                break;
            case 5:
                tri_n(pr, n);
                break;
            case 6:
                tri_p(pr, n);
                break;
            case 7:
                printf("bye-bye\n");
                exit(0);
                break;
        }
    } while (1);

    
}


void lon(int color) {
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

void gotoxy(int x, int y) {
    COORD coord;
    coord.X = x;
    coord.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}

void window1() {
    lon(2);
    gotoxy(32,11);
    printf("                                      %c MENU %c ",15,15);
    lon(9);
    gotoxy(26, 12);
    printf("                   /~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\\");
    gotoxy(26, 13);
    printf("                   |  /~~\\                                        /~~\\  |");
    gotoxy(26, 14);
    printf("                   |\\  @  |                                      |  @  /|");
    gotoxy(26, 15);
    printf("                   | \\   /|                                      |\\   / |");
    gotoxy(26, 16);
    printf("                   |  ~~  |                                      | ~~   |");
    gotoxy(26, 17);
    printf("                   |      |                                      |      |");
    gotoxy(26, 18);
    printf("                   |      |                                      |      |");
    gotoxy(26, 19);
    printf("                   |      |                                      |      |");
    gotoxy(26, 20);
    printf("                   |      |                                      |      |");
    gotoxy(26, 21);
    printf("                   |      |                                      |      |");
    gotoxy(26, 22);
    printf("                   \\      |~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|      /");
    gotoxy(26, 23);
    printf("                    \\     /                                       \\     /");
    gotoxy(26, 24);
    printf("                     \\~~~/                                         \\~~~/");
}

int stockMenu() {
    int count=1;
    char ch;

    while (1) {
        system("cls");
        window1();
        highlight(count); 

        ch = getch(); 

        switch (ch) {
            case 80: // Down arrow
                count++;
                if (count == 8)
                    count = 1;
                break;
            case 72: // Up arrow
                count--;
                if (count == 0)
                    count = 7;
                break;
            case '\r': // Enter key
                return count;
        }
    }
}
void highlight( int count) {
   
        gotoxy(55, 15);
        lon((count == 1) ? 14 : 12); 
        printf("   Ajout Pro             %c", (count == 1) ? 251 : 120);
        gotoxy(55, 16);
        lon((count == 2) ? 14 : 12);
        printf("   supprime Pro          %c", (count == 2) ? 251 : 120);
        gotoxy(55, 17);
        lon((count == 3) ? 14 : 12); 
        printf("   List Pro              %c", (count == 3) ? 251 : 120);
        gotoxy(55, 18);
        lon((count == 4) ? 14 : 12); 
        printf("   Search Pro            %c", (count == 4) ? 251 : 120);
        gotoxy(55, 19);
        lon((count == 5) ? 14 : 12);
        printf("   tri Pro  par nom      %c", (count == 5) ? 251 : 120);
        gotoxy(55, 20);
        lon((count == 6) ? 14 : 12); 
        printf("   tri Pro  par Prix     %c", (count == 6) ? 251 : 120);
        gotoxy(55, 21);
        lon((count == 7) ? 14 : 12); 
        printf("   Exit                  %c", (count == 7) ? 251 : 120);
        lon(7); 
    
}



     void login() {
    FILE *userFile = fopen("uti.txt", "r");
    if (!userFile) {
        printf("No user file found. Creating a new one...\n");
        FILE *newUserFile = fopen("uti.txt", "w");
        if (!newUserFile) {
            printf("Error creating user file.\n");
            exit(1);
        }
        fclose(newUserFile);
    } else {
        fclose(userFile);
    }

    FILE *stockFile = fopen("stock.txt", "r");
    if (!stockFile) {
        printf("No stock file found. Creating a new one...\n");
        FILE *newStockFile = fopen("stock.txt", "w");
        if (!newStockFile) {
            printf("Error creating stock file.\n");
            exit(1);
        }
        fclose(newStockFile);
    } else {
        fclose(stockFile);
    }

    char entereduti[20], enteredpas[20];
    printf("Enter your username: ");
    fgets(entereduti, sizeof(entereduti), stdin);
    entereduti[strcspn(entereduti, "\n")] = '\0'; 
    printf("Enter your pas: ");
    fgets(enteredpas, sizeof(enteredpas), stdin);
    enteredpas[strcspn(enteredpas, "\n")] = '\0'; 

    char fileuti[20], filepas[20];
    userFile = fopen("uti.txt", "r");
    if (!userFile) {
        printf("Error opening file: uti.txt\n");
        exit(1);
    }

    while (fscanf(userFile, "%s %s", fileuti, filepas) != EOF) {
        if (strcmp(entereduti, fileuti) == 0 && strcmp(enteredpas, filepas) == 0) {
            strcpy(uti, entereduti);
            fclose(userFile);
            return;
        }
    }

    fclose(userFile);

    char choice;
    printf("Login failed. Username or pas incorrect.\n");
    printf("Do you want to sign up? (y/n): ");
    scanf(" %c", &choice);

    if (choice == 'y' || choice == 'Y') {
        signup(entereduti, enteredpas);
        strcpy(uti, entereduti);
    } else {
        exit(1);
    }
}


void signup(const char* uti, const char* pas) {
    FILE *file = fopen("uti.txt", "a");
    if (!file) {
        printf("\aError opening file: uti.txt\n");
        exit(1);
    }

    fprintf(file, "%s %s\n", uti, pas);
    fclose(file);

    char stockFilename[50];
    sprintf(stockFilename, "%s_stock.txt", uti);

    file = fopen(stockFilename, "w");
    if (!file) {
        printf("\aError opening file: %s\n", stockFilename);
        exit(1);
    }

    fclose(file);

    printf("Sign up successful. Your username has been added to the system.\n");}

void choof(Pro pr[], int *n) {
   FILE *file = fopen("Pros.txt", "r");
    if (!file) {
        printf("Error opening file: Pros.txt\n");
        return;
    }

    *n = 0;
    while (fscanf(file, "%d %s %s %f %d %d %s %s %s",&pr[*n].ID,pr[*n].nom,pr[*n].desc,&pr[*n].prix,&pr[*n].quan,&pr[*n].alert,pr[*n].date_ent,pr[*n].date_der,pr[*n].uti) == 10) {
        (*n)++;
    }

    fclose(file);
}

void debut() {
	printf("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\n");
    printf("ID | Nom | Description | Prix | Quantite | Seuil | Date Entree | Date Derniere | Utilisateur\n");
    printf("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\n");
}

void affi(Pro pr[], int n) {
		int i;
    debut();
    for (i = 0; i < n; i++) {
        printf("%d | %s | %s | %.2f | %d | %d | %s | %s | %s\n",pr[i].ID,pr[i].nom,pr[i].desc,pr[i].prix,pr[i].quan,pr[i].alert,pr[i].date_ent,pr[i].date_der,pr[i].uti);
    }
}

void ajou(Pro pr[], int *n) {
    Pro newPro;
    printf("Enter Pro Details:\n");
    printf("ID: ");
    scanf("%d", &newPro.ID);
    printf("Name: ");
    scanf("%s", newPro.nom);
    printf("Description: ");
    scanf("%s", newPro.desc);
    printf("Unit Price: ");
    scanf("%f", &newPro.prix);
    printf("Quantity: ");
    scanf("%d", &newPro.quan);
    printf("Alert Threshold: ");
    scanf("%d", &newPro.alert);
    printf("Date of Last Entry (YYYY-MM-DD): ");
    scanf("%s", newPro.date_ent);
    printf("Date of Last Exit (YYYY-MM-DD): ");
    scanf("%s", newPro.date_der);
    strcpy(newPro.uti, uti);

    pr[*n] = newPro;
    (*n)++;
 done(pr, *n);
    printf("Produit added to your stock.\n");
}

void supp(Pro pr[], int *n) {
    int id_sup,i,j;
    printf("Enter the ID of the Pro to delete: ");
    scanf("%d", &id_sup);

    for (i = 0; i < *n; i++) {
        if (pr[i].ID == id_sup) {
            for (j = i; j < *n - 1; j++) {
                pr[j] = pr[j + 1];
            }
            (*n)--;
             done(pr, *n);
            printf("Pro with ID %d deleted.\n", id_sup);
            return;
        }
    }

    printf("Pro with ID %d not found.\n", id_sup);
}

void rechr(Pro pr[], int n) {
		int i;
    char searchTerm[20];
    printf("Enter the Pro name to search for: ");
    scanf("%s", searchTerm);

    debut();
    for (i = 0; i < n; i++) {
        if (strstr(pr[i].nom, searchTerm) != NULL) {
            printf("%d | %s | %s | %.2f | %d | %d | %s | %s | %s\n",pr[i].ID,pr[i].nom,pr[i].desc,pr[i].prix,pr[i].quan,pr[i].alert,pr[i].date_ent,pr[i].date_der,pr[i].uti);
        }
    }
}

void tri_n(Pro pr[], int n) {
		int i,j;
    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (strcmp(pr[j].nom, pr[j + 1].nom) > 0) {
                Pro temp = pr[j];
                pr[j] = pr[j + 1];
                pr[j + 1] = temp;
            }
        }
    }
 done(pr, n);
    printf("Pros sorted by name.\n");
}

void tri_p(Pro pr[], int n) {
	int i,j;
    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (pr[j].prix > pr[j + 1].prix) {
                Pro temp = pr[j];
                pr[j] = pr[j + 1];
                pr[j + 1] = temp;
            }
        }
    }
 done(pr, n);
    printf("Pros sorted by price.\n");
}

void done(Pro pr[], int n) {
		int i;
    FILE *file = fopen("stock.txt", "w");
    if (file == NULL) {
        printf("Error opening file.\n");
        return;
    }

    for ( i = 0; i < n; i++) {
        fprintf(file, "%d %s %s %.2f %d %d %s %s %s %s\n",pr[i].ID,pr[i].nom,pr[i].desc,pr[i].prix,pr[i].quan,pr[i].alert,pr[i].date_ent,pr[i].date_der,pr[i].uti);
    }

    fclose(file);
}
