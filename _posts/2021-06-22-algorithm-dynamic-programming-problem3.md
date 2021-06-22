---
title: "[Dynamic Programming] 바닥 공사"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Dynamic Programming]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-22
last_modified_at: 2021-06-22
sitemap :
  changefreq : daily
  priority : 1.0
---

## 바닥 공사  

난이도 : ⭐⭐  

가로의 길이가 N, 세로의 길이가 2인 직사각형 형태의 얇은 바닥이 있다. 태일이는 이 얇은 바닥을 1 x 2의 덮개, 2 x 1 의 덮개, 2 x 2의 덮개를 이용해 채우고자 한다.  

![image](https://user-images.githubusercontent.com/37467408/122863830-7cda4d00-d35e-11eb-8280-5cc5c406d7ae.PNG)  

이때 바닥을 채우는 모든 경우의 수를 구하는 프로그램을 작성하시오. 예를 들어 2 x 3 크기의 바닥을 채우는 경우의 수는 5가지이다.  

**<u>입력 조건</u>**  
- 첫째 줄에 N이 주어진다. (1 <= N <= 1,000)

**<u>출력 조건</u>**  
- 첫째 줄에 2 x N 크기의 바닥을 채우는 방법의 수를 796.796으로 나눈 나머지를 출력한다.  

**<u>입력 예시</u>**  
3  

**<u>출력 예시</u>**  
5  

> 나의 풀이  

```python
# 가로의 길이가 N, 세로의 길이가 2
n = int(input()) # (1 <= n <= 1,000)
d = [0] * 1001

d[1] = 1
d[2] = 3

for i in range(3, n + 1):
    d[i] = (d[i - 1] + (d[i - 2] * 2)) % 796796

print(d[n])
```

> 문제 해설  

다이나믹 프로그래밍 문제에서는 종종 결과를 어떤 수로 나눈 결과를 출력하라는 내용이 들어가 있는 경우가 많다. 이 문제에서도 796,796으로 나눈 나머지를 출력하라고 하는데, 이는 단지 결괏값이 굉장히 커질 수 있기 때문에 그런 것이다.  
예를 들어 N이 3일 때 바닥을 덮개로 채울 수 있는 모든 경우의 수는 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/122864874-5cab8d80-d360-11eb-82ef-6d20878edf30.PNG)  

또한, 왼쪽부터 차례대로 바닥을 덮개로 채운다고 생각하면 어렵지 않게 점화식을 세울 수 있다.  

- 왼쪽부터 i - 1까지 길이가 덮개로 이미 채워져 있으면 2 x 1의 덮개를 채우는 하나의 경우밖에 존재하지 않는다.  

![image](https://user-images.githubusercontent.com/37467408/122864972-85cc1e00-d360-11eb-8383-86cd1ca51ffc.PNG)  

- 왼쪽부터 i - 2까지 길이가 덮개로 이미 채워져 있으면 1 x 2 덮개 2개를 넣는 경우, 혹은 2 x 2의 덮개 하나를 넣는 경우로 2가지 경우가 존재한다.  

![image](https://user-images.githubusercontent.com/37467408/122865040-a7c5a080-d360-11eb-9160-6f845da4e6af.PNG)  

또한 이 문제 역시 왼쪽부터 N - 2 미만의 길이에 대해서는 고려할 필요가 없다. 왜냐하면 사용할 수 있는 덮개의 형태가 최대 2 x 2의 직사각형 형태이기 때문이다. 다시 말해 바닥을 채울 수 있는 형태는 위에서 언급한 경우밖에 없다. 따라서 다음과 같이 점화식을 세울 수 있다.  

![image](https://user-images.githubusercontent.com/37467408/122865141-cfb50400-d360-11eb-8f3e-3122df9718a7.PNG)  

왼쪽부터 N - 2까지 길이가 덮개로 이미 채워져 있는 경우 덮개를 채우는 방법은 2가지 경우가 있다. 이 두 방법은 서로 다른 것이므로, 결과적으로 위 식이 된다.  

> 답안 예시  

```python
# 정수 N을 입력받기
n = int(input())

# 앞서 계산된 결과를 저장하기 위한 DP 테이블 초기화
d = [0] * 1001

# 다이나믹 프로그래밍(Dynamic Programming) 진행(보텀업)
d[1] = 1
d[2] = 3
for i in range(3, n + 1):
    d[i] = (d[i - 1] + 2 * d[i - 2]) % 796796

# 계산된 결과 출력
print(d[n])
```

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
