---
title: "[Dynamic Programing] 금광"

categories:
  - Algorithm
tags:
  - [Dynamic Programing, Python, Flipkart]

toc:  true
toc_sticky: true

date: 2021-11-09
last_modified_at: 2021-11-09
---

## 금광  

난이도 : 🌕🌗  
푼횟수 : 🟢⚪⚪  

n x m 크기의 금광이 있습니다. 금광은 1 x 1 크기의 칸으로 나누어져 있으며, 각 칸은 특정한 크기의 금이 들어 있습니다. 채굴자는 첫 번째 열부터 출발하여 금을 캐기 시작합니다. 맨 처음에는 첫 번째 열의 어느 행에서든 출발할 수 있습니다. 이후에 m번에 걸쳐서 매번 오른쪽 위, 오른쪽, 오른쪽 아래 3가지 중 하나의 위치로 이동해야 합니다. 결과적으로 채굴자가 얻을 수 있는 금의 최대 크기를 출력하는 프로그램을 작성하세요.  
만약 다음과 같이 3 x 4 크기의 금광이 존재한다고 가정합시다.  

|---|---|---|---|
| 1 | 3 | 3 | 2 |
| 2 | 1 | 4 | 1 |
| 0 | 6 | 4 | 7 |  

가장 왼쪽 위의 위치를 (1, 1), 가장 오른쪽 아래의 위치를 (n, m)이라고 할 때, 위 예시에서는 (2, 1) -> (3, 2) -> (3, 3) -> (3, 4)의 위치로 이동하면 총 19만큼의 금을 채굴할 수 있으며, 이때의 값이 최댓값입니다.  

**<u>입력 조건</u>**  
- 첫째 줄에 테스트 케이스 T가 입력됩니다. (1 <= T <= 1000)  
- 매 테스트 케이스 첫째 줄에 n과 m이 공백으로 구분되어 입력됩니다. (1 <= n, m <= 20) 둘째 줄에 n x m개의 위치에 매장된 금의 개수가 공백으로 구분되어 입력됩니다. (1 <= 각 위치에 매장된 금의 개수 <= 100)  

**<u>출력 조건</u>**  
- 테스트 케이스마다 채굴자가 얻을 수 있는 금의 최대 크기를 출력합니다. 각 테스트 케이스는 줄 바꿈을 이용해 구분합니다.  

**<u>입력 예시</u>**  
2  
3 4  
1 3 3 2 2 1 4 1 0 6 4 7  
4 4  
1 3 1 5 2 2 4 1 5 0 2 3 0 6 1 2  

**<u>출력 예시</u>**  
19  
16  

> 나의 풀이  

```python
# 테스트 케이스 입력
for tc in range(int(input())):
    # 금광 정보 입력
    n, m = map(int, input().split())
    array = list(map(int, input().split()))

    # 다이나믹 프로그래밍을 위한 2차원 DP 테이블 초기화
    dp = []
    index = 0
    for i in range(n):
        dp.append(array[index:index + m])
        index += m

    # 다이나믹 프로그래밍 진행
    for j in range(1, m):
        for i in range(n):
            # 왼쪽 위에서 오는 경우
            if i == 0:
                left_up = 0
            else:
                left_up = dp[i - 1][j - 1]

            # 왼쪽 아래에서 오는 경우
            if i == n - 1:
                left_down = 0
            else:
                left_down = dp[i + 1][j - 1]
            # 왼쪽에서 오는 경우
            left = dp[i][j - 1]
            dp[i][j] = dp[i][j] + max(left_up, left_down, left)

    result = 0
    for i in range(n):
        result = max(result, dp[i][m - 1])

    print(result)
```


> 문제 해설  

![image](https://user-images.githubusercontent.com/37467408/140848544-5983e18e-892f-44cf-b23c-4c2ec8b695f1.png)  

금광 정보  
이때 초기에 DP 테이블은 다음과 같이 구성된다. 제일 왼쪽에 있는 열 중 아무 곳에서나 출발할 수 있으므로 왼쪽 열만 그대로 값을 남겨두면 된다.  

![image](https://user-images.githubusercontent.com/37467408/140848674-49b12a37-02cd-4840-b82f-b04982189d6f.png)  

다이나믹 프로그래밍 테이블 초기 설정  

![image](https://user-images.githubusercontent.com/37467408/140848742-352d952c-682a-4dee-9a7f-3964c2946490.png)  

(1, 2)위치에서의 최대 이익  
(1, 2)위치에서의 최대 이익은, 바로 왼쪽에서 오는 경우와 왼쪽 아래에서 오는 경우 2가지를 고려해야 한다. 이 중에서 왼쪽 아래에서 오는 경우가 '2'로 이익이 더 크기 때문에 2(왼쪽 아래) + 3(자신의 위치) = 5가 최대 이익이 된다.  

![image](https://user-images.githubusercontent.com/37467408/140848896-2abf8e8d-d3a1-40bc-83bc-cb003170f709.png)  

(2, 2)위치에서의 최대 이익  
(2, 2)위치에서의 최대 이익은, 왼쪽 위에서 오는 경우, 왼쪽에서 오는 경우, 왼쪽 아래에서 오는 경우 3가지를 고려해야 한다. 이 중에서 왼쪽에서 오는 경우가 '2'로 이익이 가장 크기 때문에 2(왼쪽) + 1(자신의 위치) = 3이 최대 이익이 된다.  
이제 이러한 방식대로, 가장 왼쪽에 있는 열부터 차례대로 DP테이블을 갱신해 나가면 최종적으로 DP 테이블은 다음과 같이 구성된다.  

![image](https://user-images.githubusercontent.com/37467408/140849157-bcae1513-0435-4fe7-9f7a-d44adf60699e.png)  

최종적인 다이나믹 프로그래밍 테이블  


<br>
기출 : Filpkart 인터뷰

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊