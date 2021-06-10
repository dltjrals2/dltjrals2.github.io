---
title: "정렬의 개요와 선택 정렬 (Selection Sort)"

categories:
  - Algorithm
tags:
  - [algorithm, Coding Test, C, Sort]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-10
last_modified_at: 2021-06-10
sitemap :
  changefreq : daily
  priority : 1.0
---
## 1. 정렬의 개요와 선택 정렬 (Selection Sort)  

다음의 숫자들을 오름차순으로 정렬하는 프로그램을 작성하세요.<br><br>
1, 10, 5, 8, 7, 6, 4, 3, 2, 9
{: .notice--primary}  

가장 직관적인 접근 방법은 `선택 정렬` 이다.  

`선택 정렬` 이란, 가장 작은 수를 선택해서 제일 앞으로 보내는 알고리즘이며 가장 기초적인 방법이다.  

`min` : 가장 적은 수를 일시적으로 담는 변수  
`temp` : 배열에서 두 숫자의 위치를 바꾸기 위해 사용하는 변수  
`시간복잡도` : O(N^2)

```c
#include <stdio.h>  

int main(void) {  
  int i, j, min, index, temp;  
  int array[10] = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};
  for(i = 0 ; i < 10 ; i++) {
    min = 9999;
    for(j = i ; j < 10 ; j++) {
      if(min > array[j]) {
        min = array[j];
        index = j;
      }
  	}
    temp = array[i];
    array[i] = array[index];
    array[index] = temp;
  }
  for(i = 0 ; i < 10 ; i++) {
  printf("%d ", array[i]);
  }
  return 0;
}

```
<br>
<br>
![image](https://user-images.githubusercontent.com/37467408/121469314-3e0bd500-c9f7-11eb-8511-f4e4de7423b5.PNG)  

`array[0]` 부터 가장 작은 값을 넣기 위해서 `for`문에서 `i = 0`으로 시작한다.  
가장 적은 수를 맨 앞으로 보냈으면 그 전에거는 볼 필요가 없어서 2중 `for`문에서 `j = i`로 시작한다.  
2중 `for`문을 다 돌면 min 값이 정해지기 때문에 해당 min 값을 `array[i]`에 넣어주는 방식으로 선택 정렬을 한다.  

---
🐢** 정보공유의 목적보다는 공부 기록용으로 쓰고 있습니다.  **🐢
{: .notice--primary}   

**참고 블로그**  
[안경잡이개발자님의 블로그](https://blog.naver.com/ndb796/221226800661)  

감사합니다.😊
