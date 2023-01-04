---
title: "파일 입출력"

categories:
  - C_grammer
tags:
  - [C Language, Grammer]

use_math: false

toc:  true
toc_sticky: true

date: 2022-12-31
last_modified_at: 2023-01-03
---

## 13. 파일 입출력  

### 13.1 파일 입출력의 작동 원리  

![TBC](https://user-images.githubusercontent.com/37467408/210134106-22e60de7-b212-4abd-9f2f-1534373367f7.PNG)  

- 프로그램을 실행하면 운영체제와 소통할 수 있는 채널이 3개 열린다. `stdout, stderr, stdin` 이다.  
- stdout과 stderr은 별도의 채널을 가지고 있다. redirection(파일의 출력값을 다른 파일의 입력값으로 사용) 할 때 이를 활용할 수 있다.  
- 한 글자씩 통신하면 느리기 때문에 buffer을 이용하여 데이터를 주고받는다.  

`텍스트/바이너리 파일 주고받기`  

![TBC](https://user-images.githubusercontent.com/37467408/210134157-c5eee57c-87a9-4cb7-a00a-0bdd4318462f.PNG)  

혼합하여 사용할 수 있다.  

![TBC](https://user-images.githubusercontent.com/37467408/210134179-fb0fd28c-b97d-4e3a-bd26-d599f299ad79.PNG)  

fprintf() 함수는 텍스트 파일 IO 스트림에 해당 데이터를 저장한다.  
fwrite() 함수는 바이너리 파일 IO 스트림에 해당 데이터를 저장한다.  

### 13.2 텍스트 파일 입출력 예제  

```cpp
int main(int argc, char* argv[])
{
    int ch;
    FILE* fr, * fw; // FILE* is pointing to group of data (struct)
    
    /*
    typedef struct _iobuf
    {
        char* ptr;
        int _cnt;
        char* _base;
        int _flag;
        int _file;
        int _charbuf;
        int _bufsize;
        char* _tmpfname;
    } FILE;
    */
    
    unsigned long count = 0;
    if(argc < 2)
    {
        printf("Usage : %s filename\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    else if( (fr = fopen(argv[1], "r")) == NULL ) // Open file to read data
    {
        printf("Can't open %s\n", argv[1]);
        exit(EXIT_FAILURE);
    }
    
    const char* out_filename = "output.txt";
    if( (fw = fopen(out_filename, "w")) == NULL )
    {
        printf("Can't open %s \n", argv[1]);
        exit(EXIT_FAILURE);
    }
    
    while( (ch = fgetc(fr) != EOF) ) // fgetc() == getc(), fgetc is more stable
    {
        // puts(ch, stdout); // same as putchar(ch) only difference is specificating exact stream
        fputc(ch, stdout); // fputc() is stable version of putc()
        fputc(ch, fw);
        count++;
    }
    fclose(fr);
    fclose(fw);
    printf("\nFILE %s has %lu characters\n", argv[1], count);
    printf("Copied to %s", out_filename);
}
```  

`Mode specifier`  
- 'r' : reading  
- 'w' : creating and writing or over-writing  
- 'a' : appending or creating and writing  
- 'r+' : both reading and writing (Not creating, the writing don't modify things in out of range)  
- 'w+' : reading and writing, over writing or creating  
- 'a+' : reading and writing, appending or creating  

위의 specifier 모두 char 이 아닌 ch타입으로 사용된다.  

따라서, double quotation(")을 사용하도록 하자.  

### 13.3 텍스트 인코딩과 코드 페이지  

![TBC](https://user-images.githubusercontent.com/37467408/210153066-10812d06-004c-470e-afd3-c43b116b0b8e.PNG)  

`CRLF`  
- Carriage return : A control character or mechanism used to reset a device's position to the begining of a line of text  
- Line Feed(LF) : New line in unix  

`UTF-8, ANSI`  
- UTF-8으로 저장했을 때 포맷이 맞지 않아 .c 파일에서는 한글이 깨져 나온다. ANSI로 변경해주면 포맷이 맞아 정상적으로 한글이 출력이 된다.  

`한글깨짐 해결방법 - 디코딩 해석 표 찾아주기`  
- UTF-8으로 저장했을 때 한글이 깨지는 이유는 디코딩이 적절하지 않기 때문이다. 프로그래머가 임의로 지정해줄 수 있다.  
- SetConsoleOutputCP(CP_UTF8)을 사용해주자.  

```cpp
#include <windows.h> // SetConsoleOutputCP() CP for CodePage
int main(int argc, char* argv[])
{
    const UINT default_cp = GetConsoleOutputCP(); // Check What codepage you use
    printf("%u \n", default_CP);
    
    int ch;
    FILE* fr, * fw; // FILE* is pointing to group of data (struct)
    const char* in_filename = "원본.txt";
    const char* out_filename = "사본.txt";
    unsigned long count = 0;
    
    if( (fr = fopen(in_filename, "r")) == NULL ) // Open file to read data
    {
        printf("Can't open %s\n", in_filename);
        exit(EXIT_FAILURE);
    }
    
    if( (fw = fopen(out_filename, "w")) == NULL )
    {
        printf("Can't open %s\n", out_filename);
        exit(EXIT_FAILURE);
    }
    
    while( (ch = fgetc(fr)) != EOF ) // fgetc() == getc(), fgetc is more stable
    {
        // putc(ch, stdout); // Same as putchar(ch) only difference is specificating exact stream
        fputc(ch, stdout); // fputc() is stable version of putc()
        fputc(ch, fw);
        count++;
    }
    fclose(fr);
    fclose(fw);
    printf("\nFILE %s has %lu characters\n", in_filename, count);
    
    SetConsoleOutputCP(default_cp); // ISO 2022 korean
    
    printf("copied to %s", out_filename);
    printf("한글 출력 테스트!");
}
```  

SetConsoleOutputCP(CP_UTF8)를 사용하면 cmd console에서 한글이 출력되지 않는다. 파일을 입출력할 때만 CP_UTF8을 사용하자.  

CMD console을 사용할 때는 default_cp로 되돌리면 한글이 깨지지 않고 출력된다.  

### 13.4 텍스트 파일 입출력 함수들  

fscanf(FILE* const _Streeam, char const* const _Format, ...)  
- Stream에 stdin을 넣으면 scanf() 처럼 사용할 수 있다. _Format은 string + format specifier을 넣고 그 이후에는 format specifier에 맞는 변수를 입력한다.  
- fscanf() 함수는 입력받은 문자열 개수를 반환한다.  

```cpp
#define MAX 31
int main(int argc, char* argv[])
{
    FILE* fp;
    char words[MAX] = { '\0' };
    const char* filename = "record.txt";
    
    if( (fp = fopen(filename, "w+") == NULL )
    {
        fprintf(stderr, "Can't open \"%s\" file.\n", filename); // redirect this in two ways
        exit(EXIT_FAILURE);                                     // stdout, stderr
    }
    
    while( (fscanf(stdin, "%30s", words) == 1) && (words[0] != '.') )
        fprintf(fp, "%s\n", words);
        
    // while ( (fgets(words, MAX, stdin) != NULL) && (words[0] != '.') )
    //    fputs(words, fp);
    
    rewind(fp); // go back to begining of file
    
    while(fscanf(fp, "%s", words) == 1) // if scanf() meet EOF then return -1
        fprintf(words, stdout);
        
    if(fclose(fp) != 0)
        fprintf(stderr, "Error closing file\n");
}
```  

`fscanf() 함수로 txt 파일의 특정한 값만 읽어보자.`  

아래 abc.txt 파일에 3번째 문자열만 읽고 싶을 때 fscanf() 를 활용할 수 있다.  

`abc.txt`  
NAME    AGE   CITY  
abc     12    hyderbad  
bef     25    delhi  
cce     65    bangalore  

`main.c`  

```cpp
#include <stdio.h>

int main()
{
    FILE* ptr = fopen("abc.txt", "r");
    if(ptr == NULL)
    {
        printf("No such file.");
        return 0;
    }
    
    char buf[100];
    while(fscanf(ptr, %*s %*s %s ", buf) == 1)
        printf("%s\n", buf);
        
    return 0;
}
```  

`fscanf() vs fgets()`  
- fscanf() 는 줄바꿈, 띄어쓰기 입력으로 변수를 구분한다. 더 이상 변수가 없으면 함수 실행이 종료된다.  
- fgets() 는 줄바꿈만 이전과 같은 역할을 한다. 띄어쓰기는 영향을 주지 않는다.  
- fscanf() 는 format specifier의 개수만큼 입력받고 입력받은 수를 반환한다. fgets()는 입력받은 문자열 포인터를 반환한다.  

`fgets() 로 파일쓰기`  

```cpp
#define LINE_MAX 20

int main()
{
    FILE* fp = fopen("text.txt", "r");
    FILE* fw = fopen("create.txt", "w");
    char line[LINE_MAX];
    while(fgets(line, LINE_MAX, fp))
    {
        printf("%s", line);
        fputs(line, fw);
    }
}
```

### 13.5 바이너리 파일 입출력  

`fopen() mode string for binary IO`  
- 기본 Mode Specifier에 'b'를 붙혀주면 된다.  

rb : reading  
wb : creating and writing or over writing  
ab+ | a+b : appending or creating and writing  
wb+ | w+b : reading and writing, over writing or creating  
ab+ | a+b : reading and writing, appending or crearing  

`C11 'x' mode fails if the file exists, instead of overwriting it`  

"wx", "wbx", "w+x", "wb+x", "w+bx"  

`binary 파일 쓰기 예제`  

```cpp
FILE* fp = fopen("binary_file", "wb");
double d = 1.0 / 3.0;
int n = 11;
int* p_arr = (int*)malloc(sizeof(int) * n);
if(!p_arr) exit(1);

for(int i = 0 ; i < n ; ++i)
    *(p_arr + i) = i * 2;
    
fwrite(&d, sizeof(d), 1, fp);
fwrite(&d, sizeof(d), 1, fp);
fwrite(p_arr, sizeof(p_arr), n, fp);

flose(fp);
free(p_arr);
// total size = double + int + int * 111 = 8 + 4 + 444 = 456byte
```  

`binary 파일 읽기 예제`  
- binary 파일은 파일의 형식을 모르면 알 수 없다. (자료형을 알고 있어야 한다.)  
- EOF(-1)은 더 이상 값을 읽을 수 없을 때 feof로 확인되는 값이다.  

```cpp
// If you didn't know the format
// Hard to decoding binary file
FILE* fp = fopen("binary_file", "rb");
double d;
int n;
fread(&d, sizeof(d), 1, fp);
fread(&n, sizeof(n), 1, fp);

int* p_arr = (*int)malloc(sizeof(int) * n);
if(!p_arr) exit(1);

fread(p_arr, sizeof(int), n, fp); // If buffer is full, save it and load again.

printf("feof = %d\n", feof(fp)); // does File meet EOF? no. it pointing the last element, so return 0
printf("%f \n", d);
printf("%d \n", n);
for(int i = 0 ; i < n ; ++i)
    printf("%d ", *(p_arr + i));
printf("\n");

fread(&n, sizeof(n), 1, fp); // read one more toward EOF
printf("feof = %d\n", feof(fp)); // Does file meet EOF? yes return 1

printf("ferror = %d\n", ferror(fp)); // No Error, return 0
fwrite(&n, sizeof(n), 1, fp); // read only, raise error
printf("ferror = %d\n", ferror(fp)); // return 1 means error

fclose(fp);
free(p_arr);
```  

> Output  
> feof = 0
> 0.333333  
> 11  
> 0 2 4 6 8 10 12 14 16 18 20  
> feof = 1  
> ferror = 0  
> ferror = 1  

fread() 함수의 반환값은 읽어들인 양으로 세 번째 parameter에 의해 결정된다. 읽어들일 양이 parameter 보다 적을 때 그만큼을 반환한다.  

### 13.6 파일 임의 접근  

`TEXT 파일 임의 접근`  

long ftell(FILE*) : 읽어들일 위치를 반환한다. (long type)  
fseek(FILE*, long int offset, int origin) : 현재 위치를 지정한 곳에서 offset만큼 이동한다.  

```cpp
/*
    test.txt << "ABCDE....."
    read 0 - 1 bytes, get A, pointing to 1byte add
    read 1 - 2 bytes, get B, pointing to 2byte add
*/

int ch;
long cur;
FILE* fp = foepn("test.txt", "r");
cur = ftell(fp); // get current pointing position
printf("ftell() = %2ld\n", cur);

// Move 2 step forward
fseek(fp, 2L, SEEK_SET); // SEEK_SET is 0 position, move to 2byte (L for long)
cur = ftell(fp);
printf("ftell() = %2ld ", cur);
ch = fgetc(fp);
printf("v : %d %c \n", ch, ch);

cur = ftell(fp);
printf("ftell() = %2ld \n", cur);

// Move 2 step backward
fseek(fp, -2L, SEEK_CUR); // SEEK_CUR is current position
cur = ftell(fp);
printf("ftell() = %2ld ", cur);
ch = fgetc(fp);
printf("v : %d %c \n", ch, ch);

// Move to EOF
fseek(fp, -1L, SEEK_END); // SEEK_END is last position
cur = ftell(fp);
printf("ftell() = %2ld ", cur);
ch = fgetc(fp);
printf("v : %d %c \n", ch, ch);

// Move to last element
fseek(fp, 0L, SEEK_END); // SEEK_END is last element
cur = ftell(fp);
printf("ftell() = %2ld ", cur);
ch = fgetc(fp);
printf("v : %d %c \n", ch, ch);
```  

`fsetpos(), fgetpos()`  
- 파일의 크기가 long 타입을 넘어서면 fsetpos()와 fgetpos() 함수를 쓰자. ( = fseek() = ftell() )  
- MSVC에서 위치를 저장하는 fpost_t 타입은 long long 이다.  

```cpp
fpos_t pt;
pt = 10;
fsetpos(fp, &pt); // == fseek()
ch = fgetc(fp);
printf("v : %d %c \n", ch, ch);
fgetpos(fp, &pt); // == ftell()
printf("fgetpos() = %lld ", pt);
```  

`Binary 파일 임의 접근`  

```cpp
// Write
FILE* fp = fopen("binary", "wb");
for(int i = 0 ; i < 20 ; ++i)
{
    double d = i * 1.11;
    fwrite(&d, sizeof(double), 1, fp);
}
flose(fp);

// Read
fp = fopen("binary", "rb");
long cur;
double d;

cur = ftell(fp);
printf("Beforer reading %2ld\n", cur) // >> Before reading 0

fread(&d, sizeof(double), 1, fp); 
cur = ftell(fp);
printf("After reading %2ld  ", cur);   // >> After reading  8  v : 0.000000
printf("v : %f\n", d);

fread(&d, sizeof(double), 1, fp);
cur = ftell(fp);
printf("After reading %2ld  ", cur);  // >> After reading 16  v : 1.110000
printf("v : %f\n", d);

fseek(fp, 88L, SEEK_SET);
fread(&d, sizeof(double), 1, fp);
cur = ftell(fp);
printf("After seek    %2ld  ", cur); // >> After seek    96  v : 12.210000
printf("v : %f\n", d);

fclose(fp);
```  

### 13.7 기타 입출력 함수들  

`ungetc()`  
- 해당 문자를 buffer에 넣는다. 다음 예제는 파일 test.txt << "Hello"를 사용한다.  

```cpp
int ch;
FILE* fp = fopen("test.txt", "r");
/* ungetc() : read 1 letter and load it to buffer */
ch = fgetc(fp);
fputc(ch, stdout);

// ungetc(ch, fp);

ch = fgetc(fp);
fputc(ch, stdout);

//   keep comment   |   uncomment
//  >> He           |  >> HH
```  

```cpp
int ch;
FILE* fp = fopen("test.txt", "r");
/* ungetc() : read 1 letter and load it to buffer */
ch = fgetc(fp);
fputc(ch, stdout);

ungetc((int)'A', fp);

ch = fgetc(fp);
fputc(ch, stdout);

ch = fgetc(fp);
fputc(ch, stdout);
// >> HAe
```  

`setvbuf()`  
- 기존 buffer를 내가 만든 변수로 바꿀 수 있다.  

```cpp
// change the settings of buffer
int ch;
FILE* fp = fopen("test.txt", "r");

char buffer[4] = { '\0' }' // use this array as a buffer
setvbuf(fp, buffer, _IOFBF, sizeof(buffer)); // _IOLBF, _IOFBF, _IONBF

ch = fgetc(fp); // Read only a character

// dump buffer
for(int i = 0 ; i < sizeof(buffer) ; ++i
    printf("%c", buffer[i]);
    
fclose(fp);
```  

_IOLBF : L = line 라인을 만나면 저장  
_IOFBF : F = Full 통째로 저장  
_IONBF : N = No 저장하지 않음  

ch = fgetc(fp) 를 실행하여 문자 하나를 읽어왔지만 아래 for 반복문을 실행해보면 Hell이 출력된다.  

ch = fgetc(fp) 가 실행될 때 buffer의 크기만큼 데이터를 읽어온다.  

`fflush() : file flush(물내리기)`  
- buffer가 빌 때까지 특정 작업을 실행시킨다.  

```cpp
fflush(fp); // work until buffer is empty
```

### 13.8 텍스트 파일을 바이너리 처럼 읽어보기  

test.txt  

Hello World  
First  
Second  

```cpp
// #include <windows.h>
unsigned char ch;
double d;
FILE* fp = fopen("test.txt", "rb");

SetConsoleOutputCP(CP_UTF8);

while (fread(&ch, sizeof(unsigned char), 1, fp) > 0)
    printf("%3hhu %c \n", ch, ch);

fclose(fp);
```  

fread() 함수의 반환값을 읽어들인 양으로 세 번째 parameter에 의해 결정된다.  

읽어들일 양이 parameter 보다 적을 때 그만큼을 반환한다.  

 72 H  
101 e  
108 l  
108 l  
111 o  
 32  
 87 W  
111 o  
114 r  
108 l  
100 d  
 13  
 10  

 70 F  
105 i  
114 r  
115 s  
116 t  
 13  
 10  

 83 S  
101 e  
 99 c  
111 o  
110 n  
100 d  

Windows는 EOF에 별도표시를 하기 위해 한 글자를 추가한다. Linux는 하지 않는다. 줄바꿈기호, 시작 지점 기호는 운영체제마다 다르다.  

텍스트 파일을 저장할 때는 운영체제가 추가적인 작업을 하기 때문에 텍스트 파일의 입출력은 운영체제마다 다르게 나타난다.  

**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊