---
title: "[Binary Search] 범위를 반씩 좁혀가는 탐색"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Binary Search]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-18
last_modified_at: 2021-06-18
sitemap :
  changefreq : daily
  priority : 1.0
---

## 순차 탐색  

이진 탐색에 대해 알아보기 전에 순차 탐색에 대해 이해해야 한다. 순차 탐색(Sequential Search)란 `리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법이다.` 보통 정렬되지 않은 리스트에서 데이터를 찾아야 할 때 사용한다. 리스트 내에 데이터가 아무리 많아도 시간만 충분하다면 항상 원하는 데이터를 찾을 수 있다는 장점이 있다. 아래 예제를 통해보자.  

**<u>step 0</u>** : 초기 단계  

![image](https://user-images.githubusercontent.com/37467408/122504709-c9a7e600-d035-11eb-9902-ee5d62978bc6.PNG)  

**<u>step 1</u>** 가장 먼저 첫 번째 데이터를 확인한다. Haneul은 찾고자 하는 문자열과 같지 않다. 따라서 다음 데이터로 이동한다.  

![image](https://user-images.githubusercontent.com/37467408/122504792-f5c36700-d035-11eb-8ab3-3a3339dd3887.PNG)  

**<u>step 2</u>** 두 번째 데이터를 확인한다. Jonggu는 찾고자 하는 문자열과 같지 않다. 다음 데이터로 이동한다.  

![image](https://user-images.githubusercontent.com/37467408/122504846-14c1f900-d036-11eb-83cf-57c4c131a2f5.PNG)  

**<u>step 3</u>** 세 번째 데이터를 확인한다. Dongbin은 찾고자 하는 문자열과 같으므로 탐색을 마친다.  

![image](https://user-images.githubusercontent.com/37467408/122504899-302d0400-d036-11eb-9837-c1258eaf71df.PNG)  

순차 탐색은 이름처럼 순차로 데이터를 탐색한다는 의미이다. 리스트의 데이터에 하나씩 방문하며 특정한 문자열과 같은지 검사하므로 구현도 간단하다.  
아래는 순차 탐색의 파이썬 코드이다.  

```python
# 순차 탐색 소스코드 구현
def sequential_search(n, target, array):
  # 각 원소를 하나씩 확인하며
  for i in range(n):
    # 현재의 원소가 찾고자 하는 원소와 동일한 경우
    if array[i] == target:
      return i + 1 # 현재 위치 반환(인덱스는 0부터 시작하므로 1 더하기)

print("생성할 원소 개수를 입력한 다음 한 칸 띄고 찾을 문자열을 입력하세요.")
input_data = input().split()
n = int(input_data[0]) # 원소의 개수
target = input_data[1] # 찾고자 하는 문자열

print("앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다.")
array = input().split()

# 순차 탐색 수행 결과 출력
print(sequential_search(n, target, array))
```  

순차 탐색은 데이터 정렬 여부와 상관없이 가장 앞에 있는 원소부터 하나씩 확인해야 한다는 점이 특징이다. 따라서, 데이터의 개수가 N개일 때 최대 N번의 비교 연산이 필요하므로 순차 탐색의 최악의 경우 시간 복잡도는 O(N)이다.  

## 이진 탐색 : 반으로 쪼개면서 탐색하기  

이진 탐색(Binary Search)은 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘이다. 이진 탐색은 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 특징이 있다.  
이진 탐색은 위치를 나타내는 변수를 3개 사용하는데 탐색하고 자하는 범위의 `시작점`, `끝 점`, 그리고 `중간점`이다. `찾으려는 데이터와 중간점(Middle) 위치에 있는 데이터를 반복적으로 비교`해서 원하는 데이터를 찾는 게 이진 탐색 과정이다.  
아래 예시를 보도록 하자.  

**<u>step 1</u>** 시작점과 끝점을 확인한 다음 둘 사이에 중간점을 정한다. 중간점이 실수일 때는 소수점 이하를 버린다. 그림에서 각각의 인덱스는 시작점은 [0], 끝점은 [9], 중간점은 (4.5에서 소수점 이하를 버려서) [4]이다. 다음으로 중간점 [4]의 데이터 8과 찾으려는 데이터 4를 비교한다. 중간점의 데이터 8이 더 크므로 중간점 이후의 값은 확인할 필요가 없다. 끝점을 [4]의 이전인 [3]으로 옮긴다.  

![image](https://user-images.githubusercontent.com/37467408/122505941-4cca3b80-d038-11eb-80d1-1d8cb351f894.PNG)  

**<u>step 2</u>** 시작점은 [0], 끝점은 [3], 중간점은 (1.5에서 소수점 이하를 버려서) [1]이다. 중간점에 위치한 데이터 2는 찾으려는 데이터 4보다 작으므로 이번에는 값이 2 이하인 데이터는 더 이상 확인할 필요가 없다. 따라서 시작점을 [2]로 변경한다.  

![image](https://user-images.githubusercontent.com/37467408/122506063-869b4200-d038-11eb-9f65-a79b9ac7fdcc.PNG)  

**<u>step 3</u>** 시작점은 [2], 끝점은 [3]이다. 이때 중간점은 (2.5에서 소수점 이하를 버려서) [2]이다. 중간점에 위치한 데이터 4는 찾으려는 데이터 4와 동일하르모 이 시점에서 탐색을 종료한다.  

![image](https://user-images.githubusercontent.com/37467408/122506184-c2cea280-d038-11eb-813f-85ade2820939.PNG)  

전체 데이터의 개수는 10개지만, 이진 탐색을 이용해 총 3번의 탐색으로 원소를 찾을 수 있었다. 이진 탐색은 한번 확인할 때마다 확인하는 원소의 개수가 절반씩 줄어든다는 점에서 시간 복잡도가 O(logN)이다. 절반씩 데이터를 줄어들도록 만든다는 점은 앞서 다룬 퀵 정렬과 공통점이 있다. 이진 탐색의 구현 방법은 재귀 함수를 이용하는 방법이고, 다른 하는 단순하게 반복문을 이용하는 방법이 있다.  

아래는 재귀 함수를 이용한 파이썬 코드이다.  

```python
# 이진 탐색 소스코드 구현(재귀 함수)
def binary_search(array, target, start, end):
  if start > end:
    return None
  mid = (start + end) // 2
  # 찾은 겨우 중간점 인덱스 반환
  if array[mid] == target:
    return mid
  # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
  elif array[mid] > target:
    return binary_search(array, target, start, mid - 1)
  # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
  else:
    return binary_search(array, target, mid + 1, end)

# n(원소의 개수)과 target(찾고자 하는 문자열)을 입력받기
n, target = list(map(int, input().split()))
# 전체 원소 입력받기
array = list(map(int, input().split()))

# 이진 탐색 수행 결과 출력
result = binary_search(array, target, 0, n - 1)
if result == None:
  print("원소가 존재하지 않습니다.")
else:
  print(result + 1)
```  

다음, 아래는 단순 반복문을 사용한 파이썬 코드이다.  

```python
#이진 탐색 소스코드 구현(반복문)
def binary_search(array, target, start, end):
  while start <= end:
    mid = (start + end) // 2
    # 찾은 경우 중간점 인덱스 반환
    if array[mid] == target:
      return mid
    # 중간점의 값보다 찾고자 하는 값이 적은 경우 왼쪽 확인
    elif array[mid] > target:
      end = mid - 1
    # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
    else:
      start = mid + 1
  return None

# n(원소의 개수)과 target(찾고자 하는 문자열)을 입력받기
n, target = list(map(int, input().split()))
# 전체 원소 입력받기
array = list(map(int, input().split()))

# 이진 탐색 수행 결과 출력
result = binary_search(array, target, 0, n - 1)
if result == None:
  print("원소가 존재하지 않습니다.")
else:
  print(result + 1)
```  

> 코딩 테스트에서의 이진 탐색  

코딩테스트에서 단골로 다오는 문제이니 이해후 외우도록 하자. 또한, 높은 난이도의 문제에서는 이진 탐색 알고리즘이 다른 알고리즘과 함께 사용되기도 한다. 더불어 코딩 테스트의 이진 문제는 탐색 범위가 큰 상황에서의 탐색을 가정하는 문제가 많으므로, 탐색 범위가 1,000만을 넘어가면 이진 탐색으로 문제에 접근해보도록 하자.  

## 트리 자료구조  

이진 탐색은 전제 조건이 데이터 정렬이다. 데이터베이스는 내부적으로 대용량 데이터 처리에 적합한 트리(Tree) 자료구조를 이용하여 항상 데이터가 정렬되어 있다. 따라서 데이터베이스에서의 탐색은 이진 탐색과는 조금 다르지만, 이진 탐색과 유사한 방법을 이용해 탐색을 항상 빠르게 수행하도록 설계되어 있어서 데이터가 많아도 탐색하는 속도가 빠르다.  
트리 자료구조는 노드와 노드의 연결로 표현하며 여기에서 노드는 정보의 단위로서 어떠한 정보를 가지고 있는 개체로 이해할 수 있다. 또한, 그래프 자료구조의 일종으로 데이터베이스 시스템이나 파일 시스템과 같은 곳에서 많은 양의 데이터를 관리하기 위한 목적으로 사용된다.  

![image](https://user-images.githubusercontent.com/37467408/122508652-44c0ca80-d03d-11eb-8aa6-e1c38d0e9260.PNG)  

트리 자료는 몇가지 주요한 특징이 있다.  

- 트리는 부모 노드와 자식 노드의 관계로 표현된다.  
- 트리의 최상단 노드를 루트 노드라고 한다.  
- 트리의 최하단 노드를 단말 노드라고 한다.  
- 트리에서 일부를 떼어내도 트리 구조이며 이를 서브 트리라 한다.  
- 트리는 파일 시스템과 같이 계층적이고 정렬된 데이터를 다루기에 적합하다.  

## 이진 탐색 트리  

트리 자료구조 중에서 가장 간단한 형태가 이진 탐색 트리이다. 이진 탐색 트리란 이진 탐색이 동작할 수 있도록 고안된, 효율적인 탐색이 가능한 자료구조이다.  

![image](https://user-images.githubusercontent.com/37467408/122508853-a08b5380-d03d-11eb-89dd-fb8c7bba67c8.PNG)  

보통 이진 탐색 트리는 위 그림과 같은데 모든 트리가 이진 탐색 트리는 아니며, 이진 탐색 트리는 다음과 같은 특징을 가진다.  

- 부모 노드보다 왼쪽 자식 노드가 작다.  
- 부모 노드보다 오른쪽 자식 노드가 크다.  

`왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드`가 성립해야지 이진 탐색트리라 할 수 있다.  

 ![image](https://user-images.githubusercontent.com/37467408/122508971-de887780-d03d-11eb-8836-cdc18049fcbc.PNG)  

이진 탐색 트리에 데이터를 넣고 빼는 방법은 알고리즘보다는 자료구조에 가깝다.  
이진 탐색 트리가 미리 구현되어 있다고 가정하고 다음 그림과 같은 이진 탐색 트리에서 데이터를 조회하는 방법을 알아보자. 다음은 찾는 원소가 '37'일 때 동작하는 과정이다.  

**<u>step 1</u>** 이진 탐색은 루트 노드부터 방문한다. 루트 노드는 '30'이고 찾는 원소값은 '37'이다. 공식에 따라 부모 노드의 왼쪽 자식 노드는 '30'이하이므로 왼쪽에 있는 모든 노드는 확인할 필요가 없다. 따라서 오른쪽 노드를 방문한다.  

![image](https://user-images.githubusercontent.com/37467408/122509227-61113700-d03e-11eb-963e-57a177eee91b.PNG)  

**<u>step 2</u>** 오른쪽 자식 노드인 '48'이 이번에는 부모 노드이다. '48'은 찾는 원소값인 '37'보다 크다. 공식에 따라 부모 노드(48)의 오른쪽 자식 노드는 모두 '48'이상이므로 확인할 필요가 없다. 따라서 왼쪽 노드를 방문한다.  

![image](https://user-images.githubusercontent.com/37467408/122509343-93bb2f80-d03e-11eb-90b5-9f203b2c4082.PNG)  

**<u>step 3</u>** 현재 방문한 노드의 값인 '37'과 찾는 원소값인 '37'이 동일하다. 따라서 탐색을 마친다.  

![image](https://user-images.githubusercontent.com/37467408/122509405-af263a80-d03e-11eb-8ff1-49799f98d678.PNG)  

자식 노드가 없을때까지 원소를 찾지 못했다면, 이진 탐색 트리에 원소가 없는 것이다.  

> 빠르게 입력받기  

이진 탐색 문제는 입력 데이터가 많거나, 탐색 범위가 매우 넓은 편이다. 이런 경우에 input() 함수를 사용해서 입력을 받으면 시간 초과로 오답처리를 받을 수 있으므로 이럴 경우에는 sys 라이브러리의 readline() 함수를 이용해야 한다.  
아래는 sys 라이브러리의 readling을 이용해 입력을 받는 파이썬 코드이다.  

```python
import sys
# 하나의 문자열 데이터 입력받기
input_data = sys.stdin.readline().rstrip()

# 입력받은 문자열 그대로 출력
print(input_data)
```  

sys 라이브러리를 사용할 때는 rstrip() 함수를 꼭 호출해야 한다. readline()으로 입력을 받으면 입력 후 엔터(Enter)가 줄 바꿈 기호로 입력되는데, 이 공백 문자를 제거하려면 rstrip() 함수를 사용해야 한다. 위 같이 rstrip() 함수를 관행적으로 외워서 사용하도록 하자.
