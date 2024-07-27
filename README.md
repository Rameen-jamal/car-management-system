# car-management-system
#include<stdio.h>
#include<conio.h>
#include<string.h>
#include<unistd.h>
#include<stdlib.h>
#include <windows.h>
struct newuser
{
	char username[50];
	char passward[50];
}user,admin;

int i;

struct carinfo
{
	char *name;
    char *model;
	int price;
	int rent;
}car[100];


// regarding passward ang login
void adminuserpass();
void login();
void adminlogin();
void costumerlogin();
void newusersignup();
void exit();
int main();

//regarding car info and adminarea


void adminarea();
void addcar();
void display();
void updatecar();
void deletecar();

//regarding buyer
int buyerarea();
int billbuy();
void displaycars();
int billrent();
int totalbil(int f, int b, int c);
void rentcars();
void soldcars();




int main()
{
	system("cls");
	int a,b;
	printf("\x1b[44m");
    printf("\x1b[33m");
	printf("\t\t=================================================\n");
	printf("\t\t|                                               |\n");
	printf("\t\t|        -----------------------------          |\n");
	printf("\t\t|     ****WELCOME TO H.R.H SHOWROOM****         |\n");
	printf("\t\t|        -----------------------------          |\n");
	printf("\t\t|                                               |\n");
	printf("\t\t=================================================\n\n\n");
	printf("Press (1) to login\nPress (2) to Sign Up\nPress (3) to Exit\n\n");
	printf("Enter choice: ");
	scanf("%d",&a);
	
	switch(a)
	{
		case 1:
			login();
			break;
		case 2:
			newusersignup();
			break;
		case 3:
			exit(0);	
			break;
	}
	
	
}


void login(){
	system("cls");
	int b;
	printf("login as:\n\n1-Admin\n2-Customer\n3-Go back\n");
	printf("\nEnter your choice: ");
	scanf("%d",&b);
		switch(b)
		{
		case 1:
			adminlogin();	
			break;
		case 2:
			costumerlogin();
			break;
		case 3:
			main();
			break;
		}
}
void adminlogin(){
	int i=0,error=0;
	system("cls");
	int a;
	struct newuser admin={"hrh","123"};
	char u_username[50],p_passward[50],ch;
	printf("\t\t\t<WELCOME ADMINS>\n\n");
	a:
	printf("Enter Username: ");
	fflush(stdin);
	
	gets(u_username);
	fflush(stdin);
	
	
	fflush(stdin);

	printf("Enter Password: ");
	fflush(stdin);
	gets(p_passward);

	if( strcmp(p_passward,admin.passward)==0 &&  strcmp(u_username,admin.username)==0)
	{
		printf("\n\n");
		adminarea();
	}
	else
	{
		error++;
		printf("\n\n");
		fflush(stdin);
		printf("\aBeep!\n");
		if(error==5){
			printf("You've exceeded the maximum number of login attempts!");
			exit(1);
		}
		printf("INCORRECT USERNAME OR PASSWORD!!\n");
		printf("login again in 2 seconds\n");
		sleep(2);
		goto a;
	
	}
	
}
void adminarea()
{
	system("cls");
	printf("\t\t\t<WELCOME HUZAIFA, RAMEEN AND HAMMAD>\n\n");
	
	int a;
	printf("\n1-Add a Car\n2-Display all Cars\n3-Modify cars\n4-Delete cars from record\n5-Go back\n\nEnter your choice: ");
	scanf("%d",&a);
	if(a==1)
		addcar();
	else if (a==2)
		display();
	else if (a==3)
		updatecar();
	else if(a==4)
		deletecar();
	else if(a==5)
		login();
}

void addcar()
{
	system("cls");
	FILE *fp;
	int a,i;
	
	fp=fopen("carinformation.txt","a+");
	printf("\t\t\t<--:ADD RECORD:-->\n\n");
	printf("Enter the number of cars you want to add to the system: ");
	scanf("%d",&a);
	for(i=0;i<a;i++)
	{
	printf("\n\t\t\t*information about car %d*\n\n",i+1);
	printf("Enter the name of the car: ");
	car[i].name = (char *)malloc(50 * sizeof(char));
	scanf("%s",car[i].name);
	printf("Enter the model of car: ");
	car[i].model = (char *)malloc(20 * sizeof(char));
	scanf("%s",car[i].model);
	printf("Set the price of car: $");
	scanf("%d",&car[i].price);
	printf("Set the rent amount per day: $");
	scanf("%d",&car[i].rent);	
	fprintf(fp, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);	
}

	fclose(fp);
	printf("\nCAR ADDED SUCCESSFULLY!!\n\n");
	printf("Press any key to go back to admin page");
	getch();

adminarea();
}

void display()
{
	system("cls");	
	FILE *fp;
	printf("\t\t\t<--:VIEW RECORD:-->\n\n");
	fp=fopen("carinformation.txt","r");
    if(fp == NULL)
	{
	    printf("Error opening file.");
	    exit(1);
	}
    printf("--------------------------------------------------------------------\n");
    printf("NAME\t\tMODEL\t\tPRICE($)\tRENT(per day($))\n");
    printf("--------------------------------------------------------------------\n");

 while(fscanf(fp, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) > 0)
	{
		printf("%s\t\t%s\t\t%d\t\t%d\n",car[i].name,car[i].model,car[i].price,car[i].rent);
	}
	fclose(fp);
	printf("--------------------------------------------------------------------\n");
	printf("\n");
	printf("Press any key to go back to admin page");
	getch();
	adminarea();
	
}

void updatecar() {
    system("cls");
    char search[20], c;
    FILE *fp, *tempFile;
    int found = 0;

    fp = fopen("carinformation.txt", "r");
    tempFile = fopen("temp.txt", "w");

    if (fp == NULL || tempFile == NULL) {
        printf("Error opening file.");
        exit(1);
    }

    printf("\t\t\t<--:MODIFY RECORD:-->\n\n");
    printf("Enter the name of the car you want to modify: ");
    fflush(stdin);
    gets(search);
    fflush(stdin);

    while (fscanf(fp, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) != EOF) {
        if (strcmp(search, car[i].name) == 0) {
            found = 1;
            printf("Enter the new name of the car: ");
            scanf("%s", car[i].name);
            printf("Enter the new model of the car: ");
            scanf("%s", car[i].model);
            printf("Enter the new price of the car: $");
            scanf("%d", &car[i].price);
            printf("Enter the new rent amount per day: $");
            scanf("%d", &car[i].rent);
        }
        fprintf(tempFile, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);
    }

    fclose(fp);
    fclose(tempFile);

    remove("carinformation.txt");
    rename("temp.txt", "carinformation.txt");

    if(!found){
        printf("\nThe car with the name %s is not present in the records.\n", search);
    }else{
        printf("\nCAR UPDATED SUCCESSFULLY!!\n\n");
    }

    printf("Do you want to modify another Car?\nPress 'y' for Yes\nPress 'n' for No\nEnter your choice: ");
    fflush(stdin);
    scanf("%c", &c);
    fflush(stdin);
    if (c == 'n') {
        printf("\nPress any key to go back to Admin Area\n");
        getch();
        adminarea();
    } else {
        updatecar();
    }
}

void deletecar()
{
	system("cls");
	char c;
	printf("\t\t\t<--:DELETE RECORD:-->\n\n");
	FILE *fp,*fp1;
	fp=fopen("carinformation.txt","r+");
	fp1=fopen("delete.txt","a+");
	int r = 0;
	char search[20];
	printf("Enter the name of the car that you want to delete: ");
	fflush(stdin);
	gets(search);
	fflush(stdin);
	if(fp1==NULL)
	{
		printf("No file exist");
		exit(0);
	}
	if(fp==NULL)
	{
		printf("No file exist");
		exit(0);	
	}

	  while (fscanf(fp, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) > 0)
	{
		if(strcmp(search,car[i].name)!=0)
		{
	
			 fprintf(fp1, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);
		}
		  else
        {
            r = 1; 
        }
	}
	fclose(fp);
	fclose(fp1);
	if (!r)
    {
        printf("\nThe car with the name %s is not present in the records.\n", search);
    }
    else
    {
        printf("\nRECORD DELETED SUCCESFULLY!!\n\n");
        remove("carinformation.txt");
        rename("delete.txt", "carinformation.txt");
    }

	printf("Do you want to delete another car?\nPress 'y' for Yes\nPress 'n' for No\nEnter your choice: ");
	scanf("%c",&c);
	if(c=='n')
	{
		
		printf("\nPress any key to go back to Admin Area\n");
		getch();
		adminarea();
	}
	else
	deletecar();
}

void newusersignup()
{
	system("cls");
	FILE *fp;
	fp=fopen("newuser.txt","w");

	printf("\t\t\t<WELCOME COSTUMER>\n\n");
	char *username = user.username;
    char *passward = user.passward;
	printf("Set your username: ");
	fflush(stdin);
	gets(user.username);
	fflush(stdin);
	printf("Set your password: ");
	fflush(stdin);
	gets(user.passward);
	fprintf(fp, "%s %s\n", user.username, user.passward);
	fclose(fp);
	printf("\nPress any key to continue");
	getch();
	login();
}

void costumerlogin()
{
	system("cls");
	printf("\n\t\t\t<WELCOME COSTUMER>\n\n");
	char use[50],pass[50],ch,a;
	FILE *fp;
	fp=fopen("newuser.txt","r");
	char *username = user.username;
    char *passward = user.passward;

	a:
		printf("\nEnter Username: ");
		fflush(stdin);
		gets(use);
		fflush(stdin);	
		printf("Enter Password: ");
		gets(pass);
		
		if(strcmp(use,user.username)==0 && strcmp(pass,user.passward)==0)
		{
			fclose(fp);
			printf("\n\n");
			printf("LOGIN SUCCESFUL!!\n\nPress any key to continue");
			getch();
			buyerarea();
		}
		else
		{
			fflush(stdin);
			printf("\n\n");
			
			printf("INCORRECT USERNAME OR PASSWORD!!\n");
			printf("login again in 2 seconds\n");
			Beep(1000,1500);
			sleep(2);
			
			goto a;	
		}
}


int buyerarea()
{
	int a,c=0,b=0,d,e,f;
	e:
	system("cls");
	printf("\n\t\t\t<WELCOME COSTUMER>\n\n");
	printf("1-Buy a car\n2-Rent a car\n3-Print bill\n4-View sold cars\n5-View rented cars\n6-Go back\n7-Exit\n\nEnter your choice: ");
	scanf("%d",&a);

	if(a==1)
	{
	b=billbuy();
	goto e;
	}
	
	else if(a==2)
	{
	c=billrent();
	goto e;
	}
		
	else if(a==3)
	{
		
	f=b+c;
	
	totalbil(f,b,c);

	}
	
	else if(a==4)
	{
	
	soldcars();
		goto e;
	}
	
	else if(a==5)
	{
		
	rentcars();
		goto e;
	}
	
	else if(a==7)
	
	exit(0);
	
	else if(a==6)
	
	login();

}

void displaycars(){
	system("cls");
	char choise[20],modelcar[20];
	FILE *fp;
	fp=fopen("carinformation.txt","r+");
	printf("--------------------------------------------------------------------\n");
    printf("NAME\t\tMODEL\t\tPRICE\t\tRENT(per day)\n");
    printf("--------------------------------------------------------------------\n");
	
  while (fscanf(fp, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) > 0)
	{
		printf("%s\t\t%s\t\t%d\t\t%d\n",car[i].name,car[i].model,car[i].price,car[i].rent);	
	}
	printf("--------------------------------------------------------------------\n");
	fclose(fp);
}

int billrent()
{
	system("cls");
	char choise[20],a;
	int b,sum=0,summ=0;
	b:
	displaycars();
	FILE *fp,*fp2,*fp3;
	fp3=fopen("rent cars.txt","a+");
	fp=fopen("carinformation.txt","r+");
	fp2=fopen("soldcars.txt","a+");
	printf("Enter the name of the car you want to rent: ");
	fflush(stdin);
	gets(choise);
	fflush(stdin);
	printf("Enter the days in which you will return the car: ");
	scanf("%d",&b);
	fflush(stdin);

	if(fp2==NULL)
	{
		printf("No file exist");
		exit(0);
	}
	if(fp==NULL)
	{
	printf("no file exist");
		exit(0);	
	}
		
	 while (fscanf(fp, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) > 0)
	{
		
		if(strcmp(choise,car[i].name)==0)
		{
		printf("\nYou rented %s car for %d days in %d dollars\n\n",car[i].name,b,car[i].rent*b);
		sum=sum+car[i].rent*b;

  		fprintf(fp3, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);

		}
		
		else if(strcmp(choise,car[i].name)!=0 )
		{
			fprintf(fp2, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);
		}	
	}
	
fflush(stdin);

		fclose(fp);
		fclose(fp2);
		fclose(fp3);
		remove("carinformation.txt");
		rename("soldcars.txt","carinformation.txt");
		fflush(stdin);

		printf("Do you want to want to rent any other Car?\nPress 'y' for Yes\nPress 'n' for no\nEnter your choice: ");
		fflush(stdin);
		scanf("%c",&a);
		if(a=='y')
		{
		fflush(stdin);
		goto b;
			
		}
		else
		{
		
		fflush(stdin);
		
		return sum;
		}
}

int billbuy(){
	system("cls");
	char choise[20],a;
	int b,sum=0,summ=0;
	b:
	displaycars();
	FILE *fp,*fp2,*fp3;
	
	fp=fopen("carinformation.txt","r+");
	fp2=fopen("soldcars.txt","a+");
	fp3=fopen("sold cars.txt","a+");
	printf("Enter the name of the car you want to buy: ");
	fflush(stdin);
	gets(choise);
	fflush(stdin);
	
	if(fp2==NULL)
	{
		printf("No file exist");
		exit(0);
	}
	if(fp==NULL)
	{
	printf("No file exist");
		exit(0);	
	}
		
	  while (fscanf(fp, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) > 0)
	{	
		if(strcmp(choise,car[i].name)==0 )
		{
		
			printf("\nCONGRATULATIONS!! You bought %s in %d dollars\n\n",car[i].name,car[i].price);
			sum=sum+car[i].price;
			fprintf(fp3, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);
		}
		else if(strcmp(choise,car[i].name)!=0 )
		{
				
			 fprintf(fp2, "%s %s %d %d\n", car[i].name, car[i].model, car[i].price, car[i].rent);
		}		
	}
	

	fflush(stdin);

	fclose(fp);
	fclose(fp2);
	fclose(fp3);
	remove("carinformation.txt");
	rename("soldcars.txt","carinformation.txt");
	fflush(stdin);

	printf("Do you want to buy any other Car?\nPress 'y' for yes\nPress 'n' for No\nEnter your choice: ");
	fflush(stdin);
	scanf("%c",&a);
	if(a=='y')
	{
		fflush(stdin);
		goto b;
	
	}
	else
	{
		fflush(stdin);
		
		return sum;
	}
}


void soldcars()
{
	FILE *fp3;
	fp3=fopen("sold cars.txt","r+");
	printf("--------------------------------------------------------------------\n");
    printf("NAME\t\tMODEL\t\tPRICE\t\tRENT(per day)\n");
    printf("--------------------------------------------------------------------\n");
 	while (fscanf(fp3, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) == 4)
	{
			printf("%s\t\t%s\t\t%d\t\t%d\n",car[i].name,car[i].model,car[i].price,car[i].rent);	
	}
	printf("--------------------------------------------------------------------\n");
	printf("Press any key to go back");
	getch();
	fclose(fp3);
}

void rentcars()
{
	FILE *fp3;
	fp3=fopen("rent cars.txt","r+");
	printf("--------------------------------------------------------------------\n");
    printf("NAME\t\tMODEL\t\tPRICE\t\tRENT(per day)\n");
    printf("--------------------------------------------------------------------\n");

	 while (fscanf(fp3, "%s %s %d %d", car[i].name, car[i].model, &car[i].price, &car[i].rent) == 4)
	{
			printf("%s\t\t%s\t\t%d\t\t%d\n",car[i].name,car[i].model,car[i].price,car[i].rent);	
	}
	printf("--------------------------------------------------------------------\n");
	printf("Press any key to go back");
	getch();
	fclose(fp3);
}

int totalbil(int f, int b, int c) {
    system("cls");
    int rentBill = c;
    int buyBill = b;

    printf("\n\n\n\n\t\t\tTHANK YOU FOR VISITING H.R.H SHOWROOM!\n\n\n");

    printf("\t\t\tRent Bill: $%d\n", rentBill);
    printf("\t\t\tBuy Bill: $%d\n", buyBill);
    int t = rentBill + buyBill;
    printf("\t\t\tTotal bill= $%d", t);

    

    printf("\n\nYour total bill is: $%d", t);
    

    fflush(stdin);
    printf("\n\n\n\t\t\tHOPE TO SEE YOU AGAIN!\n\t\t\tBEST WISHES!");
}
