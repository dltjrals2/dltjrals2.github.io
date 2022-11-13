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
last_modified_at: 2022-11-13
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

초기화 할 때만 나열한 된 값을 배열에 할당할 수 있다. 그 외에는 불가능 하다.  

<iframe src="https://tech.io/snippet-widget/crC2AYN" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

실행 결과를 확인하면 세 가지 경우에 대해 주소의 값이 같은 것을 확인할 수 있다.  

<iframe src="https://tech.io/snippet-widget/WXgV4E3" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

실행 결과를 확인해보면 4byte 씩 차이가 나는 것을 확인할 수 있다.  

### 10.3 포인터의 산술 연산  

### 10.4 포인터와 배열  

### 10.5 2차원 배열과 메모리  

### 10.6 2차원 배열 연습문제  

### 10.7 배열을 함수에게 전달해주는 방법  

### 10.8 두 개의 포인터로 배열을 함수에게 전달해주는 방법  

### 10.9 포인터 연산 총정리  

### 10.10 const와 배열과 포인터  

### 10.11 배열 매개변수와 const  

### 10.12 포인터에 대한 포인터(2중 포인터)의 작동 원리  

### 10.13 포인터의 배열과 2차원 배열  

### 10.14 2차원 배열과 포인터  

### 10.15 포인터의 호환성  

### 10.16 다차원 배열을 함수에게 전달해주는 방법  

### 10.17 변수로 길이를 정할 수 있는 배열  

### 10.18 복합 리터럴과 배열  


**🐢 현재 공부하고 있는 홍정모의 따라하며 배우는 C언어 를 학습하며 기록 및 정리하는 포스팅입니다. 🐢**
{: .notice--primary}   

수강 사이트  
<https://www.inflearn.com/course/following-c/>

감사합니다.😊

