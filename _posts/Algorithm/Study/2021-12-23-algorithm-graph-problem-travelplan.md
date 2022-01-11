---
title: "[Graph] 여행 계획"

categories:
  - Algorithm
tags:
  - [Graph, Python, 풀이 참고]

toc:  true
toc_sticky: true

date: 2021-12-23
last_modified_at: 2021-12-23
---

## 여행 계획  

난이도 : 🌕🌕   
푼횟수 : 🟡⚪⚪  

한울이가 사는 나라에는 N개의 여행지가 있으며, 각 여행지는 1 ~ N번까지의 번호로 구분됩니다. 또한 임의의 두 여행지 사이에는 두 여행지를 연결하는 도로가 존재할 수 있습니다. 이때, 여행지가 도로로 연결되어 있다면 양방향으로 이동이 가능하다는 의미입니다. 한울이는 하나의 여행 계획을 세운 뒤에 이 여행 계획이 가능한지의 여부를 판단하고자 합니다. 예를 들어 N = 5이고, 다음과 같이 도로의 정보가 주어졌다고 가정합시다.  

- 1번 여행지 - 2번 여행지  
- 1번 여행지 - 4번 여행지  
- 1번 여행지 - 5번 여행지  
- 2번 여행지 - 3번 여행지  
- 2번 여행지 - 4번 여행지  

만약 한울이의 여행 계획이 2번 -> 3번 -> 4번 -> 3번이라고 해봅시다. 이 경우 2번 -> 3번 -> 2번 -> 4번 -> 2번 -> 3번의 순서로 여행지를 방문하면, 여행 계획을 따를 수 있습니다.  
여행지의 개수와 여행지 간의 연결 정보가 주어졌을 때, 한울이의 여행 계획이 가능한지의 여부를 판별하는 프로그램을 작성하세요.  

**<u>입력 조건</u>**  
- 첫째 줄에 여행지의 수 N과 여행 계획에 속한 도시의 수 M이 주어집니다. (1 <= N, M <= 500)  
- 다음 N개의 줄에 걸쳐 N x M 행렬을 통해 임의의 두 여행지가 서로 연결되어 있는지의 여부가 주어집니다. 그 값이 1이라면 서로 연결되었다는 의미이며, 0이라면 서로 연결되어 있지 않다는 의미입니다.  
- 마지막 줄에 한울이의 여행 계획에 포함된 여행지의 번호들이 주어지며 공백으로 구분합니다.  

**<u>출력 조건</u>**  
- 첫째 줄에 한울이의 여행 계획이 가능하다면 YES를, 불가능하다면 NO를 출력합니다.  

**<u>입력 예시</u>**  
5 4  
0 1 0 1 1  
1 0 1 1 0  
0 1 0 0 0  
1 1 0 0 0  
1 0 0 0 0  
2 3 4 3  

**<u>출력 예시</u>**  
YES  

> 나의 풀이  

> 문제 해설  

문제에서 등장하는 여행지의 관계를 그래프 형태로 그린다고 하면, 기본적으로 양방향 간선을 가진 그래프로 표현할 수 있다. 다음 그래프는 문제 설명 중에서 도로 정보를 그림으로 표현한 것이다.  

![image](https://user-images.githubusercontent.com/37467408/147054234-6069de8c-c943-418e-abaa-eb385d392182.png)  

위 그림에서 여행 계획이 2번 -> 3번 -> 4번 -> 3번일 때는 2번 -> 3번 -> 2번 -> 4번 -> 2번 -> 3번 순서대로 이동하면 된다. 따라서 해당 여행 계획을 지킬 수 있으므로, 결과는 "YES"가 된다.  
이 문제는 서로소 집합 알고리즘을 이용하여, 그래프에서 노드 간의 연결성을 파악해 해결할 수 있다. 우리가 여기에서 알 수 있는 점은, `여행 계획에 해당하는 모든 노드가 같은 집합에 속하기만 하면 가능한 여행 경로`라는 것이다. 따라서 두 노드 사이에 도로가 존재하는 경우에는 union 연산을 이용해서, 서로 연결된 두 노드를 같은 집합에 속하도록 만든다.  
결과적으로 입력으로 들어온 "여행 계획"에 포함되는 모든 노드가 같은 집합에 속하는지를 체크하여 출력하면 정답 판정을 받을 수 있다. 즉, 서로소 집합 자료구조를 이용하여 문제를 해결할 수 있다.  

> 문제 답안  

```python
# 특정 원소가 속한 집합을 찾기
def find_parent(parent, x):
  # 루트 노드가 아니라면, 루트 노드를 찾을 때까지 재귀적으로 호출
  if parent[x] != x:
    parent[x] = find_parent(parent, parent[x])
  return parent[x]

# 두 원소가 속한 집합을 합치기
def union_parent(parent, a, b):
  a = find_parent(parent, a)
  b = find_parent(parent, b)
  if a < b:
    parent[b] = a
  else:
    parent[a] = b

# 여행지의 개수와 여행 계획에 속한 여행지의 개수 입력받기
n, m = map(int, input().split())
parent = [0] * (n + 1) # 부모 테이블 초기화

# 부모 테이블상에서, 부모를 자기 자신으로 초기화
for i in range(1, n + 1):
  parent[i] = i

# union 연산을 각각 수행
for i in range(n):
  data = list(map(int, input().split()))
  for j in range(n):
    if data[j] == 1: # 연결된 경우 union 연산을 수행
      union_parent(parent, i + 1, j + 1)

# 여행 계획 입력받기
plan = list(map(int, input().split()))

result = True
# 여행 계획에 속하는 모든 노드의 루트가 동일한지 확인
for j in range(m - 1):
  if find_parent(parent, plan[i]) != find_parent(parent, plan[i + 1]):
    result = False

# 여행 계획에 속하는 모든 노드가 서로 연결되어 있는지(루트가 동일한지) 확인
if result:
  print("YES")
else:
  print("NO")
```

<br>
기출 : 핵심유형  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊