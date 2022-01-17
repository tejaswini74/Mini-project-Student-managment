#include <stdio.h>
#include <stdlib.h>
#define STR_LEN 40

typedef struct student{
    char name[STR_LEN];
    int gender;
    int age;
}student;

void menu(int flag, int *num);
void read(student *p, int *num);
void init(int *num);
void show(student *p, int *num);
void showline();
void open(student *p, int *num, int flag);
void save(student *p, int *num);

student *data = NULL;

int main(){
    system("cls");
    int num = 0;
    int flag = 0;
    menu(flag++, &num);
    return 0;
}

void menu(int flag, int *num){
    if(flag == 0) printf("Welcome to student management system! Please choose the function and intput the number:\n");
    else if(flag == 1) printf("Welcome back! Please choose the function and intput the number:\n");
    else if(flag == -1) printf("Error! Please Reboot the program and try again!");
    printf("\t1.Adding data\n\t2.Viewing data\n\t3.Save data\n\t4.Open data\n\t5.Quit\n\t");
    int choice = 0;
    scanf("%d", &choice);
    switch(choice){

        case 1:{
            printf("\033[0;35mPlease input the number of data which you want to input:\033[0m");
            scanf("%d", num);
            init(num);
            read(data, num);
            return;
        }

        case 2:{
            show(data, num);
        }

        case 3:{
            save(data, num);
            return;
        }

        case 4:{
            open(data, num, flag);
        }

        case 5: exit(0);
        default: {
            printf("\033[0;31mError: Cannot identify your input!\033[0m");
            return;
        }
    }
}

void init(int *num){
    data = (student*)malloc((*num)*sizeof(student));
    for(int i = 0; i < *num; i++){
        for(int j = 0; j < STR_LEN; j++){
            data->name[j] = 0;
        }
        data->gender = 0;
        data->age = 0;
        data++;
    }
    data -= *num;
    read(data, num);
}

void read(student *p, int *num){
    for(int i = 0; i < *num; i++){
        printf("\tStudent Name:");
        scanf("%s", p->name);
        printf("\tStudent Gender(1-Male;2-Female):");
        scanf("%d",&(p->gender));
        printf("\tStudent Age:");
        scanf("%d",&(p->age));
        showline();
        printf("\033[0;33m%d students has been added, remaining %d students.\033[0m\n", i+1, *num-i);
        showline();
        p++;
    }
    showline();
    printf("\033[0;32mAll students's data has benn added to the system.now back to main system\033[0m\n");
    showline();
    menu(1, num);
}

void show(student *p, int *num){
    for(int i = 0; i < *num; i++){
        showline();
        printf("\tName: %s\n",p->name);
        if(p->gender == 1) printf("\tGender: Male\n");
        else if(p->gender == 2) printf("\tGender: Female\n");
        else printf("\tGender: Unidentified\n");
        printf("\tAge: %d\n", p->age);
        p++;
    }
    showline();
    printf("\033[0;32mAll data has benn printed on the screen! Now back to menu...\033[0m\n");
    showline();
    menu(1, num);
}

void save(student *p, int *num){
    FILE *fp = fopen("studentdata", "w+");
    if(fp){
        fwrite(p, sizeof(student), *num, fp);
        fclose(fp);
        printf("\033[42;37m Save Success!\033[0m\n");
    }
    else printf("\033[41;37m Save Failed!\033[0m\n");
    menu(1, num);
}

void open(student *p, int *num, int flag){
    *num = 0;
    FILE *fp = fopen("studentdata", "r");
    if(fp){
        fseek(fp, 0L, SEEK_END);
        long size = ftell(fp);
        *num = size/(sizeof(student));
        fseek(fp, 0L, SEEK_SET);

        if(flag == 1) free(data);
        data = (student *)malloc((*num)*sizeof(student));
        p = data;

        fread(data, sizeof(student), *num, fp);

        showline();
        printf("\033[0;34mA total of %d data were successfully read.\033[0m\n", *num);
        showline();
    }
    else printf("\033[41;37mRead Failed!\033[0m\n");
    menu(1, num);
}

void showline(){
    for(int i = 0; i < 50; i++){
        printf("*-");
    }
    printf("*\n");
}
