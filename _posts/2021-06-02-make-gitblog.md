---
title: "[Github 블로그] Github 블로그를 만들자."

categories:
  - Gitblog
tags:
  - [Blog, jekyll, Github, Git]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-02
last_modified_at: 2021-06-03
sitemap :
  changefreq : daily
  priority : 1.0
---

# 1. Github에서 새로운 Repository를 생성한다.  

자신의 저장소에서 `New`를 누른 후, Repository의 이름을 자신의 Github 계정이름으로 하여, 계정이름.github.io로 하여 생성한다.  

![image](https://user-images.githubusercontent.com/37467408/120424796-04f4b480-c3a8-11eb-8693-157d1ef4f98a.PNG)  

# 2. 생성한 Repository를 Local 환경으로 Clone 해 온다.  

## cmd를 실행해서 저장하고자 하는 위치로 이동한다.  
ex) `cd C:\dltjrals2`  
만약 D드라이브에 저장을하고 싶을 경우 cmd 창에 `D:`를 입력 후, 엔터를 치면 D드라이브로 이동할 수 있다.  

## git clone 명령어를 실행하여 Repository를 복사해온다.
📌**<u>git이 사전에 설치되어 있어야 합니다.</u>**
{: .notice--primary}  
<u>구조 : git clone + Repository 주소.git</u>  
ex) `git clone https://github.com/dltjrals2/dltjrals2.github.io.git`  

# 3. Ruby 설치하기.  
📌**<u>Window 환경 기준 > 추후, 맥북 Ruby 설치 방법 추가</u>**
{: .notice--primary}  
Jekyll은 Ruby라는 언어로 만들어졌기 때문에 Jekyll을 설치하기 위해서는 Ruby설치가 선행되어야 한다.  
Ruby Installer는 <https://rubyinstaller.org/downloads/> 여기서 WITH DEVIKIT 중 가장 위에 있는 것을 다운받아 실행시킨다.  

👍 Installer을 설치하는 과정에서 직접 환경 변수를 추가하는 번거로움을 줄이기 위해 아래 내용을 체크한다.  
✔ Add Ruby executables to your PATH  

# 4. Jekyll과 Bundle 설치하기.  
> Bundler는 루비 프로젝트에 필요한 gem들의 올바른 버전을 추적하고 설치해서 일관된 환경을 제공하는 도구이다.  
또한, Github에 올리기 전에 Local Server에서 Github에 올라가기 전에 어떠한 모습으로 post가 나올지 확인할 수 있다.

cmd를 실행시킨 후, `gem install jekyll bundler`을 수행한다. 설치가 완료 된 후에 `jekyll -v` 명령어를 수행하여 jekyll이 잘 설치되었는지 확인한다.  

# 5. jekyll 테마 설치 후, 내 Local Repository에 복사한다.

가장 많이 사용되어지는 [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes) 테마를 선택해서 진행했다. 선택 이유는 처음 Blog를 시작하는데 아는 정보도 없고 많은 자료가 필요해서 많은 사람들이 사용한 테마를 선택했다!  

[minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)로 들어가서 해당 자료를 Zip 압축 파일로 다운 받는다.  

![image](https://user-images.githubusercontent.com/37467408/120440574-a71f9700-c3be-11eb-8b62-571878d73996.PNG)  
압축을 풀고, 안에 .github폴더를 제외한 나머지 모든 폴더 및 파일을 **2번 과정**에서 clone했던 **Local Repository**에 전부 복사해준다.  

# 6. Github에 Push하기.  
`git bash`를 실행 후, `cd`명령어를 통해서 Local Repository로 이동한다. 그리고 아래의 3개의 명령어를 실행한다.  

```
git add .  
git commit -a "Commit Message"  
git push origin master  
```  

**git add** : Local Repository의 변경 파일들을 Stage에 올리는 단계이다. `.`은 변경된 모든 파일을 올리겠다는 의미이다.  

**git commit -a "Commit Message"** : Stage에 올라온 모든 파일들을 Commit Message와 함께 올리기 직전까지 확정 짓는다.  

**git push origin master** : 변경 사항들을 Github 서버에 올린다.  

---
**🐢 아직 많이 부족해서 다른 블로그를 많이 참조해서 글을 쓰고 있습니다!<br>잘못된 부분이 있으면 댓글이나 메일로 지적해주시면 감사하겠습니다! 🐢**
{: .notice--primary}   

---
**참고 블로그**  
[식빵맘님의 블로그](https://ansohxxn.github.io)  

감사합니다.😊
