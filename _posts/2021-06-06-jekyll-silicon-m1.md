---
title: "[Github 블로그] Mac OS m1 jekyll 설정"
excerpt: "에디터 : Atom"

categories:
  - Gitblog
tags:
  - [Blog, jekyll, Github, Git, Mac, M1]

toc:  true
toc_sticky: true

date: 2021-06-06
last_modified_at: 2021-06-07
sitemap :
  changefreq : daily
  priority : 1.0
---
전에 사용하던 Window 기반 노트북이 화면도 안들어오고, 비행기 이륙소리 때문에 노트북을 바꾸게 되었다.  

노트북을 알아보던 중에 Python Web 개발에 Mac OS가 굉장히 편하다는 소리를 들어서 바로 **Mac Air**로!  

프로 라인업도 고민을 해봤는데 내 역량에서 그만큼이나 고스펙은 필요하지 않을 것 같았다!  

차근차근 실력을 키워가면서 그때 맥북 프로 라인업으로 사야지😀  

블로그 작성을 위한 환경을 맞추다가 기록을 남기면 좋을 것 같아서 작성하게 되었다!  

## 1. m1 silicon 맥에서 Jekyll 설치  
📌 **<u>Homwbrew가 사전에 설치되어 있어야 합니다.</u>**
{: .notice--primary}  
저는 Terminal을 사용하지 않고, ITrem을 사용하기 때문에 우선적으로 ITrem을 Rosetta를 이용해서 실행시켰다.  

Rosetta를 이용하지 않고는 Jekyll를 실행시킬 수 없어서 위와 같이 진행하였습니다.  

```
brew install rbenv  
rbenv init  
```  

## 2. 환경 변수 등록  
계속 제대로 실행이 install이 되지 않아서 구글링 한 결과! 환경 변수 등록이 안되어 있었다는 것을 알 수 있었습니다.  
그래서, `~/.zshrc`에 환경 변수 등록을 진행했습니다.  

```
PATH="`ruby -e 'puts Gem.user_dir'`/bin:$PATH"
```

참고 사이트 : <https://askubuntu.com/questions/406643/warning-you-dont-have-a-directory-in-your-path-gem-executables-will-not-run>  

## 3. 환경 변수 까지 등록했으면 아래와 같이 입력한다.  
```
rbenv install 2.7.2  
rbenv global 2.7.2
```

```
ruby -v
[universal.x86_64-darwin20]
```  
출력 결과가 위와 같이 x86_64이어야 합니다.  

## 4. Jekyll 설치  
Local Repository로 이동해서 아래와 같이 명령어를 실행합니다.

```
arch -X86_64 gem install --user-install bundler jekyll
arch -X86_64 bundle update
arch -X86_64 bundle install
```  

위 작업이 잘 되었다면 아래 명령어로 실행하면 됩니다.
```
jekyll serve
```  

Window 노트북에서 사용하던 것처럼 똑같이 jekyll를 잘 설치할 수 있었습니다.  

물론, 삽질을 많이 했지만..😅  

---
🐢**<u>  아직 많이 부족해서 다른 블로그를 많이 참조해서 글을 쓰고 있습니다!<br>잘못된 부분이 있으면 댓글이나 메일로 지적해주시면 감사하겠습니다!  </u>**🐢
{: .notice--primary}   

**참고 블로그**  
[찬호님의 블로그](hhttps://chanho-yoon.github.io/silicon%20mac/m1-jekyll/)  

감사합니다.😊
