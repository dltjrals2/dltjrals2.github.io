---
title: "분기(IF)"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-11-02
last_modified_at: 2022-11-02
---

## 7. 분기(IF)  

### 7.1 분기문 if  

```cpp
if(expression)
    statement;
```  

expression은 0일 때 false, 0이 아닐때 true로 간주한다.  

### 7.2 표준 입출력 함수  

```cpp
// Case 1
int ch;
do{
    ch = getchar();
    putchar(ch);
} while(ch != '\n')

// Case 2
int ch;
while( (ch = getchar()) != '\n')
{
    putchar(ch);
}
```  

Case 2 에서 '=' 연산자는 우선순위가 낮으므로 ch = getchar()를 먼저 실행시켜주기 위하여 ()로 묶어줘야 한다.  

`Filtering: Convert Numbers to Asterisks`  

```cpp
// Case 1
int ch;
while( (ch = getchar()) != '\n')
{
    // Filtering
    if(ch = 'f' || ch = 'F')
        ch = 'X';
        
    for(int i = '0' ; i < '9' ; ++i)
        if(ch == i)
            ch = '*';
    putchar(ch);
}

// Case 2
char ch;
while( (ch = getchar()) != '\n')
{
    // Filtering
    if('0' <= ch && ch <= '9')
        ch = '*';
    putchar(ch);
}
```

`소문자 대문자 바꾸기`  

```cpp
int ch;
while( (ch=getchar()) != '\n')
{
    if('a' <= ch && ch <= 'z') // a = 97, A = 65
        ch = 'a' - 'A';
    else if('A' <= ch && ch <= 'Z')
        ch += 'a' - 'A';
    putchar(ch);
}
```  

### 7.3 CTYPE.H 문자 함수  

```cpp
// #include <ctype.h>
int ch;
while( (ch = getchar()) != '\n')
{
    if(islower(ch))
        ch = toupper(ch);
    else if(isupper(ch))
        ch = tolower(ch);
        
    if(!isdigit(ch))
        ch = '*';
        
    putchar(ch);
}
```

<https://www.tutorialspoint.com/c_standard_library/ctype_h.htm>  

### 7.4 else if  

```cpp
// Case 1
if(Expression1)
    Statement;
else if(Expression2)
    Statement;
    
// Case 2
if(Expression1)
    Statement
if(!Expression1 && Expression2)
    Statement
```  

### 7.6 소수 판단 예제  

`12가 소수인지 판단하기`  

```cpp
int main()
{
    unsigned num, div;
    bool isPrime; // flag, try bool type
    
    scanf("%u", &num);
    
    /*
        TODO: Check if num is a prime number
        ex) num is 4, try num % 2, num % 3
        ex) num is 5, try num % 2, num % 3, num % 4
    */
    
    isPrime = true;
    
    for(div = 2 ; (div * div) <= num ; ++div)
    {
        if(num % div == 0)
        {
            isPrime = false;
            
            if(num == div * div)
                printf("%u is divisible by %u.\n", num, div);
            else
                printf("%u is divisible by %u and % u.\n", num, div, num/div);
        }      
    }
    
    if(isPrime)
        printf("%u is a prime number.\n", num);
    else
        printf("%u is not a prime number.\n", num);
        
    return 0;
}
```  

약수는 제곱근을 기준으로 서로 짝을 짓고 있다. 이러한 성질을 이용해서 해당 숫자가 소수인지를 판단하면 유용하다.  

2부터 그 수의 제곱근까지만 검사해보면 된다. 하나라도 나누어떨어지면 소수가 아니다.  

### 7.7 논리연산자 Logical Operator  

```cpp
/*
    ! : not
    && : and
    || : or
*/
```

`Character Counting Example`  

```cpp
char ch;
int count = 0;

while((ch = getchar()) != '.')
{
    if(ch != '\n' && ch != ' ')
        count++;
}
printf("%d", count);
```  

getchar() 함수는 줄바꿈('\n')도 문자로 인식하여 "A" Enter "B" Enter "C""."을 입력하면 count = 5가 된다.  

그래서 if 조건문을 통해 Filtering을 해준다.  

`ISO164.h`  

```cpp
#include <iso646.h>
#define not !
#define and &&
#define or ||

bool test = 3 > 2 or 5 == 4;
bool test2 = 3 > 2 || 5 == 4;
```

ISO646.h 파일을 사용하면 Operator 대신에 or, and, not을 사용할 수 있다.  

우선순의 '!' > '&&' > '||'  

`Short Circuit Evalutaion`  

```cpp
int temp = (1 + 2) * (3 + 4);
printf("Before: %d\n", temp); // -> 21

if(temp == 0 && (++temp == 1024))
{
    // Do nothing
}
    
printf("After: %d\n", temp); // -> 21
```  

위 코드의 출력값은 둘 다 21이다. '&&' Operator 앞에 있는 Expression이 False 라면 '&&' Operator 뒤에 있는 Expression은 실행되지 않고 넘어간다.  

만약, Operator 앞의 값이 True라면, 두 번째 출력 값은 22가 됨을 예상할 수 있다.  

'&&', '||'은 Sequence Point 이다.  

```cpp
int x = 0, y = 1;
if (x++ > 0 && x + y == 2)
{
    printf("In if statement x=%d\n", x); // >> In if statement x=1
}
printf("x=%d y=%d \n", x, y);            // >> x=1 y=1
```

'&&' Operator가 Sequence Point이기 때문에, x++ > 0 Expression이 먼저 계산되고 뒤에 있는 Expression을 계산한다.  

`Comparison Operator의 잘못된 사용`  

```cpp
for(int i = 0 ; i < 100 ; i++)
{
    if(10 <= i <= 20) // if((10 <= i) <= 20)
        printf("%d ", i);
}
```

Expression (10<=i)의 값은 true 또는 false이다. 숫자로 나타내면 0과 1이다.  

0과 1은 20보다 항상 작으므로 if 조건문이 항상 true를 얻게 되어, 위와 같은 표현은 의도한 바와 다른 잘못된 사용 방식이다.  

### 7.8 단어 세기 예제  

```cpp
#include <ctype.h>

int main()
{
    char ch;
    int n_chars = 0; // Number of non-space characters
    int n_lines = 0;
    int n_words = 0;
    
    // 첫 라인이 n_line++를 실행시켜줄 수 있다.
    // isspace() 함수는 띄어쓰기, 줄바꾸기를 모두 Count
    // 그래서 라인바꿔주기를 따로 확인해주어야 한다.
    bool line_flag = false;
    
    // 첫 문자에 n_word++를 실행시킬 수 있다.
    // 공백 or 줄바꾸기 이후 Count를 늘린다.
    bool word_flag = false;
    
    printf("Enter text : \n");
    while((ch = getchar()) != '.')
    {
        if(!isspace(ch))
            n_chars++;
        
        if(!isspace(ch) && !line_flag)
        {
            n_lines++;
            line_flag = true;
        }
        
        if(ch = '\n')
            line_flag = false;
        
        if(!isspace(ch) && !word_flag)
        {
            n_words++;
            word_flag = true;
        }
        
        if(isspace(ch))
            word_flag = false;
    }
    
    printf("n_chars=%d,n_words=%d,  n_lines=%d", n_chars, n_words, n_lines);
    return 0;
}
```

### 7.9 조건 연산자  

```cpp
int number;
scanf("%d", &number);

// Const + Thenary Operator
const bool isEven = (number % 2 == 0) ? true : false;
```

'const' Operator을 통한 뒤에서 변수에 데이터 할당 불가, 'Thenary' Operator을 통한 선언

```cpp
// Thenory Operator with Function
(number % 2 == 0) ? printf("Even") : printf("Odd");

// Thenory Operator in flag, using it in If statement
bool isEven = (number % 2 == 0) ? true : false;
if(isEven)
    printf("Even");
else
    printf("Odd");
```

위와 같이 'Thenory' Operator을 이용하여 구현할 수 있지만, if 조건을 사용해 보기 쉽게 구현하는 것이 좋다.  

### 7.10 Loop 도우미 Continue와 break  

`Continue`  

```cpp
int main()
{
    for(int i = 0 ; i < 10 ; i++)
    {
        if( i == 5 )
            continue;
        printf("%d", i);
    }
}
```  

> 0 1 2 3 4 6 7 8 9  


`break`  

```cpp
int main()
{
    for(int i = 0 ; i < 10 ; i++)
    {
        if( i == 5 )
            break;
        printf("%d", i);
    }
}
```  

> 0 1 2 3 4  


`while and continue`  

```cpp
int main()
{
    int count = 0;
    while(count < 5)
    {
        int c = getchar();
        if(c == 'a')
            continue;
        putchar(c);
        count++;
    }
}
```  

`Continue as a placeholder`  

```cpp
int main()
{
    while(getchar() != '\n')
        continue;
}
```

while 반복문에서 {} body가 없을 때 이를 Continue로 대신 표기해 줄 수 있다.  

`break`  

```cpp
for(int i=0; i<10; ++i)
{
    for(int j=0; j<10; ++j)
    {
        if(j==5)
            break;
        printf("(%d,%d) ", i,j);
    }
}

/* 
    Output >>
    (0,0) (0,1) (0,2) (0,3) (0,4) (1,0) (1,1) (1,2) (1,3) (1,4) 
    (2,0) (2,1) (2,2) (2,3) (2,4) (3,0) (3,1) (3,2) (3,3) (3,4) 
    (4,0) (4,1) (4,2) (4,3) (4,4) (5,0) (5,1) (5,2) (5,3) (5,4) 
    (6,0) (6,1) (6,2) (6,3) (6,4) (7,0) (7,1) (7,2) (7,3) (7,4) 
    (8,0) (8,1) (8,2) (8,3) (8,4) (9,0) (9,1) (9,2) (9,3) (9,4)
*/
```  

break는 가장 가까운 for 반복문을 중지시킨다.  

### 7.11 최대, 최소, 평균 구하기 예제  

```cpp
#include <stdio.h>
#include <float.h>

int main()
{
    float max = -FLT_MAX;
    float min = FLT_MAX;
    float sum = 0.0f;
    float input;
    int n = 0;
    
    while(scanf("%f", &input) == 1)
    {
        if(input < 0.0f || input > 100.0f) continue;
        
        max = (input > max) ? input : max;
        min = (input < min) ? input : min;
        sum += input;
        
        n++;
    }
    
    if(n > 0)
        printf("min = %f, max = %f, ave = %f\n", min, max, sum / n);
    else
        printf("No input\n");
        
    return 0;
}
```  

### 7.12 다중 선택 Switch와 break  

Switch(Expression)에서 Expression은 정수 형태만 가능하다.  

```cpp
char ch;
while((ch = getchar()) != '.')
{
    switch(ch)
    {
    case 'a': case 'A':
        printf("type is start with A\n");
        break;
    case 'b':
    case 'B':
        printf("type is start with B\n");
        break;
    default:
        printf("Nothing\n");
    }
    
    while(getchar() != '\n')
        continue;
}
```  

while( getchar() != '\n') continue를 사용하면 첫 번째 문자값만 switch( ) 이하 코드가 실행된다.  

while( getchar() != '\n') continue를 주석처리하면 입력한 모든 문자를 대상으로 switch( )이하 코드가 실행된다.  

### 7.13 Goto  

잘 사용하지 않는 방법이다.

```cpp
read: c = getchar();
    putchar(c);
    if(c == '.') goto quit;
    goto read;
quit:
    return 0;
```

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊