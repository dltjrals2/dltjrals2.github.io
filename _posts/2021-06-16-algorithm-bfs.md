---
title: "[BFS] 탐색 알고리즘 BFS"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, BFS]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-16
last_modified_at: 2021-06-16
sitemap :
  changefreq : daily
  priority : 1.0
---

## 탐색 알고리즘 BFS  

BFS(Breadth First Search) 알고리즘은 `너비 우선 탐색`이라는 의미를 가진다. 즉, `가까운 노드부터 탐색하는 알고리즘`이다. BFS 구현에서는 선입선출 방식인 큐 자료구조를 이용하는 것이 정석이다.  

알고리즘의 정확한 동작 방식은 다음과 같다.  
- 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.  
- 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다.  
- 위 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.  

**<u>step 1</u>** : 시작 노드인 '1'을 큐에 삽입하고 방문 처리를 한다. 방문 처리된 노드는 회색으로, 큐에서 꺼내 현재 처리하는 노드는 하늘색으로 표현했다.  

![image](https://user-images.githubusercontent.com/37467408/122161137-1f508700-ceac-11eb-8fe6-13d604b0640d.PNG)  

**<u>step 2</u>** : 큐에서 노드 '1'을 꺼내고 방문하지 않은 인접 노드 '2', '3', '8'을 모두 큐에 삽입하고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122161215-47d88100-ceac-11eb-9a62-db43a1d53775.PNG)  

**<u>step 3</u>** : 큐에서 노드 '2'를 꺼내고 방문하지 않은 인접 노드 '7'을 큐에 삽입하고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122161282-63dc2280-ceac-11eb-9111-0e4147f8b386.PNG)  

**<u>step 4</u>** : 큐에서 노드 '3'을 꺼내고 방문하지 않은 인접 노드 '4'와 '5'를 모두 큐에 삽입하고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122161337-840be180-ceac-11eb-9fa4-c1e5e4d3c879.PNG)  

**<u>step 5</u>** : 큐에서 노드 '8'을 꺼내고 방문하지 않은 인접 노드가 없으므로 무시한다.  

![image](https://user-images.githubusercontent.com/37467408/122161396-9b4acf00-ceac-11eb-9b74-99ca1777bd71.PNG)  

**<u>step 6</u>** : 큐에서 노드 '7'을 꺼내고 방문하지 않은 인접 노드 '6'을 큐에 삽입하고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122161461-b87f9d80-ceac-11eb-92e0-0e0e468acef8.PNG)  

**<u>step 7</u>** : 남아 있는 노드에 방문하지 않은 인접 노드가 없다. 따라서 모든 노드를 차례대로 꺼내면 최종적으로 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/122161520-d2b97b80-ceac-11eb-8935-0d585adbf7d8.PNG)  

결과적으로 노드의 탐색 순서는 다음과 같다.  
1 -> 2 -> 3 -> 8 -> 7 -> 4 -> 5 -> 6  

너비 우선 탐색 알고리즘은 큐 자료구조에 기초한다는 점에서 구현이 간단하다. 구현함에 있어서 deque 라이브러리를 사용하는 것이 좋으며 탐색을 수행함에 있어서 O(N)의 시간이 소요된다. 일반적으로 실제 수행 시간은 `DFS보다 좋은 편`이라는 점을 기억하자.  

```python
from collections import deque

# BFS 메서드 정의
def bfs(graph, start, visited):
  # 큐 구현을 위해 deque 라이브러리 사용
  queue = deque([start])
  # 현재 노드를 방문 처리
  visited[start] = True
  # 큐가 빌 때까지 반복
  while queue:
    # 큐에서 하나의 원소를 뽑아 출력
    v = queue.popleft()
    print(v, end = ' ')
    # 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입
    for i in graph[v]:
      if not visited[i]:
        queue.append(i)
        visited[i] = True

# 각 노드가 연결된 정보를 리스트 자료형으로 표현(2차원 리스트)
graph = [
  [],
  [2, 3, 8],
  [1, 7],
  [1, 4, 5],
  [3, 5],
  [3, 4],
  [7],
  [2, 6, 8],
  [1, 7]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)
visited = [False] * 9

# 정의된 BFS 함수 호출
bfs(graph, 1, visited)
```  

DFS와 BFS에 대해 간단히 정리하자면 아래 표와 같다.  

![image](https://user-images.githubusercontent.com/37467408/122162021-c124a380-cead-11eb-97d5-839c09a43d33.PNG)  

1차원 배열이나 2차원 배열 또한 그래프 형태로 생각하면 수월하게 문제를 풀 수 있다.

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
