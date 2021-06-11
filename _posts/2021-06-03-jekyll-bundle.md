---
title: "[Github 블로그] jekyll 동작시키기"

categories:
  - Gitblog
tags:
  - [Blog, jekyll, Github, Git]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-03
last_modified_at: 2021-06-03
sitemap :
  changefreq : daily
  priority : 1.0
---

## 1. cmd를 동작시켜 내 Local Repositor로 이동해 chcp명령어를 입력한다.  

`cd` 명령어를 통해서 jekyll 테마를 설치한 Local Repository로 이동합니다.

> Console Code 페이지 변경 명령어  : chcp   

Console 창에서 현재의 코드 페이지 번호를 표시하거나 설정하는 기능입니다.  

Console 창에 `chcp 65001` 를 입력한다.  

아래와 같이 **chcp 65001**로 변경된 것을 확인할 수 있다.  
![image](https://user-images.githubusercontent.com/37467408/120592906-8ff1af80-c479-11eb-857e-82254bb4e580.PNG)  

이렇게 하는 이유는 발생될 수 있는 Encoding 문제를 방지하기 위해서이다.  

**참고**  
`chcp 437` : 영문  
`chcp 949` : 한글  
`chcp 65001` : UTF-8

## 2. jekyll를 동작시킨다.  

`bundle exec jekyll serve` 명령어를 통해서 내 Local에서 사용할 jekyll를 동작시킨다.  

아래와 같이 동작하면 정상 작동한 것이다.  
![image](https://user-images.githubusercontent.com/37467408/120595011-b8c77400-c47c-11eb-8229-203167287326.PNG)  

저 같은 경우에는 처음에 `bundle exec jekyll serve` 를 동작시켰을때 문제가 발생했다.  

cmd 창에서 설치가 되지 않았다. 라는 문제가 발생하여 cmd 창에서 전부 일일히 설치해주는 작업을 진행했었다.  

---
🐢**  아직 많이 부족해서 다른 블로그를 많이 참조해서 글을 쓰고 있습니다!<br>잘못된 부분이 있으면 댓글이나 메일로 지적해주시면 감사하겠습니다!  **🐢
{: .notice--primary}   

---
**참고 블로그**  
[해보리님의 블로그](https://blog.naver.com/cra2yboy)  

감사합니다.😊
