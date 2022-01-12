---
title: "[Django] 오리엔테이션"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-11
last_modified_at: 2022-01-12
---

## 웹 프레임워크의 필요성과 장고에 대한 소개  

### 다양한 개발 파트  
- 웹 프론트엔드 개발  
-- 웹 브라우저에서 구동되는 애플리케이션을 개발, HTML/CSS/JavaScript를 기반으로 다수의 언어/라이브러리  
-- React.js, Vue.js, Angular, jQuery, Bootstrap 등  
- 스마트폰 애플리케이션 개발  
-- Android/iOS 스마트폰/태블릿에서 구동되는 애플리케이션을 Java/Swift/Objective-c 언어로 개발  
-- 앱에 웹브라우저를 임베딩하여 웹 프론트엔드 기술로 앱을 개발하기도 한다.  
- 인프라 관리  
-- 자체서버, 서버/웹 호스팅, AWS/Azure/Google/Heroky 클라우드 (laaS와 PaaS) 등  
-- 백엔드 개발 : Django, Flask, Spring(Java), Ruby on Rails(Ruby) 등  

### 웹프레임워크가 왜 필요한가요?  
- 우선 웹서비스가 왜 필요한가요?  
-- 서버의 역할  
-- `모든 서비스의 근간 !!!` 어떤 서비스이든 웹서비스는 당연히 잘 해야하는 분야. 덜 회자된다고 해서 지나간 유행이 아니다.  
-- 서버없이 머신런닝만 한다고 해서 서비스가 되지 않고, 앱만 한다고 해서 서비스가 되지 않는다.  
- 만들 수 있는 것  
-- 웹서비스, 앱 서버, 챗봇 서비스 등등  
- 웹서비스를 만들때마다 반복되는 것들을 표준화해서 묵어놓음.  
-- 거의 모든 언어마다 웹프레임워크가 존재  

### 다양한 파이썬 웹프레임워크  
- Django : 백엔드 개발에 필요한 거의 모든 기능을 제공. 중복된 작업을 최대한 줄여주는 최고의 웹 프레임워크  
- Flask : 백엔드 개발에 필요한 일부분의 기능을 제공. ORM으로서 SQLAlchemy를 주로 사용  
- Sanic, Tornado 등등  

### Django의 강점  
1. Python 생태계  
- 크롤링, 자동화, 머신런닝 코드와 같은 언어  
- 표현력이 좋고, 가독성이 높은 코드  
3. 건강한 커뮤니티  
4. 풀스택 웹프레임워크  
- 백엔드 개발에 필요한 거의 모든 것을 Django에서 직접 지원  
-- API 개발에 필요한 거의 모든 것을 django-rest-framework에서 지원  
- 참고) 프론트엔드 개발에서의 트렌드는 React, Vue, Angular 이다.  
6. 10년동안 충분히 성숙  
- 2008년에 1.0 공개, 2019년 12월에 3.0 공개  

### 장고는 MTV 프레임워크  
이름만 다를 뿐, MVC(Model-View-Controller)이다.  
- Model -> 장고의 Model  
-- 데이터베이스 SQL 쿼리를 생성/수행  
- View -> 장고의 Template  
-- 복잡한 문자열 조합을 도와준다.  
- Controller -> 장고의 View  
-- HTTP 클라이언트로부터의 요청을 처리하는 함수  

### 백엔드는 서비스의 중심이다.  
백엔드는 서비스의 중심이다. 백엔드/서비스 운영을 먼저 탄탄하게 하고 나서, 그 후에 프론트/앱을 고민하는 것이 맞다.  


## 웹 프론트엔드 패러다임의 전환가 SPA 그리고 리액트 소개   

### 패러다임의 전환 : "웹 문서 -> 웹 어플리케이션"  

### SPA(Single Page Application)  
웹 문서의 기본 동작에서는 화면 전환 시에  
1. 서버로부터 새로운 화면에 대한 HTML/CSS/JavaScript를 받아와서  
2. 전체 화면을 새로 그린다.  
-> 웹 문서에 적합한 방식  

SPA 방식의 화면 전환  
1. JavaScript를 통해 화면을 변경한다. -> 화면 전환 느낌이 나도록.  
2. 필요 시에 백그라운드에서 JavaScript로 서버와 통신을 한다.  
-> 웹 어플리케이션에 적합한 방식  

### 웹 프론트에서 3가지 언어  
HTML, CSS, Heavy JavaScript  

### Heavy JavaScript  
최신 자바스크립트 사용을 권장  
- 그런데, 브라우저 호환성을 생각하면 ES5를 써야한다. class/async/await/generator/arrow function. 등이 없다.  
- -> module bundler (webpack 등)을 써서, 코드 변환(Transpiling) 하면 OK.  
C/C++로 UI 어플리케이션 개발하진 않는다.  
- UI 프레임워크/라이브러리가 필요하다.  
- -> React.js, Vue.js, Angular를 쓴다.  

### 다양한 자바스크립트의 런타임  
거의 모든 웹브라우저에서 HTML/CSS/JavaScript 지원  
- 웹 브라우저 단에서 구동  
- 웹 브라우저/버전마다 지원하는 JavaScript Spec 상이  
데스크탑에서는 NodeJS 활용  
- 데스크탑/서버 단 개발이 가능  
- 윈도우에서의 Microsoft의 전폭적인 지지  

간단한 NodeJS 코드  
```
'use strict';

const http = require('http')
const port = process.env.PORT || 8888;

http.createServer((req, res)) => {
  res.writeHead(200, {'Content-Type': 'text/plain'};
  res.end('Hello World : ');
}).listen(port)
```  

### 트랜스파일링(Transpiling)  
최신 데스크톱 브라우저와 모바일 브러우저의 ES6 지원율이 90% 이상  
EVERGREEN 브라우저의 경우, ES6 코드는 별도의 트랜스파일링 과정이 필요 X  
최산 JavaScript Features를 사용하더라도  
- 낮은 버전의 JS를 지원하는 브라우저 지원을 위해 낮은 버전의 JS코드로 변환하는 트랜스파일링이 필요  
- -> babal을 활용하여 변환  

설치  
- npm install --global babel-cli  
- npm install -save babel-preset-es2015  

---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊