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
last_modified_at: 2022-12-29
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

### 12.4 저장 공간의 다섯 가지 분류  

### 12.5 자동 변수  

### 12.6 레지스터 변수  

### 12.7 블록 영역의 정적 변수  

### 12.8 정적 변수의 외부 연결 external linkage  

### 12.9 정적 변수의 내부 연결 internal linkage  

### 12.10 변수의 저장 공간 분류 요약 정리  

### 12.11 함수의 저장 공간 분류  

### 12.12 난수 생성기 모듈 만들기 예제  

### 12.13 메모리 동적 할당  

### 12.14 메모리 누수와 free()의 중요성  

### 12.15 동적 할당 메모리를 배열처럼 사용하기  

### 12.16 calloc(), realloc()  

### 12.17 동적 할당 메모리와 저장 공간 분류  

### 12.18 자료형 한정자들 const, volatile, restrict  

### 12.19 멀티 쓰레딩  


**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊