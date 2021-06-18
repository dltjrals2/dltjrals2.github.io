---
title: "[Sorting] 위에서 아래로"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Sotring]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-18
last_modified_at: 2021-06-18
sitemap :
  changefreq : daily
  priority : 1.0
---

## 위에서 아래로  

난이도 : ⭐  

하나의 수열에는 다양한 수가 존재한다. 이러한 수는 크기에 상관없이 나열되어 있다. 이 수를 큰 수부터 작은 수의 순서로 정렬해야 한다. 수열을 내림차순으로 정렬하는 프로그램을 만드시오.  

**<u>입력 조건</u>**  
- 첫째 줄에 수열에 속해 있는 수의 개수 N이 주어진다. (1 <= N <= 500)  
- 둘째 줄부터 N + 1번째 줄까지 N개의 수가 입력된다. 수의 범위는 항상 1 이상 100,000 이하의 자연수이다.  

**<u>출력 조건</u>**  
- 입력으로 주어진 수열이 내림차순으로 정렬된 결과를 공백으로 구분하여 출력한다. 동일한 수의 순서는 자유롭게 출력해도 괜찮다.  

**<u>입력 예시</u>**  
3  
15  
27  
12  

**<u>출력 예시</u>**  
27 15 12  

> 나의 풀이  

```python
n = int(input())
array = []
for _ in range(n):
    array.append(int(input()))

array = sorted(array, reverse=True)
while array:
    print(array.pop(0), end = ' ')
```

> 문제 해설  

기본적인 정렬에 대한 문제이다. 모든 수는 1 이상 100,000 이하이므로 어떠한 정렬 알고리즘을 사용해도 문제를 해결할 수 있다. 선택 정렬, 삽입 정렬, 퀵 정렬, 계수 정렬 중 아무거나 이용해도 괜찮지만 가장 코드가 간결해지는 파이썬의 기본 정렬 라이브러리를 사용하는 것이 효과적이다.  

> 답안 예시  

```python
# N을 입력받기
n = int(input())

# N개의 정수를 입력받아 리스트에 저장
array = []
for i in range(n):
    array.append(int(input()))

# 파이썬 기본 정렬 라이브러리를 이용하여 정렬 수행
array = sorted(array, reverse=True)

# 정렬이 수행된 결과를 출력
for i in array:
    print(i, end=' ')
```  

<br>
기출 : T 기업 코딩 테스트  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
