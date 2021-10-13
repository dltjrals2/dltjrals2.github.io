---
title: "[BinarySearch] 공유기 설치"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, BinarySearch, BaekJoon]

toc:  true
toc_sticky: true

date: 2021-10-13
last_modified_at: 2021-10-13
---

## 공유기 설치  

난이도 : 🌕🌕  
푼횟수 : 🟢⚪⚪  

도현이의 집 N개가 수직선 위에 있다. 각각의 집의 좌표는 x1, ..., xN이고, 집 여러개가 같은 좌표를 가지는 일은 없다.  

도현이는 언제 어디서나 와이파이를 즐기기 위해서 집에 공유기 C개를 설치하려고 한다. 최대한 많은 곳에서 와이파이를 사용하려고 하기 때문에, 한 집에는 공유기를 하나만 설치할 수 있고, 가장 인접한 두 공유기 사이의 거리를 가능한 크게 하여 설치하려고 한다.  

C개의 공유기를 N개의 집에 적당히 설치해서, 가장 인접한 두 공유기 사이의 거리를 최대로 하는 프로그램을 작성하시오.  

**<u>입력</u>**  
- 첫째 줄에 집의 개수 N (2 ≤ N ≤ 200,000)과 공유기의 개수 C (2 ≤ C ≤ N)이 하나 이상의 빈 칸을 사이에 두고 주어진다.  
- 둘째 줄부터 N개의 줄에는 집의 좌표를 나타내는 xi (0 ≤ xi ≤ 1,000,000,000)가 한 줄에 하나씩 주어진다.  

**<u>출력</u>**  
- 첫째 줄에 가장 인접한 두 공유기 사이의 최대 거리를 출력한다.  

**<u>예제 입력 1</u>**  
5 3  
1  
2  
8  
4  
9  

**<u>예제 출력 1</u>**  
3  

**<u>힌트</u>**  
- 공유기를 1, 4, 8 또는 1, 4, 9에 설치하면 가장 인접한 두 공유기 사이의 거리는 3이고, 이 거리보다 크게 공유기를 3개 설치할 수 없다.  


> 나의 풀이  


```python
n, c = map(int, input().split())
array = []

for _ in range(n):
    array.append(int(input()))

array.sort()

def binary_search(array, start, end):
    while start <= end:
        mid = (start + end) // 2
        current = array[0]
        count = 1

        for i in range(1, n):
            if array[i] >= current + mid:
                count += 1
                current = array[i]

        if count >= c:
            global answer
            start = mid + 1
            answer = mid
        else:
            end = mid - 1

start = 1
end = array[-1] - array[0]
answer = 0

binary_search(array, start, end)
print(answer)
```


> 문제 해설  

이 문제는 '가장 인접한 두 공유기 사이의 거리'의 최댓값을 탐색해야 하는 문제로 이해할 수 있다.  

이때 각 집의 좌표가 최대 10억이므로, 이진 탐색을 이용하면 문제를 해결할 수 있다. 따라서 이진 탐색으로 '가장 인접한 두 공유기 사이의 거리'를 조절해가며, 매 순간 실제로 공유기를 설치하여 C보다 많은 개수로 공유기를 설치할 수 있는지 체크하여 문제를 해결할 수 있다.  

다만, '가장 인접한 두 공유기 사이의 거리'의 최댓값을 찾아야 하므로, C보다 많은 개수로 공유기를 설치할 수 있다면 '가장 인접한 두 공유기 사이의 거리'의 값을 증가시켜서, 더 큰 값에 대해서도 성립하는지 체크하기 위해 다시 탐색을 수행한다.  


> 문제 답안  


```python
# 집의 개수(N)와 공유기의 개수(C)를 입력받기
n, c = list(map(int, input().split()))

# 전체 집의 좌표 정보를 입력받기
array = []
for _ in range(n):
    array.append(int(input()))
array.sort() # 이진 탐색 수행을 위해 정렬 수행

start = array[1] - array[0] # 집의 좌표 중에 가장 작은 값
end = array[-1] - array[0] # 집의 좌표 중에 가장 큰 값
result = 0

while (start <= end):
    mid = (start + end) // 2 # mid는 가장 인접한 두 공유기 사이의 거리(gap)을 의미
    value = array[0]
    count = 1
    # 현재 mid값을 이용해 공유기를 설치
    for i in range(1, n): # 앞에서부터 차근차근 설치
        if array[i] >= value + mid:
            value = array[i]
            count += 1

    if count >= c # C개 이상의 공유기를 설치할 수 있는 경우, 거리를 증가
        start = mid + 1
        result = mid # 최적의 결과를 저장
    else: # C개 이상의 공유기를 설치할 수 없는 경우, 거리를 감소
        end = mid - 1

print(result)
```


<br>
기출 : 핵심유형  
링크 : <https://www.acmicpc.net/problem/2110>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊