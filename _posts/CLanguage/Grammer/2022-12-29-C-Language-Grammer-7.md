---
title: "Storage Classes, Linkage and Memory Management"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-12-29
last_modified_at: 2022-12-31
---

## 12. Storage Classes, Linkage and Memory Management  

### 12.1 메모리 레이아웃 훑어보기  

- 프로그램이 시작될 때  

```cpp
#include <stdio.h>

void func()
{
    int i = 123;
    printf("%lld\n", (long long)&i);
}

int main()
{
    const char* message = "Banana";
    printf("Apple and %s", message);
    printf("\n");
    
    void (*f_ptr)() = func; // address of a function
    
    printf("%lld\n", (long long)&message);
    printf("%lld\n", (long long)&f_ptr);
    printf("%lld\n", (long long)message);
    printf("%lld\n", (long long)f_ptr);
    printf("%lld\n", (long long)main);
    
    func();
    
    return 0;
}
```  

![TBC](https://user-images.githubusercontent.com/37467408/209958861-3c0a6002-7d20-4380-ba5c-b552c77cc857.PNG)  

프로그램이 시작되면 해당 프로그램을 실행하기 위한 코드가 Read Only Memory에 저장된다. 직접 작성한 코드가 저장되는 곳이다. 이 Memory는 프로그램이 끝날 때까지 변경되지 않는다. C++에 가면 프로그램도 코드로 동적으로 변화시킬 수 있다. 지금은 프로그램을 실행하며 필요할 때마다 코드를 가져와 실행시킨다.  

- 프로그램 전체에서 계속 사용되는 변수들  

```cpp
#include <stdio.h>

int g_i = 123; // global variable
int g_j;       // global variable

void func1()
{
    g_i++;     // uses g_i
}

void func2()
{
    g_i += 2;  // uses g_i
}

int main()
{
    int local = 1234;
    
    func1();
    func2();
    
    printf("%d", g_i); // uses g_i
    
    return 0;
}
```  

![TBC](https://user-images.githubusercontent.com/37467408/209958997-0bf240d1-ba1c-49c1-bfcc-30240f10753c.PNG)  

`Data 영역`은 BSS와 Data Segment로 나뉜다.  

`BSS`는 초기화되지 않은 global/static 변수가 저장된다. 모두 0으로 초기화된다.  

`Data Segment`에는 초기화된 global/static 변수가 저장된다.  

g_i와 g_j는 어디에서나 접근이 가능하다. 이 같은 변수를 전역변수라 한다. Data Segment 혹은 BSS Segment에 저장되어 프로그램이 끝날때까지 남아있다. 전역변수는 항상 메모리를 차지하고 있기 때문에 비효율적일 수 있다.  

- 프로그램의 일부에서 큰 메모리가 필요한 경우  

```cpp
#include <stdio.h>

#define MAX 1000

// 1. Use global variable
int g_arr[MAX];

int main()
{
    /*
        Use g_arr
        ...
        Do Not use g_arr
        ...
        Use g_arr
        ...
    */
    return 0;
}
```

![TBC](https://user-images.githubusercontent.com/37467408/209959332-4dfdb81f-cb8e-499f-9cdb-a6156eeb1f40.PNG)  

```cpp
#include <stdio.h>

#define MAX 1000

int main()
{
    // 2. Use Local in main()
    int l_arr[MAX] = { 0, };
    
    /*
        Use l_arr
        ...
        Do NOT use l_arr
        ...
        use l_arr
        ...
    */
    return 0;
}
```  

l_arr[MAX]는 main() 함수가 시작되어 메모리를 할당받은 후 main() 함수가 끝날 때 메모리를 돌려준다.  

```cpp
#include <stdio.h>

#define MAX 1000

// 3. Use local in samller function
void func()
{
    int l_arr[MAX] = { 0, };
}

int main()
{
    /*
        Call func()
        ...
        Call func()
        ...
    */
    return 0;
}
```  

코드를 실행시키다가 함수를 만나면 해당 함수를 실행시킨 뒤에 다시 돌아와서 나머지 코드를 실행시켜야 한다.  

이 때 Stack Frame은 돌아가야하는 위치를 기억하고 있다.  

l_arr[MAX]는 func() 함수가 시작 될 때, 메모리를 할당받은 후 func() 함수가 끝날 때 메모리를 돌려준다. 이같이 필요할 때만 효율적으로 사용하는 변수를 지역변수라고 한다.  

지역변수는 Stack에서 관리된다. Stack은 필요한 메모리만큼 저장공간을 늘릴 수 있다. 지역변수 l_arr[MAX]는 컴파일 시점에 저장공간이 정해져있다. OS의 도움을 받지 않고도 메모리를 할당할 수 있기 때문에 메모리 관리를 빠르게 할 수 있다.  

- 필요한 메모리의 크기를 미리 알 수 없을 경우  

```cpp
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n = 0;
    
    // n from files, internet, scanf, etc.
    
    char *arr = (char*)malloc(sizeof(char) * n);
    
    // ...
    
    free(arr);
    return 0;
}
```

![TBC](https://user-images.githubusercontent.com/37467408/209964834-bae16e19-8d71-4e9b-be38-fedf778256f3.PNG)  

메모리를 얼마나 사용할지 알 수 없을 때, Heap을 사용한다. Stack은 컴파일 때 메모리를 얼마나 사용할지 알 고 있을 때 사용한다. OS에게 메모리를 요청하여 응답받은 후 사용가능하기 때문에 Stack보다 메모리 관리 속도가 느리다.  

`Stack에 저장된 포인터 공간에 동적으로 할당받은 공간의 첫 주소를 저장하여 사용한다.`  

`Stack vs Heap`  
- `Stack`은 해당 프로그램만 사용할 수 있는 메모리 공간이다. 크기가 작다.  
- `Heap`은 OS가 사용할 수 있는 메모리 공간을 가져올 수 있기 때문에 크기가 크다.  

### 12.2 객체와 식별자, L-Value와 R-Value  

`Object와 Identifier`  

Object(객체)는 메모리 공간으로 값을 저장할 수 있다.  
- 메모리 공간은 할당받아 사용하고 반환한다.(사용할 수 있는 기한이 정해져있다.)  
- C++와 같은 OOP에서 Object는 발전된 개념으로 사용된다.  

Identifier(식별자)는 변수의 이름, 함수의 이름, Macro 등을 Identifier라고 지칭한다.  
- Identifier는 Scope(영역)을 가지고 있다.  

```cpp
int var_name = 3;
int *ptr = &var_name;
```  

위 코드는 Identifier(var_name, ptr)을 통해서 값 '3'과 주소 '&var_name'을 각 Obejct(메모리 공간에) 저장하고 있다.  

```cpp
int arr[100];
arr[0] = 7;
```  

배열의 이름 arr은 Identifier이다. arr은 Object가 아니다. arr자체는 저장공간을 가지고 있지 않고 Ampersand(&) 연산자를 사용해도 배열의 첫 값을 가리킨다.  

arr[0]은 Object이다. arr[0]은 Identifier가 아니다. [0]는 expression으로 +0만큼 주소를 이동하였다. 자기 주소를 가지고 값을 저장하기에 Object이다.  

```cpp
#include <stdio.h>

int main()
{
    /*
        Object
        - "An object is simply a block of memory that can store a value." (KNK p. 487)
        - Object has more developed meaning in C++ and object oriented programming (OOP)
        
        Identifier
        - Names for variables, functions, macros, and other entities (KNK p. 25)
    */
    
    int var_name = 3;   // creates an object called 'var_name'
    
    int* pt = &var_name;    // pt is an identifier
    *pt = 1;    // *pt is not an identifier. *pt designates an object.
    
    int arr[100];   // arr is an identifier. Is arr an object?
    arr[0] = 7;   // arr[0] is an object
    
    /*
        L-Value is an expression 'refering' to an object. (K&R p. 197)
        
        L-Value : left side of an assignment
        R-Value : right side, vairables, constant, expressions (KNK P. 67)
    */
    
    var_name = 3;   // modifiable lvalue
    
    pt = &var_name;
    int* ptr = arr;
    *pt = 7;    // *pt is not an identifier but an modifiable lvalue expression.
    
    pt = &var_name;
    int* ptr = arr;
    *pt = 7;    // *pt is not an identifier but an modifiable lvalue expression
    
    int* ptr2 = arr + 2 * var_name;   // address rvalue
    *(arr + 2 * var_name) = 456;    // lvalue ehxpression
    
    const char* str = "Constant string";    // str is a modifiable lvalue
    str = "Second string";    // "Constant string" = "Second String"    // Impossible
    // str[0] = 'A';    // Error
    // puts(str);
    
    char str2[] = "String in an array";
    str2[0] = 'A'; // OK
    // puts(str2);
    
    /*
        Identifier have scope.
        Objects have storage duration.
        Variables and function have on of the following linkage : 
            external linkage, internal linkage, or no linkage.
    */
    return 0;
}
```  

`L-Value와 R-Value`  

식 왼쪽에서 값을 할당받으면 L-Value. L-Value는 expression(표현식)이다. Object를 다른 방식으로 가리킨다. (Referring)  

```cpp
var_name = 3;
```  

변수 이름 var_name이 가리키고 있는 (Referring) 저장공간에 값 3을 복사하여 붙여넣는다.  

```cpp
int temp = var_name;
```  

var_name은 값을 임시 공간에 복사하여 temp에 붙여주고 있다.  

```cpp
temp = 1 + 2;
```  

1 + 2는 R-Value이다. 저장공간을 따로 가지고 있지 않으므로 L-Value가 될 수 없다.  

```cpp
*ptr = 1;
```  

*ptr은 expression으로 Object를 가리키고 있다. *ptr은 ptr을 indirect해준 expression이므로 indentifier가 아니다. L-Value로 사용하고 있는 expression이다.  


### 12.3 변수의 영역과 연결 상태, 객체의 지속 기간  

`Scope 개념`  

전역변수 및 정정변수는 BSS에서 한 번에 0으로 초기화해준다.  

```cpp
int g_j; // global variable uninitalized is set to 0

void funcArray(int n, int arr[n]); // VLA must use identifier

int main()
{
    printf("%d\n", g_j); // global variable uninitialized is set to 0
    
    if(g_j == 0)
        goto jump; // Label could be used before it is marked
jump:

    return0;
}
```  

`Linkage`  

`extern(external linkage)`  
- 파일은 컴파일의 최소 단위이다. 같은 말로 파일은 Translation unit(기계어를 번역하는 단위)이다.  
- 따라서 각 파일에 정의되어 있는 전역 변수는 다른 파일에서 알 수 없다.  
- 다른 파일의 전역변수(File Scope)를 사용하려며 'extern' keyword를 사용해야 한다.  

```cpp
// Link.c
int extern_i // File scope with external linkage

// Second.c
extern int extern_i;
```  

이 때 두 파일에서 extern_i 변수는 같은 메모리 공간 즉, Object을 공유하고 있다.  

`static(internal linkage)`  
- static을 사용하면 해당 변수의 scope를 파일 내로 제한한다. 다른 파일에서 'extern'을 사용해도 사용불가능하다.  

```cpp
// Link.c
static int intern_i; // file scope with internal linkage

// Second.c
extern int intern_i;

...

    printf("%d\n", intern_i); // Link Error
```  

`Storage duration(메모리의 지속기간)`  
- static storage duration : (being allocated until program is executed)  
- automatic storage duration : (local variable, stack)  
- allocated storage duration : (dynamic allocation)  
- thread storage duration : (related to multi threading)  

`Static storage duration`  
- 위에서 설명한 static keyword와 다르게 사용된다.  

```cpp
void static_func()
{
    static int static_count = 0;
    printf("%d\n", static_count++);
}

int main()
{
    static_func();
    static_func();
    return 0;
}
```  

static_func() 함수 내에 count 함수가 종료되어도 메모리 공간을 사용하고 있다. 이후 다시 Initializing이 될 것 같지만 기존의 메모리 공간을 그대로 사용함을 알 수 있다.  

프로그램이 시작 될 때, 메모리를 할당받아 프로그램이 끝날 때까지 반환하지 않는다.  

### 12.4 저장 공간의 다섯 가지 분류  

![TBC](https://user-images.githubusercontent.com/37467408/209971383-ae4fef2e-1f9c-4ab8-9c3a-8a3224bd2fe3.PNG)  

`지역변수`는 모두 자동으로 분류된다.  
`레지스터`는 CPU 내부 처리장치이다. 레지스터에 저장되면 빠르게 처리할 수 있다.  
`고정적 static`은 프로그램 시작부터 끝가지 메모리를 할당받아 사용한다.  
`할당 메모리`는 OS에 메모리 공간을 요청하면 Pointer을 받아 사용한다. 할당 메모리는 저장 공간의 분류에 포함되지 않는다.  

### 12.5 자동 변수  

![TBC](https://user-images.githubusercontent.com/37467408/210081840-df47f409-7d20-4879-9459-6e4537edc9dc.PNG)  

지역변수라고도 한다. 필요할 때마다 Stack의 데이터 공간을 늘려 사용하고 Scope를 벗어나면 반환한다. 모든 변수를 메모리에 저장한 상태로 사용하지 않기 때문에 메모리를 아낄 수 있다.  

Linkage 연결은 불가능하다. 항상 존재하는 메모리 공간이 아니기 때문에 다른 파일에서 해당 주소에 접근하면 오류가 발생된다.  

`C++과 함께 사용할 때 헷갈리니깐 사용하지 말자.`  

```cpp
auto int i; // Compiler didn't initializing it automatically
auto char i; // Global variable initialized 0 automatically
auto double d;
```  

지역변수임을 명시하기 위해 keyword 'auto'를 사용할 수 있다. C++에서 사용하는 auto와는 다른 의미로 쓰인다.  

`name hiding`  

```cpp
int i = 1;
printf("i = %i, %p \n", i, &i); // >> i = 1, 00EFFD2C
if(i == 1)
{
    int i = 2;
    printf("i = %i, %p \n", i, &i); // >> i = 2, 00EFFD20
}
printf("i = %i, %p \n", i, &i); // >> i = 1, 00EFFD2C
```  

Specifier가 동일할 때는 Stack 가장 위에 있는 Specifier, Scope 내에 있는 Specifier를 우선으로 불러온다.  

`Stack Frame`  

```cpp
void func(int k)
{
    int i = k * 2;
}

int main()
{
    int i = 1;
    func(5);
    return 0;
}
```  

func() 함수가 호출되면 Stack에 Stack Frame을 쌓는다. Stack Frame은 데이터를 내부에 저장한다. Stack Frame이 Stack에서 빠져나오면 내부에 저장된 데이터(메모리)도 모두 날라간다.  

결과적으로 main() 함수에서 할당받은 i의 object(저장공간)은 func() 함수 내부에서 사용할 수 없다. function call이 시작되면 Scope는 새로운 Stack Frame으로 한정된다.  

### 12.6 레지스터 변수  

![TBC](https://user-images.githubusercontent.com/37467408/210082715-e13e08cf-d4ad-4fff-bd02-b2c16049d180.PNG)  

레지스터는 CPU가 가지고 있는 임시 저장 공간이다. 메모리는 CPU와 분리되어 있기 때문에 BUS를 통하여 데이터를 주고 받는다. 레지스터에 등록된 데이터를 가져올 때 보다 느리다.  

프로그래머가 Register Keyword를 사용하여 컴파일러에게 register에 메모리를 할당하여 저장해달라고 요청할 수 있다. 컴파일러는 register에 메모리를 할당할 수도 있고, 무시하고 Stack 메모리를 사용할 수도 있다.  

```cpp
void temp(register int r)
{
    // do something
}

int main()
{
    register int r;
    int* ptr = &r; // Error!
}
```  

- Register 주소값은 가져올 수 없다.  
- 임베디드 프로그램이나 로봇 프로그래밍에서 특수한 목적의 컴파일러를 사용할 때 Register Keyword를 사용한다.  

### 12.7 블록 영역의 정적 변수  

![TBC](https://user-images.githubusercontent.com/37467408/210082926-980c0520-1b32-43bb-8633-00e4f1a70217.PNG)  

Life time은 블록 안이지만 메모리는 계속 할당되어 사용할 수 있다. 블록 밖에서 해당 메모리에 접근하여 값은 가져올 수 있지만 그 경우에 함수 밖에서 전역변수 static을 사용함이 바람직하다. 블록 밖에서는 invisible 상태이다.  

함수를 계속하여 사용하되 변수의 값을 새로 초기화하지 않고 그대로 사용하고 싶을 때 사용한다.  

```cpp
void static_count()
{
    static int count = 0;
    printf("static count = %d %p \n", count, &count); // Initialized only Once!!
    count++;
}

int main()
{
    static_count();
    static_count();
    static_count();
}
```  

> Output  
static count = 0 009FC140  
static count = 1 009FC140  
static count = 2 009FC140  

Static variable with no linkage는 단 한번 초기화된다.  

`참고)`  

Static int 변수와 일반 int 변수를 비교하기 위하여 내부에서 일반 int 변수를 사용하는 int count() 함수를 만들어 실험을 하게 된다면, 여러 번 함수를 호출하여 내부에 있는 int count의 주소가 동일하게 출력되는 경우가 있다.  

이는 Stack에서 뺏다 넣어 동일 위치에 있게 되기 때문이다.  

`Static variable with no linkage`의 잘못된 사용  

```cpp
int func(static int i) // Warning ( Error in GCC )
{
}
```  

함수의 Parameter는 호출되는 시점에 메모리를 할당받아 초기화된다.  

이는 Stack에서 메모리를 할당받음을 의미한다. 정적 메모리를 사용함은 적절하지 않다. 전역변수를 사용하자.  

`Static variable with no linkage을 반환하기`  

```cpp
int* count()
{
    int count = 0;
    return &count; // Bad! Deallocated memory address
}

int* static_count()
{
    static int count = 0;
    return &count; // OK
}
```  

count() 함수에서 반환되는 주소는 사용가능할지 몰라도 이미 반환된 메모리이다. 의도치 않은 동작을 실행할 수 있다.  

static_count() 함수에서 반환되는 주소는 ptr로 사용해도 된다. 이 경우는 전역변수를 사용함이 더 바람직하다.  

### 12.8 정적 변수의 외부 연결 external linkage  

![TBC](https://user-images.githubusercontent.com/37467408/210083744-7effe541-76ef-4150-b0a9-ba8c37820581.PNG)  

번역 단위란 Translation unit으로 C file을 의미한다. C file을 컴파일하여 obj 파일을 안든다.  

obj파일들끼리 linkage 작업을 하여 이어준다. 이 때 external variable, 함수를 연결한다.  

`사용 방법`  

```cpp
int global_i;
int global_arr[10];
size_t size = sizeof(int);

int x = 5; // OK, Constant expression
int y = x; // Invalid
```  

- global_i 처럼 전변수는 Compiler가 자동으로 초기화해준다. 전역변수는 한 번 초기화 해주면 계속 사용할 수 있다.  
- 전역변수는 변수가 들어간 표현으로 초기화할 수 없다. 항상 Constant를 사용해야 한다.  

`Hides global variable`  

```cpp
int global_i;

int main()
{
    printf("%i %p \n", global_i, &global_i); // >> 0 00CEC140
    int global_i = 5; // hides global variable "global_i"
    printf("%i %p \n", global_i, &global_i); // >> 5 0135F7D4
}
```  

같은 이름을 내부에서 선언하여 사용하고 싶다면 extern keyword를 활용하자.  

```cpp
int global_i;

int main()
{
    printf("%i %p \n", global_i, &global_i); // >> 0 0094C140
    extern int global_i; // hides global variable "global_i"
    printf("%i %p \n", global_i, &global_i); // >> 0 0094C140
}
```  

extern keyword를 사용하여 블럭 외부 File Scope에 있는 전역함수 메모리에 접근한다.  

`서로 다른 파일에서 Extern 활용하기`  

```cpp
// main.c
int global_i;
void func_linking();

void func_main()
{
    printf("v:%i %p in main \n", global_i, &global_i);
}

int main()
{
    func_linking();
    func_main();
}

// 1. linking.c
extern int global_i;
void func_linking()
{
    printf("v:%i %p in linking \n", global_i, &global_i);

	  global_i++;
}

// 2. linking.c
void func_linking()
{
    extern int global_i;
    printf("v:%i &%p in linking \n", global_i, &global_i);
    
    global_i++;
}
```  

이 때 extern으로 설정된 identifier는 블록 scope일 때 초기화 할 수 없다. stack 저장공간을 사용하기 때문이다.  

파일 scope일 때는 초기화가 가능하다.  

```cpp
// main.c
int global_i; // defining delcaration

// OK
extern int global_i = -10; // OK, referencing delcaration
void func_linking()
{
    printf("v: %i %p in linking \n", global_i, &global_i);
    global_i++;
}

// Error!
void func_linking()
{
    extern int global_i = -10; // Error
    printf("v: %i %p in linking \n", global_i, &global_i);
    global_i++;
}
```  

`주의`  
전역 변수는 단 한번만 초기화할 수 있다. 만약 main.c에서 초기화를 해주었다면 다른 파일에서 초기화할 수 없다. 초기화는 어디에서든 할 수 있지만, extern keyword가 없는 곳(원본), 같은 말로 defining declaration에서 초기화를 해주자.  

### 12.9 정적 변수의 내부 연결 internal linkage  

![TBC](https://user-images.githubusercontent.com/37467408/210090056-cb6e43ee-2af4-441b-ab89-5f29ffbc14d3.PNG)  

Static keyword를 사용하여 다른 파일에서 사용할 수 없게 만들 수 있다.  

```cpp
// main.c
static int i = 1; // defining declaration

// second.c
int i = 2; // defining declaration

// third.c
extern int i; // referencing delcaration
```  

main.c에서 정의된 i는 다른 파일에서 사용할 수 없다. third.c 파일에서 i는 second.c 파일에 있는 i를 불러온다.  

### 12.10 변수의 저장 공간 분류 요약 정리  

### 12.11 함수의 저장 공간 분류  

함수는 기본적으로 external, 외부 파일에서 읽을 수 있다. static으로 설정하기도 가능하다.  

```cpp
// main.c
extern void first();
extern void second();

int main()
{
    first();
    second();
}

void first()
{
    printf("Function first in main.c \n");
}

// Linking.c
static void utility() // Only validates on this file
{
    // ...
}

void second()
{
    utility();
    printf("Function second in linking.c \n");
}
```  

> Ouput  
function first in main.c  
function second in linking.c  

위 예제에서 보듯, 함수의 prototype은 기본적으로 extern, referencing declaration이다. extern keyword는 대부분 생략하여 사용한다.  

이 때 second() 함수에 static keyword를 붙이면 main.c에서 사용할 수 없기 때문에 Error가 생긴다.  

외부에서 사용할 일이 없는 utility 함수를 static으로 설정하여 사용해보자.  

함수의 prototype에서 static을 넣었다면 함수의 body에는 생략가능하다. 반대도 가능하도 둘 다 넣을 수 있다.  

### 12.12 난수 생성기 모듈 만들기 예제  

`STD library`  

```cpp
#include <stdio.h>
#include <time.h> // -> time()
#include <stdlib.h> // -> srand()

int main()
{
    srand( (unsigned int)time(0) ); // random seed
    for(int i = 0 ; i < 10 ; ++i)
        printf("%d\n" rand());
}
```  

`모듈 만들어보기`  

```cpp
// main.c
#include <stdio.h>
int main()
{
    my_srand( (unsigned int)time(0) );
    for(int i = 0 ; i < 10 ; ++i)
        printf("%d \n", my_rand());
}

// my_rand.h
int my_rand();
void my_srand(unsigned int s);

// my_rand.c
static unsigned int seed = 1;
int my_rand()
{
    seed = seed * 123456789 + 12345;
    seed = (unsigned int)(seed / 7) % 99999;
    
    return (int)seed;
}

void my_srand(unsigned int s)
{
    seed = s;
}
```

### 12.13 메모리 동적 할당  

컴파일 타임이 아닌 런타임에 저장공간이 결정되기 때문에 동적(Dynamic)이라는 말을 사용한다.  

![TBC](https://user-images.githubusercontent.com/37467408/210091111-06af05b4-b16a-47ab-b336-555fa59f443b.PNG)  

- 동적할당을 이용한 메모리는 Identifier가 없다.  
- 운영체제에서 저장 공간을 요청하면 운영체제가 포인터를 반환하는 식으로 작동한다.  
- 한 번 저장공간을 할당받으면 space와 관계없이 계속해서 저장공간을 사용한다.  

```cpp
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n = 0;
    
    // n from files, internet, scanf, etc.
    char* arr = (char*)malloc(sizeof(char) * n);
    
    // ...
    free(arr);
    return 0;
}
```  

![TBC](https://user-images.githubusercontent.com/37467408/210091369-bacc0fa3-2bff-4ade-b519-727fcdd6dc37.PNG)  

```cpp
printf("Before free %p \n", ptr); // Before free 00FC0E60
free(ptr); // no actions occures when ptr is NULL
printf("After free %p \n", ptr); // After free 00FC0E60
ptr = NULL; // Optional
```  

- n 값을 외부에서 불러오는 경우에도(Constant를 사용하지 않아도) 문제없이 동적할당이 가능하다. (런타임에 결정됨)  
- malloc() 함수는 size_t 를 argument로 받고 void pointer(주소값)을 반환한다.  
- malloc() 함수가 반환한 포인터를 캐스팅하여 배열처럼 사용한다.  
- void pointer를 generic pointer라고도 부른다.  
- void pointer는 크기가 0이기 때문에 pointer 연산이 불가능하다.  

```cpp
int n = 10;
int* ptr = NULL;
ptr = (int*)malloc(sizeof(int) * n);
if(ptr == NULL)
{
    puts("Memory allocation is failed");
    exit(EXIT_FAILURE);
}
printf("Memory aloocation success! \n");

printf("Before free %p \n", ptr); // Before free 00FC0E60
free(ptr); no action ouccurs when ptr is NULL
printf("After free %p \n", ptr);    // After free 00FC0E60
// ptr = NULL; // optional
```  

- pointer가 scope 끝을 만나 사라지면 Heap 메모리는 계속하여 사용하고 있지만 값을 불러올 수 없다.  
- free() 함수를 사용하여 Heap에서 사용하는 메모리를 반환해도 ptr 값은 변함없다. NULL값을 주면 안정적으로 사용할 수 있다.  
`동적할당 메모리 사용하기`  

```cpp
if(ptr != NULL)
{
    for(int i = 0 ; i < n ; ++i)
        printf("%i ", ptr[i]);
    printf("\n");
    
    for(int i = 0 ; i < n ; ++i)
        *(ptr + i) = (int)i;
        
    for(int i = 0 ; i < n ; ++i)
        printf("%i ", ptr[i]);
    printf("\n");
}
```  

> Output  
> -572662307 -572662307 -572662307 -572662307 -572662307 -572662307 -572662307 -572662307 -572662307 -572662307  
> 0 1 2 3 4 5 6 7 8 9  

동적메모리 할당 직후 메모리에 쓰레기 값이 들어있다.  

`VLA vs Dynamic Memory Allocation`  

![TBC](https://user-images.githubusercontent.com/37467408/210092159-d8ff5471-0c05-4175-8c4a-e54111494825.PNG)  

큰 저장공간이 필요하다면 64bit 환경에서 동적메모리를 할당 받자.  

### 12.14 메모리 누수와 free()의 중요성  

`Visual Studio Diagnostic Tools 사용하기`  

```cpp
printf("Dummy Output\n");
{
    int n = 100000000;
    int* ptr = (int*)malloc(sizeof(int) * n);
    if(!ptr)
    {
        printf("Malloc() Failed\n");
        exit(EXIT_FAILURE);
    }
    for(int i = 0 ; i < n; ++i)
        ptr[i] = i + 1;
}
printf("Dummy Output\n");
``` 

위 코드를 디버깅 모드에서 실행해보자.  

![TBC](https://user-images.githubusercontent.com/37467408/210092379-562ec2d6-cf9f-4ff9-ba80-d8fbbfa16ab5.PNG)  

프로그램에 진입하면 Heap Profiling - Take Snapshot를 클릭하여 Heap 사용량을 확인해보자.  

```cpp
int* ptr = (int*)malloc(sizeof(int) * n);
```  

위 코드에 진입하고 나서 snap shot를 다시 클릭해보자.  

![TBC](https://user-images.githubusercontent.com/37467408/210092472-97911445-2f26-4183-ba85-016c07c34117.PNG)  

두 번째 Dummy Output이 출력되었을 때 Take Snapshot을 사용해보자. Ptr은 소멸하지만 Heap 메모리공간은 변화가 없다.  

![TBC](https://user-images.githubusercontent.com/37467408/210092549-0efa1d3f-fcd9-44ef-addc-9dab05f028d3.PNG)  

따라서 동적 메모리를 활용할 때는 free() 함수를 이용해주어 더 이상 사용하지 않은 Heap 저장공간을 반환하자.  

덧붙여 ptr = NULL을 해주는 것이 좋다.  

![TBC](https://user-images.githubusercontent.com/37467408/210092610-1dcbad81-a44c-41ae-9ebb-5e91989cc295.PNG)  

```cpp
printf("Dummy Output\n");
{
    int n = 100000000;
    int* ptr = (int*)malloc(sizeof(int) * n);
    if(!ptr)
    {
        printf("Malloc() Failed\n");
        exit(EXIT_FAILURE);
    }
    for(int i = 0 ; i < n ; ++i)
        ptr[i] = i + 1;
    free(ptr); // Preventinf memory leak
    ptr = NULL; // for safety
}
printf("Dummy Output\n");
```  

`free() 함수는 해제해야 하는 저장공간이 얼마인지 알고 있을까?`  

malloc으로 메모리를 동적으로 할당받으면 malloc은 그 힙 메모리의 주소를 반환한다. 운영체제는 이 때 해당 주소에 할당한 힙 메모리의 크기를 내부적으로 저장하고 있다가 free() 함수를 사용할 때 해당 메모리를 반환하는데 쓴다.  

### 12.15 동적 할당 메모리를 배열처럼 사용하기  

`단일 변수, 1차원 배열, 2차원 배열`  

```cpp
/* One Variable */
int* ptr = NULL;
ptr = (int*)malloc(sizeof(int) * 1);

if(!ptr)
    exit(EXIT_FAILURE);
    
*ptr = -111;
printf("%i \n", *ptr);
free(ptr);
ptr = NULL;

/* 1D Array */
int n = 3;
ptr = (int*)malloc(sizeof(int) * n);

if(!ptr)
    exit(EXIT_FAILURE);
    
ptr[0] = 100;
*(ptr + 1) = 200;
*(ptr + 2) = 300;

for(int i = 0 ; i < 3 ; ++i)
    printf("%d %p \n", *(ptr + i), ptr + i);
printf("\n");
free(ptr);
ptr = NULL;

/* 2D Array */
int row = 3, col = 2;
int (*ptr2d)[2] = (int(*)[2])malloc(sizeof(int) * row * col); pointer to array has 2 elem
//int(*ptr2d)[col] = (int(*)[col])malloc(sizeof(int) * row * col); // VLA is blocked on vs

for(int r = 0 ; r < row ; r++)
    for(int c = 0 ; c < col ; c++)
        ptr2d[r][c] = c + col * r;
        
for(int r = 0 ; r < row ; r++)
{
    for(int c = 0 ; c < col ; c++)
        printf("%d ", ptr2d[r][c]);
    printf("\n");
}
```  

`1차원배열로 2차원 배열 표현하기`  

![TBC](https://user-images.githubusercontent.com/37467408/210093715-17d41077-cfe4-40d8-918e-a96c05d406d8.PNG)  

```cpp
row = 3, col = 2;
int* ptr_cv = (int*)malloc(row * col * sizeof(int));

if(!ptr_cv)
    exit(1);
    
for(int r = 0 ; r < row ; r++)
{
    for(int c = 0 ; c < col ; c++)
    {
        ptr_cv[c + col * r] = c + col * r;
        // *(ptr_cv + r * col + c) = c + col * r;
    }
}

for(int r = 0 ; r < row ; r++)
{
    for(int c = 0 ; c < col ; c++)
    {
        printf("%d ", ptr_cv[r * col + c]);
        //printf("%d ", *(ptr_cv + r * col + c));
    }
    printf("\n");
}
```  

`1차원 배열로 3차원 배열 표한하기`  

```cpp
/* Using 1D Arrays as 3D Arrays */
int depth = 2;
row = 3, col = 2;
int* ptr3d = (int*)malloc(sizeof(int) * row * col * depth);

if (!ptr3d)
    exit(0);

for (int d = 0; d < depth; d++)
{
    for (int r = 0; r < row; r++)
    {
        for (int c = 0; c < col; c++)
        {
            ptr3d[d * row * col + r * col + c] = d * row * col + r * col + c;
            //*(ptr3d + d * row * col + r * col + c) = d * row * col + r * col + c;
        }
    }
}

for (int d = 0; d < depth; d++)
{
    for (int r = 0; r < row; r++)
    {
        for (int c = 0; c < col; c++)
        {
            printf("%d ", ptr3d[d * row * col + r * col + c] );
            //printf("%d ", * (ptr3d + d * row * col + r * col + c) );
        }
        printf("\n");
    }
    printf("==========================\n");
}
```  

`1차원으로 2차원, 3차원 배열을 만들 때 Index가 길어지면 보조 함수를 사용할 수 있다.`  

```cpp
int idx2(int c, int r)
{
    return c + col * r;
}

int idx3(int c, int r, int d)
{
    static const int cr = col * row; // Save your calculating time
    return c + col * r + cr * d;
}
```  

- static 변수를 사용하면 초기화 된 이후로 연산되지 않으므로 리소스를 아낀다.  
- C에서 static 변수 값에 항상 constant 상수를 넣어주어야 한다. 따라서 위의 col, row는 define macro를 이용하자.  
- C++은 static 변수 값에 다른 변수를 넣어줄 수 있다.  

`참고`  
포인터 + 1 연산은 포인터가 가리키는 주소에서 해당 포인터의 자료형의 크기 만큼 주소를 이동한다.  

### 12.16 calloc(), realloc()  

`calloc() 함수`  

```cpp
int n = 10;
int* ptr = NULL;

ptr = (int*)calloc(n * sizeof(int));

if(!ptr)
    exit(1);
    
for(int i = 0 ; i < n ; ++i)
    printf("%d ", ptr[i]); // >> 0 0 0 0 0 0 0 0 0 0
printf("\n");
```  

- calloc() 함수는 값을 초기화해준다. 효율적으로 프로그램을 사용하기 위해 초기화를 하고 싶지 않다면 malloc을 쓰자.  

`realloc() 함수`  

```cpp
n = 15;
int* ptr2 = NULL;
ptr2 = (int*)realloc(ptr, n * sizeof(int));

if(!ptr2)
    exit(1);
else
    ptr = NULL;
    
printf("ptr : %p, ptr2 : %p \n", ptr, ptr2);
for(int i = 0 ; i < n ; ++i)
    printf("%d ", *(ptr + i));
printf("\n");

free(ptr2);
ptr2 = NULL;
```  

`void* realloc(void* _Block, size_t _Size)`  

- 변경 후, 할당받은 메모리가 더 크면 기존의 값을 복사하여준다. 나머지 값은 초기화되지 않는다.  
- 변경 전 포인터가 NULL일 경우 malloc() 함수처럼 동작한다.  
- size가 0이면 free() 함수처럼 동작한다.  
- 실패할 경우 NULL을 반환한다.  

### 12.17 동적 할당 메모리와 저장 공간 분류  

Global Variable는 Data Segment에 저장되어 있다.  

Allocated memory 주소는 Heap의 위치처점 중간즘에 위치한다.  

### 12.18 자료형 한정자들 const, volatile, restrict  

Type Qualified Types : Const, Volatile, Restrict, _Atomic  

`Const`  

```cpp
/* idempotent : Possible to add duplicates */  
const const const int n = 11;
typedef const int zip;
const zip q = 8; // == const const zip

// const int i; // Error not initialized!
// i = 111      // const variable must be initialized

int i1 = 1, i2 = 2;
const int* ptr_i1 = &i1; // const int* == int const*
ptr_i1 = &i2; // Allowed
// *ptr_i1 = -1; // Error!

int* const ptr_i2 = &i1;
//ptr_i2 = &i2;  // Error!
*ptr_i2 = -1;    // Allowed!

int const* const ptr_i3 = &i1;
//ptr_i3 = &i2;  // Error!
//*ptr_i3 = -1;  // Error!
```  

`Global constant used in many file`  

```cpp
// constants.h
static const double PI = 3.141592;
static const double GRAVITY = 9.8;

// main.c
#include "constants.h"

double area = PI * 2.0 * 2.0;
``` 

여러 파일에서 사용하는 전역변수는 main 파일에 넣기보다 별도 헤더파일을 만들어서 관리해주자. 매 번 extern keyword를 사용하기 번거롭다.  

`Volatile`  

프로그램에서 의도한 바가 없어도 값이 바뀔 수 있다.  

예를 들면 OS에서 시간 값을 해당 변수로 가져올 경우다. 컴파일러에게 값이 의도하지 않게 바뀔 수 있으니 최적화 대상이 아니라고 알려준다.  

```cpp
volatile int v1 = 1;
volatile int* pv1 = &v1;

int i1 = v1;

// ...

int i2 = v1;
```  

컴파일러가 v1값을 복사하여 i1에 넣어주었다. 그 이후 i2에도 v1 값을 복사하여 넣어준다. 최적화가 가능하다면 v1값을 복사하지 않고 캐쉬에 저장된 값을 그대로 사용할 수 있다.  

volatile은 해당 작업을 막아준다. 임베디드 프로그램에 주로 쓰인다.  

`restrict`  

포인터를 사용하면 같은 주소값을 다양한 방법으로 접근할 수 있다. 여러 방법으로 같은 주소값에 접근하여 값을 변경하면 비효율적이다. 해당 저장공간(Object)에 접근하는 방법을 하나로 제한할 때 사용한다. 이 때 컴파일러에게 특정 동작을 유도하지는 않는다. 프로그래머가 해당 변수로 접근하는 포인터를 만들지 않겠다고 알리는 기능이다.  

```cpp
int arr[5] = { 0, 1, 2, 3, 4 };
int* ptr = arr;

arr[0] += 10; // Compiler treat these differently
ptr[0] += 100;
arr[0] += 110; // is much efficient!

/* ues _restrict for optimization */
int* __restrict rest_arr = (int*)calloc(5 * sizeof(int));
if(!rest_arr)
    exit(1);
rest_arr[0] += 10;
rest_arr[0] += 100;
// rest_arr[0] += 110; // no diff
```

### 12.19 멀티 쓰레딩  

![TBC](https://user-images.githubusercontent.com/37467408/210097689-ee144730-4510-4dd4-8d29-9070c15a79bd.PNG)  

`_Atomic`  

전역 변수를 서로 다른 Thread가 사용할 때 문제가 생길 수 있다. CPU 내부 레지스터에 값을 등록해두었다가 값을 사용하거나 계산하여 메모리에 저장한다. CPU 작업 중 다른 Thread가 전역변수의 값을 바꾸어버리면 의도한대로 업데이트 되지 않은 값을 사용할 수 있다. 이를 방지하는 type qualifier는 _Atomic이다. 단점은 느리다.  

C언어적으로 multithread를 지원하지 않는다. os에서 지원하는 library를 사용해야 한다.  

`gcc`  

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>  // sleep()
#include <pthread.h> // multithreading
#include <stdatomic.h>

_Atomic int acnt = 0; // atomic type qualifier (C11)

/* A normal C Function is executed as thread
   when name of it is specified in pthread_create() */
void *myThread(void *vargp)
{
    int n = 1; // thread storage duration
    for(int i=0; i<3; ++i)
    {
        sleep(1);
        acnt += n;
        printf("Printing from Thread %d %p \n", acnt, &n);
    }
    return NULL;
}

int main()
{
    pthread_t thread_id1, thread_id2;
    
    printf("Before Thread\n");
    
    pthread_create(&thread_id1, NULL, myThread, NULL);
    pthread_create(&thread_id2, NULL, myThread, NULL);
    
    // If you didn't wait the other, the program will be end during multithreading
    pthread_join(thread_id1, NULL); // waiting until the other 
    pthread_join(thread_id2, NULL); // thread's process ends
    
    printf("After Thread\n");
    printf("Atomic %d   \n", acnt);
    
    return 0;
}
/* 
Compile cmd
  $gcc <file-name.c> -o <output-file-name> -lpthread
Run cmd
  $./<output-file-name>
*/
```  

- pthread_create() 함수가 실행되면 첫 argument에 해당 thread 주소를 삽입한다.  
- 각 Thread 내에 있는 변수는 automatic duration이다.  

`Windows`  
windows.h에서 _atomic type qualifier를 지원하지 않는다.  

```cpp
#include <windows.h>

//_Atomic int acnt = 0; // NA

DWORD WINAPI* myThread(void* vargp)
{
    int n = 1; // thread storage duration
    for (int i = 0; i < 3; ++i)
    {
        sleep(1000);
        //acnt += n; // NA
        printf("Printing from Thread \n");
    }
    return 0;
}

int main()
{
    HANDLE thread = CreateThread(NULL, 0, myThread, NULL, 0, NULL);
    // Executing multithread version of myThread( )

    if (thread)
        WaitForSingleObject(thread, INFINITE);
    // Waiting till end
    return 0;
}
```  

multithreading 중 동시에 한 변수를 접근하지 못하게 막을 수 있다. 여기서 다루지는 않는다.  


**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊