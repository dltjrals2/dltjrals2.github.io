---
title: "[Dynamic Programing] 정수 삼각형"

categories:
  - Algorithm
tags:
  - [Dynamic Programing, Python]

toc:  true
toc_sticky: true

date: 2021-11-10
last_modified_at: 2021-11-10
---

## 정수 삼각형  

난이도 : 🌕🌗  
푼횟수 : 🟢⚪⚪  

![image](https://user-images.githubusercontent.com/37467408/141035033-42b940bd-65c5-4bed-a93f-c6b50356ceb1.png)  

위 그림은 크기가 5인 정수 삼각형의 한 모습이다.  
맨 위층 7부터 시작해서 아래에 있는 수 중 하나를 선택하여 아래층으로 내려올 때, 이제까지 선택된 수의 합이 최대가 되는 경로를 구하는 프로그램을 작성하라. 아래층에 있는 수는 현재 층에서 선택된 수의 대각선 왼쪽 또는 대각선 오른쪽에 있는 것 중에서만 선택할 수 있다.  
삼각형의 크기는 1 이상 500 이하이다. 삼각형을 이루고 있는 각 수는 모두 정수이며, 범위는 0 이상 9999 이하이다.  

**<u>입력</u>**  
- 첫째 줄에 삼각형의 크기 n(1 ≤ n ≤ 500)이 주어지고, 둘째 줄부터 n+1번째 줄까지 정수 삼각형이 주어진다.  

**<u>출력</u>**  
- 첫째 줄에 합이 최대가 되는 경로에 있는 수의 합을 출력한다.  

**<u>예제 입력1</u>**  
5  
7  
3 8  
8 1 0  
2 7 4 4  
4 5 2 6 5  

**<u>예제 출력1</u>**  
30  

> 나의 풀이  

```python
n = int(input())
array = []

for _ in range(n):
    array.append(list(map(int, input().split())))

#Dynamic Programming
for i in range(1, n):
    for j in range(len(array[i])):
        # 왼쪽에서 오는 경우
        if j == 0:
            left = 0
        else:
            left = array[i - 1][j - 1]
        # 오른쪽에서 오는 경우
        if j == len(array[i]) - 1:
            right = 0
        else:
            right = array[i - 1][j]

        array[i][j] = array[i][j] + max(left, right)


result = 0
for i in range(n):
    for j in range(len(array[i])):
        result = max(result, array[i][j])

print(result)
```

> 문제 해설  

특정한 위치로 도달하기 위해서는 1) '왼쪽 위' 혹은 2) '바로 위' 2가지 위치에서만 내려올 수 있다.  
따라서 모든 위치를 기준으로 이전 위치로 가능한 2가지 위치까지의 최적의 합 중에서 더 큰 합을 가지는 경우를 선택하면 된다.  
단, dp테이블에 접근해야 할 때마다 리스트의 범위를 벗어나지 않는지 체크할 필요가 있다.  

> 문제 답안  

```python
n = int(input())
dp = [] # 다이나믹 프로그래밍을 위한 DP 테이블 초기화

for _ in range(n):
    dp.append(list(map(int, input().split())))

# 다이나믹 프로그래밍으로 두 번째 줄부터 내려가면서 확인
for i in range(1, n):
    for j in range(i + 1):
        # 왼쪽 위에서 내려오는 경우
        if j == 0:
            up_left = 0
        else:
            up_left = dp[i - 1][j - 1]
        # 바로 위에서 내려오는 경우
        if j == i:
            up = 0
        else:
            up = dp[i - 1][j]
        # 최대 힙을 저장
        dp[i][j] = dp[i][j] + max(up_left, up)

print(max(dp[n - 1]))
```

<br>
기출 : IOI 1994년도  
링크 : <https://www.acmicpc.net/problem/1932>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊