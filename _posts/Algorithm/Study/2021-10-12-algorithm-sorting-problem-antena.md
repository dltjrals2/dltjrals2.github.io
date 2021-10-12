---
title: "[Sorting] 안테나"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Sotring, BaekJoon]

toc:  true
toc_sticky: true

date: 2021-10-12
last_modified_at: 2021-10-12
---

## 안테나  

난이도 : 🌕  
푼횟수 : 🟢⚪⚪  

일직선 상의 마을에 여러 채의 집이 위치해 있다. 이중에서 특정 위치의 집에 특별히 한 개의 안테나를 설치하기로 결정했다. 효율성을 위해 안테나로부터 모든 집까지의 거리의 총 합이 최소가 되도록 설치하려고 한다. 이 때 안테나는 집이 위치한 곳에만 설치할 수 있고, 논리적으로 동일한 위치에 여러 개의 집이 존재하는 것이 가능하다.  

집들의 위치 값이 주어질 때, 안테나를 설치할 위치를 선택하는 프로그램을 작성하시오.  

예를 들어 N=4이고, 각 위치가 1, 5, 7, 9일 때를 가정하자.  

![image](https://user-images.githubusercontent.com/37467408/136877910-69fee307-95e9-496e-8125-f21a7df871b2.PNG)  

**<u>입력</u>**  
첫째 줄에 집의 수 N이 자연수로 주어진다. (1≤N≤200,000) 둘째 줄에 N채의 집에 위치가 공백을 기준으로 구분되어 1이상 100,000이하의 자연수로 주어진다.  

**<u>출력</u>**  
첫째 줄에 안테나를 설치할 위치의 값을 출력한다. 단, 안테나를 설치할 수 있는 위치 값으로 여러 개의 값이 도출될 경우 가장 작은 값을 출력한다.  

**<u>예제 입력 1</u>**  
4  
5 1 7 9  

**<u>예제 출력 1</u>**  
5  

> 나의 풀이

```python
house_number = int(input())
house_list = list(map(int, input().split()))
house_list.sort()

print(house_list[(house_number - 1) // 2])
```

> 문제 해설  

이 문제의 핵심 아이디어는 정확히 중간값에 해당하는 위치의 집에 안테나를 설치했을 때, 안테나로부터 모든 집까지의 거리의 총합이 최소가 된다는 점이다.  

따라서 이 문제는 단순히 모든 집의 위치 정보를 입력받은 뒤에, 이를 정렬해서 중간값을 출력하면 정답 판정을 받을 수 있다.  

> 문제 답안  

```python
n = int(input())
data = list(map(int, input().split()))
data.sort()

# 중간값(median)을 출력
print(data[(n - 1) // 2])
```

<br>
기출 : 2019 SW 마에스트로 입학 테스트  
링크 : <https://www.acmicpc.net/problem/18310>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
