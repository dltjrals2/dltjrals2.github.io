---
title: "[BinarySearch] 정렬된 배열에서 특정 수의 개수 구하기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, BinarySearch, Zoho]

toc:  true
toc_sticky: true

date: 2021-10-13
last_modified_at: 2021-10-13
---

## 정렬된 배열에서 특정 수의 개수 구하기  

난이도 : 🌕  
푼횟수 : 🟢⚪⚪  

N개의 원소를 포함하고 있는 수열이 오름차순으로 정렬되어 있습니다. 이때 이 수열에서 x가 등장하는 횟수를 계산하세요. 예를 들어 수열 [1, 1, 2, 2, 2, 2, 3]이 있을 때 x = 2라면, 현재 수열에서 값이 2인 원소가 4개이므로 4를 출력합니다.  
단, 이 문제는 시간 복잡도 O(logN)으로 알고리즘을 설계하지 않으면 '시간 초과' 판정을 받습니다.  

**<u>입력 조건</u>**  
- 첫째 줄에 N과 정수 형태로 공백으로 구분되어 입력됩니다. (1 <= N <= 1,000,000), (-10^9 <= x <= 10^9)  
- 둘째 줄에 N개의 원소가 정수 형태로 공백으로 구분되어 입력됩니다. (-10^9 <= 각 원소의 값 <= 10^9)  

**<u>출력 조건</u>**  
- 수열의 원소 중에서 값이 x인 원소의 개수를 출력합니다. 단 값이 x인 원소가 하나도 없다면 -1을 출력합니다.  

**<u>입력 예시 1</u>**  
7 2  
1 1 2 2 2 2 3  

**<u>출력 예시 1</u>**  
4  

**<u>입력 예시 2</u>**  
7 4  
1 1 2 2 2 2 3  

**<u>출력 예시 2</u>**  
-1  

> 나의 풀이  

```python
N, x = map(int, input().split())
data = list(map(int, input().split()))

start = 0
end = len(data) - 1
count = 0

while start <= end:
    mid = (start + end) // 2
    if data[mid] == x:
        for i in range(mid, -1, -1):
            if data[i] == x:
                count += 1
            else:
                break
        for j in range(mid + 1, len(data)):
            if data[j] == x:
                count += 1
            else:
                break
        break

    elif data[mid] > x:
        end = mid - 1
    else:
        start = mid + 1

if count > 0:
    print(count)
else:
    print(-1)
```

> 문제 해설  

x의 개수가 많지 않다면 시간 복잡도 O(logN)으로 풀리지만, <u>N개의 원소를 가지고 있는 수열이 x로만 이루어진다면 시간 복잡도는 O(N)이 될 것이다.</u>  

<u>따라서 완벽한 풀이는 아니다.</u>  

이 문제는 시간 복잡도 O(logN)으로 동작하는 알고리즘을 요구하고 있다.  

따라서 일반적인 선형 탐색으로는 문제를 해결할 수 없다. 다행히도 모든 원소가 정렬이 된 상태로 입력되므로, 이진 탐색을 이용하여 값이 x인 원소의 개수를 시간 O(logN)에 찾아낼 수 있다.  

원소들은 모두 정렬되어 있기 때문에, 수열 내에 x가 존재한다면 연속적으로 나열되어 있을 것으로 예상할 수 있다.  

따라서 x가 처음 등장하는 인덱스와 x가 마지막으로 등장하는 인덱스를 각각 계산한 뒤에, 그 인덱스의 차이를 계산하여 문제를 풀 수 있다.  

그러므로 이진 탐색 함수를 2개 작성하여 문제를 해결한다.  

하나의 함수는 데이터가 존재한다면 가장 첫 번째 위치를 찾는 이진 탐색 함수이며, 다른 하나의 함수는 데이터가 존재한다면 가장 마지막 위치를 찾는 이진 탐색 함수이다.  

이 2개를 각각 실행한 뒤에 답을 도출할 수 있다.

> 문제 답안 1  

```python
# 정렬된 수열에서 길이 x인 원소의 개수를 세는 함수
def count_by_value(array, x):
    # 데이터의 개수
    n = len(array)

    # x가 처음 등장한 인덱스 계산
    a = first(array, x, 0, n - 1)

    # 수열에 x가 존재하지 않을 경우
    if a == None:
        return 0 # 값이 x인 원소의 개수는 0개이므로 0 반환

    # x가 마지막으로 등장한 인덱스 계산
    b = last(array, x, 0, n - 1)

    # 개수를 반환
    return b - a + 1

# 처음 위치를 찾는 이진 탐색 함수
def first(array, target, start, end):
    if start > end:
        return None

    mid = (start + end) // 2
    # 해당 값을 가지는 원소 중에서 가장 왼쪽에 있는 경우에만 인덱스 반환
    if (mid == 0 or target > array[mid - 1]) and array[mid] == target:
        return mid
    # 중간점의 값 보다 찾고자 하는 값이 작거나 같은 경우 왼쪽 확인
    elif array[mid] >= target:
        return first(array, target, start, mid - 1)
    else:
        return first(array, target, mid + 1, end)
    
# 마지막 위치를 찾는 이진 탐색 함수
def last(array, target, start, end):
    if start > end:
        return None
    
    mid = (start + end) // 2
    # 해당 값을 가지는 원소 중에서 가장 오른쪽에 있는 경우에만 인덱스 반환
    if (mid == n - 1 or target < array[mid + 1]) and array[mid] == target:
        return mid
    # 중간점의 값 보다 찾고자 하는 값이 작은 경우 왼쪽 확인
    elif array[mid] > target:
        return last(array, target, start, mid - 1)
    # 중간점의 값 보다 찾고자 하는 값이 크거나 같은 경우 오른쪽 확인
    else:
        return last(array, target, mid + 1, end)
    
n, x = map(int, input().split()) # 데이터의 개수 N, 찾고자 하는 값 x를 입력받기
array = list(map(int, input().split())) # 전체 데이터 입력받기

# 값이 x인 데이터의 개수 계산
count = count_by_value(array, x)

# 값이 x인 원소가 존재하지 않는다면
if count == 0:
    print(-1)
# 값이 x인 원소가 존재한다면
else:
    print(count)
```  


> 답안 예시 2  

```python
from bisect import bisect_left, bisect_right

# 값이 [left_value, right_value]인 데이터의 개수를 반환하는 함수
def count_by_range(array, left_value, right_value):
    right_index = bisect_right(array, right_value)
    left_index = bisect_left(array, left_value)
    return right_index - left_index

n, x = map(int, input().split()) # 데이터의 개수 N, 찾고자 하는 값 x를 입력받기
array = list(map(int, input().split())) # 전체 데이터 입력받기

# 길이 [x, x] 범위에 있는 데이터의 개수 계산
count = count_by_range(array, x, x)

# 길이 x인 원소가 존재하지 않는다면
if count == 0:
    print(-1)
# 길이 x인 원소가 존재한다면
else:
    print(count)
```

<br>
기출 : Zoho 인터뷰  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊