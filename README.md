# sets
Compares sets
/******************************************************************************

Rory Corn.

Program that compares two peoples attribute, male and female and registers the 
pair as a match or not a match.

You the user will enter the number of couples to be compared, enter their 
first name, last name and attributes. If any one attribute in the males 
attributs matches any one attribute in the females it will rgister as a 
match, if not, then "no match" will be registered.

Writen in C Program.

*******************************************************************************/
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 5
//Struct PERSON to register first name, last name and age
typedef struct{
    char firstName[20];
    char lastName[20];
    int age;
}PERSON;
//User menu
void menu(void);
//Prototype
void get_value(int arr[]);
void print_value(int arr[], int n);
void function_sort(int arr[]);
int find_intersection(int array1[], int array2[], int intersection_array[]);
int find_union(int array1[], int array2[], int union_array[]);

int main()
{
    menu();
   
    int num_elements;
    
    int *array1;
    array1 = malloc(SIZE * sizeof(int));   //Dynamic array1
    if(array1 == NULL){
        printf(" Memory failed to allocate!\n "); //Checks for error in Dynamic array
        exit(-1);
    }
    int *array2;
    array2 = malloc(SIZE * sizeof(int));  //Dynamic array2
    if(array2 == NULL){
        printf(" Memory failed to allocate!\n ");  //Checks for error in Dynamic array
        exit(-1);
    }
    int *intersection_array;
    intersection_array = malloc(SIZE * sizeof(int)); //Dynamic array
    if(intersection_array == NULL){
        printf(" Memory failed to allocate!\n ");  //Checks for error in Dynamic array
        exit(-1);
    }
    int *union_array;
    union_array = malloc((SIZE * 2) * sizeof(int)); //Dynamic array
    if(union_array == NULL){
        printf(" Memory failed to allocate!\n ");  //Checks for error in Dynamic array
        exit(-1);
    }
    int reg, i;
    printf(" Enter your maximum number of couples to be registered: "); //User enters max number
    scanf("%d", &reg);
    printf(" Number entered: %d\n", reg);
    
    
    PERSON *registry;
    registry = (PERSON*)malloc(reg * sizeof(int)); //Dynamic struct
    if(registry == NULL){
        printf(" Memory failed to allocate!\n ");  //Checks for error in Dynamic struct
        exit(-1);
    }
    strcpy(registry->firstName, "");
    strcpy(registry->lastName, "");
    registry->age = 0;
    
    //User enters couple to be compared information
    for(i = 1; i <= reg; i++){
        printf(" For couple %d\n\n", i);
        printf(" Enter the couples names and ages (First name, Last name and Age on the same line)\n");
        printf(" (males 1st, press enter, then females 2nd): ");
        
        scanf("%s %s %d", registry->firstName, registry->lastName, &registry->age);
        printf(" That's Mr. %s %s age %d. Now for the female: ", registry->firstName, registry->lastName, registry->age);
        scanf("%s %s %d", registry->firstName, registry->lastName, &registry->age);
        printf(" and Ms. %s %s age %d.\n", registry->firstName, registry->lastName, registry->age);
    //input elements of Array1
    printf("\n Enter the elements of the Male Array: \n");
    get_value(array1);
    printf("\n\n Elements of the Male Array: ");
    print_value(array1, SIZE);
    
    //Sort array 1
    function_sort(array1);
    printf("\n\n Sorted elements of the Male Array: ");
    print_value(array1, SIZE);
    
    //input elements of Array2
    printf("\n Enter the elements of the Female Array: \n");
    get_value(array2);
    printf("\n\n Elements of the Female Array: ");
    print_value(array2, SIZE);
    
    //Sort array 2
    function_sort(array2);
    printf("\n\n Sorted elements of the Female Array: ");
    print_value(array2, SIZE);
    
    //Find Intersection
    num_elements = find_intersection(array1, array2, intersection_array);
    if((*intersection_array) >= 1){
        
    printf("\n\n Intersection is: ");
    print_value(intersection_array, num_elements);
    printf("\n We have a match!\n");
    }
    else{
        printf("\n\n Disjoint Set\n");
        printf("\n Sorry no match!\n");
    }
    //Find Union
    num_elements = find_union(array1, array2, union_array);
    if((*intersection_array) >= 1){
    printf("\n\n Union is: ");
    print_value(union_array, num_elements);
    printf("\n This couple does match.\n");
    }
    else{
        printf("\n\n This couple did not match.\n");
    }
    printf("\n");
    }
    //Free the Dynamic memory
    free(array1);
    free(array2);
    free(intersection_array);
    free(union_array);
    free(registry);
}
void menu(){  //User menu instructions
    printf("\n\tThis menu pairs two people that have at least one matching quality.");
    printf("\n\tOne quality or qualities that match, links the two poeple together.");
    printf("\n\tChoose 5 qualities from the menu below.");
    printf("\n\t5 for each male and 5 for each female.\n");
    printf("\n\t1. Travel 3. Works 5. Has Pets 7. Social 9. Night Owl\n");
    printf("\n\t2. Religiuos 4. Social Drinker 6. Smokes 8. Exercises 10. Early Bird\n\n");
}

void get_value(int arr[])
{
    int i, j;
    for (i = 0; i < SIZE; i++)
    {
        j = i + 1;
        printf("\n Enter quality %d: ", j);
        scanf("%d", &arr[i]);
    }
}

void print_value(int arr[], int n)
{
    int i;
    printf("{ ");
    for (i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("}");
}

void function_sort(int arr[])
{
    int i, j, temp, swapping;
    
    for (i = 1; i < SIZE; i++)
    {
        swapping = 0;
        for (j = 0; j < SIZE-i; j++)
        {
            if (arr[j] > arr[j+1])
            {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapping = 1;
            }
        }
        if (swapping == 0)
        {
            break;
        }
    }
}

int find_intersection(int array1[], int array2[], int intersection_array[])
{
    int i = 0, j = 0, k = 0;
    while ((i < SIZE) && (j < SIZE))
    {
        if (array1[i] < array2[j])
        {
            i++;
        }
        else if (array1[i] > array2[j])
        {
            j++;
        }
        else
        {
            intersection_array[k] = array1[i];
            i++;
            j++;
            k++;
        }
    }
    return(k);
}

int find_union(int array1[], int array2[], int union_array[])
{
    int i = 0, j = 0, k = 0;
    while ((i < SIZE) && (j < SIZE))
    {
        if (array1[i] < array2[j])
        {
            union_array[k] = array1[i];
            i++;
            k++;
        }
        else if (array1[i] > array2[j])
        {
            union_array[k] = array2[j];
            j++;
            k++;
        }
        else
        {
            union_array[k] = array1[i];
            i++;
            j++;
            k++;
        }
    }
    if (i == SIZE)
    {
        while (j < SIZE)
        {
            union_array[k] = array2[j];
            j++;
            k++;
        }
    }
    else
    {
        while (i < SIZE)
        {
            union_array[k] = array1[i];
            i++;
            k++;
        }
    }
    return(k);
}

