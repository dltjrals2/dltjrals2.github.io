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
last_modified_at: 2023-01-04
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

### 14.4 구조체의 배열 연습문제  

### 14.5 구조체를 다른 구조체의 멤버로 사용하기  

### 14.6 구조체와 포인터  

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