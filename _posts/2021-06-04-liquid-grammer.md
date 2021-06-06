---
title: "[Github 블로그] Liquid 문법 정리"
excerpt: "에디터 : Atom"

categories:
  - Gitblog
tags:
  - [Blog, jekyll, Github, Git, Liquid]

toc:  true
toc_sticky: true

date: 2021-06-04
last_modified_at: 2021-06-04
---

Github Blog를 만들면서 무작정 다른 사람이 한 것을 따라하는 것보다는 왜 바꾼지를 알아가면서 추후에, 내가 더 변경하거나 추가할 사항이 있을 경우 스스로 수정이 필요하니까 <span style="color:blue">**Liquid문법**</span>을 먼저 익히기로 했다!  
[리퀴드 공식문서](http://shopify.github.io/liquid/)  

## 1. Basics  
### 1. Introduction
#### Object  
`{``{` `}``}`로 감싸 안에 contents를 보여주는 형식이다.  

Input : `{% raw %}{{ page.title }}{% endraw %}`  
Output : `{{ page.title }}`
#### Tags  
> if : 조건문  

Input : `{% raw %}{% if 조건 %} [내용] {% endif %}{% endraw %}`  

> unless : 조건문의 반대  

Input : `{% raw %}{% unless 조건 %} [내용] {% endif %}{% endraw %}`  

> elsif / else  

Input : `{% raw %}{% if 조건 %} [내용] {% elsif 조건 %} [내용] {% else %} [내용] {% endfor %}{% endraw %}`  

> case / when  

Input : `{% raw %}{% case 변수 %} {% when 값1 %} [내용1] {% when 값2 %} [내용2] {% else %} [내용3] {% endcase %}{% endraw %}`  

변수 값1이면 내용1, 변수 값2이면 내용2, 변수 값3이면 내용3  

#### Filters  
> append  

Input : `{% raw %}{{ "/my/fancy/url" | append: ".html" }}{% endraw %}`  
Output : `/my/fancy/url.html`  

> prepend  

Input : `{% raw %}{{ "adam!" | capitalize | prepend: "Hello "}}{% endraw %}`  
Output : Hello Adam!  

### 2. Operators  
> contains  

`{% raw %}{% if product.title contains "Pack" %} [내용] {% endif %}{% endraw %}`  

여기서는, {% raw %}product.title{% endraw %} 내용에 Pack이 있으면 [내용] 실행  

### 3. Whitespace control  
> To be Continue
