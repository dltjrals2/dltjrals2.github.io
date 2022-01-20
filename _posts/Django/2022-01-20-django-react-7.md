---
title: "[Django] 장고 Views를 활용한 HTTP 요청 처리"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-20
last_modified_at: 2022-01-20
---

## 다양한 응답의 함수 기반 뷰 - 1  

### View  

1개의 HTTP 요청에 대해 -> 1개의 뷰가 호출  

urls.py/urlpatterns 리스트에 매핑된 `호출 가능한 객체`  
- 함수도 `호출 가능한 객체` 중의 하나  

웹 클라이언트로부터의 HTTP 요청을 처리  

크게 2가지 형태의 뷰  
- 함수 기반 뷰(Function Based View) : 장고 뷰의 `기본`  
-- `호출 가능한 객체`, 그 자체 
- 클래스 기반 뷰(Class Based View)  
-- 클래스.as_view()를 통해 `호출가능한 객체`를 생성/리턴  

### View 호출 시, 인자  
- HttpRequest 객체 및 URL Captured Values  

1번째 인자 : HttpRequest 객체  
- 현재 요청에 대한 모든 내역을 담고 있다.  

2번째~ 인자 : 현재 요청의 URL로부터 Capture된 문자열들  
- url/re_path를 통한 처리에서는 -> 모든 인자는 str 타입으로 전달  
- path를 통한 처리에서는 -> 매핑된 Converter의 to_python에 맞게 변환된 값이 인자로 전달  

### View 호출에 대한 리턴값  
- HttpResponse 객체  

필히 HttpResponse 객체를 리턴해야 한다.  
- 장고 Middleware에서는 뷰에서 HttpResponse 객체를 리턴하기를 기대 -> 다른 타입을 리턴하면 Middleware에서 처리 오류  
- django.shortcuts.render 함수는 템플릿 응답을 위한 shortcuts 함수  

파일like 객체 혹은 str/bytes 타입의 응답 지원  
- str 문자열을 직접 utf8로 인코딩할 필요가 없다.  
-- 장고 디폴트 설정에서 str 문자열을 utf8로 인코딩  
- response = HttpResponse(파일 like 객체 또는 str 객체 또는 bytes 객체)  

파일 l


## 다양한 응답의 함수 기반 뷰 - 2  



<br>
---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊