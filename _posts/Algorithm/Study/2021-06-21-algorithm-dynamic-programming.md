---
title: "[Dynamic Programming] 한 번 계산한 문제는 다시 계산하지 않는 알고리즘"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Dynamic Programming]

toc:  true
toc_sticky: true

date: 2021-06-21
last_modified_at: 2021-06-21
---

## 중복되는 연산을 줄이자  

컴퓨터로 해결하기 어려운 문제는 최적의 해를 구하기에 시간이 많이 필요하거나 메모리 공간이 매우 많이 필요한 문제이다. 왜냐하면 컴퓨터는 연산 속도에 한계가 있고, 메모리 공간을 사용할 수 있는 데이터의 개수도 한정적이기 때문이다. 그래서 연산속도와 메모리 공간을 최대한으로 활용할 수 있는 효율적인 알고리즘을 작성해야한다.  

다이나믹 프로그램으로 해결할 수 있는 대표적인 문제가 피보나치 수열이다. 아래는 재귀함수를 이용한 파이썬 코드이다.  

![image](https://user-images.githubusercontent.com/37467408/122693247-052ff380-d274-11eb-90d8-17f1b998b7ca.PNG)  

```python
# 피보나치 함수를 재귀 함수로 구현
def fibo(x):
  if x == 1 or x == 2:
    return 1
  return fibo(x - 1) + fibo(x - 2)

print(fibo(4))
```

위와 같은 방식으로 코드를 작성 할 경우, x가 커지면 수행 시간이 기하급수적으로 커지는 문제가 있다. 이러한 문제는 다이나믹 프로그래밍을 사용하면 효율적으로 해결할 수 있다. 다만 항상 사용이 가능한 것은 아니고, 다음 조건을 만족할 경우 사용이 가능하다.  
- 큰 문제를 작은 문제로 나눌 수 있다.  
- 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일하다.  

이제, 이 문제를 메모이제이션(Meomization) 기법을 사용해서 해결해보자. 메모이제이션은 다이나믹 프로그래밍을 구현하는 방법 중 한 종류로, 한 번 구한 결과를 메모리 공간에 메모해두고 같은 식을 다시 호출하면 메모한 결과를 그대로 가져오는 기법을 의미한다. 메모이제이션은 값을 저장하는 방법이므로 캐싱(caching)이라고도 한다. 메모이제이션을 구현하는 방법은 한 번 구한 정보를 리스트에 저장하는 것이다. 프로그래밍을 재귀적으로 수행하다가 같은 정보가 필요할 경우, 이미 구한 정답을 그대로 리스트에서 가져오면 된다. 이를 파이썬 코드로 나타내면 아래와 같다.  

```python
# 한 번 계산된 결과를 메모이제이션(Memoization)하기 위한 리스트 초기화
d = [0] * 100

# 피보나치 함수를 재귀적으로 구현(탑다운 다이나믹 프로그래밍)
def fibo(x):
  # 종료 조건(1혹은 2일 때 1을 반환)
  if x == 1 or x == 2:
    return 1
  # 이미 계산한 적  있는 문제라면 그대로 반환
  if d[x] != 0:
    return d[x]
  # 아직 계산하지 않은 문제라면 점화식에 따라서 피보나치 결과 반환
  d[x] = fibo(x - 1) + fibo(x - 2)
  return d[x]

  print(fibo(99))
```  

결론적으로, 다이나믹 프로그래밍이란 큰 문제를 작게 나누고, 같은 문제라면 한 번씩만 풀어 문제를 효율적으로 해결하는 알고리즘 기법이다. 퀵 정렬도 큰 문제를 작게 나누지만 두 알고리즘의 차이는 다이나믹 프로그랭은 문제들이 서로 영향을 미치고 있다는 점이다.  
예를 들어, 퀵 정렬은 한 번 기준 원소(Pivot)가 자리를 변경해서 자리를 잡게 되면 그 기준 원소의 위치는 더 이상 바뀌지 않고 그 Pivot 값을 다시 처리하는 부분 문제는 존재하지 않는다. 반면에 다이나믹 프로그래밍은 한 번 해결했던 문제를 다시금 해결한다는 점이 특징이다.  
f(6)을 해법을 다시 메모이제이션 기법을 이용하여 그려보면 6번째 피보나치 수를 호출할 때는 다음 그림처럼 색칠된 노드만 방문하게 된다.  

![image](https://user-images.githubusercontent.com/37467408/122693295-36a8bf00-d274-11eb-9936-9a1fc9062c29.PNG)  

재귀함수를 사용하면 컴퓨터 시스템에서는 함수를 다시 호출했을 때 메모리 상에 적재되는 일련의 과정을 따라야 하므로 오버헤드가 발생할 수 있다. 따라서 재귀 함수 대신에 반복문을 사용하여 오버헤드를 줄일 수 있다. 일반적으로 반복문을 이용한 다이나믹 프로그래밍이 더 성능이 좋다.  
다이나믹 프로그래밍을 이용한 피보나치 수열의 시간 복잡도는 O(N)이다. 실제로 호출되는 함수에 대해서만 확인해보면 아래와 같다.  

![image](https://user-images.githubusercontent.com/37467408/122693394-a61eae80-d274-11eb-8fc5-d685be4b52a1.PNG)  

아래의 코드를 통해 시간 복잡도가 O(N)이라는 것을 이해할 수 있다.  

```python
d = [0] * 100

def pibo(x):
  print('f(' + str(x) +')', end = ' ')
  if x == 1 or x == 2:
    return 1
  if d[x] != 0:
    return d[x]
  d[x] = pibo(x - 1) + pibo(x - 2)

  return d[x]
pibo(6)
```

이처럼 재귀 함수를 이용하여 다이나믹 프로그래밍 소스코드를 작성하는 방법을, 큰 문제를 해결하기 위해 작은 문제를 호출한다고 하여 `탑다운 방식`이라고 말한다. 반면에 단순히 반복문을 이용하여 소스코드를 작성하는 경우 작은 문제부터 차근차근 답을 도출한다고 하여 `보텀업 방식`이라고 말한다. 수열 문제에서 아래에서 위로 올라가는 보텀업 방식으로 풀면 다음과 같다.  

```python
# 앞서 계산된 결과를 저장하기 위한 DP 테이블 초기화
d = [0] * 100

# 첫 번째 피보나치 수와 두 번째 피보나치 수는 1
d[1] = 1
d[2] = 1
n = 99

# 피보나치 함수 반복문으로 구현(보텀업 다이나믹 프로그래밍)
for i in range(3, n + 1):
  d[i] = d[i - 1] + d[i - 2]

print(d[n])
```  

탑다운(메모이제이션) 방식은 '하향식'이라고도 하며, 보텀업 방식은 '상향식'이라고도 한다. 다이나믹 프로그랭의 전형적인 형태는 보텀업 방식이다. 보텀업 방식에서 사용되는 결과 저장용 리스트는 'DP 테이블'이라고도 부르며, 메모이제이션은 탑다운 방식에 국한되어 사용되는 표현이다. 메모이제이션은 이전에 계산된 결과를 일시적으로 기록해 놓는 넓은 개념을 의미하므로, 다이나믹 프로그래밍과는 별도의 개념이다.  

문제를 푸는 첫 번째 단계는 주어진 문제가 다이나믹 프로그래밍 유형임을 파악하는 것이다. 특정한 문제를 완전 탐색 알고리즘으로 접근했을 때 시간이 매우 오래 걸리면 다이나믹 프로그래밍을 적용할 수 있는지 해결하고자 하는 부분 문제들의 중복 여부를 확인해보자.  


---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
