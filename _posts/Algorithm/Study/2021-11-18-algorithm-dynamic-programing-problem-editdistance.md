---
title: "[Dynamic Programing] 편집 거리"

categories:
  - Algorithm
tags:
  - [Dynamic Programing, Python]

toc:  true
toc_sticky: true

date: 2021-11-18
last_modified_at: 2021-11-19
---

## 편집 거리

난이도 : 🌕🌗   
푼횟수 : 🔴🟢⚪  

두 개 의 문자열 A와 B가 주어졌을 때, 문자열 A를 편집하여 문자열 B로 만들고자 합니다. 문자열 A를 편집할 때는 다음의 세 연산 중에서 한 번에 하나씩 선택하여 이용할 수 있습니다.  

- 삽입(Inset) : 특정한 위치에 하나의 문자를 삽입합니다.  
- 삭제(Remove) : 특정한 위치에 있는 하나의 문자를 삭제합니다.  
- 교체(Replace) : 특정한 위치에 있는 하나의 문자를 다른 문자로 교체합니다.  

이때 편집 거리란 문자열 A를 편집하여 문자열 B로 만들기 위해 사용한 연산의 수를 의미합니다. 문자열 A를 문자열 B로 만드는 최소 편집 거리를 계산하는 프로그램을 작성하세요. 예를 들어 'sunday'와 'saturday'의 최소 편집 거리는 3입니다.  

**<u>입력 조건</u>**  
- 두 문자열 A와 B가 한 줄에 하나씩 주어집니다.  
- 각 문자열은 영문 알파벳으로만 구성되어 있으며, 각 문자열의 길이는 1보다 크거나 같고, 5,000보다 작거나 같습니다.  

**<u>출력 조건</u>**  
- 최소 편집 거리를 출력합니다.  

**<u>입력 예시 1</u>**  
cat  
cut  

**<u>출력 예시 1</u>**  
1  

**<u>입력 예시 2</u>**  
sunday  
saturday  

**<u>출력 예시 2</u>**  
3  

> 나의 풀이

```python
word_a = input()
word_b = input()

len_a, len_b = len(word_a), len(word_b)

dp = [[0] * (len_b + 1) for _ in range(len_a + 1)]

for i in range(1, len_a + 1):
    dp[i][0] = i

for j in range(1, len_b + 1):
    dp[0][j] = j

for i in range(1, len_a + 1):
    for j in range(1, len_b + 1):
        if word_a[i - 1] == word_b[j - 1]:
            dp[i][j] = dp[i - 1][j - 1]
        else:
            dp[i][j] = 1 + min(dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1])



print(dp)
```

> 문제 해설  

이 문제는 최소 편집 거리를 담을 2차원 테이블을 초기화한 뒤에, 최소 편집 거리를 계산해 테이블에 저장하는 과정으로 문제를 해결할 수 있다. 다이나믹 프로그래밍 점화식은 다음과 같다.  

- 두 문자가 같은 경우 : dp[i][j] = dp[i - 1][j - 1]  
- 두 문자가 다른 경우 : dp[i][j] = 1 + min(dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1])  

이를 말로 풀어서 쓰면 다음과 같다.  

- 행과 열에 해당하는 문자가 서로 같다면, 왼쪽 위에 해당하는 수를 그대로 삽입  
- 행과 열에 해당하는 문자가 서로 다르다면, 왼쪽(삽입), 위쪽(삭제), 왼쪽 위(교체)에 해당하는 수 중에서 가장 작은 수에 1을 더해 대입  

예를 들어 "sunday"를 "saturday"로 변경한다고 해보자. 이때 초기 2차원 테이블은 다음과 같이 구성된다. 왼쪽(열)에 있는 "sunday"라는 문자열을 위쪽(행)에 있는 "saturday"로 변경하는 비용을 계산할 수 있도록 이와 같이 테이블을 구성한 것이다. 또한 여기에서 파이는 빈 문자열을 의미한다. 빈 문자열을 "saturday"로 만들기 위해서는 8개의 문자를 삽입해야 하기 때문에, 테이블이 dp[0][8]의 값은 8이다.  

![image](https://user-images.githubusercontent.com/37467408/142369157-53ad00dc-7c54-4bc5-b2e3-9ada671b8e51.png)  

이제 점화식에 따라서 전체 테이블을 차례대로 갱신해주면 다음과 같다. 2차원 테이블은 왼쪽(열)에 있는 문자열을 위쪽(행)에 있는 문자열로 바꾸는 비용을 직관적으로 보여준다. 예를 들어 dp[3][3]의 값은 2인데, 이는 "sun"이라는 문자열을 "sat"이라는 문자열로 바꾸기 위한 최소 편집 거리가 2라는 의미가 된다.  

결과적으로 테이블의 가장 오른쪽 아래에 있는 값이 구하고자 하는 최소 편집 거리가 된다. 즉, 아래 예시에서 최소 편집 거리는 3이다.  

![image](https://user-images.githubusercontent.com/37467408/142369447-19dd9380-e0b2-45d4-91ba-530dc17e8c5b.png)  

> 문제 답안  

```python
# 최소 편집 거리(Edit Distance) 계산을 위한 다이나믹 프로그래밍
def edit_dist(str1, str2):
    n = len(str1)
    m = len(str2)

    # 다이나믹 프로그래밍을 위한 2차원 DP 테이블 초기화
    dp = [[0] * (m + 1) for _ in range(n + 1)]

    # DP 테이블 초기 설정
    for i in range(1, n + 1):
        dp[i][0] = i
    for j in range(1, m + 1):
        dp[0][j] = j

    # 최소 편집 거리 계산
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            # 문자가 같다면, 왼쪽 위에 해당하는 수를 그대로 대입
            if str1[i - 1] == str2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            # 문자가 다르다면, 3가지 경우 중에서 최솟값 찾기
            else: # 삽입(왼쪽), 삭제(위쪽), 교체(왼쪽 위) 중에서 최소 비용을 찾아 대입
                dp[i][j] = 1 + min(dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1])

    return dp[n][m]

# 두 문자열을 입력받기
str1 = input()
str2 = input()

# 최소 편집 거리 출력
print(edit_dist(str1, str2))
```

<br>
기출 : Goldman Sachs 인터뷰  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊