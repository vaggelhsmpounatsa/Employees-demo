# Employees-demo
#include <stdio.h>
 #include <stdlib.h>
 #include <conio.h>
 #include <stdlib.h>
 #include <string.h>

void addEmployee();
void displayEmployee();
void calculateEmployee();
void searchEmployee();
void removeEmployee ();

  struct employee {
        char first_name[100];
        char last_name[100];
        char speciality[100];
        char email[100];
        double  phonenumber;
        int  yearsalary;
    };

void main()
{
    
    int choice;
    while(choice!=5){
        
    printf("\t\t\t=====EMPLOYEE DATABASE MANAGEMENT SYSTEM=====");
    printf("\n\n\n\t\t\t\t     1. Add Employee\n");
    printf("\t\t\t\t     2. Display Employees\n");
    printf("\t\t\t\t     3. Search Employee\n");
    printf("\t\t\t\t     4. Remove Employee\n");
    printf("\t\t\t\t     5. Calculate Salary\n");
    printf("\t\t\t\t     6. Exit\n");
    printf("\t\t\t\t    _____________________\n");
    printf("\t\t\t\t     ");
    scanf("%d",&choice);
    
   switch(choice){
       case 1:
          addEmployee();
         break;
     case 2: 
          displayEmployee();
          printf("\t\t\t\t  press any key to exit..... \n");
         break;
     case 3:
         searchEmployee();
         printf("\n\t\t\t\t  Press any key to exit.......\n");
         break;
     case 4:
        removeEmployee();
        printf("\n\t\t\t\tPress any key to exit.......\n");
        break;
     case 6:
          printf("\n\t\t\t\tThank you, for used this software.\n\n");
          exit(0);
        break;
     default :
         printf("\n\t\t\t\t\tEnter a valid number\n\n");
         printf("\t\t\t\tPress any key to continue.......");
         break;
        	}
        }
     }
    
 void addEmployee() {
   
     char another;
     FILE *fp;
     int n,i;
     struct employee emp;
   do{

       printf("\t\t\t\t=======Add Employee Info=======\n\n\n"); 
       fp=fopen("information.xlsx","a"); 
         
        printf("\n\t\t\tEnter First Name      : ");
          scanf("%s",&emp.first_name);
          printf("\n\t\t\tEnter Last Name     : ");
          scanf("%s",&emp.last_name);
          printf("\n\t\t\tEnter Speciality    : ");
          scanf("%s",&emp.speciality);
          printf("\n\t\t\tEnter Email         : ");
          scanf("%s",&emp.email);
          printf("\n\t\t\tEnter Phone Number  : " );
          scanf("%lf",&emp.phonenumber);
          printf("\n\t\t\tEnter YearlySalary  : ");
          scanf("%d",&emp.yearsalary);
          printf("\n\t\t\t______________________________\n");
    
      if(fp==NULL){
        fprintf(stderr,"can't open file");
    }else{
        printf("\t\t\tInformation stored successfuly\n");
    }
    
    fwrite(&emp, sizeof(struct employee ), 1, fp); 
    fclose(fp);
    
    printf("\t\t\tYou want to add another record?(y/n) : ");
    
    
    scanf("%s",&another);
    
    
   }while(another=='y'||another=='Y');
}

void displayEmployee() {
 
   
     FILE *fp;

    struct employee emp;
    fp=fopen("information.xlsx","r");
    
     printf("\t\t\t\t=======Employee RECORD=======\n\n\n");
      
    if(fp==NULL){
        
        fprintf(stderr,"can't open file\n");
        exit(0);
    }else{
        printf("\t\t\t\tRECORDS :\n");
        printf("\t\t\t\t___________\n\n");
    }
        
        while(fread(&emp,sizeof(struct employee),1,fp)){
        printf("\n\t\t\t\t Employee Name  : %s %s",emp.first_name,emp.last_name);
        printf("\n\t\t\t\t Speciality    : %d",emp.speciality);
        printf("\n\t\t\t\t Email         : %s",emp.email);
        printf("\n\t\t\t\t Phone Number  : %lf",emp.phonenumber);
        printf("\n\t\t\t\t YearlySalary    : %d",emp.yearsalary);
        printf("\n\t\t\t\t ________________________________\n");
      
         }
        fclose(fp);

      
  }

void searchEmployee() {

       struct employee emp;
      FILE *fp;
     char last_name[100];
    int found =0;
   
    fp=fopen("information.txt","r");
    printf("\t\t\t\t=======SEARCH EMPLOYEE RECORD=======\n\n\n");
    printf("\t\t\tEnter the last_name : ");
   
    scanf("%d",&last_name);
     
    
    
    while(fread(&emp,sizeof(struct employee),1,fp)>0){
         
        if(emp.last_name==last_name){
           
        found=1;
        printf("\n\t\t\t\t Employee Name  : %s %s",emp.first_name,emp.last_name);
        printf("\n\t\t\t\t Speciality    : %d",emp.speciality);
        printf("\n\t\t\t\t Email         : %s",emp.email);
        printf("\n\t\t\t\t Phone Number  : %lf",emp.phonenumber);
        printf("\n\t\t\t\t YearlySalary    : %d",emp.yearsalary);
        printf("\n\t\t\t\t ________________________________\n");
  
         }
       
    }
     
    if(!found){
       printf("\n\t\t\tRecord not found\n");
    }
  
    fclose(fp);

    
}


 void removeEmployee(){
      struct employee  emp;
      FILE *fp, *fp1;
     

    char email[100];
	int found=0;
    
       printf("\t\t\t\t=======DELETE EMPLOYEE RECORD=======\n\n\n");
    fp=fopen("information.xlsx","r");
    fp1=fopen("temp.xlsx","w");
    printf("\t\t\t\tEnter the Employee's Email : ");
    scanf("%s",&email);
    
    if(fp==NULL){
         fprintf(stderr,"can't open file\n");
         exit(0);
      }
    
    while(fread(&emp,sizeof(struct employee),1,fp)){
        if(emp.email == email){
          
            found=1;
        
        }else{
           fwrite(&emp,sizeof(struct employee),1,fp1);
        }
  
    }
     fclose(fp);
     fclose(fp1);

    if(!found){
          printf("\n\t\t\t\tRecord not found\n");
        }
      if(found){ 
    remove("information.txt");
        rename("temp.xlsx","information.xlsx");
        
        printf("\n\t\t\t\tRecord deleted succesfully\n");
        }

  }
