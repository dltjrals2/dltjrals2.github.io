---
title: "[Sorting] 카드 정렬하기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Sotring, BaekJoon, 풀이실패]

toc:  true
toc_sticky: true

date: 2021-10-12
last_modified_at: 2021-10-12
---

## 카드 정렬하기  

난이도 : 🌕  
푼횟수 : 🔴⚪⚪  

정렬된 두 묶음의 숫자 카드가 있다고 하자. 각 묶음의 카드의 수를 A, B라 하면 보통 두 묶음을 합쳐서 하나로 만드는 데에는 A+B 번의 비교를 해야 한다. 이를테면, 20장의 숫자 카드 묶음과 30장의 숫자 카드 묶음을 합치려면 50번의 비교가 필요하다.  

매우 많은 숫자 카드 묶음이 책상 위에 놓여 있다. 이들을 두 묶음씩 골라 서로 합쳐나간다면, 고르는 순서에 따라서 비교 횟수가 매우 달라진다. 예를 들어 10장, 20장, 40장의 묶음이 있다면 10장과 20장을 합친 뒤, 합친 30장 묶음과 40장을 합친다면 (10 + 20) + (30 + 40) = 100번의 비교가 필요하다. 그러나 10장과 40장을 합친 뒤, 합친 50장 묶음과 20장을 합친다면 (10 + 40) + (50 + 20) = 120 번의 비교가 필요하므로 덜 효율적인 방법이다.  

N개의 숫자 카드 묶음의 각각의 크기가 주어질 때, 최소한 몇 번의 비교가 필요한지를 구하는 프로그램을 작성하시오.  

**<u>입력</u>**  
첫째 줄에 N이 주어진다. (1 ≤ N ≤ 100,000) 이어서 N개의 줄에 걸쳐 숫자 카드 묶음의 각각의 크기가 주어진다. 숫자 카드 묶음의 크기는 1,000보다 작거나 같은 양의 정수이다.  

**<u>출력</u>**  
첫째 줄에 최소 비교 횟수를 출력한다.  

**<u>예제 입력 1</u>**  
3  
10  
20  
40  

**<u>예제 출력 1</u>**  
100  

> 나의 풀이  

```python
n = int(input())
card_list = list()

for _ in range(n):
    card_list.append(int(input()))

length = len(card_list)
card_list = sorted(card_list, reverse=False)

sum = 0
answer = 0
while length > 0:
    sum += card_list.pop(0)
    sum += card_list.pop(0)
    answer = sum + (sum + card_list.pop(0))
    length = len(card_list)

print(answer)
```

> 문제 해설  

<u>제 풀이는 런타임 에러입니다.</u>  

이 문제의 핵심 아이디어는 항상 가장 작은 크기의 두 카드 묶음을 합쳤을 때 최적의 해를 보장한다는 점이다.  

따라서 매 상황에서 무조건 가장 작은 크기의 두 카드 묶음을 합치면 되다는 점에서, 이 문제는 그리디 알고리즘으로 분류할 수 있다.  

다만, 정렬 개념을 활용하는 아이디어가 필요하기 때문에 정렬 유형의 문제로 분류하였다.  

그렇다면 항상 가장 작은 크기의 두 카드 묶음을 알기 위해 어떻게 하면 될지 알아보자.  

이러한 과정을 매우 효과적으로 수행할 수 있는 자료구조는 바로 우선순위 큐이다.  

우선순위 큐는 원소를 넣었다 빼는 것만으로도 정렬된 결과를 얻을 수 있다. 우선순위 큐는 힙 자료구조를 이용해서 구현할 수 있으며, 파이썬에서는 heapq 라이브러리를 지원하고 있다.  

다른 예시로 10, 20, 40, 50의 4개의 카드 묶음이 있다고 가정하자. 힙 자료구조를 이용하여 총 비교 횟수를 구하는 과정을 그림으로 표현하면 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/136908427-cad31724-6e24-4945-9b44-346d87bf33c3.PNG)  

따라서 최소 비교 횟수는 220이 된다.  

> 문제 답안  

```python
import heapq

n = int(input())

# 힙(Heap)에 초기 카드 묶음을 모두 삽입
heap = []
for i in range(n):
    data = int(input())
    heapq.heappush(heap, data)

result = 0

# 힙(Heap)에 원소가 1개 남을 때까지
while len(heap) != 1:
    # 가장 작은 2개의 카드 묶음 꺼내기
    one = heapq.heappop(heap)
    two = heapq.heappop(heap)
    # 카드 묶음을 합쳐서 다시 삽입
    sum_value = one + two
    result += sum_value
    heapq.heappush(heap, sum_value)

print(result)
```

<br>
기출 : 핵심 유형  
링크 : <https://www.acmicpc.net/problem/1715>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊