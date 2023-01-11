---
title: "비트 다루기"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2023-01-11
last_modified_at: 2023-01-11
---

## 15. 비트 다루기  

### 15.1 비트단위 논리 연산자  

비트단위 논리 연산자는 낱개로 사용한다  

C언어에서 거듭제곱 연산자(^)를 지원하지 않는다. ^ 연산자는 bitwist에서만 사용한다.  

`Regular Logical Operators : &&, ||, and !`  

```cpp
bool have_apple = true;
bool like_apple = true;
if(have_apple & like_apple)
    eat_apple();
```  

`Bitwise Logical Operators`  
- Bitwise NOT ~ : Tilde  
- Bitwise AND & : Ampersand  
- Bitwise OR | : Vertical bar  
- Bitwise EXCLUSIVE OR ^ : caret  

`Bit 연산자의 활용`  
- bitwise 연산을 통해 메모리 낭비를 줄일 수 있다.  

```cpp
// 8 bytes(64bit)
unsigned char has_swrod = 1;
unsigned char has_shield = 0;
unsigned char has_magic_potion = 0;
unsigned char has_shoes = 1;
unsigned char has_gun = 0;
unsigned char has_pet = 1;
unsigned char has_guntlet = 0;
unsigned char has_arrow = 0;

// 8 bits
unsigned char items = 148; // Binary 10010100
```  

`Bitwise AND &`  

```cpp
/*
    Decimal           Binary
    6 = 2^2 + 2^1     00000110
    5 = 2^2 + 2^0     00000101
    4 = 2^2           00000100
*/

unsigned char a = 6;
unsigned char b = 5;

printf("%hhu", a & b);
```  

`Bitwise OR |`  

```cpp
/*
    Decimal               Binary
    6 = 2^2 + 2^1         00000110
    5 = 2^2 + 2^0         00000101
    7 = 2^2 + 2^1 + 2^0   00000111
*/

unsigned char a = 6;
unsigned char b = 5;

printf("%hhu", a | b);
```  

`Bitwise Exclusive OR ^`  

```cpp
/*
    Decimal               Binary
    6 = 2^2 + 2^1         00000110
    5 = 2^2 + 2^0         00000101
    3 = 2^1 + 2^0         00000011
*/

unsigned char a = 6;
unsigned char b = 5;

printf("%hhu", a ^ b);
```  

`Bitwise Not ~`  

```cpp
/*
    Decimal               Binary
    6 = 2^2 + 2^1         00000110
    249                   11111001
*/

unsigned char a = 6;

printf("%hhu", ~a);
```  

### 15.2 이진수를 십진수로 바꾸기 연습문제  

```cpp
#include <math.h> // pow()

unsigned char to_decimal(const char binary_digit[]);

int main()
{
    printf("Binary (string) to Decimal conversion\n");
    
    printf("%d\n", to_decimal("00000110"));
    printf("%d\n", to_decimal("00010110"));
    
    // Note : ^ (caret) means power in math, (int) pow(2, 3) == 8
    
    printf("%d\n", to_decimal("10010100"));
    
    printf("reaching end!\n");
    return 0;
}

unsigned char to_decimal(const char binary_digit[])
{
    const size_t bits = strlen(binary_digit);
    double sum = 0;
    for(size_t s = bits ; 0! = s ; s--)
    {
        if(binary_digit[s - 1] == '1')
            sum += pow(2, bits - s);
        else if(binary_digit[s - 1] != '0')
            exit(1);
    }
    return (unsigned char)sum;
}
```  

`주의`  

```cpp
for(size_t s = bits ; 0 <= s ; s--)
```  

해당 for 반복문은 infinite loop에 빠진다. size_t의 자료형은 unsigned int 형태로 0 ~ 큰 수의 범위를 가진다. 만약 0 보다 작아지면 큰 수가 되어버려 무한한 반복문이 실행된다. for 반복문은 변화식이 적용된 후 조건식을 검사하기 때문에 size_t = 0에서 unary-- 연산자가 실행되면 UINT_MAX가 되어버린다.  

이를 해결할 수 있는 방법은 세 가지가 있다. 단 첫 번째 사례처럼 Overflow를 만드는 방법은 권하지 않는다.  

```cpp
// check whether s is UINT_MAX
for(size_t s = bits - 1 ; s != UINT_MAX ; s--) // 0
{
    if(binary_digit[s] == '1')
        sum += (int)pow(2, bits - s - 1);
    else if(binary_digit[s] != '0')
        exit(1);
}

// use index - 1
for(size_t s = bits ; S != 0 ; s--)
{
    if(binary_digit[s - 1] == '1')
        sum += (int)pow(2, bits - s);
    else if(binary_digit[s - 1] != '0')
        exit(1);
}

// reverse order with variable
size_t size = 8;
for(size_t i = 0 ; i < size ; ++i)
{
    if((unsigned char)(saved_num / pow(2, size - 1 - i)) == 1)
    {
        putchar('1');
        saved_num -= (unsigned char)pow(2, size - 1 - i);
    }
    else
        putchar('0');
}
```  

1. s를 UINT_MAX와 비교하여 overflow 되었는지 검사하기  
2. index - 1을 사용하기  
3. index를 0부터 증가시켜 사용하기  

### 15.3 &를 이용해서 십진수를 이진수로 바꾸기 연습문제  

- & 연산자를 이용하면 쉽게 십진수를 이진수로 바꿀 수 있다.  
- size_t 자료형은 Linux, Windows, x86, x64 모두 작동한다. 자주 사용해주자.  
- 아래 예제에서 print_binary 함수는 음수를 bit로 나타낼 수 없다. bitwise operator를 애용하자.  
- to_decimal 함수내 sum 변수의 자료형이 double이다. pow 함수의 반환형이 double이기 때문에 형변환이 필요하다.  

```cpp
unsigned char to_decimal(const char bi[]);
void print_binary(const unsigned char num);
void print_bitwised(const unsigned char num);

int main()
{
    unsigned char i = to_decimal("01000110");
    unsigned char mask = to_decimal("00000101");
    
    print_binary(i);
    print_binary(mask);
    print_binary(i & mask);
    
    print_bitwised(i);
    print_bitwised(mask);
    print_bitwised(i & maks);
}

unsigned char to_decimal(const char bi[])
{
    double sum = 0;
    size_t size = strlen(bi);
    for(size_t t = 0 ; t < size ; ++t)
    {
        if(bi[t] == '1')
            sum += (double)pow(2, size - 1 -t);
        else if(bi[t] != '0')
            exit(1);
    }
    return (unsigned char)sum;
}

void print_binary(const unsigned char num)
{
    printf("Input is %3hu ", num);
    unsigned char saved_num = num;
    for(size_t i = 8 ; 0 < i ; --i)
    {
        if((unsigned char)(saved_num / pow(2, i - 1)) == 1)
        {
            putchar('1');
            saved_num -= (unsigned char)pow(2, i - 1);
        }
        else
            putchar('0');
    }
    printf("\n");
}

void print_bitwised(const unsigned char num)
{
    printf("Input is %3hu ", num);
    for(size_t i = 8 ; 0 < i ; --i)
    {
        unsigned char mask = (unsigned char)pow(2, i - 1);
        if((num & mask) != 0)
            putchar('1');
        else
            putchar('0');
    }
    printf("\n");
}
```  

### 15.4 비트단위 논리 연산자 확인해보기  

`이전에 구현한 함수로 논리연산자의 결과를 출력해보자.`  

```cpp
unsigned char a = 6;
unsigned char b = 5;

printf("a = %3hhu ", a);
print_bitwised(a);

printf("b = %3hhu ", b);
print_bitwised(b);

printf("a & b = %3hhu ", a & b);
print_bitwised(a & b);

printf("a | b = %3hhu ", a | b);
print_bitwised(a | b);

printf("a ^ b = %3hhu ", a ^ b);
print_bitwised(a ^ b);

printf("~a = %3hhu ", ~a);
print_bitwised(~a);
```  

> Output  
a     =   6 00000110  
b     =   5 00000101  
a & b =   4 00000100  
a | b =   7 00000111  
a ^ b =   3 00000011  
~a    = 249 11111001  

위 print_bitwised() 함수 내에서 첫 printf() 코드를 지워주면 위와 같은 결과가 나온다.  

### 15.5 2의 보수 표현법 확인해보기  

부호크기 표현(Sign-magnitude representation)과 1의 보수법은 양수 0과 음수 0을 구분한다.  

```cpp
/*
    Signed Integers
    - Sign-magnitude representation
    00000001 is 1 and 10000001 is -1
    00000000 is +0 and 10000000 is -0
    
    - One's Complement Method
    To reverse the sign, invert each bit
    00000001 is 1 and 10000000 is -1
    11111111 is -0
    from -127 to 127
*/
```  

2의 보수법은 음수 0과 양수 0이 없고 하나만 사용한다.  

`음수 정수를 bit로 나타내기 위하여 Bitwise Operator를 사용하자`  
- Bitwise &를 사용하여 구현해야 음수값을 제대로 출력할 수 있다.  

```cpp
void print_binary(const char num)
{
    printf("norm Decimal %4i \t", num);
    char saved_num = num;
    for(size_t i = 8 ; 0 < i ; --i)
    {
        if((char)(saved_num / pow(2, i - 1)) == 1)
        {
            putchar('1');
            saved_num -= (char)pow(2, i - 1);
        }
        else
            putchar('0');
    }
    printf("\n");
}

void print_bitwised(const char num)
{
    printf("Bitwise Decimal %4i \t", num);
    for(size_t i = 8 ; 0 < i ; --i)
    {
        char mask = (char)pow(2, i - 1);
        if((num & mask) != 0)
            putchar('1');
        else
            putchar('0');
    }
    printf("\n");
}

int main()
{
    print_bitwised(127);
    print_bitwised(-127);
    print_bitwised(~127 + 1);
    
    print_binary(127);
    print_binary(-127);
    print_binary(~127 + 1);
}
```  

> Output  
Bitw Decimal  127       01111111  
Bitw Decimal -127       10000001  
Bitw Decimal -127       10000001  
norm Decimal  127       01111111  
norm Decimal -127       00000000  
norm Decimal -127       00000000  

`2의 보수법을 적용하는 과정을 출력해보자.`  

```cpp
print_bitwised(12);
print_bitwised(~12 + 1);
print_bitwised(-12);

print_bitwised(7);
print_bitwised(~7 + 1);
print_bitwised(-7);
```  

> Output
Bitw Decimal   12       00001100  
Bitw Decimal  -12       11110100  
Bitw Decimal  -12       11110100  
Bitw Decimal    7       00000111  
Bitw Decimal   -7       11111001  
Bitw Decimal   -7       11111001  

### 15.6 비트단위 쉬프트 연산자  

`비트단위 연산자를 2^n의 곱셈과 나눗셈에 활용할 수 있다.`  
- 음수일 때 Right Shift를 2^n의 나눗셈으로 사용할 수 없다.  

```cpp
/*
    Bitwise shift operators
    - Left shift
    number << n : multiply number by 2*n
    
    - Right shift
    number >> n : divide by 2^n (for non-negative numbers)
*/
```  

`Bitwise shift operator 왼쪽에 있는 변수를 binary로 변환해서 생각하자.`  
- 쉬프는 연산자는 해당 Binary 숫자의 자리를 이동시킨다. 이때 저장공간 밖으로 나가는 정보는 사라진다.  

```cpp
//                            // Left Number(optr)
// 8 bit integer cases        // 76543210             76543210        76543210
printf("%4hhd \n", 1 << 3);   // 00000001          00000001???        00001000
printf("%4hhd \n", 8 >> 1);   // 00001000             ?00001000       00000100
```  

`부호를 가지는 정수는 자리 이동을 하면 어떻게 될까?`  

`Right shift`  
- 음수 정수를 다룰 때, Right shift를 2^n의 나눗셈으로 사용할 수 없다.  
- >> operator 수행하게 되면 양수/음수에 따라 다른 값을 채운다. sign을 나타내는 bit가 1이면 1을 채우고 0이면 0을 채운다.  
- 아래 예제에서 -119는 signed이고, 137은 unsigned로 두 값의 binary 모양은 동일하다.  

```cpp
//                              // Left Number(optr)
printf("%4hhd \n", -119 >> 3);  // 10001001           ???10001001     11110001 (-15)
printf("%4hhd \n", 137 >> 3);   // 10001001           ???10001001     00010001 (17)
```  

이 떄, -119 >> 3 결과값이 -119 / 3과 다름을 주의하자.  

```cpp
printf("%4hhd \n", -119 >> 3); // -15 
printf("%4hhd \n", -119 / 8);  // -14
```  

따라서 음수일 때 비트단위 right('>>') 쉬프트 연산자는 2의 거듭제곱을 나눗셈 한 것과 다르다.  

양수일 때는 동일하다.  

```cpp
printf("%4hhd \n",  137 >> 3); // 17
printf("%4hhd \n",  137 / 8 ); // 17
```  

`Left Shift`  
- << operator 수행하게 되면 양수/음수 동일하게 0을 채운다.  

```cpp
printf("%4hhd \n",  137 << 4); //  10001001   ->     10001001???? ->  10001000 (-122)
printf("%4hhd \n", -119 << 4); //  10001001   ->     10001001???? ->  10001000 ( 144)
```  

`Pow 함수를 Bitwise operator로 구현하기`  
- Pow 함수보다 bitwise operator로 구현했을 때 더 효과적이다.  

```cpp
void int_binary(const int num)
{
    printf("Decimal %12d == Binary ", num);
    conse size_t bits = sizeof(num) * CHAR_BIT;
    for(size_t i = 0 ; i  < bits ; ++i)
    {
        const int msk = 1 << (bits - 1 - i) // instead of power
        if((num & mask) == mask)            // is much effiecent way
            putchar('1');
        else
            putchar('0');
    }
    printf("\n");
}
```  

`다음은 0xffffffff를 signed과 unsigned로 표현했을 때의 값이다.`  

```cpp
printf("unsigned int %10u \n", 0xffffffff); // unsigned int 4294967295
printf("signed int %10d \n", 0xffffffff); // signed int - 1
int_binary(0xffffffff); // Decimal - 1 == Binary 11111111111111111111111111111111
```  

`right shift >> 3`  

```cpp
printf("Right shift by 3\n");
int_binary((signed)0xffffffff >> 3); // Decimal - 1 == Binary 11111111111111111111111111111111
int_binary((unsigned)0xffffffff >> 3); // Decimal 536870911 == Binary 00011111111111111111111111111111
```  

'>> 3' 연산에서 새로 들어온 값의 초기값을 확인해보자. signed일 때와 unsigned의 음수일 때 초기값이 다르다.  

`기호 bit가 동일 할 때 right shift >> 3`  

```cpp
printf("Unsigned int %10u \n", 0x00ffffff); // Unsigned int   16777215
printf("signed   int %10d \n", 0x00ffffff); // signed   int   16777215
int_binary(  (signed)0x00ffffff >>3);//Decimal 2097151==Binary 00000000000111111111111111111111
int_binary((unsigned)0x00ffffff >>3);//Decimal 2097151==Binary 00000000000111111111111111111111
```  

결과값이 동일함을 알 수 있다. right shift 연산은 MSB(1) 즉, 음수일 때만 주의하여 사용하자.  

### 15.7 비트단위 연산자의 다양한 사용법  

아이템 착용 여부를 예로 들자. true/false 혹은 0/1이면 표현할 수 있고 1bit면 충분하다. 그러나 bool 자료형은 1byte이며 이는 컴퓨터가 자료를 주고 받는 단위이기 때문에 7bit를 낭비할 수 밖에 없다.  

```cpp
bool has_sword 	= false;
bool has_shield = false;
bool has_potion = false;
bool has_guntlet = false;

bool has_hammer = false;
bool has_key = false;
bool has_ring = false;
bool has_amulet = false;
```  

위 8개의 변수는 총 7bit * 8을 낭비하고 있다. bit mask를 사용하여 낭비를 줄이자.  

`bit mask 활용하기`  
- 1bit의 정보를 shift 연산자를 사용하여 표시할 수 있다. 16진수도 사용할 수 있다.  
- c에서 ^연산자를 사용할 수 없어 pow를 사용한다.  
- c++는 binary 숫자를 직접 넣어 표현할 수 있다.  

```cpp
//                     Shift     Decimal    Binary      Hex   Octet
#define MASK_SWORD    ( 1 << 0) // 2^0     0000 0001    0x01   01
#define MASK_SHILED   ( 1 << 1) // 2^1     0000 0010    0x02   02
#define MASK_POTION   ( 1 << 2) // 2^2     0000 0100    0x04   04
#define MASK_GUNTLET  ( 1 << 3) // 2^3     0000 1000    0x08   010
#define MASK_HAMMER   ( 1 << 4) // 2^4     0001 0000    0x10   020
#define MASK_KE       ( 1 << 5) // 2^5     0010 0000    0x20   040
#define MASK_RING     ( 1 << 6) // 2^6     0100 0000    0x40   0100
#define MASK_AMULET   ( 1 << 7) // 2^7     1000 0000    0x80   0200
```  

`위 데이터를 활용하려면 masking이 필요하다`  
- and(&) 연산자를 이용하면 mask에 표시되어 있는 자료만 가져온다.  

```cpp
/*
           flag    0101 1010
    &      mask    0000 0011
    -------------------------
    mask & falg == 0000 0010
*/
```  

`Bitmask 활용하기`  

`특정 bit를 ON(아이템 착용)`  

```cpp
printf("\nGet Swrod and Amulet\n");
flags = flags | MASK_SWORD; // flag |= MASK_SWORD // is On!
char_binary(flags);

flags |= MASK_AMULET;
char_binary(flags);
```  

`특정 bit를 OFF(아이템 버리기)`  

```cpp
printf("\nGet postion and use it\n");
flags |= MASK_POTION; // flag |= MASK_SWORD // is On!
char_binary(flags);

flags = flags & ~MASK_POTION;
char_binary(flags);
```  

`특정 bit를 ON/OFF Toggling(장비세팅 토글링)`  

```cpp
printf("\nToggling Items\n");
flags = flags ^ MASK_HAMMER;
char_binary(flags);
flags = flags ^ MASK_HAMMER;
char_binary(flags);
flags = flags ^ MASK_HAMMER;
char_binary(flags);
```  

`특정 bit 확인(아이템 소유여부 확인)`  

```cpp
printf("\n Checking the Value of a Bit \n");
if ((flags & MASK_KEY) == MASK_KEY)
    printf(" >> You can enter. \n");
else
    printf(" >> You can not enter. \n");

flags |= MASK_KEY; // Obtained a Key!

if ((flags & MASK_KEY) == MASK_KEY)
    printf(" >> You can enter. \n");
else
    printf(" >> You can not enter. \n");
```  

`Bitmaks로 Trimming(필요한만큼 잘라쓰기)`  

```cpp
int int_flag = 0xffffffff; 
// 11111111 11111111 11111111 11111111 11111111 11111111 11111111 11111111
int_binary(int_flag);

int_flag &= 0xff;
// Trim by 1111 1111 ( Cutting )
int_binary(int_flag);
```  

> Output
Decimal  -1 == Binary  :11111111111111111111111111111111  
Decimal 255 == Binary  :00000000000000000000000011111111  

### 15.8 RGBA 색상 비트 마스크 연습문제  

```cpp
#define BYTE_MASK 0xff
unsigned int rgba_color = 0x66CDAAFF;
// 4bytes, medium aqua marin (102, 205, 170, 255)

unsigned char red, green, blue, alpha;

// Use right shift >> operator
alpha = rgba_color & BYTE_MASK;
blue  = rgba_color>>8 & BYTE_MASK;
green = rgba_color>>16 & BYTE_MASK;
red   = rgba_color>>24 & BYTE_MASK;
printf("R, G, B, A = %hhu, %hhu, %hhu, %hhu \n", red, green, blue, alpha);
```  

unsigned int는 4byte 자료형이다. 4byte는 32bit이다. 2^32를 16진수로 표현하려면 (2^4)^8 이므로 8자리가 필요하다.  

이때 16진수의 한 자리수(0 ~ 16)는 4bit를 표현하고 있다. 16진수의 두 자리수는 8bit이다.  

rgba_color는 8bit 단위로 색을 구분하고 있다. 따라서 8bit를 이동해야 16진수에서 그 다음 자리수의 값을 당겨온다.  

### 15.9 구조체 안의 비트 필드  

`비트 필드 : 여러 비트를 나열하여 정보를 저장하고 있는 장소`  

![TBC](https://user-images.githubusercontent.com/37467408/211719059-b39f99a4-8dc2-4c4c-b912-c8e752ea2287.PNG)  

bitwised 연산자를 사용하지 않아도 된다. 편리하다.  

`비트필드 선언`  

```cpp
struct items
{
	bool has_sword : 1;  // number means bits to use.
	bool has_shiled : 1; 
	bool has_potion : 1;
	bool has_guntlet : 1;
	bool has_hammer : 1;
	bool has_key : 1;
	bool has_ring : 1;
	bool has_amulet : 1;
}items_flag;
```  

`각 멤버 변수가 컴퓨터 저장되는 순서 Compiler에 따라 다르다.`  
- MSVC는 먼저 선언된 멤버 변수가 늦안 자리 비트에 저장된다. 출력 함수는 편의에 따라 높은 비트를 먼저 출력해주어야 하므로, 저장된 순서를 뒤집어주면 된다.  
- 위 비트필드의 크기는 1byte이므로 unsigned char 자료형을 parameter로 사용하였다.  

```cpp
void char_to_binary(unsigned char uc)
{
    const int bits = CHAR_BIT * sizeof(unsigned char);
    for(int i = bits - 1 ; i >= 0 ; i--)
        printf("%d", (uc >> i) & 1);
}

void print_binary(char* data, int bytes) // Note : extended for any size
{
    for(int i = 0 ; i < bytes ; ++i)
        char_to_binary(data[bytes - 1 - i]);
    printf("\n");
}

int main()
{
	printf("item's object takes %zd bytes \n", sizeof(items_flag)); // >> 1byte

	items_flag.has_sword   = true; // c 99 accept to assign bool type instead of number
	items_flag.has_shiled  = 1;
	items_flag.has_potion  = 0;
	items_flag.has_guntlet = 1;
	items_flag.has_hammer  = 0;
	items_flag.has_key     = 0;
	items_flag.has_ring    = 0;
	items_flag.has_amulet  = 0;
	
	// The order of member varaible was 
	print_binary((char*)&items_flag, sizeof(items_flag));
}
```  

> Output  
item's object takes 1 bytes  
00001011  

### 15.10 비트필드의 사용방법  

`공용체를 사용하면 전체 메모리 관리를 쉽게 할 수 있다.`  
- 메모리 공간을 공유하는 변수 unsigned char 자료형 값에 0을 할당하면 간단하게 비트필드를 초기화할 수 있다.  
- 아래 1 << 4는 0000 0001을 4만큼 왼쪽으로 밀어서 0001 0000 5번째 자리에 1이 있음에 유의하자.  

```cpp
union
{
    struct items bf;
    unsigned char uc;
}uni_flag;

uni_flag.uc = 0;

uni_flag.bf.has_amulet = 1; // equip amulet
print_binary((char*)&uni_flag, sizeof(uni_flag));

uni_flag.uc |= (1 << 4);    // equip 5's member variable
print_binary((char*)&uni_flag, sizeof(uni_flag));

uni_flag.bf.has_hammer = 0; // abandaon hammer(5's member)
print_binary((char*)&uni_flag, sizeof(uni_flag));
```  

> Output  
item's object takes 1 bytes  
10000000  
10010000  
10000000  

`비트 필드 이용 사례 보기 - Windows DOS`  
- 아래 코드를 보자. unsigned int 자료형을 사용하되 5bit만 할당하고 있다.  

```cpp
struct file_time
{
    unsigned int seconds : 5; // 2^5 = 32, 0 ~ 30*2 seconds
    unsigned int minutes : 6; // 2^6 = 64, 0 ~ 60 minutes
    unsigned int hours   : 5; // 2^5 = 32, 0 ~ 23 hours
};

struct file_date
{
    unsigned int day	 : 5; // 2^5 = 32,     0 ~ 31
    unsigned int month	 : 4; // 2^4 = 16,     1 ~ 12
    unsigned int year    : 7; // 2^7 = 128,    1980 ~ 
}fd;
```  

2^5는 32가지 경우의 수를 나타낼 수 있다. 0~31의 수를 표현가능하다.  

`사용예제`  

```cpp
/* 1988 12 28 */
fd.day = 28;
fd.month = 12;
fd.year = 8;
printf("Day %u, Month %u, Year %u \n", fd.day, fd.month, fd.year);
```  

> Output  
Day 28, Month 12, Year 8  

이 때 fd.day가 5비트를 사용하므로 (0 ~ 31)의 숫자를 사용할 수 있다.  

범위 밖의 숫자를 사용하면 Overflow가 발생한다.  

```cpp
fd.day = 33;
fd.month = 18;
fd.year = 8;
printf("Day %u, Month %u, Year %u \n", fd.day, fd.month, fd.year);
```  

> Output  
Day 1, Month 2, Year 8  

`비트필드의 scanf()`  
- 이 때 scanf()로 해당 변수에 값을 입력하면 다음과 같은 에러가 발생한다. scanf()는 최소단위 1byte를 기준으로 값을 입력받기 때문에 bit를 쪼개어 사용한 비트필드 멤버변수에 값을 할당할 수 없다.  

```cpp
scanf("%d", &fd.day); // Error!
```  

### 15.11 비트필드의 패딩  

다음 코드를 실행하여 결과값을 확인해보자.  

```cpp
struct 
{
    bool option1 : 7;
		bool option2 : 1;
} bbf;
printf("%zu bytes \n", sizeof(bbf));

struct {
    unsigned short option1 : 8;
		unsigned short option2 : 7;
		unsigned short option3 : 1;
} usbf;
printf("%zu bytes \n", sizeof(usbf));

struct {
		unsigned int option1 : 1;
		unsigned int option2 : 1;
} uibf;
printf("%zu bytes \n", sizeof(uibf));
```  

> Output  
1 bytes  
2 bytes  
4 bytes  

첫 번째 비트 필드는 8bit  
두 번째 비트 필드는 16bit  
세 번째 비트 필드는 2bit를 할당하였으나 4byte 공간을 차지하고 있다.  
`비트 필드 구조체의 최소 크기는 멤버 변수의 자료형 크기이다.`  

`구조체 안에 다른 자료형을 넣으면 어떻게 될까?`  

```cpp
struct 
{
    bool option1 : 1;
    bool option2 : 1;
    unsigned long long option3 : 1;
} bbf;
printf("%zu bytes \n", sizeof(bbf));
```  

출력값은 16byte가 된다. unsigned long long은 8byte를 차지한다. 이 때 정보를 주고받는 단위가 8byte이다.  

unsigned long long을 끊기지 않고 한 번에 전송하기 위하여 앞에 있는 두 bool 자료형 변수를 메모리에 할당 이후 패딩을 넣어주었다.  

따라서, 8byte, 8byte의 저장공간을 가진다.  

`확인해보기`  

```cpp
memset( (char*)&bbf, 0xff, sizeof(bbf) );
```  

이 함수는 해당 변수를 0xff으로 초기화해주고 있다. 이 때 0xff는 1111 1111 이다.  

위 함수를 사용하여 메모리 공간에서 패딩을 어디에서 추가해주었는지 확인해보자.  

```cpp
// memset reset memory's value to argument 
memset((char*)&bbf, 0xff, sizeof(bbf));
print_binary((char*)&bbf, sizeof(bbf));
bbf.option1 = 0;
bbf.option2 = 0;
bbf.option3 = 0;
print_binary((char*)&bbf, sizeof(bbf));
printf("%zu bytes\n", sizeof(bbf));
```  

출력은 값 변경 전과 후이다.  

![TBC](https://user-images.githubusercontent.com/37467408/211723246-0ed2b9cd-68f4-459c-b0ad-439a76b90bd8.PNG)  

끝에 있는 두 0은 option1과 option2에 할당해준 0값이다.  

중간에 있는 0은 unsigned long long에 할당해준 값 0이다.  

option3에 (unsigned long long 자료형) 1을 할당하면 저 값이 1이 되지만 2를 할당하면 0이 되어버린다.  

`패딩을 확인하기 위하여 unsigned long long 변수에서 16bit를 사용해보자`  

```cpp
struct
{
    bool option1 : 1;
    bool option2 : 1;
    unsigned long long option3 : 16;
}bbf;

memset((char*)&bbf, 0x00, sizeof(bbf));
```  

다시 memset 함수를 사용하여 모든 값을 0으로 초기화해준다.  

```cpp
memset((char*)&bbf, 0x00, sizeof(bbf));
print_binary((char*)&bbf, sizeof(bbf));
bbf.option1 = 1;
bbf.option2 = 1;
bbf.option3 = 0xffff;
print_binary((char*)&bbf, sizeof(bbf));
printf("%zu bytes\n", sizeof(bbf));
```  

출력해보면, 아래와 같이 아랫 줄의 출력에 패딩이 들어가 있음을 확인할 수 있다.  

![TBC](https://user-images.githubusercontent.com/37467408/211725703-79b10e43-283c-4c35-a8b0-22a5c92704d4.PNG)  

`메모리 강제 할당하기`  
- 자료형 : 0 문법을 사용하면 해당 자료형의 크기만큼 메모리를 더 할당받을 수 있다. 하드웨어 가속 때 쓰인다.  

```cpp
struct 
{
    bool option1 : 1;
    bool         : 0; // force to allocate memory
    bool option2 : 1;
}bbf;
printf("%zu bytes\n", sizeof(bbf));

struct {
    unsigned short option1 : 1;
    unsigned short option2 : 1;
    unsigned short         : 0; // force to allocate memory
    unsigned short option3 : 1;
}usbf;
printf("%zu bytes \n", sizeof(usbf));

struct {
    unsigned int option1 : 1;
    unsigned int         : 0; // force to allocate memory        
    unsigned int option2 : 1;
}uibf;
printf("%zu bytes \n", sizeof(uibf));
```  

### 15.12 메모리 줄맞춤 alignof, alignas  

`변수나 배열의 주소 간격을 알려준다. 지정할 수도 있다. MSVC에서 지원하지 않는다.`  

```cpp
#include <stdio.h>
#include <stdalign.h> // C++ style alignas, alignof without it, add underscore '_'

int main()
{
#ifdef __clang_major__
    printf("Clang detected version %d.%d\n", __clang_major__, __clang_minor__);
#endif

#ifdef __GNUC__
    printf("Gcc detected version %d.%d\n", __GNUC__, __GNUC_MINOR__);
#endif

    printf("Alignment of char = %zu\n", alignof(char));
	  printf("alignof(float[10])= %zu\n", alignof(float[10]));
	  printf("alignof(struct{char c; int n;}) = %zu\n", alignof(struct { char c; int n }));
}
```  

> Output  
Gcc detected version 8.4  
Alignment of char = 1  
alignof(float[10])= 4  
alignof(struct{char c; int n;}) = 4  
Hello, World!  

첫 번쨰 줄 alignof(char)은 1byte이다. 줄맞춤 최소 단위가 1byte임을 의미한다.  
두 번째 줄 alignof(배열)은 4byte이다. 배열을 alignof에 넣으면 각 원소의 줄맞춤 최소 단위를 출력한다.  
세 번째 줄 alignof(구조체)는 4byte이다. 이 때 최대 크기의 구조체 멤버변수를 기준으로 줄맞춤 최소단위를 맞춘다. char은 1byte int는 4byte이므로 4byte으로 줄맞춤한다.  

`줄맞춤 된 주소 확인해보기`  
- 줄맞춤이 되었다면 해당 주소의 주소지가 자료형의 크기로 나누어진다.  

```cpp
double dx;
char ca;
char cx;
double dz;
char cb;

char cz;

//printf("char alignmnet  : %zd\n", _Alignof(char));
//printf("double alignmnet: %zd\n", _Alignof(double));
printf("char alignmnet: %zd\n", alignof(char));
printf("char alignmnet: %zd\n", alignof(double));

printf("&dx: %p %lld\n", &dx, (long long)&dx % 8);
printf("&ca: %p \n", &ca);
printf("&cx: %p \n", &cx);
printf("&dz: %p %lld\n", &dz, (long long)&dz % 8);
printf("&cb: %p \n", &cb);
printf("&cz: %p \n", &cz);
```  

> Output  
char alignmnet: 1  
char alignmnet: 8  
&dx: 0x7ffc1b55c178 0  
&ca: 0x7ffc1b55c177  
&cx: 0x7ffc1b55c176  
&dz: 0x7ffc1b55c168 0  
&cb: 0x7ffc1b55c167  
&cz: 0x7ffc1b55c166  

이 때 align을 사용하여 줄바꿈 기준을 바꾸어줄 수 있다. alignas를 사용할 때 변수는 2의 제곱값만 가능하다. 아래 코드를 보면 char 자료형임에도 불구하고 8, 16 기준을 적용시킬 수 있음을 알 수 있다.  

```cpp
char cz;
char _Alignas(double) cz1;
char alignas(16) cz2;

printf("&cz : %p %lld\n", &cz,  (long long)&cz  % 8);
printf("&cz1: %p %lld\n", &cz1, (long long)&cz1 % 8);
printf("&cz2: %p %lld\n", &cz2, (long long)&cz2 % 8);
```  

> Output  
&cz : 0x7ffc569ccddf 7  
&cz1: 0x7ffc569ccdd8 0  
&cz2: 0x7ffc569ccdd0 0  

`align 함수를 배열에 적용하기`  

```cpp
unsigned char alignas(long double) c_arr[sizeof(long double)];
```

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊