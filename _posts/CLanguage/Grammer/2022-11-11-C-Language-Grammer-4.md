---
title: "배열과 포인터"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-11-11
last_modified_at: 2022-11-18
---

## 10. 배열과 포인터

### 10.1 배열과 메모리  

- 배열의 인덱스는 (해당 값의 주소 - 첫 값의 주소) / sizeof(자료형)과 같다.  
- 배열의 이름은 첫 값의 주소를 가리킨다. 따라서 Ampersand(주소 연산자) 없이도 포인터에 할당될 수 있다.  
- 배열의 이름이 첫 값의 주소를 가리키는 이유는 계산 과정에서 포인터로 형변환 되기 때문이다.  
- 배열의 이름은 L Value로 저장공간을 차지한다.  

### 10.2 배열의 기본적인 사용방법  

```cpp
int nums[3] = {0, 1, 2}; // O : Initialization
nums[3] = {10, 11, 12}; // X : Error, Unexpected behavior
```  

초기화 할때만 나열한 된 값을 배열에 할당할 수 있다. 그 외에는 불가능하다.  

<iframe src="https://tech.io/snippet-widget/FXYnM9q" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

위 결과를 실행시켜보면 &nums, nums, &nums[0]의 주소는 모두 같다.  

```cpp
// Fully Initialized
int nums0[3] = {10, 11, 12};
for(int i = 0 ; i < 3 ; ++i)
{
    printf("%d ", nums0[i]);
}
printf("\n");

// Not Initialized
int nums1[3]; // -> Trash Value
for(int i = 0 ; i < 3 ; ++i)
{
    printf("%d ", nums1[i];
}
printf("\n");

// Partially Initalized
int nums2[3] = {10}; // -> 10, 0, 0
for(int i = 0 ; i < 3 ; ++i)
{
    printf("%d ", nums2[i]);
}
printf("\n");
```  

부분적으로 값을 초기화했을 때는 나머지 값은 0이 된다.  

```cpp
// Omitting Size
int nums3[] = {10, 11, 12, 13, 14};
int count = sizeof(nums3) / sizeof(nums3[0]); // or sizeof(nums3) / sizeof(int);
for(int i = 0 ; i < count ; ++i)
    printf("%d ", nums3[i]);
printf(" size = %d \n", count);

// Designated Initializers
int nums3[] = {10, [2]=12, 13, [5]=15};
int count = sizeof(nums3) / sizeof(nums3[0]); // or sizeof(nums3) / sizeof(int);
for(int i = 0 ; i < count < ++i)
    printf("%d ", nums3[i]);
printf(" size = %d \n", count);
```  

- Designated Initializer의 출력값 >> 10 0 12 13 0 15 size = 6  
- Index를 지정하여 값을 할당할 수 있다. 값을 지정하지 않은 Index는 0으로 초기화된다.  
- Index를 지정하여 할당한 다음 값을 할당하면 Index + 1의 위치에 값을 할당한다.  

```cpp
#define Two 2
int main()
{
    // Specifying Array Size, Right Way
    int test0[Two]; // Symbolic Integer Constant
    int test1[2]; // Literal Integer Constant
    int test2[1*2];
    int test3[sizeof(int)];
    
    // Error Occured
    int test4[0];
    
    const int num = 1;
    int test5[num * 2];
    
    // Variable-Length Array
    int n = 8; // this works optionally
    int test6[n]; // MSVC Prohibiting this
}
```

### 10.3 포인터의 산술 연산  

(type*)0은 NULL을 의미한다. (type*)을 생략해서 사용하기도 한다.  

NULL Keyword를 사용함이 일반적이다.  

```cpp
int* ptr = 0;
printf("%d \n", *ptr); // ERROR!
```

- 이 때, 0은 Integer Literal이 아니다.  
- Asterisk( * ) 역참조는 에러를 발생시킨다.  

```cpp
/* char */
char* ptr = (char*)0; // or = 0;
printf("char %p %lld \n", ptr, (long long)ptr); // >> ch 00000000 0
ptr++;
printf("char %p %lld \n", ptr, (long long)ptr); // >> ch 00000001 1

/* int */
int* ptr_i = (int*)0; // or = 0;
printf("integer %p %lld \n", ptr_i, (long long)ptr_i); // >> i 00000000 0
ptr_i++;
printf("integer %p %lld \n", ptr_i, (long long)ptr_i); // >> i 00000004 4

/* double */
double* ptr_d = (double*)0 // or = 0;
printf("double %p %lld \n", ptr_d, (long long)ptr_d); // >> d 00000000 0
ptr_d++;
printf("double %p %lld \n", ptr_d, (long long)ptr_d); // >> d 00000008 8
```  

ptr++ 혹은 ptr + 1의 의미는 해당 자료형의 크기만큼 건너띈 주소를 의미한다.  

```cpp
/* void */
void* ptr_v = (double*)0; // or = 0;
printf("v %p %lld \n", ptr_v, (long long)ptr_v);
ptr_v++;
printf("v %p %lld \n", ptr_v, (long long)ptr_v);
```  

void는 크기가 0이기 때문에 ptr_v++는 컴파일 에러를 발생시킨다.  

```cpp
double arr_d[] = {10.5, 11.5, 12.5, 13.5, 14.5};
double* ptr_0 = &arr_d[0]; // or arr_d
double* ptr_1 = &arr_d[3];

printf("%p %p \n", ptr_0, ptr_1); // OK

printf("%d \n", ptr_0 + ptr_1); // X : Compile Error
```  

Pointer 끼리 +, *, / 연산은 불가능하다. 의미가 전혀 없다.  

하지만, - 연산은 가능하다.  

```cpp
printf("%d \n", ptr_0 - ptr_1); // >> -3
printf("%lld - %lld = %lld \n", (long long)ptr_1, (long long)ptr_0, (long long)(ptr_1 - ptr_0));
```  

> 17824496 - 17824472 = 3 ?  

연산 결과값은 24가 나와야 하지만 3이 나왔다. 이 때 3이 가리키는 값은 10진수 3이 아니라 3 * double(8byte)를 뜻한다. 따라서 24byte 연산 결과와 일치한다.  

이를 통하여 포인터끼리 뺄셈 값은 index의 차이. 두 주소간 거리 / 해당 자료형 크기 임을 알 수 있다.  

두 포인터의 차이는 인덱스 차이와 동일하다.  

### 10.4 포인터와 배열  

배열을 포인터에 할당하고 아래의 결과 값을 확인하자.  

<iframe src="https://tech.io/snippet-widget/0KTAbb0" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

포인터에 Index를 더하거나 빼면 배열에서 Index로 이동한 것과 동일한 주소를 가리킬 수 있다.  

배열의 이름은 Pointer 처럼 사용할 수 있다. Arr_i + 2와 Ptr + 2가 동일한 값을 가리킨다. 주의 할 점은 배열의 이름과 Pointer 가 동일하지 않다는 것이다.  

예를 들면 다음과 같다.  

```cpp
ptr++; // Ok
arr_i++ // X : Error
```  

값을 할당하고 출력해보자. 포인터 + Index, 배열 시작 + Index, 배열[Index] 모두 사용 가능하다.  

```cpp
for(int i = 0 ; i < count ; ++i)
    arr_i[i] = 10 * i;
    
printf("arr + 3 : %d, Ptr + 3 : %d, arr[3] : %d\n", *(arr_i + 3), *(ptr + 3), arr_i[3]);
printf("        : %d,         : %d, arr[3] : %d\n", *arr_i + 3, *ptr + 3, arr_i[3]);
```

> 30 30 30  
> 3 3 30  

후위연산자의 활용  

```cpp
int* ptr = arr_i;

for(int i = 0 ; i < count ; i++)
{
    printf("%d %d \n", *ptr++, arr_i[i]);
    /* 
          == printf("%d %d \n", *ptr, arr_i[i]);
          ptr++;
          == printf("%d %d \n", *(ptr + i), arr_i[i]);
    */ 
}
```  

> 0 0  
> 10 10  
> 20 20  
> 30 30  
> 40 40  

후위 연산자는 Semicolon(;)을 만난 뒤에 실행된다. postfix(후위 연산자)는 Sequence Point를 만나면 실행된다. Semicolon(;), Comma(,), Logical Operator(&&)이 있다.  

후위 연산자를 사용한 표현이 (*ptr++) 배열에 인덱스럴 넣어 접근한 표현 (arr_i[i])보다 빨랐으나 컴파일러의 발전으로 그 차이가 미미하다.  

컴퓨터 성능이 좋아짐에 따라 읽기 좋은 코드를 더 선호하게 되었다.  

### 10.5 2차원 배열과 메모리  

```cpp
int arr[2][3] = { {1, 2, 3}, {4, 5, 6} };
```  

다차원 배열의 Index는 뒤에서부터 해석한다. 위 예제는 3개짜리 배열이 2개 있다.  

![TBC](https://user-images.githubusercontent.com/37467408/202352715-4e620e81-1ad8-4c57-86e0-72d08e817218.PNG)  

위는 이해를 돕기 위한 그림이다. 실제 컴퓨터는 다음과 같이 메모리를 활용하고 있다.  

![TBC](https://user-images.githubusercontent.com/37467408/202352790-1664fa30-854b-44d1-b79d-2e94cc696b03.PNG)  

위의 원리로 아래와 같은 코드도 동작한다.  

```cpp
int arr[2][3] = {1, 2, 3, 4, 5, 6};
```  

다음도 가능하다.  

```cpp
int arr[2][3] = {0, 1, 2, 3, 4, 5};

int* ptr = &arr[0][0] // warning : int* ptr = arr;
for(int i = 0 ; i < 6 ; ++i)
    *ptr++ = 10 * i; // == *(ptr + i) = 10 * i
    
ptr = &arr[0][0];
for(int i = 0 ; i < 6 ; ++i)
    printf("%d ", *ptr++); // == printf("%d ", *(ptr + i));
```  

배열 자체 주소와 배열 첫 값의 주소가 동일하다. 이 점을 이용하여 ptr = arr 을 사용하면 Warning 표시가 나온다.  

arr은 3개짜리 배열을 가리키는 포인터로 int(*)[3] 로 표기할 수 있다. int(*)[3] -> (int*)를 변환하는데서 경고를 주고 있으나 둘다 주소값이기에 문제없이 작동한다.  

&arr[0][0]을 사용하면 깔끔하게 배열의 첫 메모리 주소를 할당할 수 있다.  

배열 자체 주소를 이용하여 값을 읽으려면 dereference를 2번 해줘야 한다. 2차원 배열은 2중 포인터로 만들었기 때문이다.  

```cpp
printf("%d \n", arr[1][1]);
printf("%d \n", *(*(arr + 1) + 1)); // O
printf("%d \n", *(arr + 1)); // X : Error
```

### 10.6 2차원 배열 연습문제  

### 10.7 배열을 함수에게 전달해주는 방법  

`함수에서 배열을 Argument로 받을 때 포인터로 받는다.`  

<iframe src="https://tech.io/snippet-widget/dJiJQbL" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

arr1은 예를 들자.  

double 자료형의 배열이고 4개를 넣을 수 있으므로 8byte * 4 = 32가 나와야 한다. main에서 실행시켰을 때 32가 나왔다.  

함수 내부에서 호출된 사이즈 출력 함수는 배열의 개수와 관계없이 8byte 로 나온다. 포인터의 메모리 사이즈이다.  

함수가 배열을 Argument로 전달할 경우, 포인터로 받는 것을 알 수 있다.  

`Average 함수 구현 예제`  

average 함수에서 Parameter는 arr[] 배열로 구현되어 있다.  

실제로 포인터로 받지만 배열로 표시하였을 때 장점이 있을까?  

사용자가 입력할 때 배열을 넣어 동작하는 함수라 알릴 수 있다.  

```cpp
double average(double [], int n);
double average(double *, int n);

int main()
{
    double arr1[] = {0, 1, 2, 4};
    double arr2[] = {0, 1, 2, 3, 4, 6};
    
    printf("Out side, Size of Array1 : %zd\n", sizeof(arr1));
    printf("Out side, Size of Array2 : %zd\n", sizeof(arr2));
    
    printf("arr1's avg = %f\n", average(arr1, sizeof(arr1) / sizeof(arr1[0])));
    printf("arr2's avg = %f\n", average(arr2, sizeof(arr2) / sizeof(arr2[0])));
}

// arr[] is pointer, the first address of value
double average(double arr[], int n) // so it needs array's size
{
    printf("In Function, Size of Array : %zd\n", sizeof(arr));
    double avg = 0.0;
    for(int i = 0 ; i < n ; ++i)
        avg += arr[i];
    return avg / (double)n; // still works well without castings, but...
}

// Same as above
double average(double *arr, int n)
{
}
```

### 10.8 두 개의 포인터로 배열을 함수에게 전달해주는 방법  

C++ Iterator 가 아래와 같은 방식으로 동작한다.  

```cpp
double average(double *, double *);

int main()
{
    double arr1[] = {0, 1, 2, 3, 4};
    
    printf("Size of Array1 : %zd\n", sizeof(arr1));
    
    // arr1 + 4(multiply 4byte = int)
    printf("avg = %f\n", average(arr1, arr1 + 4));
}

double average(double *start, double *end)
{
    // minus operator btw pointer indicate index diff
    int count = end - start;
    // int count = 0;
    
    double avg = 0.0;
    while(start < end)
    {
        avg += *start++;
        // count++;
    }
    return avg / (double)count; // still works well without casting, but..
}
```  

### 10.9 포인터 연산 총정리  

```cpp
int arr[] = {10, 20, 30, 40};
int *ptr1, *ptr2, *ptr3;

ptr1 = arr;
printf("ptr1 val [%p]\n", ptr1); // >> ptr1 val [010FF848]
printf("ptr1 add [%p]\n", &ptr1); // >> ptr1 add [010FF83C]
printf("ptr1 dref [%d]\n", *ptr1); // >> ptr1 dref [10]

ptr2 = &arr[0];
printf("ptr2 val [%p]\n", ptr2); // >> ptr2 val [010FF848]
printf("ptr2 add [%p]\n", &ptr2); // >> ptr2 add [010FF830]
printf("ptr2 dref [%d]\n", *ptr2); // >> ptr2 dref [10]

ptr3 = arr + 3; // Note : arr + 3 (mul 4byte)
printf("ptr3 val [%p]\n", ptr3); // >> ptr3 val [010FF854]
printf("ptr3 add [%p]\n", &ptr3); // >> ptr3 add [010FF824]
printf("ptr3 dref [%d]\n", *ptr3); // >> ptr3 dref [40]
```  

`포인터끼리 -, = 연산`  

포인터 간 거리를 printf() 함수를 통해 표시할 때 %td 형식 지정자를 사용한다.  

```cpp
// settings
int arr[] = {10, 20, 30, 40};
int* ptr1, *ptr3;
ptr1 = arr;
ptr3 = arr + 3;

/* Differencing */
printf("Diff [%td]\n\n", ptr3 - ptr1); // Note : t is for pointer diff

if(ptr1 == ptr3)
    printf("%p == %p\n\n", ptr1, ptr3);
else
    printf("%p != %p\n\n", ptr1, ptr3);
    
/* Warning : Incompatible Types */
double d = 3.14;
double *ptr_d = &d;
if(ptr1 == ptr_d)
    printf("%p == %p\n\n", ptr1, ptr_d);
else
    printf("%p != %p\n\n", ptr1, ptr_d);
    
/* Solution for Incompatible Types */
if((double*)ptr1 == ptr_d);
if(ptr1 == (int*)ptr_d);
if((void*)ptr1 == (void*)ptr_d);
```

### 10.10 const와 배열과 포인터  

`포인터로 Const 자료형 접근하기`  

```cpp
const int arr[] = {10, 20, 30};
printArray(arr, 3);

int *ptr1 = arr;
*ptr1 = -1; // Warning
printfArray(arr, 3);

ptr[1] = -2; // Warning == arr[1] = -2;
printfArray(arr, 3);
```

> { 10 20 30 }
> { -1 20 30 }
> { -1 -2 30 }  

const 자료형은 해당 이름으로 접근하여 데이터를 바꿀 수 없다.  

포인터를 새로 만들어 접근했을 때는 가능하다. Warning이 표시되지만 컴파일이 가능하고 작동에도 이상이 없다.  

이를 방지하는 방법은 포인터를 const로 만든다.  

```cpp
const int *ptr2 = arr;
*ptr2 = -1;     // Error
ptr2[1] = -2;   // Error
```  

포인터를 const로 지정하여도 주소의 위치를 옮기는 연산자는 작동한다.  

```cpp
printArray(ptr2, 3);
ptr2++; // Note : Possible ptr += 1, ptr -= 1, ++ptr, ptr--
printArray(ptr2, 2);
```  

> { 10 20 30 }  
> { 20 30 }  

주소 위치를 옮기는 연산자의 사용을 막아보자.  

```cpp
const int* const ptr3 = arr;
ptr3 += 1; // Error
```  

const를 두 번 적어주면 위 코드는 에러를 발생시켜 컴파일이 불가능하다.  

이 때 Asterisk(*) 연산자는 int 자료형 뒤에 위치함을 유의하자.  

포인터와 const를 함께 사용할 때,  

- `첫 번째 const` : 포인터 주소 값에 있는 `데이터 값`을 바꿀 수 없다.  
- `두 번째 const` : 포인터가 가리키고 있는 `주소 값`을 바꿀 수 없다.  

`상수 배열을 함수의 Argument로 넘겨 변환하기`  

const 배열(상수 배열)이지만 새로 만든 포인터로 접근하면 값을 바꿀 수 있다. C++에서는 type error가 발생한다.  

```cpp
void add_val(int *arr, int n)
{
    for(int i = 0 ; i < n ; ++i)
        *(arr + i) += 10;
}

int main()
{
    const int arr[] = {1, 2, 3};
    printArray(arr, 3);
    add_val(arr, 3);
    printArray(arr, 3);
}
```  

> { 1 2 3 }  
> { 11 12 13 }  

`const를 parameter 자료형 앞에 정의하기`  

<iframe src="https://tech.io/snippet-widget/a1DpAwT" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

const를 자료형 앞에 놓으면 arr 주소값에 있는 값을 변환시킬 수 없다.  

*arr = -100은 에러를 발생시킨다.  

하지만, arr 주소값은 변환시킬 수 있다. arr += 1을 했을 때 출력값을 보면 4byte(int)만큼 이동했다.  

함수 내부에서 생성된 arr 값은 main에서 정의된 arr과 다르다.  

함수 내 arr의 주소값은 main 내에 arr 주소값은 다르다. 따라서 main 내에 arr값에는 변화가 없다.  

`const를 Paramter 자료형 뒤에 정의하기`  

```cpp
void add_val(int * const arr, int n)
{
    // arr++; // Error!
    *arr += 2; // Warning
}

int main()
{
    const int arr[] = {1, 2, 3};
    printArray(arr, 3);
    add_val(arr, 3);
    printArray(arr, 3);
}
```  

> { 1 2 3 }  
> { 3 2 3 }  

위에서 add_val() 함수 내부에서 arr의 주소값을 바꾸면 에러가 생긴다. arr 주소값이 가리키는 데이터는 변경시킬 수 있다.  

이때 Warning 표시가 뜨지만 컴파일이 가능하다. 런타임 에러도 없다.  

### 10.11 배열 매개변수와 const  

<iframe src="https://tech.io/snippet-widget/uVbWBGx" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

### 10.12 포인터에 대한 포인터(2중 포인터)의 작동 원리  

<iframe src="https://tech.io/snippet-widget/ztevf2n" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

쉽게 생각하면 된다. Pointer의 데이터는 가르키는 주소가 들어있고, Pointer 자체도 변수기 때문에 메모리가 할당되어 있고 주소가 있다!!  

![TBC](https://user-images.githubusercontent.com/37467408/202391776-085bf335-73a9-4b7d-b047-9c2ed11ee692.PNG)  

이때 이중 포인터의 역참조를 두 번 할 때 순서를 표시하면 다음과 같다.  

```cpp
int **d_ptr = &ptr;
// Same
int *(*d_ptr) = &ptr;

**d_ptr = 20;
// Same
*(*d_ptr) = 20;
```  

### 10.13 포인터의 배열과 2차원 배열  

```cpp
int arr[2][3] = { { 0, 1, 2 }, { 3, 4, 5 } };

// (int*) parr[2]
int *p_arr[2] = { arr[0], arr[1] };
```  

![TBC](https://user-images.githubusercontent.com/37467408/202393748-3483a803-f585-4b5d-8b11-7a22b59f43cc.PNG)  

위는 이해를 돕기 위한 그림이고, 실제 메모리 구조는 다음과 같다.  

![TBC](https://user-images.githubusercontent.com/37467408/202393928-1f1878fd-fd40-4b10-9e6c-88237eb6acea.PNG)  

```cpp
int arr1[] = { 10, 11, 12 };
int arr2[] = { 13, 14, 15 };

// (int*) parr[2]
int *p_arr[2] = { arr1, arr2 };

for(int j = 0 ; j < 2 ; ++j)
{
    for(int i = 0 ; i < 3 ; ++i)
    {
        printf("%d ", p_arr[j][i]);
        printf("%d ", *(p_arr[j] + i);
        printf("%d ", *(*(p_arr + j) + i); // Useful for Dynamic Allocation
        printf("%d ", (*(p_arr + j))[i]);
    }
    printf("\n");
}
```  

이 때, p_arr 배열은 주소값을 여러개 가진다. p_arr[] = { arr1의 주소, arr2의 주소 } 이다.  

따라서 p_arr + j 는 단순히 p_arr[0]에서 + j index에 있는 값을 뜻한다. 여기서 dereference를 하면 해당 주소값으로 이동한다.  

arr1의 주소값으로 이동하면 arr1 배열에 있는 첫 인덱스 arr1[0]을 가리고 있을 것이다.  

`*((p_arr + j)[i]) vs (*(p_arr + j))[i]`  

`*(p_arr + j)[i]`와 `(*(p_arr + j))[i]`는 다르다. * 보다 [Index]가 우선순위이다. 연산자를 사용하기 전에 ()를 사용하느냐의 차이가 다른 결과값을 출력한다.  

```cpp
printf("%d ", *((p_arr + 0)[1]);
printf("%p \n", (p_arr + 0)[1]);

printf("%d ", (*(p_arr + 0))[1]);
printf("%p \n", &(*(p_arr + 0))[1]);
```  

> 13 005CF780  
> 11 005CF754  

`*((p_arr + 0)[1]) 해석`  

p_arr + 0은 p_arr 배열의 첫 값 p_arr[0]을 가리킨다. 이 때 [1]은 단순희 자료형의 크기만큼 주소를 이동하므로 p_arr + 0 = p_arr[0]  

(p_arr[0])[1] = p_arr[1] = &arr2 = arr2[0] = 13 이 된다.  

`(*(p_arr + 0))[1] 해석`  

*(p_arr + 0)은 p_arr[0] = &arr1  

*(p_arr[0]) = arr1  

(arr1[1]) = 11  

포인터는 주소 값을 가지고 자기 자신의 주소값도 등록되어 있다.  

배열은 지정한 자료형의 값을 가지나 자기 자신의 주소는 배열의 첫 값이다.  

```cpp
int *ptr = arr1;
printf("%p \n", &ptr); // 010FF9D0  ptr's 의 Address
printf("%p \n", ptr); // 010FF9A0 ptr's value
printf("%p \n", arr1); // 010FF9A0 arr1's Address
printf("%p \n", &arr1[0]); // 010FF9A0 arr1's 1st element's Address
```  

`String 예제`  

```cpp
#include <stdio.h>

int main()
{
    char* nameA[] = { "Aladdin", "Jasmine", "Magic Carpet", "Genie" };
    const int nA = sizeof(nameA) / sizeof(char*);

    for (int i = 0; i < nA; ++i)
        printf("%12s %p\n", nameA[i], &nameA[i]);
    printf("\n");


    char nameB[][16] = { "Aladdin", "Jasmine", "Magic Carpet", "Genie" };
    const int nB = sizeof(nameB) / sizeof(char[16]);

    for (int i = 0; i < nB; ++i)
        printf("%12s %p\n", nameB[i], &nameB[i]);
    printf("\n");
}
```  

> Aladdin 012FF878  
> Jasmine 012FF87C  
> Magic Carpet 012FF880  
> Genie 012FF884  
> Aladdin 012FF888  
> Jasmine 012FF898  
> Magic Carpet 012FF8A8  
> Genie 012FF8B8  

포인터 배열은 배열의 각 값간 주소 차이가 포인터의 사이즈(4byte - 32bit, 8byte - 64bit)이다.  

이중 배열에서 바깥 배열의 원소를 생각해보자.  

각 원소는 안쪽 배열 크기(위 예제에서 sizeof(char) * 16) 만큼 떨어져있다.  

아래 예제는 32bit 환경에서 실핼되었다.  

`nameA의 Memory 사용`  
ㅣ
nameA는 포인터의 배열이다. 각 값(원소)는 수고값(포인터)이며 각 값은 포인터 크기(4byte)만큼 떨어져 있다.  

해당 주소값에 가면 character 배열 값을 얻을 수 있다.  

> Charater // 포인터 주소 // 값  
> Aladdin(8) // 012FF878 // 0x00372100 (8byte)  
> Jasmine(8) // 012FF87C // 0x00372108 (8byte)  
> Magic Carpet(c) // 012FF880 // 0x00372110 (16byte-padding)  
> Genie(6) // 012FF884 // 0x00372120  

`nameB의 Memory 사용`  

nameB는 2차원 배열이다. nameA는 주소 값(포인터)만 배열 내부에 저장 되어 있고, 값은 외부에 있는 주소로 갔을 때 얻을 수 있었다.  

nameB 2차원 배열로 모든 값이 내부에 저장되어 있다.  

![TBC](https://user-images.githubusercontent.com/37467408/202493128-45b720a6-b8ca-4a4e-877b-ac396320feac.PNG)  

#### 10.13 - 1 배열의 이름과 배열의 주소  

```cpp
char array[16];

printf("array %p\t%zd\n", array, sizeof(array));
printf("array + 1 %p\n", array + 1); // Move datatype size

printf("&array %p\t%zd\n", &array, sizeof(&array));
printf("&array + 1 %p\n", &array + 1); // Move entire size of entry
```

> Output  
> array    00F3F994    16  
> array + 1    00F3995    // 95 - 94 = 0x01 size of element(char)
> &array    00F3F994    4  
> &array + 1    00F3F9A4    / A4 - 94 = 0x10 size of array  

배열의 이름은 배열이 가지고 있는 첫 값을 가리킨다.  

& 연산자를 사용한 배열은 배열 전체를 가리키고 있다. 주소에서 +1 연산의 의미는 해당 자료형만큼의 이동을 의미한다.  

array는 첫 값의 자료형만큼 이동하고 &array는 배열의 크기만큼 이동함을 확인할 수 있다.  

#### 10.13 - 2 배열의 타입  

```cpp
int a[5]; // type of a = int []
&a;       // type of &a = (int*)[5]
```

### 10.14 2차원 배열과 포인터  

![TBC](https://user-images.githubusercontent.com/37467408/202587275-af8cd0f4-e8b5-4b44-8f93-46903b36fbb6.PNG)  

arr[0] 주소와 &arr[0] 주소는 동일하다.  

배열은 자기 자신의 주소를 별도로 보관하지 않기 때문이다. 만약 arr 포인터의 배열이었다면 arr[0]. &arr[0]는 달라진다.  

<iframe src="https://tech.io/snippet-widget/xUoY124" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

`예제`  

pa는 포인터로 (double 4개를 가지는 배열)을 가리킨다. 주소 연산 pa + 1은 배열 사이즈(32byte)만큼 이동한다.  

ap는 배열로 ptr를 2개를 가지고 있다. 주소연산 ap + 1은 포인터 크기(4byte)만큼 이동한다.  

```cpp
float arr2d[2][4] = { {1.0f, 2.0f, 3.0f, 4.0f}, {5.0f, 6.0f, 7.0f, 8.0f} };

float (*pa)[4]; // 4개의 Float형 변수를 가지고 있는 배열에 대한 포인터 1개
float *ap[2]; // 원소가 2개인 포인터의 배열

printf("%zu\n", sizeof(pa)); // size = 4byte > 포인터가 1개
printf("%zu\n", sizeof(ap)); // size = 8byte > 포인터가 2개

pa = arr2d;
pa[0] = arr2d[0]; // Error : 포인터가 1개이기 때문에
pa[1] = arr2d[1]; // Error : 배열처럼 쓸 수 없다.

ap = arr2d; // Error : 포인터의 배열이기 때문에 에러
ap[0] = arr2d[0];
ar[1] = arr2d[1];
```  

`연습하기`  

```cpp
char arr[2][3] = { { 'a', 'b', 'c' }, { 'd', 'e', 'f' } };

printf("arr        %p %zd \n", arr,        sizeof(arr)        );
printf("arr[0]     %p %zd \n", arr[0],     sizeof(arr[0])     );
printf("&arr[0]    %p %zd \n", &arr[0],    sizeof(&arr[0])    );
printf("&arr[0][0] %p %zd \n", &arr[0][0], sizeof(&arr[0][0]) );
```

> Output  
> arr 008FFBF0 6 // size of entire array  
> arr[0] 008FFBF0 3 // size of char[3] array  
> &arr[0] 008FFBF0 4 // & + Elem : Pointer  
> &arr[0][0] 008FFBF0  4 // & + Elem : Pointer  
>   
> arr + 1 008FFBF3 4 // Move 3byte  
> arr[0] + 1 008FFBF1 4 // Move 1byte  
> &arr[0] + 1 008FFBF3 4 // Move 3byte  
> &arr[0][0] + 1 008FFBF1 4 // Move 1byte  

`정리`  

포인터는 자기 자신이 차지하고 있는 저장공간을 가지고 있다. 이 주소는 Identifier(식별자 or 변수명)으로 접근한다.  

이 저장공간 안에 다른 값의 저장공간을 가리키는 주소값을 담는다.  

배열은 첫 값의 주소를 자기 자신의 주소값으로 가질 뿐이다. 포인터식으로 이야기하면 자기 자신의 주소와 값이 동일하다. 신기한 점은 배열에서 & 연산자와 sizeof 연산자를 사용해보면 & Array와 Array는 다른 결과값을 출력한다.  

1. sizeof(array)는 배열 전체 공간을 출력하고, sizeof(&array)는 포인터가 차지하는 공간을 출력한다.  
2. array + 1은 배열 첫 값의 공간만큼 이동하고, &array + 1은 배열 전체 공간만큼 이동한다.  

배열을 어떻게 사용하는지를 기준으로 결과값을 예상해보면 될 것 같다. Array + i 형태는 Index로 쓰이므로 배열 첫 값의 공간만큼 이동하고, sizeof(array)는 배열의 크기를 확인하는 것이다.  

`Bracket[]이 Asterisk * 보다 우선순위가 높다`  

때문에 아래 두 선언은 다른 자료형을 가진다. 먼저 실행되는 연산자를 기준으로 해당 값의 자료형이 출력된다.  

```cpp
char (*t1)[2]; // char[2]에 대한 포인터 1개
char *t2[2]; // 원소가 2개인 포인터 배열
```  

### 10.15 포인터의 호환성  

```cpp
/* Promotion */
int i = 5;
double d;
d = i;

/* Imcompatible (int*) -> (double*) */
int* ptr_i = &i;
double* ptr_d = &d;
ptr_d = ptr_i // Warning : Imcompatible
ptr_d = (double*)ptr_i; // OK. but not recommended

/* ptr to array[n] */
char (*ptr_arr)[3]; pointer to char[3]
char arr1[2][3];
char arr2[3][2];

ptr_arr = arr1; // OK
ptr_arr = arr2; // Warning

/* Double Pointer */
char *ptr_c;
char **d_ptr = &ptr_c;
*d_ptr = arr2[0]; // ptr to char[]
d_ptr = arr[2]; // warning
                // d_ptr (char**) : a ptr to ptr to char
                // arr2 char(*)[2] : a ptr to array of 2 elem
```  

`상수 Keyword const 활용`  

오른쪽에서 왼쪽으로 읽는다.  

```cpp
int * ptr; // ptr is a pointer to int
const int * const ptr; // ptr is constant pointer to const int
const int * ptr; // ptr is pointer to int const
int * const ptr; // ptr is const pointer to int
```  

`예제`  
```cpp
/* const keyword */
int x = 20;
const int const y = 20;

int *p1 = &x;
const int *p2 = &y;

p1 = p2;
*p1 = -1;
//*p2 = 50; // Error!

printf("*p1 : %i, *p2 : %i \n", *p1, *p2);
```  

> Output  
> *p1 : -1, *p2 : -1  

`값을 const로 지정하기`  

```cpp
/* const keyword */
int x = 20;
const int y = -20;
    
int *p1 = &x;
const int *p2 = &y; 

p2 = p1;              // You can change address
*p2 = -23;          // Error! Can't change value
```  

`const ptr`  

```cpp
/* const keyword */
int x = 20;
const int y = -20; // y value is constant
    
int *p1 = &x;
int* const p2 = &y; 

*p2 = 124;             // Can change value
p2 = p1;              // Error!
```  

`ptr to ptr to const value`  

```cpp
int  z = 1;
int* p3 = &z;
const int** d_ptr = &p3; // same as below
d_ptr = &p3;            // OK 
*d_ptr = p3;           // OK
**d_ptr = 2;          // Error!
```  

`ptr to const ptr to const value`  

```cpp
const int* const* d_ptr = &p3; // same as below
d_ptr = &p3;                  // OK
*d_ptr = p3;                 // Error!
```  

`const ptr to ptr to const value`  

```cpp
const int** const d_ptr = &p3; // same as below
*d_ptr = p3;                  // OK
d_ptr = &p3;                 // Error!
```

### 10.16 다차원 배열을 함수에게 전달해주는 방법  

```cpp
/* int data[ROWS][COLS] = { { 0, 1, 2}, { 3, 4, 5 } }; */

// sum2d(data, 2);

int sum2d(int arr[][COLS], int row)
{
    int total = 0;
    for(int r = 0 ; r < row ; ++r)
        for(int c = 0 ; c < COLS ; ++c)
            total += arr[r][c];
    return total;
}

int sum2d(int (*arr)[COLS], int row)
{
    int total = 0;
    for(int r=0; r< row; ++r)
        for(int c=0; c<COLS; ++c)
            total += arr[r][c];
    return total;
}
```  

고차원 배열을 사용할 떄 첫 []는 입력해둬도 컴파일러가 기억하지 않는다.  

아래 함수는 모두 동일하다.  

```cpp
int funct(int arr[2][3][4][5], int row);
int funct(int arr[][3][4][5], int row);
int funct(int (*arr)[3][4][5], int row);
```  

### 10.17 변수로 길이를 정할 수 있는 배열  

`Variable-Length Arrays(VLAs)`  

Visual Studio에서는 지원하지 않는다.  

```cpp
int main()
{
    int n;
    scanf("%i", &n);
    
    int arr[n];
    for(int i=0; i<n; ++i)
        arr[i] = 10+i;
        
    for(int i=0; i<n; ++i)
        printf("%d ", arr[i]);
}
```  

배열의 길이가 정해진 이후에는 변경이 불가능하다.  

`함수에서 VLAs 활용`  

```cpp
int sum2d(int row, int col, int arr[row][col]);
```

함수의 Parameter에서 VLAs를 사용할 수 있다.  

### 10.18 복합 리터럴과 배열  

같은 자료형끼리 묶은 배열 Literal은 함수의 Argument 사용에 용의하다.  

포인터에도 복합 리터럴을 바로 할당할 수 있다.  

```cpp
int b[2] = {1, 2};
(int[2]) {1, 2}; // Compound Literal

int c[2] = (int[2]) {1, 2}; // Error!

int *ptr;
ptr = (int[2]){1, 2};

int (*ptr)[COLS];
ptr2 = (int[2][COLS]){ {1, 2, ,3}, {4, 5, 6} };
```

이름 없이 자료를 활용할 수 있다.

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊


