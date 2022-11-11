---
title: "함수"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-11-10
last_modified_at: 2022-11-11
---

## 9. 함수  

### 9.1 함수가 필요할 때  

### 9.2 함수의 프로토타입  

```cpp
// prototype
void print_multiple_chars();
void print_multiple_chars(char, int, bool);

int main()
{
    print_multiple_chars('*', WIDTH, true);
}

void print_multiple_chars(char c, int n_stars, bool print_newline) // formal (argument/parameter)
{
    for(int i = 0 ; i < n_stars ; ++i)
        printf("%c", c); // putchar(c)
        
    if(prine_newline)
        printf("\n");
}
```  

함수의 프로토타입에서 매개변수를 지정하지 않고 비워놓아도 상관없다. 혹은 자료형만 표기할 수도 있다.  

### 9.3 함수의 자료형과 반환값  

정수형(int) 반환 자료형은 생략 가능하다.  

반환 자료형이 없다면 정수형(int)으로 가정한다.  

반환 자료형은 무조건 명시해주는게 좋다.  

### 9.4 변수의 영영과 지역 변수  

```cpp
int int_max(int i, int j)

int main()
{
    int a;
    
    a = int_max(1, 2);
    printf("%d\n", a);
    printf("%p\n", &a);
    
    {
        int a;
        a = int_max(4, 5);
        printf("%d\n", a);
        printf("%p\n", &a);
        
        int b = 123;
    }
    // b = 456; // Error
    
    printf("%d\n", a);
    printf("%p\n", &a);
    
    return 0;
}

int int_max(int i, int j)
{
    int m;
    m = i > j ? i : j;
    return m;
}
```

### 9.5 지역 변수와 스택  

```cpp
int main()
{
    // Main Scope A
    int a = 10;
    printf("%d\n", a);
    printf("%p\n", &a);
    
    {
        // Local Scope A
        int a = 5;
        printf("%d\n", a);
        printf("%p\n", &a);
    }
    
    printf("%d\n", a);
    printf("%p\n", &a);
}
```

### 9.6 재귀 호출  

<iframe src="https://tech.io/snippet-widget/WSEPflj" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

재귀 호출을 사용할 때 주의 사항은 Stop Condition을 고려해야 한다는 것이다.  

그리고, 재귀 호출이 끝나면서 아래 printf 함수가 호출이 되면서 위 코드를 실행한 결과가 나오게 된다는 것이다. 이 내용은 9.7 재귀 호출과 스택에서 더 자세히 다루도록 하자.  

### 9.7 재귀 호출과 스택  

위와 같이 Level이 1, 2, 3, 4, 4, 3, 2, 1의 순서로 나오는 이유는 다음과 같다.  

![TBC](https://user-images.githubusercontent.com/37467408/201272116-a4332f33-ffe3-49fe-82b5-363293f6196e.PNG)  

위와 같이 처음에 Level 1 실행 되다고 재귀 호출로 Level 2가 실행이 되고 이런식으로 Level 4 까지 호출이 된다. n = 4는 stop condition에 해당하기 때문에 더 이상 재귀 호출이 되지 않고 위에서 부터 하나씩 나가면서 printf 함수가 호출돼 위와 같은 결과가 나오게 되는 것이다.  

### 9.8 팩토리얼 예제  

```cpp
/*
    Loop vs Recursion
    factorial : 3! = 3 * 2 * 1, 0! = 1
    5! = 5 * 4! = 5 * 4 * 3! = 5 * 4 * 3* 2! = 5 * 4 * 3 * 2 * 1! = 5 * 4 * 3 * 2 * 1 * 0!
*/

long loop_factorial(int n);
long recursive_factorial(int n);

int main()
{
    int num = 5;
    
    printf("%d\n", loop_factorial(num));
    printf("%d\n", recursive_factorial(num));
    
    return 0;
}

long loop_factorial(int n)
{
    long ans;
    
    for(ans = 1 ; n > 1 ; n--)
        ans *= n;
        
    return ans;
}

long recursive_factorial(int n)
{
    if(n > 0)
    {
        // TODO : return ...;
        return n * recursive_factorial(n - 1); //  tail(end) recursion
    }
    else
        return 1;
}
```

### 9.9 이진수 변환 예제  

```cpp
/*
    10
    10 / 2 = 5, remainder = 0
    5 / 2 = 2, remainder = 1
    2 / 2 = 1, remainder = 0
    1 / 2 = 0, remainder = 1
*/

void print_binary(unsigned long n);
void print_binary_loop(unsigned long n);

int main()
{
    unsigned long num = 10;
    
    print_binary_loop(num);
    print_binary(num);
    
    print("\n");
    
    return 0;
}

// Note : Printing order is reserved!
void print_binary_loop(unsigned long num);
{
    while(1)
    {
        int quotient = num / 2;
        int remainder = num % 2;
        
        printf("%d", remainder); // remainder is 0 or 1
        
        num = quotient;
        
        if(num == 0)
            break;
    }
}

void print_binary(unsigned long num)
{
    int remainder = num % 2;
    
    if(num >= 2)
        print_binary(num / 2);
        
    printf("%d", remainder);
    return;
}
```

### 9.10 피보나치 예제와 재귀 호출의 장단점  

```cpp
/*
    Fibonacci Sequence
    1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144
    
    ex) fibonacci(5) = 3 + 2 = fibonacci(4) + fibonacci(3)
    ex) fibonacci(6) = 8 = 5 + 3 = fibonacci(5) + fibonacci(4)
*/

int fibonacci(int number);

int main()
{
    for(int count = 1; count < 13; ++count)
        printf("%d ", fibonacci(count));
        
    return 0;
}

int fibonacci(int number)
{
    printf("fibonacci with %d\n", number);
    
    if(number > 2)
        return fibonacci(number - 1) + fibonacci(number - 2); // double recursion
    else
        reutnr 1;
}

// Note : Memory (in)efficiency, redundant computation
```

### 9.11 헤더 파일 만드는 방법  

Compile + Linking 과정을 생략하려면 Visual Studio를 사용하자.  

```cpp
// Print_Function.h
// #include <stdio.h> // Library is not needed
#progma once
void point_str(char* chr);

// Print_Function.c
#include <stdio.h> // Put this Library on here
#include "Print_Function.h"
void print_str(char* str)
{
    print("%s", chr)
}

// main.c
#include "Print_Function.h"
int main()
{
    char sentence[] = "Nice to meet you!";
    print_str(sentence);
}
```  

- 위 코드에서 <stdio.h> 라이브러리는 Print_Function.c에서 사용하고 있다.  

두 가지 중 하나를 선택할 수 있다. main.c에서 <stdio.h> 라이브러리를 포함시켜주거나, Print_Function.h 에서 <stdio.h>를 포함시켜주어야 한다. 따라서 Print_Function.c에서 <stdio.h>를 지우고 main.c에서 포함시켜줘도 정상적으로 동작한다.  

`main.c에서 라이브러리를 포함하는 방법을 권장한다`  

- 코드 파일 Print_Function.c에서 헤더파일 Print_Function.h을 포함시켜주는 이유  

Visual Studio가 Header에 선언되어있는 함수명과 변수명을 가져와 자동완성 기능을 지원해준다. 즉, 편리하다.  

- Prototype이 정의된 헤더만 포함시켰는데도 구현 부분이 잘 동작하는 이유  

컴파일러가 찾아서 연결시켜준다. (Visual Studio가 해준다.)  

- 함수의 정의와 선언을 구분된 파일에 보관하는 이유(헤더파일과 실행파일을 나누는 이유)  

함수는 한 번만 정의할 수 있다.(함수의 {} 바디는 하나이다.) 함수를 여러 번 정의하는 것은 오류이다.  

### 9.12 포인터의 작동 원리  

![IMG_0027](https://user-images.githubusercontent.com/37467408/201288733-131ae4c4-18cd-4018-8fca-68682c162ae7.PNG)  

![IMG_0028](https://user-images.githubusercontent.com/37467408/201288779-371106e1-9baa-40ed-8ca3-b5e63b80b56f.PNG)  

![IMG_0029](https://user-images.githubusercontent.com/37467408/201288831-5a30f430-fbde-43d9-b576-090137dd7b31.PNG)  

### 9.13 포인터의 기본적인 사용 방법  

<iframe src="https://tech.io/snippet-widget/lsxrbE8" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

### 9.14 포인터와 코딩 스타일  

```cpp
int *a, b; // a: int pointer, b: int variable
int* a, b; // a: int pointer, b: int variable
```  

위와 같이 자료 선언했을 때 a만 포인터 자료형이 된다. b는 int 자료형이다.  

```cpp
int *a, *b; // a: int pointer, b: int pointer
```  

Asterisk(*)를 꼭 붙여주어야 포인터 자료형이 된다.  

### 9.15 NULL 포인터와 런타임 에러  

```cpp
int *a_ptr = NULL;

int a;
scanf("%d", &a);

if(a > 10)
    a_ptr = &a;
    
if(a_ptr != NULL)
    printf("a_ptr is not NULL, redirect a_ptr: %d\n", *a_ptr);
```  

NULL 포인터는 주로 포인터 자료형을 사용해도 될 지 확인할 때 쓴다.

### 9.16 디버거로 메모리 들여다보기  

![Memory](https://user-images.githubusercontent.com/37467408/201291889-4b68b9b0-c37d-4474-85c9-771789ffa87d.png)  

```cpp
int a = 1;
int b = 2;
int c = 3;
int d = 4;

printf("%p\n", &a);
printf("%p\n", &b);
printf("%p\n", &c);
printf("%p\n", &d);
```  

위 printf() 함수의 출력값은 아래에 나와있는 값과 같다. 이 때 나온 값은 2자리씩 묶어서 표기한다.  

![Memory](https://user-images.githubusercontent.com/37467408/201292119-1a01c47c-b4ad-4426-86fd-e5e9e1b9ca1f.png)  

a값을 키워서 a = 257로 바꿔보자. 이 때 표시되는 값은 01 01이다.  

![Memory](https://user-images.githubusercontent.com/37467408/201292414-2fac50d0-3652-4f49-ac1d-14ccbca64565.png)  

16^1 16^0 16^3 16^2  
0    1    0    1  
     16        256  

띄어쓰기로 구분되어있는 숫자를 읽을 때는 왼쪽 -> 오른쪽으로 커진다. 띄어쓰기 없이 출력되는 숫자는 순서가 반대로 되어있어서 혼동되기 쉽다.  

![Memory](https://user-images.githubusercontent.com/37467408/201292917-2f977218-6e99-4103-8af7-12c06a0be0ab.png)  

위 그림에서 INT_MAX와 255를 HEX로 나타내고 있다. INT_MAX를 binary로 나타내면 다음과 같다.  

0111 1111 1111 1111 1111 1111 1111 1111  

맨 앞과 숫자가 7임을 알 수가 있다. 그래서 마지막 7 f(15)가 나온다.  

뒤에 있는 cc를 보자.  

MSVC Compiler가 Debug build 기능을 사용한 채로 메모리 주소를 보면 선언한 자료형보다 더 큰 크기의 공간을 사용하고 있다.  

위 예제에서 int 자료형이 사용하고 있는 공간은 32bit = 4byte이다. 그러면 나머지 뒤에 있는 cc cc cc 는 무엇일까?  

cc는 초기화되지 않은 stack space를 사용하지 않는지 확인할 때 쓰인다.  

### 9.17 포인터 변수의 크기  

x86: 4byte  
x64: 8byte  

<iframe src="https://tech.io/snippet-widget/TGkmGQH" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

아래 숫자는 byte를 나타낸다.

>variable size 	 char:1 float:4 double:8  
ptr size 		 char:8 float:8 double:8  
indirected size	 char:8 float:8 double:8  
type size 	 char:8 float:8 double:8  

### 9.18 포인터형 매개변수  

c에서는 함수 Parameter에 Ampersand(&) 사용이 불가능하다.  

```cpp
void swap(int *i, int *j)
{
    printf("%p %p\n", u, v);
    
    int temp = *u;
    *u = *v;
    *v = temp;
}

int main()
{
    int a = 123;
    int b = 456;
    
    printf("%p %p\n", &a, &b);
    swap(&a, &b);
    printf("%d %d\n", a, b);
    
    return 0;
```

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊