---
title: "[Sorting] 성적이 낮은 순서로 학생 출력하기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Sotring]

toc:  true
toc_sticky: true

date: 2021-06-18
last_modified_at: 2021-06-18
---

## 성적이 낮은 순서로 학생 출력하기  

난이도 : ⭐  

N명의 학생 정보가 있다. 학생 정보는 학생의 이름과 학생의 성적으로 구분된다. 각 학생의 이름과 성적 정보가 주어졌을 때 성적이 낮은 순서대로 학생의 이름을 출력하는 프로그램을 작성하시오.  

**<u>입력 조건</u>**  
- 첫 번째 줄에 학생의 수 N이 입력된다. (1 <= N <= 100,000)  
- 두 번째 줄부터 N + 1번째 줄에는 학생의 이름을 나타내는 문자열 A와 학생의 성적을 나타내는 정수 B가 공백으로 구분되어 입력된다. 문자열 A의 길이와 학생의 성적은 100 이하의 자연수이다.  

**<u>출력 조건</u>**  
- 모든 학생의 이름을 성적이 낮은 순서대로 출력한다. 성적이 동일한 학생들의 순서는 자유롭게 출력해도 괜찮다.  

**<u>입력 예시</u>**  
2  
홍길동 95  
이순신 77  

**<u>출력 예시</u>**  
이순신 홍길동  

> 나의 풀이  

```python
n = int(input())
array =[]

for _ in range(n):
    data = input().split()
    array.append((data[0], int(data[1])))

array = sorted(array, reverse=False, key = lambda x : x[1])

for i in range(n):
    print(array[i][0], end=' ')
```  

> 문제 해설  

학생의 정보가 최대 100,000개까지 입력될 수 있으므로 최악의 경우 O(NlogN)을 보장하는 알고리즘을 이용하거나 O(N)을 보장하는 계수 정렬을 이용하면 된다. 입력은 학생의 이름과 점수이고 정렬의 기준이 점수이므로 이런 경우에도, 파이썬의 기본 정렬 라이브러리를 사용하는 것이 효과적이다.  

> 답안 예시  

```python
# N을 입력받기
n = int(input())

# N명의 학생 정보를 입력받아 리스트에 저장
array = []
for i in range(n):
    input_data = input().split()
    # 이름은 문자열 그대로, 점수는 정수형으로 변환하여 저장
    array.append((input_data[0], int(input_data[1])))

# 키(key)를 이용하여, 점수를 기준으로 정렬
array = sorted(array, key = lambda student : student[1])

# 정렬이 수행된 결과를 출력
for student in array:
    print(student[0], end=' ')
```

<br>
기출 : D 기업 프로그래밍 콘테스트 예선  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
