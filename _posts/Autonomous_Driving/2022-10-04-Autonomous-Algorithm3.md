---
title: "DWA(Dynamic Window Approach) Algorithm"

categories:
  - Autonomous_Algorithm
tags:
  - [DWA, Dynamic Window Approach, Autonomous Drive, Python, Robotics]

use_math: true

toc:  true
toc_sticky: true

date: 2022-10-04
last_modified_at: 2022-10-05
---

### DWA 알고리즘의 Flow Chart  

![image](https://user-images.githubusercontent.com/37467408/193846011-7dd701fb-645a-4aa4-8fb6-91700b54abf6.png)  

### main()  

- 전체적으로 쓰일 변수 및 배열의 선언, 초기화  
- 목적지에 도달할 때까지 연산 반복  
- 변수 목록  
-- 배의 상태 : x[x좌표, y좌표, yaw, 선속도, 각속도]  
-- 목표점 : goal[x좌표 gx, y좌표 gy]  
-- 설정값 : config  
-- 경로 : trajectory(x 배열 계속 추가)  
-- 장애물 : ob((x좌표, y좌표)형태 배열)  
-- 선속도 각속도의 쌍 : u  
-- 추정 경로 : predicted_trajectory  
-- 목표지점까지 거리 : dist_to_goal  

경로의 trajectory의 목록은 아래와 같다.  

![image](https://user-images.githubusercontent.com/37467408/193847349-313c14d4-6e44-4ca2-a641-6c642ea52cde.png)  

장애물의 ob는 config ob 이다.  

선속과 각속도의 쌍 : u, 추정 경로 : predicted_trajectory는 dwa_control의 return 값이다.  

### dwa_control  

- DWA 계속 연산  
- 파라미터 : 배의 상태 x, 설정 값 config, 목표점 goal, 장애물 ob  
- 리턴 값 : u, trajectory  
- 변수 목록  
-- dw : dynamic window 계산  
-- u : 최종 계산된 [v, yaw_rate 쌍]  
-- trajectory : 최종 계산된 경로  

dw(dynamic window 계산)은 calc_dynamic_window의 return 값이다.  

u(최종 계산된 [v, yaw_rate 쌍]), trajectory(최종 계산된 경로)는 calc_control_and_trajectory의 return 값이다.  

### calc_dynamic_window  

- 현재 위치 기준으로 Dynamic Window(Vs, Vd, dw) 계산  
- 파라미터 : 배 상태 x, 설정 값 config  
- 리턴값 : Dynamic Window dw  
- 변수 목록  
-- Vs [최저 속도, 최고 속도, -최고 각속도, +최고 각속도]  
-- Vd [선속도 - 최고가속도 x dt, 선속도 + 최고가속도 x dt, 각속도 - 최대회전 x dt, 각속도 + 최대회전 x dt]  
-- dw [max 선속도, min 선속도, max 각속도] (Vs와 Vd 중 최저/최고 값 선택)  

### calc_control_and_trajecty  

- Dynamic Window로 최종 input 계산  
- 파라미터 : 배 상태 x, Dynamic Window dw, 설정값 config, 목표점 goal, 장애물 ob  
- 리턴값 : 최적[v, yaw_rate] 쌍 best_u, 최적 경로 best_trajectory  
- 구성  
-- 선속도 v를 max 선속도 ~ min 선속도를 v_resolution으로 나눠 반복문  
-- 각속도 yaw_rate도 max, min을 yaw_rate_resolution 간격으로 나눠 반복문  
-- trajectory 별로 cost 계산

trejectory는 predict_tajectory의 return 값이다.  

### predict_trajectory  

- predict_time까지 반복하여 경로 도출  
- 파라미터 : 초기 배 상태 x_init, 선속도 v, 각속도 yaw_rate, 설정값 config  
- 리턴값 : 경로 trajectory  
- 구성  
-- 단위 시간만큼 배 상태 x를 업데이트해서 trajectory에 배열 합치기 -> 판단 시간만큼 반복  

#### motion  

- 배 상태 x를 새로 만듦  
- 파라미터 : 배 상태 x, 선속도 각속도 쌍인 u, 설정값의 단위시간 config, dt  
- 리턴값 : 배 상태 x  
- 구성  
-- x좌표 += 선속도 * cos(yaw) * dt  
-- y좌표 += 선속도 * sin(yaw) * dt  
-- yaw += 각속도 * dt  
-- 선속도 = v  
-- 각속도 = yaw_rate  

### calc_to_goal_cost  
- 각각 다른 각도에서 goal cost 계산  
- 파라미터 : 경로 trajectory, 목표점 goal  
- 리턴값 : goal cost  
- 구성  
-- dx = (목표점 x좌표) - (경로 가장 마지막 상태 x의 x좌표)  
-- dy = (목표점 y좌표) - (경로 가장 마지막 상태 x의 y좌표)  
-- error_angle = $tan^-1(dy,dx)$  
-- cost_angle = (error_angle) - (경로 가장 마지막 상태 x의 yaw)  
-- cost = $tan^-1(sin(cost_angle), cos(cost_angle))$  

### calc_obstacle_cost  
- 목표지점까지의 거리의 역수 변환  
- 파라미터 : 경로 trajectory, 장애물 ob, 설정값 config  
- 리턴값 : 1/최소거리 r  
- 구성  
-- ox = 장애물 전체의 x좌표 배열  
-- oy = 장애물 전체의 y좌표 배열  
-- dx = 전체 경로의 x좌표 - ox  
-- dy = 전체 경로의 y좌표 - oy  
-- r = 빗변 계산(유클리드 거리)  

### dwa_control  
- DWA 계속 연산  
- 파라미터 : 배 상태 x, 설정값 config, 목표점 goal, 장애물 ob  
- 리턴값 : u, trajectory  
- 구조  
-- 배 상태 x에 motion()  
-- 경로 trajectory에 상태 히스토리 기록  
-- dist_to_goal = 배의 목표점 간 x, y 차이로 유클리드 거리 -> 이 값이 config.robot_radius보다 작으면 도착  

---
**🐢 현재 공부하고 있는 자율주행/자율운항을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

참고 사이트   
[Github - AtsushiSakai/PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics/blob/master/PathPlanning/DynamicWindowApproach/dynamic_window_approach.py)  
[LUMOS MAXIMA](https://velog.io/@717lumos/Autonomous-Ship-DWA-Algorithm)

감사합니다.😊