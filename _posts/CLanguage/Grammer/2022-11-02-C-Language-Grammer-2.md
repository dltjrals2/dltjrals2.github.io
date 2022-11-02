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


**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊