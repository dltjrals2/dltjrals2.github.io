---
title: "구조체"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2023-01-04
last_modified_at: 2023-01-05
---

## 14. 구조체  

### 14.1 구조체가 필요한 이유  

1. 여러 자료형 배열을 Index base로 관리하기 어렵다.  
- 자료형이 서로 다르지만, 함께 사용하면 편리한 오브젝트들 끼리 모아놓으면 좋을 것 같다.  

```cpp
/* Arrays */
// 배열은 자료형이 같은 데이터 오브젝트들이 나열된 형태
char names[MAX_PATIENTS][MAX_NAME];
float heights[MAX_PATIENTS];
float weights[MAX_PATIENTS];

strcpy(names[0], "John Wick");
heights[0] = 180.0f;
weights[0] = 90.0f;

printf("%s %f %f\n", names[0], heights[0], weights[0]);
```  

![TBC](https://user-images.githubusercontent.com/37467408/210505681-76a4cf72-88f0-4642-848e-5b652c8f2d41.PNG)  

2. 구조체를 이용하여 Tag 하나로 관리할 수 있다.  
- 구조체 내부 member variable에 접근하기 위하여 Dot(.) Operator를 사용한다.  
- 메모리를 효율적으로 사용하기 위해 멤버 member variable 사이에 빈공간(padding)을 둔다.  

```cpp
/* Structures */
struct Patient
{
    char name[MAX_NAME];
    float height;
    float weight;
    int age;
};

struct Patient p1, p2, p3; // structure variables
// struct Patient pat[MAX_PARTIENTS];

strcpy(p1.name, "John Wick");
p1.height = 180;
p1.weight = 90;

strcpy(p2.name, "Dwayne Johnson");
p2.height = 180;
p2.weight = 120;
```  

![TBC](https://user-images.githubusercontent.com/37467408/210506410-cc4d1a10-a42c-4bfb-a203-48ba73c25a73.PNG)  

### 14.2 구조체의 기본적인 사용법  

```cpp
#define MAX 41

struct person // person is a tag of structure
{
    char name[MAX]; // member
    int age; // member
    float height; // member
};

int main()
{
    int flag; // Receive return value of scanf()
    
    /* Structure variable */
    struct person genie;
    
    /* dot(.) is structure membership operator (member access operator, member operator) */
    strcpy(genie.name, "Will Smith"); // strncpy(genie.name, "Kim genie", MAX);
    genie.age = 20;
    
    /* dot(.) has higher precedence than & */
    flag = scanf("%f", &genie.height);
    printf("height: %f\n", genie.height);
    
    /* Initialization */
    struct person princess = { // follow the declaration order
        "Normal Scott",
        18,
        160.0f
    };
    // struct person princess = { "Normal Scott", 18, 160.0f };
    
    /* Designated Initializers */
    struct person beauty = { // Insert variable on exact delcaration
        .age = 19,    // Don't have to follow declaration order
        .name = "Bell";
        .height = 150.0f;
    };
    // struct person beauty = { .age = 19, .name = "Bell", .height = 150.0f };
    
    /* Pointer to a struct variable */
    struct person* someone;
    someone = &genie;
    // someone = (struct person*)malloc(sizeof(struct person)); // free later
    
    /* Indirect member operator (or structure pointer operator)
        arrow(->) operator usage : "pointer identifier" -> "member vairalbe" */
    someone->age = 99;
    (*someone).age = 155.f;
}
```  

`함수 안에서 구조체 선언하기`  

```cpp
#include <stdio.h>

#define MAX 30

void printBookInfo()
{
    /* Structure declarations in a function */
    struct book
    {
        char title[MAX];
        float price;
    };
    
    struct book book_one = { "The history of korea", 1.5f };
    printf("%s %f \n", book_one.title, book_one.price);
}

int main()
{
    printBookInfo();
}
```  

`임시 구조체 선언 후 구조체 변수 선언하기`  

```cpp
/* No tag, temporary struct use only once, Initializing such variable */
struct
{
    char farm[MAX];
    float price;
} apple, grape; // initialized!

apple.price = 1.2f;
strcpy(apple.farm, "DaeGu");

grape.price = 1.0f;
strcpy(grape.farm, "DaeGu");
```  

`typedef 사용하기`  
- 간결하게 구조체 변수를 선언할 수 있다.  

```cpp
/* typedef makes 'structure keyword' be short */
typedef struct person persontype; // replace (struct person) to (persontype)
persontype pt;

typedef struct person person; // much cleaner
person ps;

typedef struct {
    char name[NAX];
    int age;
} friend;

friend f1 = { "Jay", 15 };
friend f2 = { "Joe", 15 };

printf("f1 %s %i \n", f1.name, f2.age);
printf("f1 %s %i \n", f2.name, f2.age);
```

### 14.3 구조체의 메모리 할당  

`정렬이 잘 되어 패딩이 없는 구조체`  

```cpp
/* Well alligned structure */
struct Aligned
{                 // If declare struct's variable
    int i;        // | 0 1 2 3 | 4 5 6 7 | 8 9 10 11 12 13 14 15 |
    float f;      // |  int i  | float f |      double d         |
    double d;     // 4 + 4 + 8 = 16 byte
}

struct Aligned aligned;
printf("Struct Aligned a1\n"); // Struct Aligned a1
printf("Sizeof %zd\nn", sizeof(struct Aligned)); // Sizeof 16
printf("%p \n", &aligned); // 00F3F760
printf("%p \n", &aligned.i); // 00F3F760
printf("%p \n", &aligned.f); // 00F3F764
printf("%p \n", &aligned.d); // 00F3F768
```

`메모리 패딩이 필요한 구조체`  
1. 주소를 표현하는 최소단위 8bit = 1byte  
2. CPU - RAM이 정보를 주고 받을 때 최소 단위는 4byte(32bit system)이다. 이 때 최소 단위를 word라 부른다.  

```cpp
/* padding (struct member alignment) - 1 word : 4 bytes in x86, 8bytes in x64 */
struct Padded1
{             // without padding
    char c;   // | 0 | 1 2 3 4 | 5 6 7 8 9 10 11 12 |
    float f;  // | c |  float  |      double        |
    double d; // 1 + 4 + 8 = 13
};

struct Padded1 paded1;
printf("\nStruct Padded1 \n");
printf("Sizeof %zd\n", sizeof(paded1)); // Sizeof 16
printf("%p \n", &paded1); // 00D3F7C4
printf("%p \n", &paded1.c); // 00D3F7C4
printf("%p \n", &paded1.f); // 00D3F7C8
printf("%p \n", &paded1.d); // 00D3F7CC

/* Added padding
   | 0 1 2 3 | 4 5 6 7 | 8 9 10 11 12 13 14 15 |
   | char(?) |  float  |        double         |
       ^ add padding 4 + 4 + 8 = 16
*/
```  

만약 패딩 없이 word를 보낸다고 가정하자. 첫 word는 char형과 float형을 섞어서 보낸다.  

두 번째 word는 짤린 float형과 double형의 앞부분을 잘라서 보낸다.  

세 번째 word는 double의 뒷부분이 비어있거나 쓰레기값을 보낼 것이다.  

비어있는 뒷 부분을 앞쪽으로 당겨 사용하여도 괜찮지 않을까? 이것이 패딩이 생긴 이유이다.  

위에서 구조체 member variable 순서를 바꾸면 어떻게 되는지 살펴보자.  

`정렬이 잘못 되어 메모리를 낭비하고 있는 구조체`  

위 Padded1과 동일한 변수를 담고 있지만 순서가 다르다. Paddded1은 16byte, Padded2는 24byte로 8byte를 낭비한다.  

```cpp
struct Padded2
{               // without padding
    float f;    // | 0 1 2 3 | 4 5 6 7 8 9 10 11 | 12 |
    double d;   // |  float  |      double       |char|
    char c;     // 4 + 8 + 1 = 13
};

struct Padded2 paded2;
printf("\nStruct padded2 \n");
printf("Sizeof %zd\n", sizeof(paded2)); // Sizeof 24
printf("%p \n", &paded2);   // 00B5FAF8
printf("%p \n", &paded2.f); // 00B5FAF8
printf("%p \n", &paded2.d); // 00B5FB00
printf("%p \n", &paded2.c); // 00B5FB08

/* Bad Aligned
   | 0 1 2 3 4 5 6 7 | 8 9 10 11 12 13 14 15 | 16 17 18 19 20 21 22 23 |
   |    float f      |      double d         |        char c           |
*/
```  

`문자열도 똑같이 적용할 수 있다`  

문자열 개수를 16에서 17로 바꾸어보자. 구조체의 크기가 8byte 증가한다.  

```cpp
struct Person
{
    char name[16];
    double d;
    float f;
};

struct Person person;
printf("\nPerson \n");
printf("Sizeof %zd\n", sizeof(person)); // size 40 -> size 32
printf("%p \n", &person); // name[17] -> name[16]
printf("%p \n", &person.name); // 00E5FD10 -> 00F3FC30
printf("%p \n", &person.d); // 00E5FD28 -> 00F3FC40
printf("%p \n", &person.f); // 00E5FD30 -> 00F3FC48
```

`구조체 배열`  

```cpp
struct Person Group[3];
printf("Size of a structure array : %zd \n", sizeof(Group));

>> Size of a structure array : 96
```  

`구조체 패딩 기준 설정하기`  

![TBC](https://user-images.githubusercontent.com/37467408/210682601-a039226d-ea4d-4aba-aa0d-4b116838938c.PNG)  

1byte로 바꾸면 패딩이 없어진다.  

`x86과 x64에서 패딩의 크기가 같은 이유`  

메모리 정렬(Memory Align)  
1. 구조체 메모리 정렬을 사용하는 메모리 패딩의 크기는 멤버 변수의 자료형이 결정한다. > 워드 사이즈가 아니다. 따라서 운영체제가 아닌 컴파일러가 결정한다.  
2. 변수의 주소가 자료형의 배수들 일 때 CPU가 접근하기 좋다  
- char는 1의 배수, int와 float는 4의 배수, double은 8의 배수 메모리 주소를 가진다.  
- char과 char[]은 어떤 메모리 주소도 사용할 수 있다.  
- 해당 배수가 아니라면 패딩 이후 해당 배수의 메모리 주소에서부터 object를 사용할 것이다.  
3. padding rule은 구조체 멤버 변수 중 가장 큰 자료형을 따른다.  

구조체의 주소  
1. 64bit system에서 구조체 주소는 16byte 배수를 사용한다. (제일 큰 자료형 - long double이 16byte)  
2. 64bit system이라도 구조체가 char 자료형만 담고 있다면 어떤 주소라도 사용 가능하다.  
3. 구조체 사이 padding으로 사용된 저장공간을 활용할 수 있다. 컴파일러가 해준다.  

### 14.4 구조체의 배열 연습문제  

```cpp
#define MAX_TITLE 16
#define MAX_AUTHOR 16
#define MAX_BOOKS 3

struct book
{
    char title[MAX_TITLE];
    char author[MAX_TITLE];
    int price;
};

char* s_gets(char* str, int n)
{
    char* result;
    char* find;
    
    result = fgets(str, n, stdin); // scanf() save data at every space or newline
    if(result != NULL)
    {
        find = strchr(str, '\n'); // search newline
        if(find) // if find
            *find = '\0'; // replace character to null
        else
        {
            while(1) // there is no newline in console input
            {
                char ch = getchar();
                if(ch != '\n')
                {
                    printf("dispose! %c \n", ch); // discharge rest of buffer
                    continue;
                }
                else
                    break; // catch newline
            }
        }
    }
    return result;
}

int main()
{
    struct book books[MAX_BOOKS] = { {"\0", "\0", 0}, };
    int count = 0;
    
    while(1)
    {
        printf("Input a book title or press [Enter] to stop.\n>>");
        if(s_gets(books[count].title, MAX_TITLE) == NULL) // Helped by lecture to use if
            break;
        if(books[count].title[0] == '\0') // s_gets convert \n to \0
            break;
            
        printf("Input the author.\n>>");
        s_gets(books[count].author, MAX_TITLE);
        
        printf("Input the price.\n>>");
        int flag = scanf("%i", &(books[count].price)); // Helped by lecture to use if
        while (getchar() != '\n') // scanf() needs clearing buffer
            continue;
            
        count++;
        
        if(count == MAX_BOOKS)
        {
            printf("Books are enough...\n>>");
            break;
        }
    }
    
    if(0 < count)
    {
        printf("\n Book list\n");
        for(int i = 0 ; i < count ; ++i)
        {
            printf("title : %s \n\t\t written by : %s \n\t\t price : %i \n", books[i].title, books[i].author, books[i].price);
        }
    }
    else
        printf("No books to show\n");
}
```

### 14.5 구조체를 다른 구조체의 멤버로 사용하기  

```cpp
#define LEN 20

struct names
{
    char given[LEN]; // First Name
    char family[LEN]; // Last Name
};

struct reservation
{
    struct names guest; // a nested structure
    struct names host;
    char food[LEN];
    char place[LEN];
    
    // time
    int year;
    int month;
    int day;
    int hour;
    int minutes;
};

int main()
{
    // Use designated initialization
    struct reservation res = {
        .guest = {"Nick", "Carraway"},
        .host = {"Jay", "Gatsby"},
        .place = {"The Gatsby mansion"},
        .food = {"Pizza"},
        .year = 1925,
        .month = 4,
        .day = 10,
        .hour = 18,
        .minutes = 30
    };
    // Complete letter with above structure
    
    /*
        Dear Nick Carraway,
        I would like to serve you Pizza.
        Please visit The Gatsby mansion on 10/4/1925 at 18:30.
        Sincerely,
        Joy Gatsby
    */
    const char* formatted = "Dear %s %s, \n\
    I would like to serve you %s. \n\
    Please visit %s on %i/%i/%i at %i:%i. \n\
    Sincerely, \n\
    %s %s\n";
    
    printf(formatted, res.guest.given, res.guest.family, res.food, res.place, res.day, res.month, res.year, res.hour, res.minutes, res.host.given, res.host.family);
}
```

### 14.6 구조체와 포인터  

`구조체 포인터 사용하기`  

```cpp
#define LEN 20

struct names {
    char given[LEN];
    char family[LEN];
};

struct friend {
    struct names full_name;
    char mobile[LEN];
};

int main()
{
    struct friend friends[2] = {
        { {"Ariana", "Grande"}, "1234-1234" },
        { {"Taylor", "Swift"}, "6556-6556" }
    };
    
    struct friend* best_friend;
    best_friend = &friends[0];
    
    printf("%zd \n", sizeof(struct friend));
    printf("%p %s \n", best_friend, best_friend->full_name.given);
    
    best_friend++;
    printf("%p %s \n", best_friend, (*best_friend).full_name.given);
}
```  

`대입 연산자 활용하기`  

- Q. 멤버 변수가 배열일 때 대입연산자는 어떻게 작동할까? 배열의 주소를 그대로 넘겨줄까?  
- A. 멤버 변수가 배열일 때 대입연산자는 새로운 저장공간을 만든다.  

```cpp
struct data
{
    int i;
    char c;
    float arr[2];
};

int main()
{
    struct data d1 = { 111, 'A', }; // OR { 111, 'A', {1.1f, 2.2f} };
    d1.arr[0] = 1.1f;
    d1.arr[1] = 2.2f;
    
    printf("%d %c %p \n", d1.i, d1.c, d1.arr); // >> 111 A 010FFE50
    printf("%f %f \n", d1.arr[0], d1.arr[1]);
    printf("%p %p \n\n", &d1.arr[0], &d1.arr[1]); // >> 010FFE50 010FFE54
    
    struct data d2 = d1; // assignment operator
    printf("%d %c %p \n", d2.i, d2.c, d2.arr); // >> 111 A 010FFE38  
    printf("%f %f \n", d2.arr[0], d2.arr[1]);
    printf("%p %p \n\n", &d2.arr[0], &d2.arr[1]); // >> 010FFE38 010FFE3C
}
```  

구조체 멤버가 포인터일 때 대입연산자는 기존의 저장공간을 그대로 사용한다.  

```cpp
struct data
{
    int i;
    char c;
    float* arr;
};

int main()
{
    struct data d1 = { 111, 'A', NULL };
    d1.arr = (float*)malloc(sizeof(float) * 2);
    d1.arr[0] = 1.1f;
    d1.arr[1] = 2.2f;
    
    printf("%d %c %p \n", d1.i, d1.c, d1.arr);    // >> 111 A 00E74E60
	  printf("%f %f \n", d1.arr[0], d1.arr[1]);
	  printf("%p %p \n\n", &d1.arr[0], &d1.arr[1]); // >> 00E74E60 00E74E64

  	struct data d2 = d1; //  assignment operator  
  	printf("%d %c %p \n", d2.i, d2.c, d2.arr);    // >> 111 A 00E74E60
  	printf("%f %f \n", d2.arr[0], d2.arr[1]);
	  printf("%p %p \n\n", &d2.arr[0], &d2.arr[1]); // >> 00E74E60 00E74E64
}
```

### 14.7 구조체를 함수로 전달하는 방법  

### 14.8 구조체와 함수 연습문제  

### 14.9 구조체와 할당 메모리  

### 14.10 복합 리터럴  

### 14.11 신축성있는 배열 멤버  

### 14.12 익명 구조체  

### 14.13 구조체의 배열을 사용하는 함수  

### 14.14 구조체 파일 입출력 연습문제  

### 14.15 공용체의 원리  

### 14.16 공용체와 구조체를 함께 사용하기  

### 14.17 익명 공용체  

### 14.18 열거형  

### 14.19 열거형 연습문제  

### 14.20 이름공간 공유하기  

### 14.21 함수 포인터의 원리  

### 14.22 함수 포인터의 사용 방법  

### 14.23 자료형에게 별명을 붙여주는 typedef  

### 14.24 복잡한 선언을 해석하는 요령  

### 14.25 qsort() 함수 포인터 연습문제  

### 14.26 함수 포인터의 배열 연습문제  

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊