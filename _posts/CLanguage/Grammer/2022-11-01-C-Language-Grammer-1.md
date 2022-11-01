---
title: "문자 입출력과 유효성 검증"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-11-01
last_modified_at: 2022-11-01
---

## 8. 문자 입출력과 유효성 검증  

### 8.1 입출력 버퍼 

buffer을 사용하지 않는 입력  

```cpp
#include<conio.h> // Only Windows, _getch(), _getche()

int main()
{
    char c;
    
    while( c = _getche() != '.') // 'e' for echo
        putchar(c)
}
```

> Output  
> hheelllloo  wwoorrlldd  


buffer을 사용하지 않으면 위와 같이 출력된다. 위 코드는 아래의 내용을 구현한 것이다.  

![IMG_0018](https://user-images.githubusercontent.com/37467408/199147657-29d9d603-483c-4819-8ffa-d732510e0fff.PNG)  

### 8.2 파일의 끝  

```cpp
char c;

while(1)
{
    c = getchar();
    printf("%d\n", c);
    if(c == EOF)
        break;
}
```  

위 코드를 실행한 이후 "Ctrl+Z" 단축어를 입력하면 프로그램이 종료된다. 즉, "Ctrl+Z" 단축어가 EOF이고 End Of File의 약어이다.  

EOF는 값 -1로 입력이 끝났음을 표시한다. 사용자가 컴퓨터의 정보를 주고받는 Stream 개념에서 EOF는 언제 데이터 입력이 끊났는지 알려줄 수 있다.  

### 8.3 입출력 방향 재지정  

`Input/Output Redirection`

```commandline
/* 입력값을 txt 파일에서 받아온다. */
D://{FileDirectory}//builded.exe < input.txt

/* 결과값을 지정한 txt 파일에 저장한다. */
D://{FileDirectory}//builded.exe > output.txt

/* input.txt 파일에서 입력값을 받아오고 결과값을 output.txt에 저장한다. */
D://{FileDirectory}//builded.exe < input.txt > output.txt

/* 결과값을 기존에 있던 txt 파일 뒤에더 덧붙힌다. */
D://{FileDirectory}//builded.exe >> output.txt
```  

`Copy and PipeLine`  

```commandline
/* Copy File */
copy builded.exe copied.exe

/* result of first execution will be inserted as input at second execution */
copied.exe | builded.exe
```  

copied.exe 의 output을 PipeLine을 통해 builded.exe 의 input으로 넣어준다.  

### 8.4 사용자의 인터페이스는 친절하게  

```cpp
int count = 0;

while(1)
{
    printf("Current count is %d. Continue? (y/n)\n", count);
    
    int c = getchar();
    
    // 첫 번째 문자만 읽는다.
    if(c == 'n')
        break;
    else if(c == 'y')
        count++;
    else
        printf("Please input y or n\n");

    // 첫 번째 문자만 남기고 전부 지우도록 한다.
    while(getchar() != '\n')
        continue;
}
```  

첫 getchar() 함수는 입력값을 여러 개를 받아 buffer에 저장하고, 맨 앞의 문자만 읽는다.  

두 번째 while 문에서는 줄 바꾸기('\n') 이 될 때까지 문자를 하나씩 다 읽어서 버퍼에서 지우도록 한다.  

### 8.5 숫자와 문자를 섞어서 입력받기  

```cpp
void printRepeatly(char c, int col, int row);

int main()
{
    char c;
    int cols, rows;
    
    printf("Input one character and two intgers for rows and columns\n");
    
    while( (c = getchar()) != '\n')
    {
        scanf("%d %d", &rows, &cols);
        
        // Buffer 비우기
        while(getchar() != '\n')
            continue;
        
        printRepeatly(c, rows, cols);
        printf("Input one character and two intgers for rows and columns\n");
        printf("Press Enter to quit\n");
    }
    return 0;
}

void printRepeatly(char c, int rows, int cols)
{
    for(int i = 0 ; i < rows ; i++)
    {
        for(int j = 0 ; j < cols ; j++)
            putchar(c);
        putchar(c);
    }
}
```  

scanf() 함수는 자료형에 맞지 않는 데이터가 들어오면 종료된다. 예를 들어 scanf() 함수 내부에서 character 자료형을 입력받을 경우 '\n', 'space'는 buffer에 입력된다. 만약 scanf() 함수 내부에서 int 자료형을 입력받을 경우 '\n', 'space'는 입력되지 않고 무시된다.  

### 8.6 입력 확인하기

```cpp
long get_long(void);

int main()
{
    long number;
    
    while(1)
    {
        printf("Please input a integer between 1 and 100.\n");
        number = get_long();
        
        if(number > 1 && number < 100)
        {
            printf("OK. Thank you.\n");
            break;
        }
        else
            printf("Wrong Input. Please try again.\n");
    }
    
    printf("Your input %d is between 1 and 100. Thank you.\n", number);
    
    return 0;
}

long get_long(void)
{
    printf("Please input an integer and press enter.\n");
    
    long input;
    char c;
    
    while(scanf("%ld", &input) != 1)
    {
        printf("Your input - ");
        
        while((c = getchar()) != '\n')
            putchar(c); // input left in buffer
            
        printf(" - is not an integer. Please try again.\n");
    }
    
    printf("Your input %ld is an integer. Thank you.\n", input);
    
    return input;
}
```  

### 8.7 입력 스트림과 숫자  

```cpp
int main()
{
    char str[255];
    int i, i2;
    double d;
    
    scanf("%s %d %lf", str, &i, &i2);
    printf("%s %d %f\n", str, i, d);
    
    // or (see the difference)
    
    scanf("%s %d %d", str, &i, &i2);
    printf("%s %d %d\n", str, i, i2);
    
    // or (see the difference)
    char c;
    while((c = getchar()) != '\n')
        putchar(c)
    printf("\n");
}
```

### 8.8 메뉴 만들기 예제  

```cpp
char get_choice(void);
char get_first_char(void);
int get_integer(void);
void count(void);

int main()
{
    int user_choice;
    
    while((user_choice = get_choice()) != 'q')
    {
        switch(user_choice)
        {
        case 'a':
            printf("Avengers assemble!\n");
            break;
        case 'b':
            printf('\b'); // beep
            break;
        case 'c':
            count();
            break;
        default:
            printf("Error with %d. \n", user_choice);
            exit(1);
            break;
        }
    }
    return 0;
}

void count(void)
{
    int n, i;
    
    printf("Enter an integer:\n");
    n = get_integer();
    for(i = 1 ; i <= n ; ++i)
        printf("%d\n", i);
    while(getchar() != '\n')
        continue;
}

char get_choice(void)
{
    int user_input;
    
    printf("Enter the letter of your choice:\n");
    printf("a. avengers\tb. beep\n");
    printf("c. count\tq. quit\n");
    
    user_input = get_first_char();
    
    while((user_input < 'a' || user_input > 'c') && user_input != 'q')
    {
        printf("Please try again.\n");
        user_input = get_first_char();
    }
    return user_input;
}

char get_first_char(void)
{
    int ch;
    
    ch = getchar();
    while(getchar() != '\n')
        continue;
        
    return ch;
}

int get_integer(void)
{
    int input;
    char c;
    
    while(scanf("%d", &input) != 1)
    {
        while((c = getchar() != '\n')
            putchar(c);
        printf(" is not an integer.\nPlease try again.");
    }
    return input;
}
```  

while(getchar() != '\n') continue; 구문을 이용해서 buffer을 비워주지 않으면 다음 반복문이 시작되면서 getchar() 함수가 실행이 될 때, Buffer안의 값을 꺼내기 때문에 입력값을 받지 않는다. 

### 8.9 텍스트파일 읽기  

```cpp
int main()
{
    int c;
    FILE *file = NULL;
    char fileName[]= "test.txt";
    file = fopen(fileName, "r");
    if(file == NULL)
    {
        printf("Failed to open file.\n");
        exit(1);
    }
    while((c = getc(file)) != EOF)
        putchar(c);
    fclose(file);
}
```  

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊