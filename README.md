# Quiz--2
# Quiz
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


## [Structure]
