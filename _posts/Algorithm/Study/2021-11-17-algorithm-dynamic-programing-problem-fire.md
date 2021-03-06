---
title: "[Dynamic Programing] 퇴사"

categories:
  - Algorithm
tags:
  - [Dynamic Programing, Python]

toc:  true
toc_sticky: true

date: 2021-11-17
last_modified_at: 2021-11-17
---

## 퇴사  

난이도 : 🌕🌕  
푼횟수 : 🟢⚪⚪  

상담원으로 일하고 있는 백준이는 퇴사를 하려고 한다.  

오늘부터 N+1일째 되는 날 퇴사를 하기 위해서, 남은 N일 동안 최대한 많은 상담을 하려고 한다.  

백준이는 비서에게 최대한 많은 상담을 잡으라고 부탁을 했고, 비서는 하루에 하나씩 서로 다른 사람의 상담을 잡아놓았다.  

각각의 상담은 상담을 완료하는데 걸리는 기간 Ti와 상담을 했을 때 받을 수 있는 금액 Pi로 이루어져 있다.  

N = 7인 경우에 다음과 같은 상담 일정표를 보자.  

![image](https://user-images.githubusercontent.com/37467408/142090927-9084875d-b4a0-46b3-b87c-271a19accd29.png)  

1일에 잡혀있는 상담은 총 3일이 걸리며, 상담했을 때 받을 수 있는 금액은 10이다. 5일에 잡혀있는 상담은 총 2일이 걸리며, 받을 수 있는 금액은 15이다.  

상담을 하는데 필요한 기간은 1일보다 클 수 있기 때문에, 모든 상담을 할 수는 없다. 예를 들어서 1일에 상담을 하게 되면, 2일, 3일에 있는 상담은 할 수 없게 된다. 2일에 있는 상담을 하게 되면, 3, 4, 5, 6일에 잡혀있는 상담은 할 수 없다.  

또한, N+1일째에는 회사에 없기 때문에, 6, 7일에 있는 상담을 할 수 없다.  

퇴사 전에 할 수 있는 상담의 최대 이익은 1일, 4일, 5일에 있는 상담을 하는 것이며, 이때의 이익은 10+20+15=45이다.  

상담을 적절히 했을 때, 백준이가 얻을 수 있는 최대 수익을 구하는 프로그램을 작성하시오.  

**<u>입력</u>**  
- 첫째 줄에 N (1 ≤ N ≤ 15)이 주어진다.  

- 둘째 줄부터 N개의 줄에 Ti와 Pi가 공백으로 구분되어서 주어지며, 1일부터 N일까지 순서대로 주어진다. (1 ≤ Ti ≤ 5, 1 ≤ Pi ≤ 1,000)  

**<u>출력</u>**  
- 첫째 줄에 백준이가 얻을 수 있는 최대 이익을 출력한다.  

**<u>예제 입력1</u>**  
7  
3 10  
5 20  
1 10  
1 20  
2 15  
4 40  
2 200  

**<u>예제 출력1</u>**  
45  

**<u>예제 입력2</u>**  
10  
1 1  
1 2  
1 3  
1 4  
1 5  
1 6  
1 7  
1 8  
1 9  
1 10  

**<u>예제 출력2</u>**  
45  

> 나의 풀이  

```python
n = int(input())
table = []
dp = [0] * (n + 1)
max_value = 0

for _ in range(n):
    table.append(list(map(int, input().split())))

for i in range(n - 1, -1, -1):
    time = table[i][0] + i
    if time <= n:
        dp[i] = max(table[i][1] + dp[time], max_value)
        max_value = dp[i]
    else:
        dp[i] = max_value

print(max_value)
```


> 문제 해설  

이 문제를 풀 때는 뒤쪽 날짜부터 거꾸로 확인하는 방식으로 접근하여 해결하는 다이나믹 프로그래밍의 아이디어를 떠올릴 수 있다.  

![image](https://user-images.githubusercontent.com/37467408/142090927-9084875d-b4a0-46b3-b87c-271a19accd29.png)  

1일 차에 상담을 진행한다고 해보면, 이 경우 3일에 걸쳐서 상담을 진행해야 한다. 결과적으로 4일부터 다시 상담을 진행할 수 있다. 그러므로 1일 차에 상담을 진행하는 경우, 최대 이익은 '1일 차의 상담 금액 + 4일부터의 최대 상담 금액'이 된다.  

이러한 원리를 이용하여 뒤쪽 날짜부터 거꾸로 계산하며 문제를 해결할 수 있다. 즉, 뒤쪽부터 매 상담에 대하여 '현재 상담 일자의 이윤(p[i]) + 현재 상담을 마친 일자부터의 최대 이윤(dp[t[i] + i])'을 계산하면 된다. 이후에 계산된 각각의 값 중에서 최댓값을 출력하면 된다.  

'`dp[i] = i번째 날부터 마지막 날까지 낼 수 있는 최대 이익`'이라고 하면 점화식은 dp[i] = max(p[i] + dp[t[i] + i], max_value)가 된다. 이때 max_value는 뒤에서부터 계산할 때, 현재까지의 최대 상담금액에 해당하는 변수이다.  

> 문제 답안  

```python
n = int(input()) # 전체 상담 개수
t = [] # 각 상담을 완료하는 데 걸리는 기간
p = [] # 각 상담을 완료했을 때 받을 수 있는 금액
dp = [0] * (n + 1) # 다이나믹 프로그래밍을 위한 1차원 dp 테이블 초기화
max_value = 0

for _ in range(n):
    x, y = map(int, input().split())
    t.append(x)
    p.append(y)

# 리스트를 뒤에서부터 거꾸로 확인
for i in range(n - 1, -1, -1):
    time = t[i] + i
    # 상담이 기간 안에 끝나는 경우
    if time <= n:
        # 점화식에 맞게, 현재까지의 최고 이익 계산
        dp[i] = max(p[i] + dp[time], max_value)
        max_value = dp[i]
    # 상담이 기간을 벗어나는 경우
    else:
        dp[i] = max_value

print(max_value)
```

<br>
기출 : 삼성전자 SW 역량테스트  
링크 : <https://www.acmicpc.net/problem/14501>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
