---
title: "[Binary Search] 부품 찾기"

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

## 부품 찾기

난이도 : ⭐⭐  

동빈이네 전자 매장에는 부품이 N개 있다. 각 부품은 정수 형태의 고유한 번호가 있다. 어느 날 손님이 M개 종류의 부품을 대량으로 구매하겠다며 당일 날 견적서를 요청했다. 동빈이는 때를 놓치지 않고 손님이 문의한 부품 M개 종류를 모두 확인해서 견적서를 작성해야 한다. 이때 가게 안에 부품이 모두 있는지 확인하는 프로그램을 작성해보자.  
예를 들어 가게의 부품이 총 5개일 때 부품 번호가 다음과 같다고 하자.  

![image](https://user-images.githubusercontent.com/37467408/122511501-5062c000-d042-11eb-90b2-9b9a7261c724.PNG)  

손님은 총 3개의 부품이 있는지 확인 요청했는데 부품 번호는 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/122511559-64a6bd00-d042-11eb-89be-c70c8d87655f.PNG)  

이때 손님이 요청한 부품 번호의 순서대로 부품을 확인해 부품이 있으면 yes를, 없으면 no를 출력한다. 구분을 공백으로 한다.  

**<u>입력 조건</u>**  
- 첫째 줄에 정수 N이 주어진다. (1 <= N <= 1,000,000)  
- 둘째 줄에는 공백으로 구분하여 N개의 정수가 주어진다. 이때 정수는 1보다 크고 1,000,000 이하이다.  
- 셋째 줄에는 정수 M이 주어진다. (1 <= M <= 100,000)  
- 넷째 줄에는 공백으로 구분하여 M개의 정수가 주어진다. 이때 정수는 1보다 크고 1,000,000 이하이다.   

**<u>출력 조건</u>**  
- 첫째 줄에 공백으로 구분하여 각 부품이 존재하면 yes를, 없으면 no를 출력한다.  

**<u>입력 예시</u>**  
5  
8 3 7 9 2  
3  
5 7 9  

**<u>출력 예시</u>**  
no yes yes  

> 나의 풀이  

```python
def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    if arrA[mid] == target:
        return mid
    elif arrA[mid] > target:
        return binary_search(arrA, target, start, mid - 1)
    else:
        return binary_search(arrA, target, mid + 1, end)

n = int(input())
arrA = list(map(int, input().split()))
arrA.sort()
m = int(input())
arrB = list(map(int, input().split()))

for i in arrB:
    result = binary_search(arrA, i, 0, n - 1)
    if result != None:
        print('Yes', end = ' ')
    else:
        print('No', end = ' ')
```

> 문제 해설  

이처럼 다량의 데이터 검색은 이진 탐색 알고리즘을 이용해 효과적으로 처리할 수 있다.  
먼저 매장 내 N개의 부품을 번호를 기준으로 정렬하자. 그 이후에 M개의 찾고자 하는 부품이 각각 매장에 존재하는지 검사하면 된다.  
최악의 경우 시간복잡도 O(MlogN)이고, N개의 부품을 정렬하는데 O(NlogN)이다. 결과적으로 이진 탐색을 사용하는 문제 풀이 방법의 경우 시간 복잡도는 O((M + N)logN)이다.  
아래는 이진 탐색 알고리즘을 이용한 풀이다.

> 답안 예시1 : 반복문 이용  

```python
# 이진 탐색 소스코드 구현(반복문)
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        # 찾은 경우 중간점 인덱스 반환
        if array[mid] == target:
            return mid
        # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
        elif array[mid] > target:
            end = mid - 1
        # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
        else:
            start = mid + 1
    return None

# N(가게의 부품 개수) 입력
n = int(input())
# 가게에 있는 전체 부품 번호를 공백으로 구분하여 입력
array = list(map(int, input().split()))
array.sort() # 이진 탐색을 수행하기 위해 사전에 정렬 수행
# M(손님이 확인 요청한 부품 개수) 입력
m = int(input())
# 손님이 확인 요청한 전체 부품 번호를 공백으로 구분하여 입력
x = list(map(int, input().split()))

# 손님이 확인 요청한 부품 번호를 하나씩 확인
for i in x:
    # 해당 부품이 존재하는지 확인
    result = binary_search(array, i, 0, n - 1)
    if result != None:
        print('yes', end=' ')
    else:
        print('no', end=' ')
```  

> 답안 예시2 : 계수 정렬 이용  

```python
# N(가게의 부품 개수)를 입력받기
n = int(input())
array = [0] * 1000001

# 가게에 있는 전체 부품 번호를 입력받아서 기록
for i in input().split():
    array[int(i)] = 1

# M(손님이 확인 요청한 부품 개수)을 입력받기
m = int(input())
# 손님이 확인 요청한 전체 부품 번호를 공백으로 구분하여 입력
x = list(map(int, input().split()))

# 손님이 확인 요청한 부품 번호를 하나씩 확인
for i in x:
    # 해당 부품이 존재하는지 확인
    if array[i] == 1:
        print('yes', end=' ')
    else:
        print('no', end=' ')
```  

> 답안 예시3 : 집합 자료형 이용  

```python
# N(가게의 부품 개수)을 입력받기
n = int(input())
# 가게에 있는 전체 부품 번호를 입력받아서 집합(set) 자료형에 기록
array = set(map(int, input().split()))

# M(손님이 확인 요청한 부품 개수)을 입력받기
m = int(input())
# 손님이 확인 요청한 전체 부품 번호를 공백으로 구분하여 입력받기
x = list(map(int, input().split()))

# 손님이 확인 요청한 부품 번호를 하나씩 확인
for i in x:
    # 해당 부품이 존재하는지 확인
    if i in array:
        print('yes', end=' ')
    else:
        print('no', end=' ')
```



---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
