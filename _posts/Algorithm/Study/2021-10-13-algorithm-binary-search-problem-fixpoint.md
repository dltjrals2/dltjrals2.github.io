---
title: "[BinarySearch] 고정점 찾기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, BinarySearch, Amazon]

toc:  true
toc_sticky: true

date: 2021-10-13
last_modified_at: 2021-10-13
---

## 고정점 찾기  

난이도 : 🌕🌗  
푼횟수 : 🟢⚪⚪  

고정점이란, 수열의 원소 중에서 `그 값이 인덱스와 동일한 원소`를 의미합니다. 예를 들어 수열 a = {-15, -4, 2, 8, 13}이 있을 때 a[2] = 2 이므로, 고정점은 2가 된다.  

하나의 수열이 N개의 서로 다른 원소를 포함하고 있으며, 모든 원소가 오름차순으로 정렬되어 있습니다. 이때 이 수열에서 고정점이 있다면, 고정점을 출력하는 프로그램을 작성하세요. 고정점은 최대 1개만 존재합니다. 만약 고정점이 없다면 -1을 출력합니다.  

단, 이 문제는 시간 복잡도 O(logN)으로 알고리즘을 설계하지 않으면 '시간 초과' 판정을 받습니다.

**<u>입력 조건</u>**  
- 첫째 줄에 N이 입력된다. (1 <= N <= 1,000,000)  
- 둘째 줄에 N개의 원소가 정수 형태로 공백으로 구분되어 입력됩니다. (-10^9 <= 각 원소의 값 <= 10^9)  

**<u>출력 조건</u>**  
- 고정점을 출력한다. 고정점이 없다면 -1을 출력한다.  

**<u>입력 예시 1</u>**  
5  
-15 -6 1 3 7  

**<u>출력 예시 1</u>**  
3  

**<u>입력 예시 2</u>**  
7  
-15 -4 2 8 9 13 15  

**<u>출력 예시 2</u>**  
2  

**<u>입력 예시 3</u>**  
7  
-15 -4 3 8 9 13 15  

**<u>출력 예시 3</u>**  
-1  

> 나의 풀이  

```python
n = int(input())
data = list(map(int, input().split()))

start = 0
end = n - 1

def find_fixed_point(array, start, end):
    while start <= end:
        mid = (start + end) // 2
        if mid == data[mid]:
            return mid
        elif mid > data[mid]:
            start = mid + 1
        else:
            end = mid - 1

    return None

result = find_fixed_point(data, start, end)

if result == None:
    print(-1)
else:
    print(result)
```

> 문제 해설  

이 문제의 요구사항인 시간 복잡도 O(logN)으로 고정점을 찾으려면 선형탐색으로는 조건에 맞게 문제를 해결할 수 없다.  

따라서 이진 탐색을 수행해서 빠르게 고정점을 찾아야 한다. 이미 배열이 정렬되어 있으므로 바로 이진 탐색을 적용할 수 있다.  

이진 탐색을 수행할 때는 '찾고자 하는 값'이 '중간값'과 동일하다고 가정하고, 탐색을 수행하면 된다.  

그래서 중간점이 가리키는 위치의 값보다 중간점이 작은 경우에는 왼쪽 부분을 탐색하고, 중간점이 가리키는 위치의 값보다 중간점이 큰 경우에는 오른쪽 부분을 탐색하는 것을 반복하면 된다.


> 문제 답안  

```python
# 이진 탐색 소스코드 구현(재귀 함수)
def binary_search(array, start, end):
    if start > end:
        return None
    mid = (start + end) // 2

    # 고정점을 찾은 경우 인덱스 반환
    if array[mid] == mid:
        return mid
    # 중간점이 가리키는 위치의 값보다 중간점이 작은 경우 왼쪽 확인
    elif array[mid] > mid:
        return binary_search(array, start, mid - 1)
    # 중간점이 가리키는 위치의 값보다 중간점이 큰 경우 오른쪽 확인
    else:
        return binary_search(array, start + 1, end)

n = int(input())
array = list(map(int, input().split()))

# 이진 탐색(Binary Search) 수행
index = binary_search(array, 0, n - 1)

# 고정점이 없는 경우 -1 출력
if index == None:
    print(-1)
# 고정점이 없는 경우 해당 인덱스 출력
else:
    print(index)
```

<br>
기출 : Amazon 인터뷰  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊