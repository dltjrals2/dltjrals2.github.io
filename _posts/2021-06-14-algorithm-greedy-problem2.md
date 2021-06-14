---
title: "[Greedy Algorithm] 숫자 카드 게임"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Greedy]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-14
last_modified_at: 2021-06-14
sitemap :
  changefreq : daily
  priority : 1.0
---
## 1. 숫자 카드 게임  

난이도 : ⭐  

> 문제  

숫자 카드 게임은 여러 개의 숫자 카드 중에서 가장 높은 숫자가 쓰인 카드 한장을 뽑는 게임이다.  
단, 게임의 룰을 지키며 카드를 뽑아야 하고 룰은 다음과 같다.  
- 숫자가 쓰인 카드들이 N X M 형태로 놓여 있다. 이때 N은 행의 개수를 의미하며, M은 열의 개수를 의미한다.  
- 먼저 뽑고자 하는 카드가 포함되어 있는 행을 선택한다.  
- 그다음 선택된 행에 포함된 카드들 중 가장 숫자가 낮은 카드를 뽑아야 한다.  
- 따라서 처음에 카드를 골라낼 행을 선택할 때, 이후에 해당 행에서 가장 숫가자 낮은 카드를 뽑을 것을 고려하여 최종적으로 가장 높은 숫자의 카드를 뽑을 수 있도록 전략을 세워야 한다.  

예를 들어, 3 X 3 형태로 카드들이 다음과 같이 놓여 있다고 가정하자.  

![image](https://user-images.githubusercontent.com/37467408/121828784-909d0800-ccfb-11eb-90df-651fd8c34eb6.PNG)  

> 나의 문제 풀이  


> 문제 해설  

`Greedy Algorithm` 유형의 문제는 문제 해결을 위한 아이디어를 떠올리는게 중요하다.  
이 문제는 `각 행마다 가장 작은 수를 찾은 뒤에 그 수 중에서 가장 큰 수` 찾는 것이다.  
이 문제를 해결하기 위해서는 리스트에서 가장 작은 원소를 찾아주는 min() 함수를 이용하거나, 2중 반복문 구조를 이용해야 한다. 나는 min()함수를 이용해서 이 문제를 풀었는데 책과 똑같이 풀어서 나의 풀이는 Skip 했다.

> min() 함수를 이용하는 답안

```python
# N, M을 공백으로 구분하여 입력받기
n, m = map(int, input().split())

result = 0
# 한 줄씩 입력받아 확인
for i in range(n):
    data = list(map(int, input().split()))
    # 현재 줄에서 '가장 적은 수' 찾기
    min_value = min(data):
    # '가장 적은 수'들 중에서 가장 큰 수 찾기
    result = max(result, min_value)

print(result) # 최종 답안 출력
```

> 2중 반복문 구조를 이용하는 답안

```python
# N, M을 공백으로 구분하여 입력받기
n, m = map(int, input().split())

result = 0
# 한 줄씩 입력받아 확인
for i in range(n):
    data = list(map(int, input().split()))
    # 현재 줄에서 '가장 적은 수' 찾기
    min_value = 10001
    for a in data:
        min_value = min(min_value, a)
    # '가장 적은 수'들 중에서 가장 큰 수 찾기
    result = max(result, min_value)

print(result) # 최종 답안 출력
```
<br>
기출 : 2019 국가 교육기관 코딩 테스트  


---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
