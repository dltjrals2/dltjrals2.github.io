---
title: "[Sorting] 국영수"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Sotring, BaekJoon]

toc:  true
toc_sticky: true

date: 2021-10-12
last_modified_at: 2021-10-12
---

## 국영수  

난이도 : 🌕  
푼횟수 : 🟢⚪⚪  

도현이네 반 학생 N명의 이름과 국어, 영어, 수학 점수가 주어진다. 이때, 다음과 같은 조건으로 학생의 성적을 정렬하는 프로그램을 작성하시오.  

1. 국어 점수가 감소하는 순서로  
2. 국어 점수가 같으면 영어 점수가 증가하는 순서로  
3. 국어 점수와 영어 점수가 같으면 수학 점수가 감소하는 순서로  
4. 모든 점수가 같으면 이름이 사전 순으로 증가하는 순서로 (단, 아스키 코드에서 대문자는 소문자보다 작으므로 사전순으로 앞에 온다.)  

**<u>입력</u>**  
첫째 줄에 도현이네 반의 학생의 수 N (1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 한 줄에 하나씩 각 학생의 이름, 국어, 영어, 수학 점수가 공백으로 구분해 주어진다. 점수는 1보다 크거나 같고, 100보다 작거나 같은 자연수이다. 이름은 알파벳 대소문자로 이루어진 문자열이고, 길이는 10자리를 넘지 않는다.  

**<u>출력</u>**  
문제에 나와있는 정렬 기준으로 정렬한 후 첫째 줄부터 N개의 줄에 걸쳐 각 학생의 이름을 출력한다.  

**<u>예제 입력 1</u>**  
12  
Junkyu 50 60 100  
Sangkeun 80 60 50  
Sunyoung 80 70 100  
Soong 50 60 90  
Haebin 50 60 100  
Kangsoo 60 80 100  
Donghyuk 80 60 100  
Sei 70 70 70  
Wonseob 70 70 90  
Sanghyun 70 70 80  
nsj 80 80 80  
Taewhan 50 60 90  

**<u>예제 출력 1</u>**  
Donghyuk  
Sangkeun  
Sunyoung  
nsj  
Wonseob  
Sanghyun  
Sei  
Kangsoo  
Haebin  
Junkyu  
Soong  
Taewhan  

> 나의 풀이  

```python
student_number = int(input())
student_list = []

for _ in range(student_number):
    input_data = input().split()
    student_list.append((input_data[0], int(input_data[1]), int(input_data[2]), int(input_data[3])))

student_list = sorted(student_list, key = lambda x : (-x[1], x[2], -x[3], x[0]))

for student in student_list:
    print(student[0])
```  

> 문제 해설  

파이썬에서는 튜플을 원소로 하는 리스트가 있을 때, 그 리스트를 정렬하면 기본적으로 각 튜플을 구성하는 원소의 순서와 맞게 정렬된다는 특징이 있다.  

예를 들어 튜플이 3개의 원소로 구성된다면 모든 원소가 첫 번째 원소의 순서에 맞게 정렬되고, 첫 번째 원소의 값이 같은 경우 두 번째 원소의 순서에 맞게 정렬되고, 거기에 두 번째 원소의 값까지 같은 경우 세 번째 원소의 순서에 맞게 정렬된다.  

또한, 리스트의 원소를 정렬할 때는 sort() 함수의 key 속성에 값을 대입하여 내가 원하는 '조건'에 맞게 튜플을 정렬시킬 수 있다.  

<u>key = lambda x : (-x[1], x[2], -x[3], x[0])</u>에서의 의미는 아래와 같다.  

1. 두 번째 원소를 기준으로 내림차순 정렬  
2. 두 번째 원소가 같은 경우, 세 번째 원소를 기준으로 오름차순 정렬  
3. 세 번째 원소가 같은 경우, 네 번째 원소를 기준으로 내림차순 정렬  
4. 네 번째 원소가 같은 경우, 첫 번째 원소를 기준으로 오름차순 정렬  

> 문제 답안  

```python
n = int(input())
students = [] # 학생 정보를 담을 리스트

# 모든 학생 정보를 입력받기
for _ in range(n):
  students.append(input().split())

students.sort(key = lambda x : (-int(x[1]), int(x[2]), -int(x[3]), x[0]))

# 정렬된 학생 정보에서 이름만 출력
for student in students:
  print(student[0])
```

<br>
기출 : 핵심 유형  
링크 : <https://www.acmicpc.net/problem/10825>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
