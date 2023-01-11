---
title: "구조체 - 2"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2023-01-10
last_modified_at: 2023-01-10
---

## 14. 구조체  

### 14.15 공용체의 원리  

`공용체 vs 구조체`  

![TBC](https://user-images.githubusercontent.com/37467408/211437759-0293b224-ee09-4bf3-85a6-246d4d8503a6.PNG)  

공용체는 서로 다른 자료형이 같은 메모리 주소를 공유한다. (같은 저장공간을 사용한다.)  

`공용체를 만들어 주소를 확인해보자.`  
1. 공용체 멤버변수 중 저장공간이 가장 많이 필요한 double을 기준으로 메모리를 할당받았다.  
2. 공용체, 공용체 멤버변수는 모두 같은 주소값을 공유하고 있다.  

```cpp
/*
    Union
    - Store different data types in the same memory space
*/

union my_union
{
    int i;
    double d;
    char c;
};
union my_union uni;

typedef union
{
    int i;
    double d;
    char c;
}Simple_union;
Simple_union sim_uni;

printf("Size of union : %zd\n", sizeof(union my_union));
printf("address union : %p\n", &uni);
printf("each member's add : %p %p %p \n", &(uni.i), &(uni.d), &(uni.c));

union my_union union_list[10];
printf("size of union arr : %zd\n", sizeof(union_list));
```  

> Output  
size of union : 8  
addres  union : 007AFB94  
each member's add :007AFB94 007AFB94 007AFB94  
size of union arr : 80  

`공용체 멤버 변수에 값을 할당하고 다른 변수가 어떻게 변하는지 확인해보자.`  

```cpp
uni.c = 'A';
printf("%c %x %d\n", uni.c, (int)uni.c, uni.i);

uni.i = 0;
uni.c = 'A';
printf("%c %x %d \n", uni.c, (int)uni.c, uni.i);

uni.d = 1.2;
printf("%c %x %d %f \n", uni.c, (int)uni.c, uni.i, uni.d);
```

> Output
A 41 -858993599
A 41 65
3 33 858993459 1.200000 // first 3 is '3' = 51  

`Union's Assignment Operator, Pointer and Usage`  

```cpp
/*
    Initializing union
*/

// You can assigning only one member variable
union my_union uni_assignment = uni;
union my_union uni_firs_only = { 10 }; // { 10, 3.14 } <= Error!
union my_union uni_designate = { .c = 'A' };
union my_union uni_donot = { .d = 1.23, .i = 100 }; // Do not use in this way
printf("%c %f %d \n", uni_donot.i, uni_donot.d, uni_donot.c); // >> d 0.000000 100

uni.i = 123;
uni.d = 1.2;
uni.c = 'k';
printf("%c %f %d \n", uni.i, uni.d, uni.c); // >> k 1.200000 107

union my_union* uni_ptr = &uni; // Pointer to union
int x = uni_ptr->c;

uni.c = 'A'; // Don't do this uni.d has 'A's
double readl = 3.14 * uni.d; // Bianry data is converting to double
```

### 14.16 공용체와 구조체를 함께 사용하기  

멤버 변수 여럿 중 단 하나만 사용할 때, 멤버 변수를 서로 배타적으로 사용할 때 공용체를 사용해보자.  

주로 구조체와 함께 사용한다.  

```cpp
struct personal_owner
{
    char rrn_h[7]; // Resident Registration Number
    char rrn_t[8]; // 830422 - 1185600
};

struct company_owner
{
    char crn_h[4]; // Company Registration Number
    char crn_m[3]; // 111 - 22 - 33333
    char crn_t[6];
};

union data
{
    struct personal_owner pers; // mutually exlusive
    struct company_owner comp;
};

struct car_data
{
    char model[15];
    int status; // 0 = personal, 1 = company
    union data ownerinfo;
};

void print_car(struct car_data car)
{
    printf("-------------------------------------------\n");
    printf("Car model : %s\n", car.model);
    if(car.status == 0) // personal = 0, company = 0
        printf("Personal Owner : %s - %s\n", car.ownerinfo.pers.rrn_h, car.ownerinfo.pers.rrn_t);
    else
        printf("Company Owner : %s - %s - %s\n", car.ownerinfo.comp.crn_h, car.ownerinfo.comp.crn_m, car.ownerinfo.comp.crn_t);
    printf("-------------------------------------------\n");
}

int main()
{
    struct car_data my_car = { .model = "Avante", .status = 0, .ownerinfo.pers = { .rrn_h = "990830", .rrn_t = "1123456" };
    struct car_data company_car = { .model = "Sonata", .status = 1, .ownerinfo.comp = { .crn_h = "111", .crn_m = "22", .crn_t = "33333" }};
    
    print_car(my_car);
    print_car(company_car);
    return 0;
}
```

### 14.17 익명 공용체  

위 코드에서 union data를 struct 안에서 익명으로 만들 수 있다. 멤버 변수를 한 번 덜 호출할 수 있어 편하다.  

```cpp
struct car_data
{
    char model[15];
    int status; // Personal = 0, Company = 1
    union{
        struct personal_owner pers; // mutually exclusive
        struct company_owner comp;
    };
};
```  

전체 코드는 다음과 같다.  

```cpp
struct personal_owner
{
	char rrn_h[7]; // Resident Registration Number
	char rrn_t[8]; // 830422 - 1185600
};

struct company_owner
{
	char crn_h[4]; // Company Registration Number
	char crn_m[3]; // 111 - 22 - 33333
	char crn_t[6];
};

struct car_data
{
	char model[15];
	int status; /* 0 = psersonal, 1 = compnay */
	union{
		struct personal_owner pers; // mutually exclusive
		struct company_owner  comp;
	};
};

void print_car(struct car_data car)
{
	printf("-------------------------------------------\n");
	printf("Car model : %s\n", car.model);
	if (car.status == 0) /* personal = 0, company = 1 */
		printf("Personal Owner : %s-%s\n", car.pers.rrn_h,
			car.pers.rrn_t);
	else
		printf("Company Owner : %s-%s-%s\n", car.comp.crn_h,
			car.comp.crn_m, car.comp.crn_t);
	printf("-------------------------------------------\n");
}

int main()
{
	struct car_data my_car = { .model = "Avante", .status = 0,
		.pers = {.rrn_h = "990830",.rrn_t = "1123456"} };

	struct car_data company_car = { .model = "Sonata", .status = 1,
		.comp = {.crn_h = "111", .crn_m = "22",.crn_t = "33333"} };

	print_car(my_car);
	print_car(company_car);
	return 0;
}
```  

`같은 자료형이지만 다양한 식별자를 사용하고 싶다면 공용체를 사용하자.`  
- 배열 식별자와 개별 식별자를 함께 사용할 때 특히 유용하다.  

```cpp
struct vector2D
{ 
    union{
        struct { double x, y };
        struct { double i, j };
        struct { double arr[2] };
    };
};
typedef struct vector2D Vec2;
Vec2 v2 = { 3.14, 2.99 };
printf("%.2f %.2f \n", v2.x, v2.y); // >> 3.14 2.99
printf("%.2f %.2f \n", v2.i, v2.j); // >> 3.14 2.99
printf("%.2f %.2f \n", v2.arr[0]. v2.arr[1]); // >> 3.14 2.99

for(int i = 0 ; i < 2 ; ++i)
    printf("%.2f ", v2.arr[i]);
```  

### 14.18 열거형  

`숫자를 이용하며 다양한 변수나 상태를 표현하라면 다음과 같은 방법이 있다.`  
1. 숫자 - 특징 대응 표 만들기  
2. define으로 정의하기  

```cpp
// Red : 0, Orage : 1, Yellow : 2, Green : 3, ...
// this make programmer hard to remember everything
int color = 0;
if(color == 0)
    printf("Red\n");
else if(color == 1)
    printf("Orange\n");
else if(color == 2)
    printf("Yellow\n");
    
// Preprocessor coping every degine words to it's usage
// Compiler can't catch any errors if define words has problem
#define RED 1
#define ORANGE 2
#define YELLOW 3
color = YELLOW;
if(color == YELLOW)
    printf("Yellow\n");
```  

열거형을 쓰면 간단하게 바뀐다.  

`열거형 사용해보기`  
- 열거형을 사용하면 정수 상수를 기호, 단어로 부를 수 있다.  
- 열거형은 읽기 쉽고 유지보수에 유리하다.  
- 컴파일러가 확인할 수 있어 에러 발견이 쉽다.  

```cpp
/*
    Enumerated type
    - Symbolic names to represent integer constants
    - Improve readability and make it easy to maintain
    - 'enum' specifier ( 'struct' specifier, 'union' specifier )
    
    Enumerators
    - The symbolic constants
*/

enum spectrum { red, orange, yellow, green, blue, violet } // each vairable is eumerator
//               0      1       2      3      4      5

enum spectrum color; // you can assign any enumerator of this type
color = blue; // like this
if(color == yellow)
    printf("yello\n");

for(color = red ; color <= violet ; color++) // Note: ++operator doesn't allowed in C++
    printf("%d \n", color);
printf("red = %d, orange = %d\n", red, orange);
```  

`열거형은 개별 변수의 값을 정해줄 수 있다`  
- 이를 활용하여 if 조건문에 특정 정수 상수를 넣어줄 수 있다. 유지보수가 쉽다.  

```cpp
enum levels{
    low = 100,
    medium = 500,
    high = 2000;
};

int score = 700; // TODO : user input
if(score > high)
    printf("High score!\n");
else if(score > medium)
    printf("Good job!\n");
else if(socre > low)
    printf("Not bad\n");
else
    printf("Do your best\n");
```  

할당해주지 않은 변수는 자동으로 할당된다. 다음 예제를 보자.  

```cpp
enum pet{
    cat,
    dog = 10,
    lion,
    tiger,
}
printf("cat = %d\n", cat); // >> cat = 0
printf("lion = %d\n", lion); // >> lion = 11
printf("tiger = %d\n", tiger); // >> tiger = 12
```

### 14.19 열거형 연습문제  

`열거형을 이용하여 for문을 쉽게 사용할 수 있다.`  

```cpp
for(color = red; color <= blud ; ++color)
```  

`문제풀이`  

```cpp
#define LEN 30

enum spectrum
{
    red,
    orange,
    yellow,
    green,
    blue
};
const char* colors[] = { "red", "orange", "yellow", "green", "blue" };

int main()
{
    char choice[LEN] = { 0, };
    enum spectrum color;
    bool color_is_found = false;
    
    while(1)
    {
        printf("Input any color you want(Enter to quit)\n");
        if(scanf("%[^\n]%*c", choice) != 1)
        {
            printf("Good day to see you\n");
            exit(1);
        }
        
        // for(color = red ; color <= blue ; ++color) { ... }
        for(int i = 0 ; i < 5 ; ++i)
        {
            if(strcmp(choice, colors[i]) == 0)
            {
                color_is_found = true;
                color = i;
                break;
            }
        }
        if(color_is_found)
            printf("%s is %i\n", colors[color], color);
        else
            printf("try it again\n");
        color_is_found = false;
    }
    return 0;
}
```  

### 14.20 이름공간 공유하기  

`Namespace`  
- Restricted space that program could recognize identifier  
- Specifier's tag and this identifier could use same name (C++ block this)  
- C++ : You can declare duplicated identifier with different namespace in one place  

1. 같은 Scope 내에 중복된 식별자를 가질 수 없다. 자료형이 달라도 마찬가지이다.  

```cpp
double myname = 3.14;
int myname = 345; // Error!
```  

```cpp
enum myname = { Kim, Lee, Park };
enum myname myname = Kim;
int myname = 345; // Error
```  

다른 Scope 라면 중복된 식별자를 가질 수 있다. 이 때 바깥 Scope에 있는 식별자는 내부에서 사용할 수 없다.  

```cpp
int myname = 123;
{
    int myname = 345; // myname out of scope is hiding
}
```  

2. Specifier에서 사용한 tag는 해당 변수의 식별자로 사용할 수 있다. (C++에서는 금지한다.) >> 사용하지 말자.  

```cpp
struct test
{
    int i;
    int j;
};
struct test test = { .i = 1, .j = 2 };
int test = 111; // Error!
```  

```cpp
struct rect { double x; double y; };
struct rect rect = { .x = 1.1, .y = 2.2 };

// Struct two and int two are in different categories
struct two { int one; int two; };
int two = 123; // OK in C(Error in C++) but not recommand
```  

3. typedef, specifier, identifier 3개 이상 겹치면 안된다.  
- 아래 예제에서 thr을 three로 바꾸거나 fo를 four로 바꾸었을 때 컴파일 에러가 발생한다.  

```cpp
struct three
{
    int one;
    int two;
    int three;
};
typedef struct three three;
three thr = { 1, 2 };
printf("thr %i %i \n", thr.one, thr.two); // OK

struct four
{
    int one;
    int two;
};
typedef struct four fo;
fo four = { 1, 2 };
printf("four %i %i \n", four.one, four.two); // OK
```  

같은 Namespace에서 함수명과 변수명이 동일하면 Error  

```cpp
int iamfunction()
{
    return 0;
}

int main()
{
    int i = iamfunction();
    int iamfunction = iamfunction(); // Error!
}
```  

`Namespace Pollution`  
- 전역변수는 다른 파일에서 선언되더라도 중복된 식별자를 가질 수 업다. Namespace 내부에 정의하면 해결가능하다.  

```cpp
// Other.c
int i = 5;

// main.c
int i = 5;
int main()
{
    // Error occured at linking
    return 0;
}
```  

### 14.21 함수 포인터의 원리  

`함수 실행의 의미`  
- 컴파일러는 식별자를 메모리 주소로 번역하여 찾아간다. 해당 주소에 저장되어 있는 명령어를 실행한다.  

`프로그램은 어떻게 실행될까?`  
- 코드 작성 -> 컴파일 -> 실행파일 생성(하드디스크에 저장) -> OS에 요청(실행파일 실행) -> OS : 프로그램 메모리를 복사, RAM에 붙여넣기 -> RAM의 TextSegment(Read Only)에 저장된다.  

![TBC](https://user-images.githubusercontent.com/37467408/211465003-c718be13-7003-4193-91c2-0237ea8c1042.PNG)  

위 코드에서 message 자기 자신의 주소는 stack에 저장되어 있다. message 값은 TextSegment에 저장되어있다.  

f_ptr(함수 포인터) 자기 자신의 주소는 Stack에 저장되어 있다. f_ptr(함수 포인터) 값은 TextSegment에 저장되어있다.  

main(함수)의 이름은 포인터이다. main 함수는 TextSegment에 저장되어있다.  

`예제`  

1. 함수 포인터는 해당 함수 주소값을 저장한다.  
2. 함수의 이름은 포인터이므로 Ampersand(&)를 사용하지 않아도 된다.(사용해도 된다.)  
3. 포인터 문법대로 indirection하여 사용할 수 있다. 물론 indirection 없이도 사용가능하다.  

```cpp
void func1()
{
    printf("func1 is working\n");
}

int func2(int i)
{
    printf("func2 is working\n");
    return i * -1;
}

double func3(int i, double d)
{
    printf("func3 is working\n");
    return i + d;
}

int main()
{
// return   identifier  parameter should be match to declaration
    void     (*pf1)        ()     = func1 // &func1 also work
    int (*pf2)(int) = func2;
    
    double (*pf3)(int, double) = func3;
    
    pf1();
    (*pf1)();
    
    int test2 = pf2(5);
    int test2_ = (*pf2)(5);
    printf("%i %i", test2, test2_);
}
```  

### 14.22 함수 포인터의 사용 방법  

`다음 코드에서 ToUpper 함수와 ToLower 함수는 Parameter와 반환값의 자료형이 동일하다.`  

```cpp
// ToUpper and ToLower has same form so you can use same func ptr
void ToUpper(char* str) // Convert Every char to Uppercase
{
    while(*str)
    {
        *str = toupper(*str);
        str++;
    }
}

void ToLower(char* str)
{
    while(*str)
    {
        *str = tolower(*str);
        str++;
    }
}

int main()
{
    char str[] = "Hello, World!";
    void (*func_ptr)(char*);
    
    func_ptr = ToUpper;
    // func_ptr = &ToUpper; // Acceptable
    // func_ptr = ToUpper(str); // Not Acceptable in C,
                                // It execute function and assign it's value on func_ptr
                                
    printf("String Literal add %p\n", ("Hello, World!"));
    printf("Function pointer add %p\n", ToUpper);
    printf("Variable add %p\n", str);
    
    func_ptr(str);
    (*func_ptr)(str); // Accept in ANSI, Not in K&R
    printf("ToUpper >> %s\n", str);
    
    func_ptr = ToLower;
    func_ptr(str);
    printf("ToLower >> %s\n", str);
    
    return 0; 
}
```  

`함수 포인터를 활용하여 ToUpper 함수와 ToLower 함수를 간단하게 사용하자`  

```cpp
void UpdateString(char* str, int (*func_ptr)(int))
{
    while(*str)
    {
        *str = func_ptr(*str); // (*func_ptr)(str)
        str++;
    }
}

int main()
{
    char str[] = "Hello, World!";
    
    UpdateString(str, toupper);
    printf("Update toupper >> %s \n", str);
    
    UpdateString(str, tolower);
    printf("Update tolower >> %s \n", str);
    
    return 0;
}
```  

### 14.23 자료형에게 별명을 붙여주는 typedef  

`특징`  
- 특정 자료형을 다른 이름으로 부를 수 있다.  
- 새로운 자료형을 만들지 않는다.  

`장점`  
- 사용의도를 자료형에 나타낼 수 있다.  
- 짧게 쓸 수 있다.  
- 사용 환경에 맞게 자동으로 변환시킬 수 있다.  

`typedef의 장점 - Portable(이식성이 높다)`  
- 개발 환경마다 다른 자료형을 가지는 자료형을 쉽게 사용학 위해 typedef를 사용할 수 있다. 아래 코드에서 size_t는 x86, x64 환경에서 각각 다른 값을 가진다.  

```cpp
typedef unsigned char BYTE;
BYTE x, y[10] = { 0, }, *z = &x;
{
    typedef unsigned char byte;
    
    size_t s = sizeof(byte); // unsigned int (x86), unsigned long long (x64)
    
    /*
        above code solve this imcompatible code problem
        // unsigned int s = sizeof(byte); // x86
        // unsigned long long s = sizeof(byte); // x64
    */
    // byte b; // Error! typedef belong to above scope
}
```  

size_t의 정의를 찾아가자. 다음은 typedef의 활용법을 잘 보여주고 있다.  

```cpp
#ifdef _WIN64
    typedef unsigned __int64 size_t;
#else
    typedef unsigned int size_t;
```  

`자주 사용하는 함수 time의 반환값은 환경에 따라 달라진다. time_t가 typedef로 정의되어 있기 때문에 가변적으로 사용가능하다.`  

```cpp
// #include <time.h>
/*
    This function returns the time since 00:00:00 UTC
    January 1, 197 (Unix time stamp) in sencods
*/
time_t t = time(NULL);
printf("%lld\n", t);
```  

`time_t의 정의를 찾아가면 typedef를 활용하고 있다.`  

```cpp
// correct.h
#ifndef _CRT_NO_TIME_T
    #ifdef _USE_32BIT_TIME_T
        typedef __time32_t time_t;
    #else
        typedef __time64_t time_t;
        
// ...
typedef long __time32_t;
typedef __int64 __time64_t;
```  

`typedef vs #define`  
- 전처리기는 #define에 등록된 Macro를 해당값으로 복사/붙여넣기 한다.  
- 에러가 만들기 쉬우니 typedef를 권장한다.  

```cpp
/*
    - typedef interpretation is performed by the compiler, not the preprocessor
    - mode flexible than #define
*/
typedef char* STRING;
STRING name = "John Wick", actor = "Keanu Reeves";
printf("typedef %s %s\n", name, actor);

#define STRING_C char*
STRING_C name_c = "Daenerys Targaryen", *actor_c = "Emilia Clarke";
printf("#define %s %s\n", name_c, actor_c);
```  

STRING을 char*로 대체했다. *actor_c에서 Asterisk(*)가 없으면 char 자료형이 된다. 실수하기 쉽다.  

`typedef를 활용하여 구조체 자료형 나타내기`  

```cpp
typedef struct complex
{
    float real;
    float img;
} COMPLEX; // it makes skip to type "typedef struct comple COMPLEX"

typedef struct
{
    double width;
    double height;
} rect; // No tag
rect rec1 = { 1.1, 2.2 };
rect rec2 = rec1;
```  

`복잡한 함수포인터 typedef로 해석하기`  
- 복잡한 포인터를 어떻게 해석하는지 다음 장에서 다룬다.  
- typedef를 사용하면 간단하게 표현할 수 있다는 점만 기억하고 넘어가자.  

```cpp
/*
    "One Good way to understand complicated
    declarations is small steps with typedef"
    - K&R book chapter 5.12
*/
char char3[3] = { 'A', 'B', '\0' };

// Complicated Function Declarations
char (*complicated_func())[3] // Returns Pointer to char[3]
{
    return &char3; // Returns a pointer pointing to char[3]
}

char (*(*fptr1)())[3] = complicated_func; // so messy

// bring me in peace and calm
typedef char(*FuncReturningPtr())[3]; // Function Return Pointer to char[3]
typedef char(*(*PTRFuncReturningPtr)())[3];

FuncReturningPtr* fptr2 = complicated_func;
PTRFuncReturningPtr fptr3 = complicated_func;

typedef char c_triple[3];
c_triple* complicated_func_simp()
{
    return &char3;
}

int main()
{
    char(*returned)[3] = fptr1();
    c_triple *returned2 = fptr2();
    c_triple *returned3 = fptr3();
    c_triple *returned4 = complicated_func_simp();
    printf("%c %c %c %c \n", (*returned)[0], (*returned2)[1], (*returned3)[0], (*returned4)[1] );
    
    return 0;
}
```

### 14.24 복잡한 선언을 해석하는 요령  

`실무에서 복잡한 선언을 보기는 어렵다. 보통은 typedef로 간소화하여 사용하고 이해한다. 원리만 알고가자.`  

```cpp
/*
    * indicates a pointer
    () indicates a function, not priority
    [] indicates an array
    
    Deciphering Complex Declarations (KNK 18.4)
    - Always read declarations from the inside out.  
    - When there's a choice, always favar [] and () over *(asterisk)
*/
```  

1. identifier 식별자를 찾는다.  
2. 식별자를 기준으로 항상 안쪽에서 바깥쪽으로 읽어라.  
3. []와 ()는 *(asterisk) 보다 우선순위를 높게 해석한다.  

`복잡한 함수 포인터 살펴보기`  

```cpp
int temp(int a)
{
    return 0;
}

int (*g(int a))(int) // Not used in pratically
{                    // Normally convert to typedef style
    return temp;
}
```  

위 사례처럼 복잡한 g 함수포인터는 사용하지 않는다. typedef를 이용하여 간소화 해줌이 바람직하다.  

`반환값이 복잡할 때 typedef로 간소화하기`  

```cpp
int* ap[10]; // Identifier ap is array saving pointer

typedef int* ptr_int;
ptr_int typedef_ap[10];

float* fp(float); // fp is function takes float parameter, return float ptr

typedef float* ptr_float;
ptr_float fp(float);
```  

`복잡한 함수 포인터를 해석해보자`  

```cpp
void(*pf)(int);
/*
    void(*pf2)(int);
           1              :: 1. this is pointer
                2         :: 2. take int type parameter so function ptr
      3                   :: 3. return void type
*/

int* (*x[10])(void);
/*
    int* (*x[10])(void);
              1             :: 1. array save 10 elem
           2                :: 2. pointer
                    3       :: 3. take void type parameter, so function ptr
      4                     :: 4. return int pointer
*/
```  

`잘못된 사례`  
1. 함수는 배열을 반환할 수 없다.  
- 배열을 가리키는 포인터는 반환할 수 있다.  

```cpp
// A function can't return an array
int f(int)[]; // Error - Wrong

// But it can return a pointer to an array
int(*f(int))[]
```  

2. 함수는 함수를 반환할 수 없다. (C 한정)  
- 함수가 함수를 가리키는 포인터를 반환할 수 있다.  

```cpp
// A function can't return a function
int g(int)(int); // Error! - Wrong

// But it can return a pointer to function
int (*g(int))(int);
```  

3. 여러 함수를 담은 배열은 존재할 수 없다.  
- 함수 포인터를 담은 배열은 가능하다.  

```cpp
// An array of functions are impossible
int a[10](int) // Error! - Wrong

// But an array of function pointers are possible
int* (*x2[10])(void)
```  

x2는 함수포인터를 담은 배열이다. 이를 간단하게 나타내보자.  

```cpp
// using typedef to simplify declaration
typedef int FCN(int);
typedef FCN* FCN_PTR;
typedef FCN_PTR FCN_PTR_ARRAY[10];
FCN_PTR_ARRAY x3;
```  

`추가 예제`  

```cpp
int board[6][4]; // an array of arrays of int
int** db_ptr;

int* risk[10]; // array saves 10 in pointer
int(*risk)[10]; // pointer pointing to array saving 10 int

int* off[3][4]; // 4 x 3 array saves 12 int pointers
int(*uff)[3][4]; // pointer pointing to 4 * 3 int array
int(*uof[3])[4]; // array saves 3 pointers pointing to 4 element int array
```

int(*uof[3])[4]가 해석하기 어렵다. 3개의 int pointer가 4개짜리 배열을 가리키고 있다.  

```cpp
char* fump(int); // function returning char*
char (*frump)(int); // function pointer, pointed function retucn char
char (*flump[3])(int); // array of 3 pointers to functions that return type char
```  

char (*flump[3])(int)가 해석하기 어렵다. parameter와 반환값은 쉽계 에측이 가능하다. (*flump[3])은 배열 안에 함수 포인터가 3개 있음을 가리킨다.  

```cpp
typedef int arr[5];
typedef arr* p_arr;
typedef p_arr ten_p_arr[10];

arr five_arr;
p_arr ptr_five_arr;
ten_p_arr ptr_five_arr_ten_times;
```  

### 14.25 qsort() 함수 포인터 연습문제  

`예제를 보고 float 자료형으로 구현해보자`  

```cpp
#include <stdio.h>
#include <stdlib.h>
int compare(const void* first, const void* second)
{
    if(*(int*)first > *(int*)second)
        return 1;
    else if(*(int*)first < *(int*)second)
        return -1;
    else
        return 0;
}

int main()
{
    int arr[] = { 8, 2, 5, 3, 6, 11 };
    int n = sizeof(arr) / sizeof(arr[0]);
    qsort(arr, n, sizeof(int), compare)
    
    for(int i = 0 ; i < n ; ++i)
        printf("%d ", arr[i]);
}
```  

`struct 자료형으로 구현하기`  

```cpp
struct kid
{
    char name[10];
    int height;
};

// TODO : Try increasing / decreasing order
int compare_kid(const void* first, const void* second)
{
    if(((struct kid*)first->height > ((struct kid*)second)->height)
        return 1;
    else if(((struct kid*)first-> height < ((struct kid*)second->height)
        return -1;
    else
        return 0;
    
    /*
        typedef struct kid* kid_ptr;
        kid_ptr kid_ptr_1 = (kid_ptr)first;
        kid_ptr kid_ptr_2 = (kid_ptr)second;
        if(kid_ptr_1->height > kid_ptr_2->height)
            return 1;
        else if(kid_ptr_1->height < kid_ptr_2->height)
            return -1;
        else
            return 0;
    */
}

int main()
{
    struct kid friends[] = { "Jack Jack", 40, "Genie", 300, "Aladdin", 170, "Piona", 150 };
    // struct kid friends[] = { { "Jack Jack", 40 }, { "Genie", 300 }, { "Aladdin", 170 }, { "Piona", 150 } };
    
    const int n = sizeof(friends) / sizeof(struct kid);
    qsort(friends, n, sizeof(struct kid), compare_kid);
    for(int i = 0 ; i < n ; ++i)
        printf("%s  \tis %3i height\n", friends[i].name, friends[i].height);
}
```  

구조체 배열을 정의할 때 각 구조체 변수의 값을 직렬로 늘려놓아도 된다.  

### 14.26 함수 포인터의 배열 연습문제  

```cpp
void update_string(char* str, int(*pf)(int));
void ToUpper(char* str);
void ToLower(char* str);
void Transpose(char* str);

int main()
{
    char option[] = { 'u', 'l' }; // let's add 't'
    int n = sizeof(option) / sizeof(option[0]);
    
    typedef void (*FUNC_TYPE)(char*);
    FUNC_TYPE operation[] = { ToUpper, ToLower }; // You can add transpose
    
    printf("Enter a string\n>> ");
    char input[100];
    
    int scanned;
    while((scanned = scanf("%[^\n]%*c", input)) != 1)
        printf("Please try again\n");
        
    while(1)
    {
        printf("Choose an option:\n");
        printf("u ) to upper\n");
        printf("l ) to lower\n");
        
        char c;
        while(scanf("%c%*[^\n]*c", &c) != 1)
            printf("Please try again\n");
            
        bool found = false;
        for(int i = 0 ; i < n ; ++i)
        {
            if(option[i] == c)
            {
                (*(operation[i]))(input);
                found = true;
                break;
            }
        }
        if(found)
            break;
        else
            printf("Wrong Input, try again\n");
    }
    printf("result = %s \n", input);
}

void update_string(char* str, int(*pf)(int))
{
    while(*str)
    {
        *str = (*pf)(*str);
        str++;
    }
}

void ToUpper(char* str)
{
    while(*str != '\0')
    {
        *str = toupper(*str);
        str++;
    }
}

void ToLower(char* str)
{
    while(*str != '\0')
    {
        *str = tolower(*str);
        str++;
    }
}

void Transpose(char* str)
{
    while(*str)
    {
        if(islower(*str))
            *str = toupper(*str);
        else if(isupper(*str))
            *str = tolower(*str);
        str++;
    }
}
```

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊