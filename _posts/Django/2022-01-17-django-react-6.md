---
title: "[Django] 장고 Models를 활용한 데이터베이스 처리 - 4"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-17
last_modified_at: 2022-01-17
---

## 관계를 표현하는 모델 필드 (ManyToManyField)  

### ManyToManyField  

M : N 관계에서 어느 쪽이라도 필드 지정 가능  

ManyToManyField(to, blank=False)  

```python
# 방법 1)
class Post(models.Model):
    tag_set = models.ManyToManyField('Tag', blank=True)

class Article(models.Model):
    tag_set = models.ManyToManyField('Tag', blank=True)

class Tag(models.Model):
    name = models.CharField(max_length=100, unique=True)

# 방법 2)
class Post(models.Model):
    # ...

class Article(models.Model):
    # ...

class Tag(models.Model):
    name = models.CharField(max_length=100, unique=True)
    post_set = models.ManyToManyField('Post', blank=True)
    article_set = models.ManyToManyField('Article', blank=True)
```

### RDBMS지만, DB따라 NoSQL기능도 지원  
- ex) 하나의 Post 안에 다수의 댓글 저장 가능  

djkoch/jsonfield  
- 대게의 DB엔진에서 사용 가능  
- TextField/CharField를 래핑  
- dict 등의 타입에 대한 저장을 직렬화하여 문자열로 저장  
-- 내부 필드에 대해 쿼리 불가  

django.contrib.postgres.fields.JSONField  
- 내부적으로 PostgreSQL의 jsonb 타입  
- 내부 필드에 대해 쿼리 지원  

adadchainz/django-mysql  
- MySQL 5.7 이상에서 json 필드 지원  

## 마이그레이션을 통한 데이터베이스 스키마 관리 - 1  



## 마이그레이션을 통한 데이터베이스 스키마 관리 - 2  


---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊