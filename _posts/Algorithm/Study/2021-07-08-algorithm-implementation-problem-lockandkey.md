---
title: "[Implementation] 자물쇠와 열쇠"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Implementation, 풀이실패]

toc:  true
toc_sticky: true

date: 2021-07-08
last_modified_at: 2021-07-20
---

## 자물쇠와 열쇠  

난이도 : ⭐⭐  
푼횟수 : 🔴⚪⚪  

고고학자인 "튜브"는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.  
잠겨있는 자물쇠는 격자 한 칸의 크기가 1 x 1인 N x N 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 M x M 크기인 정사각 격자 형태로 되어 있습니다.  
자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.  
열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.  

**<u>제한 사항</u>**  
- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.  
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.  
- M은 항상 N 이하입니다.  
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.  
- 0은 홈 부분, 1은 돌기 부분을 나타냅니다.  

**<u>입출력 예</u>**  
![image](https://user-images.githubusercontent.com/37467408/124863048-633d3480-dff1-11eb-8b10-6fbeae657e1e.PNG)  

**<u>입출력 예에 대한 설명</u>**  
![image](https://user-images.githubusercontent.com/37467408/124863134-8d8ef200-dff1-11eb-99df-79454ca1630b.PNG)  
key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.  

> 나의 풀이  

> 문제 해설  

먼저 효율적인 알고리즘을 작성해야하는지 알아보자. 자물쇠와 열쇠 크기는 20 x 20보다 작다. 크기가 20 x 20인 2차원 리스트에 있는 모든 원소에 접근할 때는 400만큼의 연산이 필요하다.  
일반적인 코딩 테스트 채점 환경에서 1초에 2,000만에서 1억 정도의 연산을 처리할 수 있다. 그렇기 때문에 완전 탐색을 이용해서 열쇠를 이동이나 회전시켜서 자물쇠에 끼워보는 작업을 전부 시도해보는 접근 방법을 이용할 수 있다.  
헤당 문제의 해결 아이디어는 완전 탐색이다. 다만, 완전 탐색을 수월하게 하기 위해서 자물쇠 리스트의 크기를 3배 이상으로 변경하면 계산이 수월해진다. 이때 가장 먼저 자물쇠를 크기가 3배인 새로운 리스틀 만들어 중앙 부분으로 옮긴다.  

![image](https://user-images.githubusercontent.com/37467408/126250241-3a711b17-507d-48c7-9380-ff9bdde3fd6e.PNG)  

이제 열쇠 배열을 왼쪽 위부터 시작해서 한 칸씩 이동하는 방식으로 차례대로 자물쇠의 모든 흠을 채울 수 있는지 확인하면 된다. 문제에서는 0은 홈 부분, 1은 돌기 부분을 나타낸다. 따라서 자물쇠 리스트에 열쇠 리스트의 값을 더한 뒤에 더한 결과를 확인했을 때 자물쇠 모든 값이 정확히 1인지를 확인하면 된다. 만약 모든 값이 1이라면 자물쇠의 홈 부분을 정확히 채운 것이라고 할 수 있다.  

![image](https://user-images.githubusercontent.com/37467408/126250515-f945b252-ce7e-4b89-a9bf-f8ec72fcd2b1.PNG)  

> 문제 답안  

```python
# 2차원 리스트 90도 회전
def rotate_a_matrix_by_90_degree(a):
    n = len(a) # 행 길이 계산
    m = len(a[0]) # 열 길이 계산
    result = [[0] * n for _ in range(m)] # 결과 리스트
    for i in range(n):
        for j in range(m):
            result[j][n - i - 1] = a[i][j]
    return result

# 자물쇠와 중간 부분이 모두 1인지 확인
def check(new_lock):
    lock_length = len(new_lock) // 3
    for i in range(lock_length, lock_length * 2):
        for j in range(lock_length, lock_length * 2):
            if new_lock[i][j] != 1:
                return False
    return True

def solution(key, lock):
    n = len(lock)
    m = len(key)
    # 자물쇠의 크기를 기존의 3배로 변환
    new_lock = [[0] * (n * 3) for _ in range(n * 3)]
    # 새로운 자물쇠의 중앙 부분에 기존의 자물쇠 넣기
    for i in range(n):
        for j in range(n):
            new_lock[i + n][j + n] = lock[i][j]

    # 4가지 방향에 대해서 확인
    for rotation in range(4):
        key = rotate_a_matrix_by_90_degree(key) # 열쇠 회전
        for x in range(n * 2):
            for y in range(n * 2):
                # 자물쇠에 열쇠를 끼워 넣기
                for i in range(m):
                    for j in range(m):
                        new_lock[x + i][y + j] += key[i][j]
                        # 새로운 자물쇠에 열쇠가 정확히 들어맞는지 검사
                if check(new_lock) == True:
                    return True
                # 자물쇠에서 열쇠를 다시 빼기
                for i in range(m):
                    for j in range(m):
                        new_lock[x + i][y + j] -= key[i][j]
    return False
```

<br>
기출 : 2020 카카오 신입 공채  
링크 : <https://programmers.co.kr/learn/courses/30/lessons/60059>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
