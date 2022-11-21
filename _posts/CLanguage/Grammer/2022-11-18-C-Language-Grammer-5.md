---
title: "문자열 함수들"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-11-18
last_modified_at: 2022-11-21
---

## 11. 문자열 함수들  

### 11.1 문자열을 정의하는 방법들  

![TBC](https://user-images.githubusercontent.com/37467408/202934780-ece101ef-fb23-4d8f-b943-2fe6f68e60e1.PNG)  

`Put() 함수의 활용법`  

```cpp
#define MESSAGE "A Symbolic string constant"
#define MAXLENGTH 81

puts("Puts() adds a newline at the end");

puts(MESSAGE);

char words[MAXLENGTH] = "A string in an array";
puts(words);

char Nums[50] = "One " "Two " "Three " "Four ";
puts(Nums);
```  

`Output`  

> Puts() adds a newline at the end  
> A Symbolic string constant  
> A string is an array  
> One Two Three Four  

`문자형 배열을 포인터로 저장했을 때 수정할 수 없다.`  

```cpp
// const char* ptr1 = "A pointer to a string"; // Good  
char* ptr1 = "A pointer to a string"; // Bad

puts(ptr1);

ptr1[8] = 'A'; // Work at Release Mode
               // Error! Didn't work on Debug Mode
puts(ptr1);
```

문자형 배열은 다른 자료형과 달리 읽기 전용 메모리에 저장된다.  

문자혈 배열을 사용할 때는 const를 사용하자.  

`문자형 배열의 활용1`  

```cpp
printf("%s, %p, %c \n", "We", "are", *"humans");

char test[10] = "Test!";
for(int i = 0 ; i < 10 ; ++i)
    printf("%d ", (int)test[i]);
printf("\n");
```  

'Output'  
> We, 001F7D50, h  
> 84 101 115 116 33 0 0 0 0 0  

`문자형 배열의 활용2`  

```cpp
const char test2[10] = { 'T', 'e', 's', 't', '!', '\0' }; // Compiler automatically add '\0'
char test3[2 * sizeof(int)] = { 'T' };
```

### 11.2 메모리 레이아웃과 문자열  

![TBC](https://user-images.githubusercontent.com/37467408/202935949-9909f6d1-093c-4eca-9a78-9763ddc7b985.PNG)  

- Stack : 얼마의 저장공간을 사용해야 하는지 알 수 있을 때 사용한다. 지역변수가 다음에 해당한다. 다른 처리에 비해 속도가 빠르다.  
- Heap : 얼마의 저장공간을 사용해야 하는지 알 수 없을 때 사용한다.  
- BSS Segment(초기화되지 않은 전역/정적 변수) : 0으로 초기화  
- DATA Segment(초기화된 전역/정적 변수) : 정해진 값으로 초기화  
- Test Segment : 컴파일된 코드는 변경되지 않는다. 읽기 전용이다.  

![TBC](https://user-images.githubusercontent.com/37467408/202936165-f5ef8d49-6b10-41a9-9411-148d532e31f4.PNG)  

arr[]은 저장공간을 가지고 있고 크기가 정해져있다. Argument로 사용될 때는 Stack에 저장된다.  

전역/정적 변수로 사용되면 DATA Segment에 저장된다. String literal이라고 부른다.  

`(char*) pointer str에 접근하여 값을 바꾸어보자`  

![TBC](https://user-images.githubusercontent.com/37467408/202936271-622f21b1-cb1c-4307-9101-294c0c354579.PNG)  

*str은 DATA Segment 중에서 Read Only DATA Segment에 저장된다. 그래서 *str을 통하여 값 변경을 시도하면 에러가 발생되는 것이다. 다음 예제를 보자  

```cpp
const char* ptr1 = "I am a string!.";
const char* ptr2 = "I am a string!.";
const char* ptr3 = "I am a different string!.";
printf("%p %p %p %p\n", ptr1, ptr2, "I am a string!.", ptr3);

const char arr1[] = "I am a string!.";
const char arr2[] = "I am a string!.";
const char arr3[] = "I am a different string!.";
const char arr4[] = "I am another string!.";
printf("%p %p %p %p\n", arr1, arr2, arr3, arr4);
```  

`output`  
> 00A87B90 00A87B90 00A87B90 00A87C4C  
> 004FF794 004FF77C 004FF758 004FF738  

주소값을 보면 ptr1 == ptr2 == "I am a string!."이다. 이는 컴파일러가 중복되는 자료를 동일한 위치에 저장하여 읽어오고 있음을 알 수 있다.  

따라서 ptr[0] = 'A'와 같이 값을 바꾸면 참조하고 있는 다른 값도 바꾸어버리기 때문에 Compiler가 이러한 명령에 Error 메세지를 출력한다.  

이 때, ptr1, ptr2 자기 자신의 주소값은 개별적이다. (ptr1++) 연산은 가능하며 첫 값을 빼고 출력할 것이다.  

배열은 문자열 값이 모두 동일하더라도 서로 다른 공간에 저장되어 있음을 알 수 있다.  

`주소 위치에서 메모리 레이아웃 확인하기`  

위 메모리 레이아웃을 참고하여 다음 코드에서 Stack, Heap의 주소가 어느쯤에 저장되는지 가늠해보자.  

```cpp
void test(int i)
{
    int j;
    // printf("stack high func's v    %p %p\n", &i, &j);
    printf("stack high func's ad %llu %llu\n", (unsigned long long)&i, (unsigned long long)&j);
}

int main()
{
    const char* ptr1 = "I am a string!.";
    const char* ptr2 = "I am a string!.";
    const char* ptr3 = "I am a different string!.";
    //printf("row data   str's v   %p %p %p %p\n", ptr1, ptr2, "I am a string!.", ptr3);
    printf("row data   str's v   %llu %llu %llu %llu\n", (unsigned long long)ptr1, (unsigned long long)ptr2, (unsigned long long)"I am a string!.", (unsigned long long)ptr3);
    
    //printf("stack high str's add %p %p %p %p\n", &ptr1, &ptr2, &"I am a string!.", &ptr3);
    printf("stack high str's add %llu %llu %llu %llu\n", (unsigned long long) &ptr1, (unsigned long long) &ptr2, (unsigned long long) &"I am a string!.", (unsigned long long) &ptr3);

    const char arr1[] = "I am a string!.";
    const char arr2[] = "I am a string!.";
    const char arr3[] = "I am a different string!.";
    const char arr4[] = "I am another string!.";
    //printf("stack high arr's add %p %p %p %p\n", arr1, arr2, arr3, arr4);printf("stack high arr's add %llu %llu %llu %llu\n", (unsigned long long)arr1, (unsigned long long)arr2, (unsigned long long)arr3, (unsigned long long)arr4);
    
    test(1);
    
    char* c_ptr = (char*)malloc(sizeof(char) * 100);
    //printf("Heap middle dynmic   %p\n", c_ptr);
    printf("Heap middle dynmic   %llu\n", (unsigned long long)c_ptr);
}
```  

`Output`  
> row data   str's v   140697274523344 140697274523344 140697274523344 140697274523360  
> stack high str's add 751626614664 751626614696 140697274523344 751626614728  
> stack high arr's add 751626614760 751626614808 751626614856 751626614920  
> stack high func's ad 751626614608 751626614356  
> Heap middle dynmic   2847777573344  

gcc는 반대 방향으로 저장된다. 위는 MSVC의 결과값이다.  

`배열에 포인터 할당하기 / 포인터에 배열 할당하기`  

```cpp
char arr[] = "This is Array";
char* ptr = "This is pointer";

// arr = ptr; // Error
ptr = arr;

printf("%s %s", arr, ptr);
```  

배열은 초기화 할 때 나열하여 할당할 수 있다.  

`PutChar()`  

아스키 코드를 Argument로 받아 문자로 출력한다.  

### 11.3 문자열의 배열  

```cpp
const char arr[] = "This is Array";
const char* ptr = "This is Pointer";
printf("%s %p \n", arr, arr);
printf("%s %p \n", ptr, ptr);

const char* ptr_list[] = { arr, ptr };

for (int i = 0; i < sizeof(ptr_list) / sizeof(char*); ++i)
    printf("%s %p \n", ptr_list[i], ptr_list[i]);
```  

문자열 배열을 출력해보자. 우선 string(문자열)은 주소값으로 Argument를 넘기고 %s로 형식지정을 해주면 '\0'까지 값을 읽는다.  

문자열 배열을 출력하려면 문자열의 주소를 가지고 있다가 넘겨주면 된다.  

`여러 문자열을 가리키는 포인터 / 문자열 배열`  

```cpp
const char* data1[3] = { "First", "Second", "Third" };
// "First" is in text segment, only exist on exe file, unchangable

const char data2[3][6] = { "One", "Two", "Three" };
// text literal "One" is unchangable in exe file
// but program copying it on memory and save this on data2[0].

const char* temp1 = "First";
const char* temp2 = "One";

const char ar1[] = "First";
const char ar2[] = "One";

printf("    \t   \t  | ptr\t     arr \n");
printf("arr ptr\t %p | %p %p\n", data1[0], temp1, ar1);
printf("2d arr \t %p | %p %p\n", data2[0], temp2, ar2);
```  

`Output`  
>                   | ptr      arr  
> arr ptr  004A7B90 | 004A7B90 0135F754  
> //             ^^         ^^       ^^  
> //        Literal    Literal    independant add  
>  
> 2d arr   0135F77C | 004A7B30 0135F748  
> //       all of this is independant except temp2  

data1은 여러 문자열을 가리키는 포인터이다. 이 때 문자열은 Literal 형태로 저장되어 있으며 변경할 수 없다.  

data2는 2차원 배열로 Literal 문자열을 복사하여 해당 주소지에 복사하였다.  

### 11.4 문자열을 입력받는 다양한 방법들  

scanf()의 장점은 다양한 자료형을 동시에 입력받을 수 있다는 것이다.  

scanf() 함수의 사용법과 주의점을 알아보자.  

```cpp
char* name; // Error!
char name[128];

scanf("%s", name);
printf("%s \n", name);
```  

위 예제에서 scanf() 함수를 보면, name에 Ampersand(&)를 사용하지 않았다.  

배열의 이름은 주소로 쓸 수 있기 때문이다.  

`scanf() 함수로 특정 문자까지만 입력을 받자. \n 대신 다른 입력으로 입출력을 제어할 수 있다.`  

아래 코드는 ; 문자가 나타낼 때까지 입력을 받는다.  

```cpp
char inputString[128];

printf("Enter a multi line string( press ';' to end input)\n");
scanf("%[^;]s", inputString);

printf("Input String = %s", inputString);
```  

`scanf() format specifier의 올바른 사용 방법`  

scanf() 함수는 마지막에 '\0' null character 를 입력시킨다.  

문자 배열을 넘어서 값을 할당하면 에러가 발생한다.  

```cpp
char str1[5], str2[5];
scanf("%4s %4s", str1, str2);
printf("%s %s \n", str1, str2);
```  

`Output`  
> 0123456789ABC  
> 0123 4567  

scanf() 함수에서 %4s 한정자(format Specifier)를 사용하여 입력 개수를 제한할 수 있다.  

마지막 자리는 '\0'을 위해 비워둔다. 만약 %5s를 사용하면 Runtime-Error가 발생한다.  

```cpp
scanf_s("%4s %4s", str1, 5, str2, 5);
```  

scanf_s 를 사용하여 argument에 문자열 개수를 명시할 수 있다. %4s 한정자의 입력 개수가 저장공간보다 크면 마찬가지로 Runtime - Error 가 발생한다.  

`scnaf() 함수의 한계 - 내부에서 구현된 buffer 가 작동하고 있다.`  

```cpp
char input[20] = "";
int result = scanf("%10s", input); // >> 123 456 789 0AB
printf("{ %s } \n", input);
result = scanf("%10s", input);     // skip to read any console input
printf("{ %s } \n", input);
```  

`Output`  
> { 123 }  
> { 456 }  

scanf() 함수는 space, enter('\n')를 만나기 전까지 저장하고 나머지를 buffer에 남겨둔다.  

따라서 위 예제에서 첫 scanf() 함수는 console에서 입력을 받지만 두 번째 scanf() 함수는 buffer에 있는 값을 바로 가져온다.  

형식 지정자(%10s) 10 글자를 읽어오라고 표시했지만 의미는 없다.  

scanf() 함수의 반환값은 0과 1로 입력 여부를 따진다.  

`scanf() vs gets()`  

scanf() : 문자열 하나를 읽음  

gets() : Enter '\n' 입력 전까지 읽어들이고 마지막에 '\n'을 지우고 '\0'를 더해준다.  
- 입력 값이 저장공간보다 크면 Runtime - Error!  

puts() : 출력 후 '\n'을 더해준다.  

gets_s(char*, size_t) : 문자열 저장공간만큼만 입력받는다.  
- 입력 값이 저장공간보다 크면 Runtime - Error!  

입력 값의 개수와 저장할 수 있는 공간을 동시에 입력 할 필요가 없는 함수를 소개한다.  

`fgets() 와 fputs()`  

fgets() 함수는 character를 입력받아 char* 에 저장한다.  

character 개수가 num - 1 이거나 EOF, Newline Character를 만날 때까지 지속한다.  

fgets() 함수는 정해진 공간을 사용할 수 있도록 입력값을 받는다. fgets(char* _Buffer, int _MaxCount, FILE* _Stream)  

```cpp
char name[8];

fgets(name, 8, stdin);
fputs(name, stdout);

for(int i = 0 ; i < 8 ; ++i)
    printf("%i ", (int)name[i]);
```  

`Output`  
> ABCD  
> ABCD  
> 65 66 67 68 10 0 -52 -52  
> ^  ^  ^  ^  ^  ^  
> A  B  C  D  \n \0  

fgets() 함수는 파일을 불러올 때 사용하는 함수로 stdin 자리에 File의 포인터를 입력하여 사용한다.  

만약 standard input을 넣으면 Console의 입력을 받는다. Console 입력을 받을 때 '\n'을 삭제하지 않고 문자열에 저장한다.  

fputs() 함수는 파일을 출력할 때 사용하는 함수다. stdout 자리에 File 포인터를 입력하여 사용한다.  

문자열 끝에 '\n'을 넣지 않는다.  

`fgets()의 반환값과 EOF`  

fgets() 함수는 입력값이 저장된 주소를 반환한다. 만약 EOF를 만나면 NULL을 반환한다.  

```cpp
char name[8];
printf("%p %p \n", name, fgets(name, 8, stdin));
fputs(name, stdout);
```  

`Output`  
> // ABCD 입력            // ^Z입력  
> ABCD                   |  ^Z  
> 00F6FA38 00F6FA38      |  012FF894 00000000  
> ABCD                   |  Trash value  

위 성질을 이용하여 custom read 함수를 만들어보자.  

```cpp
char* custom_read_input(char* str, int n)
{
    int i = 0;
    char* check_null = fgets(str, n, stdin);
    
    if(check_null)
    {
        while(str[i] != '\n' && str[i] != '\0')
            i++;
        if(str[i] == '\n')
            str[i] = '\0';
        else
            while(getchar() != '\n') // clear buffer
                continue;
    }
    return check_null;
}
```  

check_null 포인터는 str과 동일한 값을 가지는데 굳이 만든 이유는 NULL을 확인하기 위해서이다.  

fgets() 입력을 받을 때 EOF(Ctrl + Z) 값을 받으면 check_null은 NULL이 된다.  

str값을 바로 사용하면 NULL을 확인할 수 없다.  

`while과 함계 fgets() 사용하기`  

```cpp
char my_buffer[4];
while( fgets(my_buffer, 4, stdin) != NULL && my_buffer[0] != '\n' )
    fputs(my_buffer, stdout)
```  

my_buffer는 4개의 char을 담을 수 있다.  

while과 함계 사용할 때는 입력 버퍼에 따로 저장되기 때문에 4개를 넘는 char를 한꺼번에 입력받고 출력한다.  

이 때 3의 배수만큼 입력하고 다음 줄로 넘어가면 종료된다. ABC(Enter)를 예로 들자.  

'A''B''C''\n'이 입력 버퍼에 들어간다. my_buffer 마지막에 '\0'을 넣어야 하므로 my_buffer = {'A', 'B', 'C', '\0'} 이 되고, '\n'은 입력 버퍼에 남아있다가 그 다음번 입력 때 my_buffer안으로 들어가 종료된다.  

### 11.5 문자열을 출력하는 다양한 방법들  

`Puts() 함수 사용하기`  

puts() 함수는 포인터를 Argument 로 사용한다. 값을 넣으면 에러가 생긴다.  

```cpp
/* Array */
char str[60] = "String Array is initialized";
put(str);
puts(str + 3);
puts(&str[3]);

// puts(str[3]); // Error!
```  

`Output`  
> String Array is initialized  
> ing Array is initialized  
> ing Array is initialized  

```cpp
/* Pointer */
const char* ptr = "A Pointer is initialized";
puts(ptr);
puts(ptr + 3);
//puts(ptr[3]); // Error!
```  

`Output`  
> A Pointer is initialized  
> ointer is initialized  

```cpp
/* Macro */
#define TEST "A String from %define."

puts(TEST);
puts(TEST + 2);
```  

`Output`  
> A string from %define.  
> string from %define.  

```cpp
/* '\0' 없는 문자열 */
char str[] = { 'H', 'I', '!' };
puts(str);
```  

`Output`  
> HI!╠╠╠╠╠+8m┼▄■Å  

'\0'을 찾을 때까지 출력한다.  

`fputs()`  

파일 입출력 시 사용하는 함수로 stdout을 입력시 해당 주소에 있는 값을 console로 출력한다.  

```cpp
/* putchar() 함수를 이용하여 puts() 함수 구현해보기 */
int custom_put(const char* str)
{
    int i = 0;
    while(*str != '\0') // == while(*str)
    {
        putchar(*str++);
        i++;
    }
    putchar('\n');
    return i;
}
```  

### 11.6 다양한 문자열 함수들  

- strlen() : '\0'을 제외한 문자의 개수를 반환한다.  
- strcat(char*, char*) : 첫 문자열 끝에 두 번째 문자열 붙여준다. 이 때 공간이 부족하면 Runtime - Error!  
- strncat(char*, char*, int) : 첫 문자열 끝에 두 번째 문자열을 int 개수만큼 붙인다.  

- strcmp(char*, char*) : 두 문자열을 비교한다. 다른 문자가 발견되면 ASCII 로 앞 문자열에서 뒷 문자열을 뺀다. 음수이면 -1 양수이면 +1을 반환한다.  
- strncmp(char*, char*, int) : 두 문자열을 지정한 int만큼 비교한다.  

```cpp
printf("%2d \n", strcmp("B", "A"));   // >>  1
printf("%2d \n", strcmp("A", "A"));   // >>  0
printf("%2d \n", strcmp("A", "B"));   // >> -1

printf("%2d \n", strcmp("as", "ai")); // >>  1
printf("%2d \n", strcmp("ai", "ai")); // >>  0
printf("%2d \n", strcmp("ai", "as")); // >> -1
```  

- strcpy(char* _Destination, char* _Source) : Source 문자열을 Destination 문자열에 복사한다. 초기화할 때만 배열에 나열된 값을 넣을 수 있다. str1 = str2는 불가능하다.  
- strncpy(char*, char*, int) : 뒷 문자열에서 int 개수만큼 복사하여 첫 문자열에 붙여놓는다. 이 때 int 개수가 작으면 '\0'이 포함되지 않는다.  

- sprintf(char* Buffer, const char* _Format) : 형식지정자(Format Specifier)를 활용하여 생성된 _Format 문자열을 _Buffer에 넣는다. 형식 지정자의 수만큼 Argument를 추가할 수 있다.  

```cpp
char str[20] = "";
for(int i = 0 ; i <= 10 ; ++i)
{
    sprintf(str, "%02d.jpeg", i);
    puts(str);
}
```  

`Output`  
> 01.jpeg  
> 02.jpeg  
> 03.jpeg  
> 04.jpeg  
> 05.jpeg  
> 06.jpeg  
> 07.jpeg  
> 08.jpeg  
> 09.jpeg  
> 10.jpeg  

`기타`  

- strchr(const char* _Str, int _ASCII) : 문자열에서 ASCII 코드와 일치하는 문자를 찾는다. 해당 위치와 포인터를 반환한다.  
- strpbrk(const char* _Str, const char* _Control) : _Str에서 문자를 찾는다. _Control 문자열에 속해있는 문자를 찾는 즉시 해당 위치의 포인터를 반환한다.  
- strrchr(const char* _Str, int _ASCII) : _Str 문자열을 역순으로 탐색한다. _ASCII 코드의 문자를 만나면 즉시 그 위치의 포인터를 반환한다.  
- strstr(const char* _Str, const char* _SubStr) : _Str 문자열에서 _SubStr 문자열을 찾는다. 찾았을 때 _Substr 문자열의 시작점 위치를 포인터로 반환한다.  

* 위 함수들이 찾기를 실패하면 NULL을 반환한다.  

`strcat() 함수 구현해보기`  

```cpp
#include <stdio.h>

char* string_concat(char* str, char* added)
{
    while(*str != '\0')
        str++;
        
    while(*added != '\0')
    {
        *str = *added;
        str++;
        added++;
    }
    *str = '\0';
    
    return str;
}

int main()
{
    char str1[30] = "Origin";
    char str2[30] = "New";
    string_concat(str1, str2);
    printf("%s\n", str1);
    
    return 0;
}
```  

`strcmp() 함수 구현해보기`  

```cpp
#include <stdio.h>

int string_compare(char* str1, char* str2)
{
    while(*str1 && *str2)
    {
        if(*str1 != *str2)
            break;
        str1++;
        str2++;
    }

    if(*str1 == '\0' && *str2 == '\0')
        return 0;

    int result = (int)*str1 - (int)*str2;
    return (result > 0) ? 1 : -1;
}

int main()
{
    int result;
    result = string_compare("Apple", "Banana");
    printf("%d", result);
    return 0;
}
```  

`strcpy() 함수 구현해보기`  
```cpp
#include <stdio.h>

void string_copy(char* str1, char* str2)
{
    while(*str2 != '\0')
    {
        *str1++ = *str2++;
    }
    *str1 = '\0';
}

int main()
{
    char str1[20] = "";
    char str2[20] = "Apple";
    string_copy(str1, str2);
    printf("%s\n", str1);
}
```

### 11.7 선택 정렬 문제 풀이  

```cpp
#include <stdio.h>

void swap(int *xp, int *yp);
void printArray(int arr[], int size);
void selectionSort(int arr[], int n);

int main()
{
    int arr[] = {64, 25, 12, 22, 11};
    int n = sizeof(arr) / sizeof(int);

    selectionSort(arr, n); // ascending order

    printArray(arr, n);

    return 0;
}

void printArray(int arr[], int size)
{
    int i;
    for(i = 0 ; i < size ; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

void swap(int* xp, int* yp)
{
    int temp = *xp;
    *xp = *yp;
    *yp = temp;
}

void selectionSort(int arr[], int n)
{
    int i, j, min_idx;

    for(i = 0 ; i < n - 1 ; i++)
    {
        min_idx = i;
        for(j = i + 1 ; j < n ; j++)
        {
            if(arr[j] < arr[min_idx])
                min_idx = j;
        }

        swap(&arr[min_idx], &arr[i]);
    }
}
```  

### 11.8 문자열의 포인터를 정렬하기  

```cpp
// { autofold
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
// }

void swap(char** a, char** b)
{
    char* temp = *a;
    *a = *b;
    *b = temp;
}

void printStrLiteral(char** arr, int n)
{
    for (int i = 0; i < n; ++i)
        printf("%s ", *(arr+i) );
    printf("\n");
}

void selectionSort(char** arr, int n)
{
    for (int j = 0; j < n; ++j)
    {
        int min_idx = j;
        for (int i = j+1; i < n; ++i)
        {
            if (strcmp(*(arr + i), *(arr+ min_idx)) < 0)
                min_idx = i;
        }
        swap(arr + j, arr + min_idx);
        //char* temp = *(arr + j);
        //*(arr + j) = *(arr+ min_idx);
        //*(arr+ min_idx) = temp;
    }
}

int main()
{
    char* arr[] = { "Cherry", "AppleBee", "Pineappple", "Apple", "Orange" };
    int n = sizeof(arr) / sizeof(arr[0]);

    printStrLiteral(arr, 5);
    selectionSort(arr, 5);
    printStrLiteral(arr, 5);
}
```  

dereference를 활용하기보다 []를 활용하면 이해하기 쉽다.  

```cpp
void selectionSort(char* arr[], int n)
{
    for(int j  = 0 ; j < n ; ++j)
    {
        int min_idx = j;
        for(int i = j + 1, i < n ; ++i)
        {
            if(strcmp(arr[i], arr[min_idx]) < 0
                min_idx = i;
        }
        swap(&arr[i], &arr[min_idx]);
    }
}
```

### 11.9 문자 함수를 문자열에 사용하기  

```cpp
// { autofold
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
// }

#define NUM_LIMITS 1024

void ToUpper(char *str)
{
    while(*str != '\0')
    {
        *str = toupper(*str);
        str++;
    }
}

int PunctCount(const char* str)
{
    int count = 0;
    while(*str != '\0')
    {
        if(ispunct(*str))
            count++;
        str++;
    }
    return count;
}

int main()
{
    char str[NUM_LIMITS];
    char* cnvrtNewline = NULL;
    fgets(str, NUM_LIMITS, stdin);
    cnvrtNewline = strchr(sr, '\n');
    if(*cnvrtNewline == '\n')
        *cnvrtNewline = '\0';
    printf("deleted newLine { %s } \n",str);

    ToUpper(str);
    printf("To Upper        { %s } \n", str);

    int count = PunctCount(str);
    printf("Punctuation = %i", count);
}
```

### 11.10 명령줄 인수  

`.exe 파일 생성하기`  

main() 함수의 Parameter에서 int 명령어 개수, 명렁어 배열을 입력받자.  

```cpp
int main(int argc, char* argv[]) // Count, ptr to str array
{
    int count;
    printf("The Command Line has %d Arguments : \n", argc);
    
    for(count = 0 ; count < argc ; count++)
        printf("Arg %d : %s \n", count, argv[count]);
    printf("\n");
    
    return 0;
}
```

### 11.11 문자열을 숫자로 바꾸는 방법들  

`atoi(), atof(), atol()`  

```cpp
int main(int argc, char* argv[]) // count, ptr to str Array
{
    /*
		    string to integer, double, long
		           atoi(), atof(), atol()
	  */
	  if (argc < 3)
		    printf("Wrong Usage of %s \n", argv[0]);
	  else
	  {
		    /* Example 1 */
		    //int times = atoi(argv[1]);
		    //for (int i = 0; i < times; ++i)
		    //puts(argv[2]);


		    /* Example 2 */
		    printf("Sum = %i \n", atoi(argv[1]) + atoi(argv[2]) );
	  }
	  return 0;
}
```  

`strol(), strtoul(), strtod()`  

strtod( )는 10진수만 변환할 수 있다.  

strtol( ) 와 stroul( )는 16진수 8진수 2진수 모두 변환가능하다.  

```cpp
// 10진수
/*
  string to long,  unsigned long, double
           strtol()  strtoul()    strtod()
*/
char extract_nums[] = "-1024Hello";
char* find_str;
long l = strtol(extract_nums, &find_str, 10);
printf("    original: %s   \n", extract_nums);
printf("extract_nums: %ld \n", l);
printf("    find_str: %s  \n", find_str);
```  

> Output  
> original: -1024Hello  
> extract_nums: -1024  
> find_str: Hello  

```cpp
// 16진수
char extract_nums2[] = "10FFHello";
char* find_str2;
long l2 = strtol(extract_nums2, &find_str2, 16);
printf("    original: %s  \n", extract_nums2);
printf("extract_nums: %x  \n", l2);
printf("    find_str: %s  \n", find_str2);
```  

> Output  
> original: 10FFHello  
> extract_nums: 10ff  
> find_str: Hello  

`숫자를 문자열로 변환하기`  

itoa() 함수와 ftoa() 함수는 표준 라이브러리에 포함되어 있지 않다. _itroa()로 대신 쓸 수 있다.  

```cpp
/*           
              Numbers to strings
    Use sprintf() instead of _itoa(), ftoa()
*/

char str[30];
_itoa(20, str, 16);     // decimal 16 to hex str
puts(str);

sprintf(str, "%x", 32); // decimal 32 to hex str
puts(str);
```  

> Output  
> 14  
> 20  

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊