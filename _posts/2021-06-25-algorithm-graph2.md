---
title: "[Graph] 그래프 이론2"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Graph, Kruscal Algorithm]

toc:  true
toc_sticky: true
show_date: true
read_time: false
use_math: true

date: 2021-06-25
last_modified_at: 2021-06-28
sitemap :
  changefreq : daily
  priority : 1.0
---

## 다양한 그래프 알고리즘  

## 신장 트리  

신장 트리는 그래프 알고리즘 문제로 자주 출제되는 문제 유형이다. 기본적으로 신장 트리(Spanning Tree)란 `하나의 그래프가 있을 때 모든 노드를 포함하면서 존재하지 않는 부분 그래프`를 의미한다. 이때 모든 노드가 포함되어 서로 연결되면서 사이클이 존재하지 않는다는 조건은 트리의 성립 조건이기도 하다. 예를 들어 다음 왼쪽과 같은 그래프에서는 여러 개의 신장 트리를 찾을 수 있다. 바로 오른쪽이 그 중 하나이다.  

![image](https://user-images.githubusercontent.com/37467408/123352256-e1352080-d599-11eb-91bf-ab84f44ff51a.PNG)  

반면에 다음 왼쪽 그림은 그래프가 '노드 1'을 포함하고 있지 않기 때문에 신장 트리에 해당하지 않는다. 또한 오른쪽 그림은 사이클이 존재하므로 신장 트리가 아니다.  

![image](https://user-images.githubusercontent.com/37467408/123352324-04f86680-d59a-11eb-9d24-9c7f3d39b290.PNG)  

### 크루스칼 알고리즘  

예를 들어 N개의 도시가 존재하는 상황에서 두 도시 아이에 도로를 놓아 전체 도시가 서로 연결될 수 있게 도로를 설치하는 경우를 생각해보자. 2개의 도시 A, B를 선택했을 때, 도시 A에서 도시 B로 이동하는 경로가 반드시 존재하도록 도로를 설치하고자 한다. 모든 도시를 '연결'할 때, 최소한의 비용으로 연결하려면 어떤 알고리즘을 이용해야 할까?  
예를 들어 아래와 같은 그래프가 있다고 가정해보자.  
3개의 도시가 있고 각 도시 간 도로를 건설하는 비용은 23, 13, 25이다.  

![image](https://user-images.githubusercontent.com/37467408/123352575-894ae980-d59a-11eb-812e-1756d3b979dc.PNG)  

여기서 노드 1, 2, 3을 모두 연결하기 위해서 가장 최소한의 비용을 가지는 신장 트리는 36이다.  

![image](https://user-images.githubusercontent.com/37467408/123352620-a67fb800-d59a-11eb-8ff6-d26ff9a64d76.PNG)  

이처럼 신장 트리 중에서 최소 비용으로 만들 수 있는 신장 트리를 찾는 알고리즘을 '최소 신장 트리 알고리즘'이라고 한다. 대표적인 신장 트리 알고리즘으로는 `크루스칼 알고리즘(Kruscal Algorithm)`이 있다.  
크루스칼 알고리즘을 사용하면 가장 적은 비용으로 모든 노드를 연결할 수 있는데 크루스칼 알고리즘은 그리디 알고리즘으로 분류된다. 먼저 모든 노드에 대하여 정렬을 수행한 뒤에 가장 거리가 짧은 간선부터 집합에 포함시키면 된다. 이때 사이클을 발생시킬 수 있는 간선의 경우, 집합에 포함시키지 않는다. 구체적인 알고리즘을 살펴보면 다음과 같다.  

1. 간선 데이터를 비용에 따라 오름차순으로 정렬한다.  
2. 간선을 하나씩 확인하며 현재의 간선이 사이클을 발생시키는지 확인한다.  
- 사이클이 발생하지 않는 경우 최소 신장 트리에 포함시킨다.  
- 사이클이 발생하는 경우 최소 신장 트리에 포함시키지 않는다.  
3. 모든 간선에 대하여 2번의 과정을 반복한다.  

다음 그래프의 최소 신장 트리를 구해보자.  

![image](https://user-images.githubusercontent.com/37467408/123353179-e85d2e00-d59b-11eb-98e9-6274cb709e2a.PNG)  

최소 신장 트리는 다음 그래프와 같이 하늘색으로 칠한 간선들을 포함시키면 만들 수 있다. 최소 신장 트리는 일종의 트리 자료구조이므로, 최종적으로 신장 트리에 포함되는 간선의 개수가 `노드의 개수 - 1`과 같다는 특징이 있다. 예를 들어 아래 예시에서는 노드의 개수가 7이고 간선의 개수가 6인 것을 확인할 수 있다.  

![image](https://user-images.githubusercontent.com/37467408/123353997-b2b94480-d59d-11eb-9e54-fe1db37571d7.PNG)  

따라서 크루스칼 알고리즘의 핵심 원리는 가장 거리가 짧은 간선부터 차례대로 집합에 추가하면 된다는 것이다. 다만, 사이클을 발생시키는 간선은 제외하고 연결한다. 이렇게 하면 항상 최적의 해를 보장할 수 있다. 크루스칼 알고리즘을 이용해서 최소 신장 트리를 찾는 과정을 단계별로 확인해보자.  

**<u>step 0</u>** 초기 단계에서는 그래프의 모든 간선 정보만 따로 빼내어 정렬을 수행한다. 현재 전체 그래프에 존재하는 간선이 9개인 것을 알 수 있다. 따라서 맨 처음에는 모든 간선을 정렬한다. 실제로는 전체 간선 데이터를 리스트에 담은 뒤에 이를 정렬하지만, 가독성을 위해 다음 그림에서는 노드 데이터 순서에 따라 테이블 내에 데이터를 나열했다.  

![image](https://user-images.githubusercontent.com/37467408/123356295-6290b100-d5a2-11eb-8f2e-1fa4401b43fb.PNG)  

**<u>step 1</u>** 첫 번째 단계에서는 가장 짧은 간선을 선택한다. 따라서 (3, 4)가 선택되고 이것을 집합에 포함하면 된다. 다시 말해 노드 3과 노드 4에 대하여 union 함수를 수행하면 된다. 그래서 노드 3과 노드 4를 동일한 집합에 속하도록 한다.  

![image](https://user-images.githubusercontent.com/37467408/123373050-aba32e00-d5bf-11eb-98cb-c3a13c7559e5.PNG)  

**<u>step 2</u>** 그다음으로 비용이 가장 적은 간선인 (4, 7)을 선택한다. 현재 노드 4와 노드 7은 같은 집합에 속해 있지 않기 때문에, 노드 4와 노드 7에 대하여 union 함수를 호출한다.  

![image](https://user-images.githubusercontent.com/37467408/123373253-08064d80-d5c0-11eb-9275-dd51c39b2b4a.PNG)  

**<u>step 3</u>** 그다음으로 비용이 가장 적은 간선인 (4, 6)을 선택한다. 현재 노드 4와 노드 6은 같은 집합에 속해 있지 않기 때문에, 노드 4와 노드 6에 대하여 union 함수를 호출한다.  

![image](https://user-images.githubusercontent.com/37467408/123373394-46037180-d5c0-11eb-8b34-aa08b71b56b6.PNG)  

**<u>step 4</u>** 그다음으로 비용이 가장 적은 간선인 (6, 7)을 선택한다. 선택된 노드 6과 노드 7의 루트 노드를 확인한다. 다만, 노드 6과 노드 7의 루트가 이미 동일한 집합에 포함되어 있으므로 신장 트리에 포함하지 않아야 한다. 따라서 union 함수를 호출하지 않는다. 앞으로의 단계에 대해서, 처리가 되었지만 신장 트리에 포함되지 않은 간선은 점선으로 표시하겠다.  

![image](https://user-images.githubusercontent.com/37467408/123373594-9549a200-d5c0-11eb-86f3-8c7e36465edd.PNG)  

**<u>step 5</u>** 그다음으로 비용이 가장 작은 간선 (1, 2)를 선택한다. 현재 노드 1과 노드 2는 같은 집합에 속해 있지 않기 때문에, 노드 1과 노드 2에 대하여 union 함수를 호출한다.  

![image](https://user-images.githubusercontent.com/37467408/123373679-bf9b5f80-d5c0-11eb-9ddb-af71c2e7b434.PNG)  

**<u>step 6</u>** 그다음으로 비용이 가장 작은 간선 (2, 6)을 선택한다. 현재 노드 2와 노드 6은 같은 집합에 속해 있지 않기 때문에, 노드 2와 노드 6에 대하여 union 함수를 호출한다.  

![image](https://user-images.githubusercontent.com/37467408/123373753-e78ac300-d5c0-11eb-87fc-c2b0602df2ed.PNG)  

**<u>step 7</u>** 그다음으로 비용이 가장 작은 간선인 (2, 3)을 선택한다. 선택된 노드 2와 노드 3의 루트 노드를 확인한다. 다만, 노드 2와 노드 3의 루트가 이미 동일한 집합에 포함되어 있으므로 union 함수를 호출하지 않는다.  

![image](https://user-images.githubusercontent.com/37467408/123373824-11dc8080-d5c1-11eb-85d6-9acbde1182b9.PNG)  

**<u>step 8</u>** 그다음으로 비용이 가장 작은 간선 (5, 6)을 선택한다. 현재 노드 5와 노드 6은 같은 집합에 속해 있지 않기 때문에, 노드 5와 노드 6에 대하여 union 함수를 호출한다.  

![image](https://user-images.githubusercontent.com/37467408/123374941-df338780-d5c2-11eb-9ee6-2dd1b2a73f33.PNG)  

**<u>step 9</u>** 그다음으로 비용이 가장 작은 간선 (1, 5)를 선택한다. 선택된 노드 1과 노드 5의 루트 노드를 확인한다. 다만, 노드 1과 노드 5의 루트 노드를 확인한다. 다만, 노드 1과 노드 5의 루트가 이미 동일한 집합에 포함되어 있으므로 union 함수를 호출하지 않는다.  

![image](https://user-images.githubusercontent.com/37467408/123565394-acbea000-d7f7-11eb-8208-ff4c2254a7cd.PNG)  

최소 신장 트리에 있는 간선의 비용만 모두 더하면, 그 값이 최종 비용에 해당한다.  
최소 신장 트리를 만드는데 필요한 비용을 계산하는 크루스칼 알고리즘 소스코드는 다음과 같다.  

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

# 노드의 개수와 간선(union 연산)의 개수 입력받기
v, e = map(int, input().split())
parent = [0] * (v + 1) # 부모 테이블 초기화

# 모든 간선을 담을 리스트와 최종 비용을 담을 변수
edges = []
result = 0

# 부모 테이블상에서, 부모를 자기 자신으로 초기화
for i in range(1, v + 1):
  parent[i] = i

# 모든 간선에 대한 정보를 입력받기
for _ in range(e):
  a, b, cost = map(int, input().split())
  # 비용순으로 정렬하기 위해서 튜플의 첫 번째 원소를 비용으로 설정
  edges.append((cost, a, b))

# 간선을 비용순으로 정렬
edges.sort()

# 간선을 하나씩 확인하며
for edge in edges:
  cost, a, b = edge
  # 사이클이 발생하지 않는 경우에만 집합에 포함
  if find_parent(parent, a) != find_parent(parent, b):
    union_parent(parent, a, b)
    result += cost

return result
```  

> 크루스칼 알고리즘의 시간 복잡도  

크루스칼 알고리즘은 간선의 개수가 E개일 때, O(ElogE)의 시간 복잡도를 가진다. 시간이 가장 오래 걸리는 부분이 간선을 정렬하는 작업이며, E개의 데이터를 정렬했을 때의 시간 복잡도는 O(ElogE)이기 때문이다.  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
