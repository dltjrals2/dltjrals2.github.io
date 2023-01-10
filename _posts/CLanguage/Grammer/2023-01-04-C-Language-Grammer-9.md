---
title: "구조체 - 1"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2023-01-04
last_modified_at: 2023-01-10
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

```cpp
#define LEN 50

struct fortune
{
    char back_name[LEN];
    double bank_saving;
    char fund_name[LEN];
    double fund_invest;
};

double sum(const struct fortune *my_fortune); // <- double sum(double*, double*)

int main()
{
    struct fortune my_fortune =
    {
        .bank_name = "Wells-Fargo",
        .bank_saving = 4032.273,
        .fund_name = "JPMorgan Chase",
        .fund_invest = 8543.946
    };
    
    printf("Total : $%.2f\n", sum(&my_fortune));
    // printf("Total : $%.2f\n", sum(&my_fourtune.bank_saving, &my_fortune.fund_invest));
    return 0;
}

double sum(const struct fortune *my_fortune)
{
    // return (*my_fortune).bank_saving + (*my_fortune).fund_invest;
    return my_fortune->bank_saving + my_fortune->fund_invest;
}
```  

함수 파라미터가 call by value일 때 문제점  
1. 메모리 낭비(같은 변수 값이 복사된다. 크기가 거대하면 연산이 느리다.)  
2. 동적할당된 변수는 복사되지 않고 주소값을 그대로 가져옴  

따라서 pointer를 사용해서 주소 값을 전달하자.  

### 14.8 구조체와 함수 연습문제  

> 객체지향 프로젝트의 Object = 구조체(다른 타입의 변수들) + 구조체 멤버 변수를 사용하는 함수  

`포인터를 활용하여 이름 입출력하기`  

```cpp
#define LEN 30

typedef struct
{
    char first[LEN];
    char last[LEN];
    int num;
}Name_count;

void receive_input(Name_count*);
void count_characters(Name_count*);
void show_result(const Name_count*);
char* s_gets(char* st, int n);

int main()
{
    Name_count user_name;
    receive_input(&user_name);
    count_characters(&user_name);
    
    show result(&user_name); // no needs to type const
    
    return 0;
}

/*
void receive_input(Name_count* user)
{
    printf("Insert your first name!\n>>");
    s_gets(user->first, LEN);
    
    printf("Insert your last name!\n>>");
    s_gets(user->last, LEN);
}
*/

void receive_input(Name_count* user)
{
    int flag;
    printf("Input your first name \n>>");
    flag = scanf("%s", user->first); // %[^\n] means read str
    
    // flag = scanf("%[^\n]%*c", user->first); // %[^\n] means read str
    if(flag != 1)                              // until reached to \n
        printf("Wrong input");                 // %*c means delete thie on buffer
        
    printf("Input your last name \n>>");
    flag = scanf("%s", user->last);
    // flag = scanf("%[^\n]%*c", user_last);
    if(flag != 1)
        printf("Wrong input");
}

/*
void count_characters(Name_count* user)
{
    int count = 0;
    char* ptr = user->first;
    while(*ptr != '\0')
    {
        count++;
        ptr++;
    }
    ptr = user->last;
    while(*ptr != '\0')
    {
        count++;
        ptr++;
    }
    user->num = count;
}
*/

void char_characters(Name_count* user)
{
    user->num = strlen(user->first) + strlen(user->last);
}

void show_result(const Name_count* user)
{
    printf("Your name is %s %s has %i characters\n", user->last, user->first, user->num);
}

/*
char* s_gets(char* st, int n)
{
    char* start = st;
    char ch;
    while((ch = fgetc(stdin) != 0 && ch != '\n'))
    {
        *st = ch;
        st++;
    }
    *st = '\0';
    return start;
}
*/

char* s_gets(char* str, int n)
{
    char* start = str;
    char* ptr = NULL;
    
    fgets(str, LEN, stdin);
    if(start != NULL)
    {
        ptr = strchr(start, '\n');
        if(ptr)
            *ptr = '\0';
        else
        {
            while(getc(stdin) != '\n')
                continue;
        }
    }
}
```  

`대입연산자를 활용하여 이름 입출력하기`  
strcpy를 사용하지 않고 대입 연산자로 해당 값을 바꾸어줄수 있다. 새로운 구조체 변수를 만들어 대입해주면 된다.  

```cpp
#define LEN 30

typedef struct
{
    char first[LEN];
    char last[LEN];
    int num;
}Name_count;

Name_count receive_input();
Name_count count_characters(Name_count);
void show_result(const Name_count);
char* s_gets(char* st, int n);

int main()
{
    Name_count user_name;
    
    user_name = receive_input;
    
    user_name = count_characters(user_name);
    
    show_result(user_name); // no needs to type const
    
    return 0;
}

Name_count receive_input()
{
    char* ptr = NULL;
    Name_count user;
    
    printf("Input your last name\n>>");
    s_gets(user.last, LEN);
    
    printf("Input your first name\n>>");
    s_gets(user.first, LEN);
    
    return user;
}

Name_count count_characters(Name_count user)
{
    user.num = strlen(user.first) + strlen(user.last);
    printf("\n num : %i \n", user.num);
    return user;
}

void show_result(const Name_count user)
{
    printf("your name is %s %s has %i characters\n", user.last, user.first, user.num);
}

char* s_gets(char* st, int n)
{
    char* start = st;
    char ch;
    while((ch = fgetc(stdin)) != 0 && ch = '\n');
    {
        *st = ch;
        st++;
    }
    *st = '\0';
    return start;
}
```  

`scanf() 함수에서 사용하는 format specifier`  
%[^\n]%*c 가 나올때까지 문자를 읽는다. 이 때 \n은 buffer에 남아있다. 이를 지워주기 위하여 %*c를 사용하자.  
다음 코드는 &*c와 있을 때와 없을 때를 비교하는 코드이다.  

```cpp
char test[10];
// int flag = scanf("%[^\n]%*c", test); // use %c for deleting buffer
int flag = scanf("%[^\n]", test);

for(int i = 0 ; i < 10 ; ++i)
{
    printf("%3i #c \n", (int)test[i], test[i]);
}

char ch;

while((ch = getc(stdin)) != NULL)
{
    printf("omitting %c\n", ch);
    continue;
}
```  

### 14.9 구조체와 할당 메모리  

`동적 할당의 잘못된 예`  
frame과 lname이 Text Segment에 저장되어있다. scanf 함수로 해당 주소에 있는 값을 바꾸려고 하면 에러가 발생한다.  

```cpp
struct namect {
    char* fname; // use malloc()
    char* lname; // use malloc()
    int letters;
};

struct namect p = { "John", "King" };
printf("%s %s\n", p.fname, p.lname);

int flag = scanf("%[^\n]%*c", p.lname);
printf("%s %s\n", p.fname, p.lname);
```  

`동적 할당의 적절한 예`  
buffer를 만들어 활용하면 동적 할당을 사용할 수 있다.  

```cpp
char buffer[LEN] = { 0, };
int flag = scanf("%[^\n]%*c", buffer);
p.fname = (char*)malloc(sizeof(buffer) + 1);

int(p.fname != NULL)
    strcpy(p.fname, buffer);
printf("%s %s\n", p.fname, p.lname);
```  

`동적할당으로 이름 입출력하기`  
1. bufferㅇ에 입력값을 저장한 후, 메모리를 할당한다.
2. 해당 메모리에 strcpy() 함수를 사용하여 값을 복사하여 넣는다.  

```cpp
#define LEN 30

struct namect {
    char* fname; // use malloc()
    char* lname; // use malloc()
    int letters;
};

void getinfo(struct namect*);         // allocate memory
void makeinfo(struct namect*);
void showinfo(const struct namect*);
void cleanup(struct namect*);         // free memory when done

int main()
{
    struct namect temp = { NULL, NULL, 0 };
    struct namect* user = &temp;
    getinfo(user);
    makeinfo(user);
    showinfo(user);
    cleanup(user);
}

void getinfo(struct namect* user)
{
    printf("Input your first name\n>>");
    
    char buffer[LEN] = { 0, };
    int flag = scanf("%[^\n]%*c", buffer);
    if(flag != 1) { // flag is counting how many input is there.
        printf("Bad input!\n");
        exit(1);
    }
    
    user->fname = (char*)malloc(strlen(buffer) + 1); // add 1 for '\0'
    if(user->fname == NULL) // sizeof char = 0
    {
        printf("Impossible to allocate memory!");
        exit(1);
    }
    strcpy(user->fname, buffer);
    
    printf("Input your last name\n>>");
    flag = scanf("%[^\n]%*c", buffer);
    user->lname = (char*)malloc(strlen(buffer) + 1);
    if(!user->lname)
    {
        printf("Impossible to allocate memory!");
        exit(1);
    }
    strcpy(user->lname, buffer);
}

void makeinfo(struct namect* user)
{
    user->letters = strlen(user->fname) + strlen(user->lname);
}

void showinfo(struct namect* user)
{
    printf("Your name is %s %s has %i characters\n", user->fname, user->lname, user->letters);
}

void cleanup(struct namect* user)
{
    free(user->fname);
    free(user->lname);
    printf("cleaned up memory!\n");
}
```

### 14.10 복합 리터럴  

한 번 초기화 한 구조체는 변수를 한꺼번에 바꿀 수 없다. 초기화할 때 처럼 assignment operator = { .. }가 불가능하다.  

이 때 여러 리터럴 자료형을 합친 복합 리터럴을 사용하여 임시 구조체를 만든 후, 초기화된 구조체에 값을 할당할 수 있다.  

```cpp
struct rectangle
{
    int width;
    int height;
};

int rect_area_callbyval(struct rectangle r)
{
    return r.width * r.height;
}

int rect_area_callbyref(struct rectangle* r)
{
    return r->width * r->height;
}

int main()
{
    /* Bad usage */
    struct rectangle rectangel1 = { 3, 3 };
    // rectange1 = { 5, 5 }; // Error! Only possible in initialization
    
    /* Assign value on each member variable */
    rectangle1.width = 5;
    rectangle1.height = 5;
    
    /* Use Assignment Operator */
    struct rectangle temp_r = { 10, 10 };
    rectangle1 = temp_r;
    rectangle1 = (struct rectangle){ 15, 15 };
    
    /* Use temporary struct vairable on argument */
    rect_area_callbyval((struct rectangle) { 20, 20 });
    rect_area_callbyref(&(struct rectangle) { .height = 25, .width = 25 });
}
```  

### 14.11 신축성있는 배열 멤버  

구조체 내에 정의되는 배열을 동적으로 사용할 수 있다. 포인터가 아니다 배열의 공간을 동적으로 할당할 수 있다.  

이 때 배열인 멤버변수는 반드시 마지막 순서에 선언되어라. 구조체 크기를 할당받을 때 배열의 크기를 고려하여 더해주면 된다.  

`동적할당 포인터 vs 배열`  

1. flexible array는 배열이기 때문에 포인터와 다르다. 자기 자신이 별도의 저장공간을 가지는 포인터와 달리 배열의 식별자가 가리키는 주소는 배열 첫 값이다. 때문에 메모리를 아낄 수 있다. `포인터 메모리(4byte)를 아낄 수 있다.` 아래 코드에서 struct 내부에서 flex values의 sizeof를 확인해보면 0byte임을 확인할 수 있다.  
2. 동적할당된 메모리는 힙에서 어느 위치에 저장되었는지 알 수 없다. 배열은 이전 멤버변수에 이어진 주소값을 가진다. (구조체와 일 열로 나열된 메모리 주소를 가진다.)  

```cpp
struct flex
{
    size_t count;
    double average;
    double values[]; // flexible array should be laast member variable
};

/*
struct flex
{
    size_t count;
    double average;
    double* values; // Dynamic allocation, pointer has own's data
};
*/

const size_t n = 3;
struct flex* s_ptr = (struct flex*)malloc(sizeof(struct flex) + n * sizeof(double));
if(s_ptr == NULL)
    exit(1);

s_ptr->count = n;
for(size_t i = 0 ; i < s_ptr->count; ++i)
    s_ptr->values[i] = 1.1 * i;
    
s_ptr->average = 0.0;
for(unsigned i = 0 ; i < s_ptr->count ; ++i)
    s_ptr->average += s_ptr->values[i];
s_ptr->average /= (double)s_ptr->count;

printf("Flexible Array member \n");
printf("Sizeof struct flex  %zd\n", sizeof(struct flex));
printf("Sizeof *s_ptr       %zd\n", sizeof(*s_ptr));
printf("Sizeof malloc       %zd\n", sizeof(struct flex) + n * sizeof(double));

printf("struct ptr      add %p\n", s_ptr);
printf("     count      add %p\n", &s_ptr->count);
printf("     count     size %zd\n",sizeof(s_ptr->count));
printf("s_ptr->average  add %p\n", &s_ptr->average);
printf("s_ptr->values   add %p\n", &s_ptr->values);
printf("s_ptr->values   val %p\n", s_ptr->values);
printf("s_ptr->values  size %zd\n", sizeof(s_ptr->values));
```  

> Output  
Flexible Array member  
Sizeof struct flex  16  
Sizeof *s_ptr       16  
Sizeof malloc       40  
struct ptr      add 00E64D40  
     count      add 00E64D40  
     count     size 4  
s_ptr->average  add 00E64D48  
s_ptr->values   add 00E64D50  
s_ptr->values   val 00E64D50  
s_ptr->values  size 0  

1. size_t count와 double average의 주소값 차이는 8이다. size_t의 크기는 4이다. padding이 되었음을 확인할 수 있다.  
2. struct와 *s_ptr의 sizeof는 모두 16이다. 배열까지 합치면 40byte이어야 한다. 구조체 크기에 배열 값이 포함되지 않았음을 확인할 수 있다. 동적할당을 받았기 때문에 컴파일러는 배열의 크기를 알 수 없다. 이 때문에 포인터를 dereference 후 대입연산자를 사용하여 16byte만 복사하여 할당한다. memcpy() 함수를 사용하면 모두 복사할 수 있다.  

`다음 코드를 실행하면 new_ptr에 있는 배열에 쓰레기값이 들어있음을 확인할 수 있다.`  

```cpp
struct flex* new_ptr = (struct flex*)malloc(sizeof(struct flex) + n * sizeof(double));
if(new_ptr == NULL)
    exit(1);

*new_ptr = *s_ptr;

for(size_t i = 0 ; i < s_ptr->count ; ++i)
    printf("%f ", s_ptr->values[i]);
printf("\n");

for(size_t i = 0 ; i < new_ptr->count ; ++i)
    printf("%f ", new_ptr->values[i]);
```  

> Output  
0.000000 1.100000 2.200000  
-6277438562204192487878988888393020692503707483087375482269988814848.000000 -6277438562204192487878988888393020692503707483087375482269988814848.000000 -6277438562204192487878988888393020692503707483087375482269988814848.000000  

따라서 대입연산자를 활용하기 보다는 memcpy()을 사용해주자.  

### 14.12 익명 구조체  

`Nested structure의 사용예제`  

```cpp
struct names // tag is names
{
    char first[20];
    char last[20];
};

struct person // tag is person
{
    int id;
    struct names name; // nested structure member;
}

int main()
{
    struct person CEO = {
        .id = 100,
        .name = { "Bill", "Gates" }
    };
    
    printf("%i\n", CEO.id);
    puts(CEO.name.first);
    puts(CEO.name.last);
}
```  

`Nested structure를 사용하는 대신 Anonymous(Tempolary) Structure를 사용할 수 있다.`  

`장점`  
1. Nested Struct를 위해 struct를 따로 정의하지 않는다.  
2. 멤버 변수를 호출하기 위해 Tag를 한 번만 사용하면 된다.  

```cpp
struct person_simple // tag is person_simple
{
    int id;
    struct { // anonynous structure
        char first[20];
        char last[20];
    };
}

int main()
{
    struct person_simple CEO = {
        .id = 111,
        {"Steve", "Jobs"}
    };
    
    printf("%i\n", CEO.id);
    puts(CEO.first);
    puts(CEO.last);
}
```

### 14.13 구조체의 배열을 사용하는 함수  

`구조체 배열`  

```cpp
#define LEN 30

struct book
{
    char name[LEN];
    char author[LEN];
};

void print_books(const struct book books[], int n);

int main()
{
    struct book my_books[3]; // = { { "The Great Gatsby", "F. Scott Fitzagrald" }, { ... }, ... };
    my_books[0] = (struct book){ "The Great Gatsby", "F. Scott Fitzagrald" };
    my_books[1] = (struct book){ "Hemlet", "William Shakespear" };
    my_books[2] = (struct book){ "The Odyssey", "Homer" };
    
    print_books(my_books, 3);
    
    return 0;
}

void print_books(const struct book books[], int n)
{
    for(int i = 0 ; i < n ; ++i)
        printf("%s written by %s. \n", books[i].name, books[i].author);
}
```  

`동적할당을 활용한 구조체 포인터`  

```cpp
#define LEN 30

struct book
{
    char name[LEN];
    char author[LEN];
};

void print_books(const struct book *books, int n);

int main()
{
    struct book* my_books = (struct book*)malloc(sizeof(struct book) * 3);
    if(my_books == NULL)
        exit(1);
        
    my_books[0] = (struct book){ "The Great Gatsby", "F. Scott Fitzagrald" };
    my_books[1] = (strcut book){ "Hamlet", "William Shakespear" };
    my_books[2] = (struct book){ "The Odyssey", "Homer" };
    print_books(my_books, 3);
    
    return 0;
}

void print_books(const struct book* books, int n)
{
    for(int i = 0 ; i < n ; ++i)
        printf("%s written by %s.\n", (books + i)->name, (*(books + i)).author); // also books[i]
}
```  

### 14.14 구조체 파일 입출력 연습문제  

1. text 파일에 책 이름과 저자를 추가해보자. text 파일 끝에 반드시 next line character를 추가해주어야 한다.  
2. text 파일 입출력을 binary 형식(wb, rb)로 처리하면 빠르다.  
3. binary 파일을 읽을 수 없기 때문에 디버깅이 어렵다.  
4. malloc은 컴파일 타임에 크기 변수로 오버플로우 될지 확인할 수 없다. calloc을 사용해주자. 오버플로우 될지 미리 계산한다.  

```cpp
#define LEN 30

struct book
{
    char name[LEN];
    char author[LEN];
};

void print_books(const struct book *books, int n)

void write_books(const char* filename, const struct book *books, int n);
void read_books_ref(const char* filename, struct book** books_qtr, int *n);
struct book* read_books_val(const char* filename, int* n);

void write_books_binary(const char* filename, const struct book *books, int n);
struct book* read_book_binary(const char* filename, int* n);

int main()
{
    int temp;
    int n = 3;
    const char* file_n = "books.bat";
    struct book* my_books = (struct book*)malloc(sizeof(struct book) * n);
    
    if(my_books == NULL)
    {
        printf("Malloc failed");
        exit(1);
    }
    
    my_books[0] = (struct book){ "The Great Gatsby", "F. Scott Fitzagrald" };
    my_books[1] = (strcut book){ "Hamlet", "William Shakespear" };
    my_books[2] = (struct book){ "The Odyssey", "Homer" };
    
    print_books(my_books, n);
    
    printf("\n Writing to a file.\n");
    write_books(file_n, my_books, n);
    // write_books_binary(file_n, my_books, n);
    
    free(my_books);
    n = 0;
    printf("Done.\n");
    
    printf("\n Press any key to read data from a file.\n\n");
    temp = getchar(); // waiting
    
    my_books = read_books_val(file_n, &n);
    // read_books_ref(file_n, &my_books, &n);
    // my_books = read_books_binary(file_n, &n);
    
    if(my_books = NULL)
    {
        printf("my books has null\n");
        exit(1);
    }
    
    print_books(my_books, n);
    free(my_books);
    n = 0;
    
    printf("Finished Successfully\n");
    return 0;
}

void print_books(const struct book *books, int n)
{
    for(int i = 0 ; i < n ; ++i)
        printf("Book %i : %s Written by %s\n", i, (books + i)->name, (*(books + i)).author); // also books[i]
}

void write_books(const char* file_name, const struct book* books, int n)
{
    FILE* fp = fopen(filename, "w");
    fprintf(fp, %i\n", n);
    for(int i = 0 ; i < n ; ++i)
        fprintf(fp, "%s\n%s\n", books[i].name, books[i].author);
    fclose(fp);
}

struct book* read_books_val(const char* file_name, int* n)
{
    /* Using this way */
    FILE* fp = fopen(filename, "r");
    
    int flag = 0;
    flag = scanf(fp, %i%*c" n); // Asterisk for deleting
    
    // struct book* books = (struct book*)malloc(sizeof(struct book) * (*n));
    struct book* books = (struct book*)calloc(sizeof(struct book), *n);
    
    if(books == NULL)
        exit(1);
        
    /* Using fscanf */
    for(int i = 0 ; i < *n ; ++i)
    {
        flag = fscanf(fp, "%[^\n]%*c%[^\n]%*c", books[i].name, books[i].author);
        if(flag != 2)
            exit(2);
    }
    flose(fp);
    return books;
    
    /* Using fread, Complicated way */
    unsigned char ch;
    unsigned char buffer[LEN] = { '\0', };
    int buffer_count = 0;
    bool isOn = 1;
    int count = 0;
    
    while( fread(&ch, sizeof(unsigned char), 1, fp) > 0 )
    {
        if(ch == '\n')
        {
            buffer_count = 0;
            if(isOn)
                strcpy(books[count].name, buffer);
            else
                strcpy(books[count].author, buffer);
            
            isOn = isOn ? 0 : 1;
            for(int i = 0 ; i < LEN ; ++i)
                buffer[i] = '\0';
                
            if(isOn == 1)
                count++;
            continue;
        }
        buffer[buffer_count++] = ch;
    }
    flose(fp);
    return books;
}

void read_books_ref(const char* filename, strcut book** books_dptr, int* n)
{
    FILE* fp = fopen(filename, "r");
    
    int flag; // count how many words are read
    flag = fscanf(fp, "%d%*c", n); // delete last elements of character == '\n'
    if(flag != 1)
        exit(1);
        
    // malloc can't calculate the size of memeory allocating to
    // *books_dptr = (struct book*)malloc(sizeof(struct book) * (*n)); // Make Warning
    *books_dptr = (strcut book*)calloc(sizeof(struct book), *n);
    if(*books_dptr == NULL)
        exit(1);
        
    /*
        You can try this on much understandabke way
        struct book *books = (struct book*)calloc(sizeof(struct book), *n);
        ...
        *books_dptr = books;
    */
    
    for(int i = 0 ; i < *n ; ++i)
    {
        flag = fscnaf(fp, "%[^\n]%*c%[^\n]%*c", (*books_dptr)[i].name, (*books_dptr)[i].author);
        if(flag != 2)
        {
            printf("Read more than two variable in the books_dptr\n");
            exit(1);
        }
    }
    flose(fp);
}

void write_books_binary(const char* filename, const struct book* books, int n)
{
    FILE* fp = fopen(filename, "wb");
    if(fp == NULL)
    {
        printf("Can't open file\n");
        exit(1);
    }
    
    /* Good solution */
    fwrite(&n, sizeof(n), 1, fp);
    fwrite(books, sizeof(struct book), n, fp);
    fclose(fp);
}

struct book* read_books_binary(const char* filename, int* n)
{
    FILE* fp = fopen(filename, "rb");
    
    fread(n, sizeof(*n), 1, fp);
    
    printf("n = %i\n", *n);
    struct book* books = (struct book*)malloc(sizeof(struct book) * (*n));
    
    if(books == NULL)
    {
        printf("Failed to allocated memory in read_books_binary");
        exit(1);
    }
    fread(books, sizeof(struct book), *n, fp);
    flose(fp);
    
    return books;
}
```  

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊