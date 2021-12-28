---
title: "주요 라이브러리 문법과 유의점 - 1"

categories:
  - Appendix
tags:
  - [Python, 내장 함수]

toc:  true
toc_sticky: true

date: 2021-12-28
last_modified_at: 2021-12-28
---

## 주요 라이브러리 문법과 유의점 - 1  

표준 라이브러리란 `특정한 프로그래밍 언어에서 자주 사용되는 표준 소스코드를 미리 구현해 놓은 라이브러리`를 의미한다. 코딩 테스트에서는 대부분 표준 라이브러리를 사용할 수 있도록 허용하므로 표준 라이브러리를 사용하면 소스코드 작성량에 대한 부담을 줄일 수 있다.  

파이썬 표준 라이브러리는 다음 공식 문서에서 자세히 확인할 수 있다.  

<https://docs.python.org/ko/3/library/index.html>  

코딩 테스트를 준비하며 반드시 알아야 하는 라이브러리는 6가지 정도이다.  

- 내장 함수: print(), input()과 같은 기본 입출력 기능부터 sorted()와 같은 정렬 기능을 포함하고 있는 기본 내장 라이브러리이다. 파이썬 프로그램을 작성할 때 없어서는 안되는 필수적인 기능을 포함하고 있다.  
- itertools: 파이썬에서 반복되는 형태의 데이터를 처리하는 기능을 제공하는 라이브러리이다. 순열과 조합 라이브러리를 제공한다.  
- heapq: 힙(Heap) 기능을 제공하는 라이브러리이다. 우선순위 큐 기능을 구현하기 위해 사용한다.  
- bisect: 이진 탐색(Binary Search) 기능을 제공하는 라이브러리이다.  
- collections: 덱(deque), 카운터(Counter)등의 유용한 자료구조를 포함하고 있는 라이브러리이다.  
- math: 필수적인 수학적 기능을 제공하는 라이브러리이다. 팩토리얼, 제곱근, 최대공약수(GCD), 삼각함수 관련 함수부터 파이(pi)와 같은 상수를 포함하고 있다.  

### 내장 함수  

파이썬에는 별도의 import 명령어 없이 사용할 수 있는 내장 함수가 존재한다. 내장 함수를 프로그램 작성에 있어 가장 기본적이면서 필수적인 기능을 포함하고 있다. 대표적인 내장 함수를 input(), print()이다.  

먼저 sum() 함수는 리스트와 같은 iterable 객체가 주어졌을 때, 모든 원소의 합을 반환한다. 리스트 [1, 2, 3, 4, 5]의 모든 원소의 값을 더하는 예시를 확인해보자.  

```python
result = sum([1, 2, 3, 4, 5])
print(result)
```  

> 15  

min() 함수는 파라미터가 2개 이상 들어왔을 때 가장 작은 값을 반환한다. 예를 들어 특정한 4개의 정수 중에서 가장 작은 수를 출력하는 예시를 확인해보자.  

```python
result = min(7, 3, 5, 2)
print(result)
```  

> 2  

max() 함수는 파라미터가 2개 이상 들어왔을 때 가장 큰 값을 반환한다. 마찬가지로 4개의 정수 중에서 가장 큰 수를 출력하는 예시를 확인해보자.  

```python
result = max(7, 3, 5, 2)
print(result)
```  

> 7  

eval() 함수는 수학 수식이 문자열 형식으로 들어오면 해당 수식을 계산한 결과를 반환한다. 예를 들어 문자열 형태로 주어진 수식 (3 + 5) * 7을 계산하는 소스코드는 다음과 같다.  

```python
result = eval("(3 + 5) * 7")
print(result)
```  

> 56  

sorted() 함수는 iterable 객체가 들어왔을 때, 정렬된 결과를 반환한다. key 속성으로 정렬 기준을 명시할 수 있으며, reverse 속성으로 정렬된 결과 리스트를 뒤집을지의 여부를 설정할 수 있다. 리스트 [9, 1, 8, 5, 4]를 오름차순으로 정렬하는 예시와 내림차순으로 정렬하는 예시는 다음과 같다.  

```python
result = sorted([9, 1, 8, 5, 4]) # 오름차순으로 정렬
print(result)
result = sorted([9, 1, 8, 5, 4], reverse = True) # 내림차순으로 정렬
print(result)
```  

> [1, 4, 5, 8, 9]  
> [9, 8, 5, 4, 1]  

파이썬에서는 리스트의 원소로 리스트나 튜플이 존재할 때 특정한 기준에 따라서 정렬을 수행할 수 있다. 정렬 기준은 key 속성을 이용해 명시할 수 있다. 예를 들어 리스트 [('홍길동', 35), ('이순신', 75), ('아무개', 50)]이 있을 때, 원소를 튜플의 두 번째 원소(수)를 기준으로 내림차순으로 정렬하고자 한다면 다음과 같이 사용할 수 있다.  

```python
result = sorted([('홍길동', 35), ('이순신', 75), ('아무개', 50)], key = lambda x : x[1], reverse = True)
print(result)
```  

> [('이순신', 75), ('아무개', 50), ('홍길동', 35)]

리스트와 같은 iterable 객체는 기본으로 sort() 함수를 내장하고 있어 굳이 sorted() 함수를 사용하지 않고도 sort() 함수를 사용해서 정렬할 수 있다. 이 경우 리스트 객체의 내부 값이 정렬된 값으로 바로 변경된다. 리스트 [9, 1, 8, 5, 4]를 오름차순으로 정렬하는 예시는 다음과 같다.  

```python
data = [9, 1, 8, 5, 4]
data.sort()
print(data)
```

> [1, 4, 5, 8, 9]  

### itertools  

itertools는 파이썬에서 반복되는 데이터를 처리하는 기능을 포함하고 있는 라이브러리이다. 제공하는 클래스는 매우 다양하지만, 코딩 테스트에서 가장 유용하게 사용할 수 있는 클래스는 permutations, combinations이다.  

permutations는 리스트와 같은 iterable 객체에서 r개의 데이터를 뽑아 일렬로 나열하는 모든 경우(순열)을 계사내준다. permutations는 클래스이므로 객체 초키화 이후에는 리스트 자료형으로 변환하여 사용한다. 리스트 ['A', 'B', 'C']에서 3개(r = 3)를 뽑아 나열하는 모든 경우를 출력하는 예시는 다음과 같다.  

```python
from itertools import permutations

data = ['A', 'B', 'C'] # 데이터 준비
result = list(permutations(data, 3)) # 모든 순열 구하기

print(result)
```  

> [('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]  

combinations는 리스트와 같은 iterable 객체에서 r개의 데이터를 뽑아 순서를 고려하지 않고 나열하는 모든 경우(조합)를 계산한다. combinations는 클래스이므로 객체 초기화 이후에는 리스트 자료형으로 변환하여 사용한다. 리스트 ['A', 'B', 'C']에서 2개(r = 2)를 뽑아 순서에 상관없이 나열하는 모든 경우를 출력하는 예시는 다음과 같다.  

```python
from itertools import combinations

data = ['A', 'B', 'C'] # 데이터 준비
result = list(combinations(data, 2)) # 2개를 뽑는 모든 조합 구하기

print(result)
```  

> [('A', 'B'), ('A', 'C'), ('B', 'C')]  

product는 permutations와 같이 리스트와 같은 iterable 객체에서 r개의 데이터를 뽑아 일렬로 나열하는 모든 경우(순열)을 계산한다. 다만 원소를 중복하여 뽑는다. product 객체를 초기화할 때는 뽑가자 하는 데이터의 수를 repeat 속성값으로 넣어준다. product는 클래스이므로 객체 초기화 이후에는 리스트 자료형으로 변환하여 사용한다. 리스트 ['A', 'B', 'C']에서 중복을 포함하여 2개(r = 2)를 뽑아 나열하는 모든 경우를 출력하는 예시는 다음과 같다.  

```python
from itertools import product

data = ['A', 'B', 'C'] # 데이터 준비
result = list(product(data, repeat = 2)) # 2개를 뽑는 모든 순열 구하기(증복 허용)

print(result)
```  

> [('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'B'), ('B', 'C'), ('C', 'A'), ('C', 'B'), ('C', 'C')]  

combinations_with_replacement는 combinations와 같이 리스트와 같은 iterable 객체에서 r개의 데이터를 뽑아 순서를 고려하지 않고 나열하는 모든 경우(조합)를 계산한다. 다만 원소를 중복해서 뽑는다. combinations_with_replacement는 클래스이므로 객체 초기화 이후에는 리스트 자료형으로 변환하여 사용해야 한다. 리스트 ['A', 'B', 'C']에서 중복을 포함하여 2개(r = 2)를 뽑아 순서에 상관없이 나열하는 모든 경우를 출력하는 예시는 다음과 같다.  

```python
from itertools import combinations_with_replacement

data = ['A', 'B', 'C'] # 데이터 준비
result = list(combinations_with_replacement(data, 2)) # 2개를 뽑는 모든 조합 구하기(중복 허용)
print(result)
```  

> [('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]  


---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
