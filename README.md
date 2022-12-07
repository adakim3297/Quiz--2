# Quiz 정리
<주의사항>

학생들이 strings 과 structures 구문을 외워 기말고사를 잘 준비할 수 있도록 유도하는 퀴즈
이 범위에서는 구문을 중심으로 다음 항목을 다룹니다.
- 문자열(문자열 조작 기능 포함)
- 구조(구조 변수 및 포인터 포함)

비공개 시험. 다음은 허용되지 않습니다.
- 모든 문서 액세스(강의 슬라이드, 대화 열기 등)
- 인터넷 검색
- 컴파일러 또는 IntelliSense 편집기(예: VS-Code) 사용

<공부 내용>

## [String] 
### String in C Language
* End of string -> null characters ('\0')
* 'H' : char // "H" : string // " " : Empty string
* str[i] == *(str + i);
* &str[i] == str + i;
* str2 = str1; (x) --> strcpy(str2, str1); (O)
* str1 = "Good Day"; (X) --> strcpy(str1, "Good Day");
* char str[9] = "Good Day";

    char *pStr = NULL;

    pStr = str; (O)

### String Input/Output Functions
* scanf("%s", message); // & is not necessary
* Console string I/O

--> Input
    
    char *gets(char* buffer);  (unsafe) {\n -> \0}
    char *fgets(char *buffer, int buffer_size, FILE *sp); {\n + \0}

--> Output

    int puts(char* str); (unsafe)
    int fputs(char *str, FILE *sp);

### String Manipulation Functions
* #include <string.h>
* string length : strlen

        int strlen(const char *string);

* string copy : strcpy, strcpy_s

        char *strcpy(char *toStr, const char *fromStr); (unsafe)

        char *strncpy(char *toStr, const char *fromStr, size_tmaxLen);

* string compare : strcmp, strncmp

        int strcmp(const char*str1, const char*str2);

        int strncmp(const char*str, const char*str2, int MaxLen)

* string concatenation(연속, 연접)

        char *strcat(char *str1, const char *str2);

        char *strncat(char *str, const char *str2, int maxLen);

* {Example}

    char str1[10] = "123";
    char str2[10] = "456";
    char str3[10];

    // such as "str3 = str1"
    strcpy(str3, str1);

    // comparison of strings
    strcmp(str1, str2)  < 0 > --> 뒤에가 더 큼
    strcmp(str1, str3) == 0  --> same

    // such as "str3 = str1 + str2";
    strcpy(str3,str2);
    strcpy(str3,str2);

### String/Data Conversion
* string -> values : sscanf

    ex : "256" -> 256 // "3.14" -> 3.14F
    
        int sscanf(char *str, const char *format_string, address_list);
        
        ex : ("35x50","%d x %d", &width, &height);

* values -> string : sprintf

        int sprintf(char *str, const char *format_string,value_list);

        ex : char message[128];
        sprintf(message, "width = %d, height = %d\n", width, height);

* string -> integer/Float

        #include <stdlib.h>
        int atoi(const char *str); -> integer 로 바꾼다.

        long atol(const char *str); -> long 으로 바꾼다.

        float atof(const char *str); -> float 로 바꾼다.



{String Example}

        #include <stdio.h>
        #include <string.h>

        int main(){

            int a = 0, b = 0, c = 0;
            char str1[30], str2[30], str3[30];

            printf("Input two numbers : ");
            scanf("%s %s", str1, str2); // assume input "123 456"

            a = atoi(str1);
            sscanf(str2, "%d", &b);

            c = a + b;

            strcpy(str3, str1);
            strcat(str3, str2);

            printf("%d + %d = %d\n", a, b, c);
            printf("\"%s\" + \"%s\" = \"%s\"\n", str1, str2, str3);

            return 0;
        }   

    결과 :

    123 + 456 = 579
    "123" + "456" = "123456"




        #include <stdio.h>

        int my_strlen(char str[]);
        char* my_strcpy(char dest[], char src[]);
        char* my_strcat(char dest[], char src[]);
        int my_strcmp(char dest[], char src[]);

        int main()
        {
            char str1[] = "HELLO";
            char str2[] = "ABCDE";
            char str3[256] = "";

            printf("my_strlen(str1) = %d\n", my_strlen(str1)); // 5

            my_strcpy(str3, str1);
            printf("str3 = %s\n", str3); // HELLO


            printf("my_strcmp(str1, str3) = %d\n", my_strcmp(str1, str3)); // 0 ; because same string
            printf("my_strcmp(str1, str2) = %d\n", my_strcmp(str1, str2)); // positive 
            printf("my_strcmp(str2, str1) = %d\n", my_strcmp(str2, str1)); // negative

            my_strcat(str3, str2);
            printf("str3 = %s\n", str3);

        return 0;
        }

        int my_strlen(char str[])
        {
            int i = 0;
            while(str[i] != 0)
            i++;

            return i;
        }

        char* my_strcpy(char dest[], char src[])
        {
            int i = 0;
            for(i = 0; src[i] != 0; i++)
            dest[i] = src[i];

            dest[i] = 0;// null

            return dest;
        }
        /*
        char* my_strcat(char dest[], char src[])

        {
        int dest_len = 0;       // the length of dest
        char result[256] = " ";
        int i = 0;
        int j =0;
        // copy scr[i] to dest[i + dest_len], for all i's

        for(i = 0; i < dest[i] != 0; i++){
        result[i] = dest[i];
        }
        for(j=0; j< src[j] != 0; j++){
        result[i+j] = src[i];
        }

        result[i+j] = 0;

        return result;
        */


        //     dest == "HELLO"
        //     src == "ABCDE"
        //     after function call, dest should be "HELLOABCDE" 

        char* my_strcat(char dest[], char src[])
        {
            //    my_strcpy(dest + my_strlen(dest), src);   // the simplest solution

            int dest_len = 0;    
            // get the length of dest
            while(dest[dest_len] != 0)
            dest_len++;

            // copy src[i] to dest[i + dest_len], for all i's
            int i = 0; 
            for(i = 0; src[i] != 0; i++)
            dest[i + dest_len] = src[i];

            dest[i + dest_len] = 0;

            return dest;
        }


        int my_strcmp(char str1[], char str2[])
        {
            int i = 0;
            int j = 0;
            int retval = 0;
            for(i = 0; i < str1[i] != 0; i++){
                for(j = 0; j < str2[j] != 0; j++){
                    if(str1[i] == str2[j])
                    retval = 0;
                    else if(str1[i] < str2[j])
                    retval = -1;
                    else
                    retval = 1;
                }
            }
            return retval;
        }


        /*
        dest == "HELLO"
        src == "ABCDE"
        after function call, dest should be "HELLOABCDE" 

        */
        /*
        char* my_strcat(char dest[], char src[])
        {
            char *p = dest;
            while(*p != 0)
            p++;

            while(*src != 0)
                *(p++) = *(src++);
            *p = 0;

            return dest;
        }
        */

        /*
        int my_strcmp(char str1[], char str2[])
        {
            int i = 0;

            for(i = 0; str1[i] == str2[i] && str1[i] != 0; i++){}

            return (int)str1[i] - (int)str2[i];
        }
        */




# summarize

## String length

size_t strlen( const char* str );

## String copy

char* strcpy( char* dest, const char* src );

char *strncpy( char *dest, const char *src, size_t count );

errno_t strcpy_s( char *dest, rsize_t destsz, const char *src );


## String concatenation

char *strcat( char *dest, const char *src );

char *strncat( char *dest, const char *src, size_t count);

errno_t strcat_s(char *dest, rsize_t destsz, const char *src
);

## String comparison

int strcmp( const char *lhs, const char *rhs );

int strncmp( const char *lhs, const char *rhs, size_t count );



        #include <stdio.h>
        #include <stdlib.h>
        #include <string.h>

        #define FILE_NAME "studentInfo.txt"

        #define MAX_STUDENT 512

        char ID[MAX_STUDENT][16] = { 0 };
        char first_name[MAX_STUDENT][64] = { 0 };
        char second_name[MAX_STUDENT][64] = { 0 };
        char tel[MAX_STUDENT][32] = { 0 };
        int n = 0;

        int main(){

        FILE *fp = fopen(FILE_NAME,"r");
        if(fp == NULL){
        printf("Failed to open file %s.\n", FILE_NAME);
        exit(-1);
        }

        int ret = 0;
        for(n = 0; ret != EOF; n++){
        ret = fscanf(fp,"%s", ID[n]);
        if(ret == EOF)
        break;

        fscanf(fp,"%s", first_name[n]);
        fscanf(fp,"%s", last_name[n]);
        fscanf(fp,"%s", tel[n]);

        }

        fclose(fp);

        printf("Totally, %d students. \n", n);
        for(int i = 0; i<n; i++){
        printf("%d) %s %s %s %s\n", i, ID[i], first_name[i], last_name[i], tel[i]);
        }

        char target_name[64] = "";
        // read target name
        printf("Input first name to find: ");
        scanf("%s", target_names);

        // find the index of the target name
        int p = 0;
        for([p=0; p<n; p++){
        //    if(first_name[p] == target_name) // compares the addresses of first_name[p] amd target_name (incorrect)
        if(strcmp(first_name[p], target_name) == 0) // compares the contents of first_name[p] and target_name
        break;
        }

        //display the result
        if(p == n)
        printf("Failed to find name %s.\n",target_name);
        else
        printf("The phone number of %s is %s.\n", target_name, tel[p]);

        return 0;
        }




        /*

        string manuplation functions (declared in string.h)
        // #include <string.h>

        char str1[64] = "ABC";
        char str2[64] = "123";

        *string comparison
        int strcmp(const char str1[], const char str2[]);
        compares the contents of str1 and str2
        if str1 is the same as str2, returns 0
        if str1 precedes str2, returns a negative number
        if str2 precedes str1, returns a positive number

        if(str1 == str2)      // compares addresses (incorrect)
        if(strcmp(str1, str2) == 0)   // compares contents (correct)
        if(*str1 == *str2)      // compares only the first characters (incorrect)   *str1 == str1[0]

        * string copy
        str2 = str1;        // incorrect

        strcpy(str2, str1) // correct

        char* strcpy(char dest[], const char src[]);
        copy the content of src to dest

        * string concatenation

        str3 = str1 + str2; // to make "ABC123" (incorrect)

        char* strcat(char dest[], const char src[]);

        strcpy(str3, str1);
        // str3 == "ABC"
        strcat(str3, str2);
        // str3 == "ABC123"




        address operator &: variable --> address
        dereferencing operator *: address --> content (variable)

        The operand of * should be a pointer.




## [Structure

### typedef
*       void * malloc(size_t size);
* typedef <existing type> <new_type>;
    
        typedef unsigned int size_t;

        typedef int INTEGER;
        INTEGER x;

        typedef char* STRING;
        STRING stringPtrArray[20];

### Enumerated type
*       enum tag { identifier_list};
        
        enum Color {RED, BLUE, GREEN, WHITE};

* Function parameter

        int func(enum Color curColor);
* Definition of Color type
    
        enum Color {RED, BLUE, GREEN, WHITE};
* variable declarations
    enum Color x, y, z;


    EXAMPE

            /*
        // syntax
        //    sprintf(string, format_string, value_list);
        //    sscanf(string, format_string, address_list);

        char str[100];
        int x = 123;
        // convert x into string
        sprintf(str, "%d", x);


        char str[] = "ABC 123";
        char s1[100] = "";
        int x = 0;

        // convert str into a string (s1) and an integer (x)
        sscanf(str, "%s %d", s1, &x);

        */

        #include <stdio.h>

        // #define HDH 1
        // #define NTH 2
        // #define NMH 3
        // #define OH 4

        // enum <tag> { list_of_symbols };
        enum BUILDING_IDs { UNKNOWN, HDH, NTH, NMH, OH }; 

        // typedef <existing_type> <new_type_name>;
        typedef enum BUILDING_IDs building_id;

        //void PrintBuildingName(int buildingID);
        //void PrintBuildingName(enum BUILDING_IDs buildingID);
        void PrintBuildingName(building_id buildingID);

        int main()
        {
        //    int buildingID = 0;
        //    enum BUILDING_IDs buildingID = 0;
            building_id buildingID = 0;

            printf("What building are you staying in? (1: HDH, 2: NTH, 3: NMH, 4: OH): ");

            scanf("%d", &buildingID);

            printf("Good! You are staying in ");
            PrintBuildingName(buildingID);
            printf(".\n");

            return 0;
        }

        //void PrintBuildingName(int buildingID)
        //void PrintBuildingName(enum BUILDING_IDs buildingID)
        void PrintBuildingName(building_id buildingID)
        {
            switch(buildingID){
        //    case 1:
            case HDH:
                printf("HDH");
                break;

        //    case 2:
            case NTH:
                printf("NTH");
                break;

        //    case 3:
            case NMH:
                printf("NMH");
                break;

        //    case 4:
            case OH:
                printf("OH");
                break;

            default:
                printf("Unknown building name");
                break;
            }
        }

        /*
        define an enumeration for colors (COLORS)
        BLUE, RED, GREEN, WHITE, BLACK 
        */

### Structures
* struct [tag] {
    field list // field (member variable) declarations
};

        경우 1
        struct STUDENT{
            char id[10];
            char name[26];
            enum Major major;
        };

        경우 2
        typedef struct{
            char id[10];
            char name[26];
            enum Major major;
        }STUDENT;


* Defining structure VS Declaring structure variable

    making molding frame VS making a product by molding

* Structure Variable Declaration
    struct tag var_name;
        
        경우 1 : struct STUDENT student[50];

    typename var_name;

        경우 2 : STUDENT student[50];

* Accessing Structures

        typedef struct{
            int x;
            int y;
        }Point;

        typedef struct{
            int width;
            int height;
        }Size;

        Size Getsize(Point p1, Point p2){
            Size s;
            s.width = abs(p1.x-p2.x);
            s.height = abs(p1.y-p1.y);

            return s;
        }

Example : Multiply Fractions

        // numerator, denominator
        #include <stdio.h>
        typedef struct{
            int numerator;
            int denominator;
        }Fraction;

        int main(){

            Fraction fr1;
            Fraction fr2;
            Fraction res;

            printf("Key first fraction in the form of x/y: ");
            scanf("%d/%d", &fr1.numerator, &fr1.denominator);
            printf("Key second fraction in the form of x/y: ");
            scanf("%d/%d", &fr2.numerator, &fr2.denominator);

            res.numerator = fr1.numerator * fr2.numerator;
            res.denominator = fr1.denominator * fr2.denominator;

            printf("\nThe result of %d/%d * %d/%d is %d/%d",fr1.numerator,fr1.denominator,fr2.numerator,fr2.denominator,res.numerator,res.denominator);

            return 0;
        }

* Pointer To Structures

        typedef struct{
            int X;
            int Y;
            float t;
            char u;
        }SAMPLE;

        ...
        SAMPLE sam1;
        SAMPLE* ptr = NULL;
        ...
        ptr = &sam1;
        ...


        따라서 2가지로 나눠서 볼 수 있음
        1. sma1.x
        2. (*ptr).x  --> *ptr.x (incorrect)
        (*pointerName).fieldName = pointerName -> fieldName


Example : Clock

    #include <stdio.h>

    typedef struct{
        int hr;
        int min;
        int sec;
    }CLOCK;

    void increment(CLOCK* pClock);
    void show(CLOCK* pClock);

    int main(){

        int i = 0;
        CLOCK clock = {14,38,56};

        for(i=0; i<6; ++i){
            increment(&clock);
            show(&clock);
        }

        return 0;
    }

    void increment(CLOCK *pClock){
        (pClock->sec) ++;
        if(pClock->sec == 60){
            pClock->sec = 0;
            (pClock->min)++;
            if(pClock->min == 60){
                pClock->min = 0;
                (pCLock->hr)++;
                if(pClock->hr == 60){
                    pClock->hr = 0;
                }
            }
        }
    }

    void show (CLOCK *PClock){
        printf("%02d:%02d:%02d\n",pClock->hr,pClock->min,pClock->sec);
    }


* Nested Structures

        typedef struct{
            int month;
            int day;
            int year;
        }Date;

        typedef struct{
            int hour;
            int min;
            int sec;
        }Time;

        typedef struct{
            Date date;
            Time time;
        }Stamp;


        ex) Stamp stamp;
        stamp.date.month = 11;

### Unions

* 


EXAMPLE

        structure: a custom data type that combines existing types
        declare a structure to represent computers (name: Computer)
        a computer is represented by the CPU model, RAM size, and SSD size

        struct tComputer {
        char CPU[64];
        int RAM;
        int SSD;   
        };

        typedef struct {
        char CPU[64];
        int RAM;
        int SSD;
        } Computer;

        structure variable: 
        declare a structure variable 'comp'

        Computer comp = { "" };

        structure pointer:
        declare a structure pointer 'comp_ptr'

        Computer *comp_ptr = NULL;

        Set comp to ("Ryzen 5600", 16, 512)

        strcpy(comp.CPU, "Ryzen 5600");
        comp.RAM = 16;
        comp.SSD = 512;

        Set comp to ("Ryzen 5600G", 32, 1024) through comp_ptr

        comp_ptr = &comp;      // makes the relation comp_ptr -----> comp

        strcpy(comp_ptr->CPU, "Ryzen 5600G");
        comp_ptr->RAM = 32;
        comp_ptr->SSD = 1024;
        */


        /*
        Stamp.min    INCORRECT
        stamp.min    INCORRECT
        Stamp.Time.min INCORRECT
        stamp.time.min CORRECT

        <struct_variable>.<field_name>

        */

        #include <stdio.h>
        #include <stdlib.h>

        #define FILE_NAME "studentInfo.txt"
        #define MAX_STUDENT 256

        char ID[MAX_STUDENT][16] = { 0 };
        char first_name[MAX_STUDENT][64] = { 0 };
        char last_name[MAX_STUDENT][64] = { 0 };
        char tel[MAX_STUDENT][16] = { 0 };

        // TO DO: declare a structure to represent students (name it Student)
        typedef struct{
            char ID[16];
            char first_name[64];
            char last_name[64];
            char tel[16];
        }Student;

        int main(){
        // TO DO: declare an array of Student
            Student student[MAX_STUDENT]; // students[i].ID is the i-th element

        int no_student = 0;


        // TO DO: modify main to work with the struct array

            // open file
            FILE *fp = fopen(FILE_NAME, "r");
            if(fp == NULL){
                printf("Failed to open %s.\n", FILE_NAME);
                exit(-1);
            }

            // read student info
            while(1){
                int ret = fscanf(fp, "%s", student[no_student].ID);
                if(ret == EOF)
                    break;
                    
                fscanf(fp, "%s", student[no_student].first_name);
                fscanf(fp, "%s", student[no_student].last_name);
                fscanf(fp, "%s", student[no_student].tel);

                no_student++;
            }

            // close file
            fclose(fp);

            // display student info
            for(int i = 0; i < no_student; i++)
                printf("%s %s %s %s\n", student[i].ID, student[i].first_name, student[i].last_name, student[i].tel);

            return 0;
        }

        /*
        // Mission: rewrite this program using structures

        #include <stdio.h>
        #include <stdlib.h>

        #define FILE_NAME "studentInfo.txt"
        #define MAX_STUDENT 256

        // char ID[MAX_STUDENT][16] = { 0 };
        // char first_name[MAX_STUDENT][64] = { 0 };
        // char last_name[MAX_STUDENT][64] = { 0 };
        // char tel[MAX_STUDENT][16] = { 0 };

        // TO DO: declare a structure to represent students (name it Student)
        typedef struct {
            char ID[16];
            char first_name[64];
            char last_name[64];
            char tel[16];
        } Student;

        // TO DO: declare an array of Student
        //Student students[MAX_STUDENT];      // students[i] is the i-th element
        Student *students[MAX_STUDENT];      // students[i] is the i-th element
        int no_student = 0;


        // TO DO: modify main to work with the struct array


        int main()
        {
            // open file
            FILE *fp = fopen(FILE_NAME, "r");
            if(fp == NULL){
                printf("Failed to open %s.\n", FILE_NAME);
                exit(-1);
            }

            // read student info
            while(1){
                // dynamic memory allocation
                students[no_student] = (Student*)malloc(sizeof(Student));

        //        int ret = fscanf(fp, "%s", ID[no_student]);
                int ret = fscanf(fp, "%s", students[no_student]->ID);
                if(ret == EOF)
                    break;
                    
                fscanf(fp, "%s", students[no_student]->first_name);
                fscanf(fp, "%s", students[no_student]->last_name);
                fscanf(fp, "%s", students[no_student]->tel);

                no_student++;
            }

            // close file
            fclose(fp);

            // display student info
            for(int i = 0; i < no_student; i++)
                printf("%s %s %s %s\n", students[i]->ID, students[i]->first_name, students[i]->last_name, students[i]->tel);


            // deallocate all dynamic memory blocks
            for(int i = 0; i < no_student; i++){
                free(students[i]);
                students[i] = NULL;         // for safety
            }

            return 0;
        }

        */



        #include <stdio.h>

    // declare a structure to represent fractions
    typedef struct{
        int numerator;
        int denominator;
    }Fraction;


    Fraction MultiplyFraction(Fraction f1, Fraction f2);

    int main(){

        // declare three fraction variables, fr1, fr2, and res 
        Fraction fr1;
        Fraction fr2;
        Fraction res;

        // read two fractions fr1 and fr2 from the user
        printf("Fraction numerator and denominator x/y: ");
        scanf("%d/%d", &fr1.numerator, &fr1.denominator);

        printf("Fraction numerator and denominator x/y: ");
        scanf("%d/%d", &fr2.numerator, &fr2.denominator);

        // multiply fr1 and fr2 into res
        
        Fraction MultiplyFraction(fr1, fr2);
        // res.numerator = fr1.numerator * fr2.numerator;
        // res.denominator = fr1.denominator * fr2.denominator;

        // display result
        printf("numerator : %d, denominator : %d\n",res.numerator, res.denominator);

        return 0;

    }

    // Next mission:
    //    * write the following function
    //    Fraction MultiplyFraction(Fraction f1, Fraction f2);

    //    * simplify main() using MultiplyFraction()

    Fraction MultiplyFraction(Fraction f1, Fraction f2)
    {
        Fraction res;
        ret.num = fr1.num * fr2.num;
        ret.denom = fr1.denom * fr2.denom;

        return ret;    
    }


    /*
    Fighter.type = 'A';     // wrong unless type is a static field
    f.type = 'A';           // correct
    */


    /*
    *ptr === sam1
    ptr->x === (*ptr).x === sam1.x
    *ptr.x === *(ptr.x) != sam1.x

    for a structure variable, use . operator (e.g., sam1.x)
    for a structure pointer, use -> operator (e.g., ptr->x)
    */
