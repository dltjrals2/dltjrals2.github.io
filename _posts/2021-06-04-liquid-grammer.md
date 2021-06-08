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
<<<<<<< HEAD
last_modified_at: 2021-06-04
sitemap :
  changefreq : daily
  priority : 1.0
=======
last_modified_at: 2021-06-08
>>>>>>> 5a4413830d16bacda7b8e80d48ba28283717f613
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
> {% raw %}{{-, -}}, {{%-, and -%}}{% endraw %}  

Liquid에서는 하이픈을 태그에 포함시켜서 왼쪽 또는 오른쪽에서 공백을 제거할 수 있다.

Input :  
`{% raw %}{% assign my_variable = "tomato" %}{% endraw %}`  
`{% raw %}{{ my_variable }}{% endraw %}`

Output :  

`tomato`  

Input :  
`{% raw %}{% assign my_variable = "tomato" -%}{% endraw %}`
`{{ my_variable }}`  

Output :  
`tomato`  

하이픈을 assign 태그 끝에 추가함으로써, 공백이 제거됨을 확인할 수 있다.  

## 2. Tags  
### 1. Iteration  
> for  

반복문이라고 생각하면 된다.  

Input :  
`{% raw %}{% for product in collection.products %}{% endraw %}`  
`{% raw %}{{ product_title }}{% endraw %}`  
`{% raw %}{% endfor %}{% endraw %}`  

Output :  
`hat shirt pants`  

> else  

Input :  
`{% raw %}{% for product in collection.products %}{% endraw %}`  
`{% raw %}{ product.title }{% endraw %}`  
`{% raw %}{% else %}{% endraw %}`  
`The collection is empty`  
`{% raw %}{% endfor %}{% endraw %}`  

Output :  
`The collection is empty`  

이외에도, `break`, `continue`, `limit`, `offset`, `range` 등등으로 반복횟수와 범위를 제어할 수 있다.  

> cycle  

Input :  
`{% raw %}{% cycle "first": "one", "two", "three" %}{% endraw %}`
`{% raw %}{% cycle "second": "one", "two", "three" %}{% endraw %}`  
`{% raw %}{% cycle "second": "one", "two", "three" %}{% endraw %}`  
`{% raw %}{% cycle "first": "one", "two", "three" %}{% endraw %}`  

Output :  
{% cycle "first": "one", "two", "three" %}  
{% cycle "second": "one", "two", "three" %}  
{% cycle "second": "one", "two", "three" %}  
{% cycle "first": "one", "two", "three" %}  

> tablerow  

HTML 테이블을 생성한다.  

### 2. Template  
> comment  

주석  
`{% raw %}{% comment %} Content {% endcomment %}{% endraw %}`  

> raw  

Liquid 코드 그대로 보여준다.  
`{% raw %}{% raw %}{% endraw %}`  

### 3. Variable  
> assign  

변수를 새로 만들고 할당함  

Input :  
`{% raw %}{% assign foo = "bar" %}{% endraw %}`
`{% raw %}{{ foo }}{% endraw %}`  

Output :  
{% assign foo = "bar" %}  
{{ foo }}  

> capture  

변수에 string을 저장한다.  

Input :  
`{% raw %}{% capture my_variable %}I am being captured.{% endcapture %}{% endraw %}`  

Output :  
`I am being captured.`  

> increment, decrement  

변수의 값을 증가시키고 감소시킨다.  

## 3. Filters  
Filter의 양은 많아서 직접 사이트에서 보기를 추천드립니다.  

---
🐢**<u>  아직 많이 부족해서 다른 블로그를 많이 참조해서 글을 쓰고 있습니다!<br>잘못된 부분이 있으면 댓글이나 메일로 지적해주시면 감사하겠습니다!  </u>**🐢
{: .notice--primary}   

---
**참고 블로그**  
[Liquid 공식문서](http://shopify.github.io/liquid/)  

감사합니다.😊
