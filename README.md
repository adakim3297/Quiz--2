# Quiz-2 정리
<주의사항>

학생들이 strings 과 structures 구문을 외워 기말고사를 잘 준비할 수 있도록 유도하는 퀴즈
이 범위에서는 구문을 중심으로 다음 항목을 다룹니다.
- 문자열(문자열 조작 기능 제외)
- 구조(구조 변수 및 포인터 제외)

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





## [Structure]
