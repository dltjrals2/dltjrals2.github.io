---
title: "자율 주행(운항)의 위치 추정 방식"

categories:
  - Autonomous_Algorithm
tags:
  - [Autonomous Drive, Python]

use_math: true

toc:  true
toc_sticky: true

date: 2022-10-06
last_modified_at: 2022-10-06
---

# 자율 주행(운항)의 현재 위치 추정  

## 위치 추정  

### GPS를 이용한 방식  

#### RTK의 개념  

RTK는 Real Time Kinematic의 약어 이다. 위도, 경도, 고도 등 이미 정밀한 위치값을 알고 있는 기지국(예: 위성기준점)에 GPS 기지국을 설치하여 그곳의 GPS 위상 오차를 계산하고, 근처의 이동국은 해당 데이터를 받아 GPS 오차를 계산해 실시간으로 cm 단위 급의 정말도의 좌표값을 알 수 있는 측위 기법이다. 기지국과 멀어질수록 정밀도는 낮아진다.  

#### 자율주행/운항과 GNSS  

자율주행 혹은 자율운항의 위치 측정에 GNSS는 시간과 위도, 경도, 방위각, 속도 등을 사용할 때 쓸 수 있다.  

GNSS로 연속 측정을 할 때 오차의 누적은 없을 수도 있으나, 기상 상태나 주변 환경에 따라 신뢰도 등에 영향을 받을 수 있다.  

### SLAM을 이용한 방식  

#### SLAM에 대한 개념  

LiDAR로 `SLAM`(Simultaneous localization and mapping, 동시적 위치추정 및 지도 작성) 혹은 카메라로 VSLAM(Visual SLAM) 을 사용하는 방식이 있다.  

제가 사용했던 방식은 LRF(Laser Range Finder)를 이용하여 SLAM 방식인데, 모바일 로봇을 사용할 공간에 위치하여 일정 시간 반복하여 SLAM 작업을 하여 해당 공간의 Map을 알아낼 수 있다.  

![image](https://user-images.githubusercontent.com/37467408/194200585-ffe2e943-62b8-49f4-ad4e-4302258fc0cb.png)  

위와 같이 계측할 환경에 빨간색 선으로 추정하며 Map이 확정이 된 환경은 검정색 선으로 마무리가 되며, 2차원 점유 격자 지도가 완성이 된다.  

2차원 점유 격자 지도는 연속적인 주위 환경을 2차원 셀의 배열로 표현하고 셀들이 각각 차지하는 공간이 얼마나 점유되어 있는가에 대한 확률치를 구하는 식으로 점유 격자 지도가 완성이 된다.  

흰색은 이동 가능한 자유 영역(Free area), 검은색은 이동 불가능한 점유 영역(Occupied area), 회색은 확인되지 않은 미지 영역(Unknown area)으로 표현이 된다.  

해당 영역들은 0에서 255의 값으로 표현하는 그레이 스케일 값으로 나타낸다. 이 값은 점유 상태를 표현한 점유 확률을 베이즈 정리의 사후 확률(Posterior Probability)을 통해 구하게 된다. 점유 확률(occ)는 occ = (255 - color_avg) / 255.0 으로 표현이 된다. 만약, 이미지가 24 비트라면 color_avg = (한 셀의 그레이 스케일 값 / 0xFFFFFF * 255) 이 된다.  

이 점유 확률(occ)가 1에 가까울수록 점유되었을 확률이 높아지고 0에 가까울수록 비 점유되었다는 것을 의미한다.

`과연 실시간 자율 주행과 자율 운항에 있어서 SLAM이 이용이 될 수 있을까? 주행을 하면서 Mapping을 한다는 것은 정확도가 떨어져 사고의 위험성이 생길 수 있지 않을까? 라는 의문이 든다..`  

#### SLAM 작업을 하며 현재 위치 추정  

로봇이 미지의 영역(Unknown Area)를 이동하면서 주변 환경을 센싱하며 지도를 작성하며, 동시에 로봇의 현재 위치를 추정한다. 이동량을 바퀴의 회전축과 회전량으로 측정할 수 있다.  

![image](https://user-images.githubusercontent.com/37467408/194202512-6157af01-660a-4350-9a2d-6c9464ec2031.png)  

x, y : 회전축  
D : 바퀴간 거리  
r : 바퀴의 반지름  

![image](https://user-images.githubusercontent.com/37467408/194202660-0bd6fd9d-fce0-411b-ae86-8a094e29c294.png)  

좌/우측 모터의 엔코더 값을 이용하여 좌/우측 휠의 선속도를 구하는 식은 아래와 같다.  

$ v_l = \frac{E_lc - E_lp}{T_e} \times \frac{\pi}{180} (^rad/_s)$  
$ v_r = \frac{E_rc - E_rp}{T_e} \times \frac{\pi}{180} (^rad/_s)$  
<br>
$ V_l = v_l \times r (^m/_s)$  
$ V_r = v_r \times r (^m/_s)$  
<br>  

좌/우측 휠의 선속도를 이용해 로봇의 선속도와 각속도를 구하는 식은 아래와 같다.  

$ v = \frac{V_l + V_r}{2} (^m/_s)$  
$ \omega = \frac{V_r - V_l}{D} (^m/_s)$  
<br>  

로봇의 선속도와 각속도를 이용하여 이동한 위치 및 각도의 근삿값을 구하는 식은 아래와 같다.  

$ x_k+1 = x_k + T_e v_k cos(\theta_k + \frac{T_e \omega_k}{2}) $  
$ y_k+1 = y_k + T_e v_k sin(\theta_k + \frac{T_e \omega_k}{2}) $  

$ \theta_{k+1} = \theta_k + \omega_k T_e $  

$ E_lc, E_lp : 좌측 모터의 엔코더 값(현재와 이전값) $  
$ E_rc, E_rp : 우측 모터의 엔코더 값(현재와 이전값) $  
$ T_e : 경과시간 $  
$ v_l, v_r : 좌/우측 휠의 각속도 $  
$ r : 휠의 반지름 $  
$ V_l, V_r : 좌/우측 휠의 선속도 $  
$ v : 로봇의 선속도 $  
$ \omega : 로봇의 각속도 $  

위 과정을 통해 로봇의 이동한 위치와 각도의 근삿값을 구하여 위치를 추정할 수 있다.  

`자동차 or 로봇은 바퀴가 있어 바퀴의 회전량을 구할 수 있지만, 선박의 경우 이동량을 어떻게 측정해야 할까?`  

#### 벽, 물체 등의 장애물 추정과 SLAM 작업  

벽, 물체 등의 장애물을 파악하는 방법으로 LRF(Laser Range Finder) 센서를 이용하고, 해당 센서로 SLAM 작업을 실시한다.  

LRF 센서로 측정되는 벽과 물체 등의 장애물과의 거리를 구하는 공식은 아래와 같다.  

$ D = \frac{1}{2} c t = \frac{1}{2} \frac{c \varphi}{\omega} = \frac{c}{4 \pi f} (N \pi + \bigtriangleup \varphi) = \frac{\lambda}{4} (N + \bigtriangleup N) $  

레이저를 발사하고 돌아온 시간을 반으로 나누어 계산한다.  

c : 광원의 속도  
t : 반사되어 돌아온 시간  
$ \varphi $ : 위상 delay  
w : 광파의 각주파수  
$ \lambda $ : 파장  
N : 파장 반주기  

SLAM에 필요한 데이터는 아래의 사진과 같다.  

![image](https://user-images.githubusercontent.com/37467408/194216776-72c521d6-d101-4421-b585-8b97167cd71a.png)  

scan(거리 값) : 위에서 LRF 센서를 이용한 거리 값이다. LRF 센서를 이용하여 X-Y 평면을 스캔하여 거리 값으로 지정한다.  

tf(센서의 위치정보 반환 값) : 로봇에 고정된 센서가 로봇과 함께 움직이며 로봇의 이동량인 Odometry 에 의존된다. 이를 계산하여 위치값으로 만든다.  

위, 2가지 정보를 기반으로 SLAM을 실행하게 되고, Map을 작성할 수 있다.  

이제, 네비게이션이 이동 가능한 계산을 위한 costmap 을 살펴보자.

엔코더로 부터 얻은 Odometry와 위 정보를 기반으로 로봇의 위치를 추정하여 로봇의 위치를 얻어낼 수 있다.  

로봇에 장착된 LRF 센서를 사용하여 로봇과 장애물과의 거리를 계산하고, 이때 얻은 로봇의 위치, 센서의 위치, 장애물 위치 정보, 점유 격자 지도를 고정지도로 불러와서 사용한다.  

로봇 네비게이션을 진행할 때, 위 4가지 요소로 장애물 영역, 장애물과 충돌이 예상되는 영역, 로봇이 이동 가능한 영역을 계산하는데 이를 costmap 이라고 한다.  

costmap은 0에서 255까지의 값으로 표현된다.  

- 000 ~ 000 : 로봇이 자유롭게 이동 가능한 자유 영역(free area)  
- 001 ~ 127 : 충돌하지 않는 영역  
- 128 ~ 252 : 충돌 가능성이 있는 영역  
- 253 ~ 254 : 충돌 영역  
- 255 ~ 255 : 로봇이 이동 불가능한 점유 영역(occupied area)  

![image](https://user-images.githubusercontent.com/37467408/194218864-6b6850a9-12a9-498d-8c80-88bf8f99feaa.png)  

위 사진에 대한 설명은 아래와 같다.  

- 빨간색 : LRF 센서에서 얻은 거리 값으로 장애물을 표시한다.  
- 노란색 : 실제 장애물 표시  
- 하늘색 : 로봇 중심의 위치가 이 영역으로 오게 되면 충돌하게 되는 지점 표시  
- 파란색 : 안전한 지역 표시  

AMCL(Adaptive Monte Carlo Localization) 방식을 이용한 Localization  

AMCL 방식은 Particle Filter을 이용한다. LRF 센서와 엔코더로 부터의 거리, 이동 정보를 얻어서 베이즈 추론을 이용해 로봇이 위치하고 있을 가능성을 확률로 계산한다.  

`GPS 방식은 바다의 한 중앙을 항해하는 선박에서는 적용하기 힘들 것 같다. 기지국과 멀어지므로 GPS의 정확도가 낮아질 수 박에 없다. 그러면 SLAM 방식은 정해진 항로에 대해서는 SLAM을 통한 MAP을 가지고 있고, 지속적으로 지도 데이터를 업데이트 해주는 방향으로 하는게 좋을 것 같다.`

**🐢 현재 공부하고 있는 자율주행/자율운항을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

참고 블로그  
[AMCL 내용](https://intrepidgeeks.com/tutorial/chapter-vi-estimation-of-magnetic-position-with-amcl)  
[LRF 센서](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=iotsensor&logNo=221179629275)  
[모바일 로봇의 위치 추정](https://techfair.kaist.ac.kr/sub0911/file_down/id/307)  

감사합니다.😊