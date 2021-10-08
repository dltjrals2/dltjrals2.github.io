---
title: "[DFS/BFS] 음료수 얼려 먹기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS, BFS]

toc:  true
toc_sticky: true

date: 2021-06-16
last_modified_at: 2021-06-16
---
## 음료수 얼려 먹기  

난이도 : ⭐⭐  

N X M 크기의 얼음 틀이 있다. 구멍이 뚫려 있는 부분은 0, 칸막이가 존재하는 부분은 1로 표시된다. 구멍이 뚫려 있는 부분끼리 상, 하, 좌, 우로 붙어 있는 경우 서로 연결되어 있는 것으로 간주한다. 이때 얼음 틀의 모양이 주어졌을 때 생성되는 총 아이스크림의 개수를 구하는 프로그램을 작성하시오. 다음의 4 X 5 얼음 틀 예시에서는 아이스크림 총 3개 생성된다.  

![image](https://user-images.githubusercontent.com/37467408/122177034-30a38e80-cec0-11eb-8155-eb3a20904518.PNG)  


**<u>입력 조건</u>**  
- 첫 번째 줄에 얼음 틀의 세로 길이 N과 가로 길이 M이 주어진다. (1 <= N, M <= 1000)  
- 두 번째 줄부터 N + 1번째 줄까지 얼음 틀의 형태가 주어진다.  
- 이때 구멍이 뚫려있는 부분은 0, 그렇지 않은 부분은 1이다.    

**<u>출력 조건</u>**  
- 한 번에 만들 수 있는 아이스크림의 개수를 출력한다.  

**<u>입력 예시</u>**  

![image](https://user-images.githubusercontent.com/37467408/122177391-8841fa00-cec0-11eb-8220-210edce8b31b.PNG)  

**<u>출력 예시</u>**  
8  

> 나의 풀이  

```python
n, m = 4, 5

graph = [
    [0, 0, 1, 1, 0],
    [0, 0, 0, 1, 1],
    [1, 1, 1, 1, 1],
    [0, 0, 0, 0, 0]
]

def dfs(x, y):
    if x <= -1 or x >= n or y <= -1 or y >= m:
        return False
    if graph[x][y] == 0:
        graph[x][y] = 1
        dfs(x - 1, y)
        dfs(x + 1, y)
        dfs(x, y - 1)
        dfs(x, y + 1)
        return True
    return False

count = 0
for i in range(n):
    for j in range(m):
        if dfs(i, j) == True:
            count += 1

print(count)
```

> 문제 해설  

이 문제는 DFS로 해결할 수 있다. 얼음을 얼릴 수 있는 공간이 상, 하, 좌, 우로 연결되어 있다고 표현할 수 있으므로 그래프 형태로 모델링 할 수 있다.  

- 특정한 지점의 주변 상, 하, 좌, 우를 살펴본 뒤에 주변 지점 중에서 값이 '0'이면서 아직 방문하지 않은 지점이 있다면 해당 지점을 방문한다.  
- 방문한 지점에서 다시 상, 하, 좌, 우를 살펴보면서 방문을 다시 진행하면, 연결된 모든 지점을 방문할 수 있다.  
- 위 과정을 반복하면서 방문하지 않은 지점의 수를 센다.  

> 답안 예시  

```python
# N, M을 공백으로 구분하여 입력받기
n, m = map(int ,input().split())

# 2차원 리스트의 맵 정보 입력받기
graph = []
for i in range(n):
    graph.append(list(map(int, input())))

# DFS로 특정한 노드를 방문한 뒤에 연결된 모든 노드들도 방문
def dfs(x, y):
    # 주어진 범위를 벗어나는 경우에는 즉시 종료
    if x <= -1 or x >= n or y <= -1 or y >= m:
        return False
    # 현재 노드를 아직 방문하지 않았다면
    if graph[x][y] == 0:
        # 해당 노드 방문 처리
        graph[x][y] = 1
        # 상, 하, 좌, 우의 위치도 모두 재귀적으로 호출
        dfs(x - 1, y)
        dfs(x , y - 1)
        dfs(x + 1, y)
        dfs(x, y + 1)
        return True
    return False

# 모든 노드에 대하여 음료수 채우기
result = 0
for i in range(n):
    for j in range(m):
        # 현재 위치에서 DFS 수행
        if dfs(i, j) == True:
            result += 1

# 정답 출력
print(result)
```

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
