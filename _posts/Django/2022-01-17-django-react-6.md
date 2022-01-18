---
title: "[Django] 장고 Models를 활용한 데이터베이스 처리 - 4"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-17
last_modified_at: 2022-01-18
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

### Mirgrations  

모델의 변경내역을 "데이터베이스 스키마"로 반영시키는 효율적인 방법을 제공  

관련 명령  
- 마이그레이션 파일 생성  
```commandline
>> python manage.py makemigrations <앱이름>
```  

- 지정 마이그레이션의 SQL 내역 출력  
```commandline
>> python manage.py sqlmigrate <앱이름> <마이그레이션-이름>
```  

- 지정 데이터베이스에 마이그레이션 적용  
```commandline
>> python manage.py migrate <앱이름>
```  

- 마이그레이션 적용 현황 출력  
```commandline
python manage.py showmigrations <앱이름>
```  

### Migration 파일  

데이터베이스에 어떤 변화를 가하는 Operation들을 나열  
- 테이블 생성/삭제, 필드 추가/삭제 등  
- 커스텀 파이썬/SQL Operation  
-- 데이터 마이그레이션 등  

대개 모델로부터 자동 생성 -> makemigrations 명령  
- 모델 참조없이 빈 마이그레이션 파일 만들어서 직접 채워넣기도,  

주의) 같은 Migration 파일이라 할지라도, DB종류에 따라 다른 SQL이 생성된다.  
- 모든 데이터베이스 엔진들이 같은 기능을 제공하진 않는다.  
- 예) SQLite DB에서는 기존 테이블에 컬럼 추가가 지원되지 않는다.  

### 마이그레이션 파일 생성 및 적용  

![image](https://user-images.githubusercontent.com/37467408/149957318-4654392a-f9b1-4440-a08c-859a3beffad4.png)  

### 언제 makemigrations를 하는가?  

모델 필드 관련된 어떠한 변경이라도 발생 시에 마이그레이션 파일 생성  
- 실제로 DB Scheme에 가해지는 변화가 없더라도 수행  

마이그레이션 파일은 `모델의 변경내역을 누적`하는 역할  
- 적용된 마이그레이션 파일을 절대 삭제하면 안된다.  
- 마이그레이션 파일이 너무 많을 경우, squashmigrations 명령으로 다수의 마이그레이션 파일을 통합할 수 있다.  

### 마이그레이션 Migrate (정/역 방향)  

python manage.py migrate <앱이름>  
- 미적용 <마이그레이션-파일>부터 <최근-마이그레이션-파일>까지 `정방향으로 순차적으로` 수행  

python manage.py migrate <앱이름> <마이그레이션-이름>  
- 지정된 <마이그레이션-이름>이 현재 적용된 마이그레이션보다  
-- 이후라면, 정방향으로 순차적으로 지정 마이그레이션 forward 수행  
-- 이전이라면, 역방향으로 순차적으로 지정 마이그레이션 backward 수행  

### 마이그레이션 이름 지정  
- 전체 이름(파일명)을 지정하지 않더라도, 1개를 판별할 수 있는 일부만 지정해도 OK  

shop/migrations/0001_initial.py
shop/migrations/0002_create_field.py  
shop/migrations/0002_update_field.py  

```commandline
>> python manage.py migrate blog 000 # FAIL (3개 파일에 매칭)  
>> python manage.py migrate blog 100 # FAIL (매칭되는 파일이 없음)  
>> python manage.py migrate blog 0001 # OK  
>> python manage.py migrate blog 0002 # FAIL (2개 파일에 매칭)  
>> python manage.py migrate blog 0002_c # OK  
>> python manage.py migrate blog 0002_create # OK  
>> python manage.py migrate blog 0002_update # OK  
>> python manage.py migrate blog zero # shop앱의 모든 마이그레이션을 rollback  
```  

### 마이그레이션 순서는 파일명으로 정렬순?  

```python
from django.db import migrations, models

class Migration(migrations.Migration):
    dependencied = [
        ('shop', '0001_initial'),
    ]
    operations = [
        ...
    ]
```  

### id 필드가 생기는 이유  

모든 DB 테이블에는 각 Row의 식별기준인 "기본키(Primary Key)"가 필요  
- 장고에서는 기본키로서 id(AutoField) 필드를 디폴트 생성  
- 다른 필드를 기본키로 지정하고 싶다면 primary_key=True 옵션 적용  

## 마이그레이션을 통한 데이터베이스 스키마 관리 - 2  

### 새로운 필드가 필수필드라면?  

필수필드 여부 : blank/null 옵션이 모두 False 일 때 (디폴트)  

makemigrations 명령을 수행할 때, 기존 Record들에 어떤 값을 채워넣을 지 묻는다.  
- 선택1) 지금 그 값을 입력하겠다.  
- 선택2) 명령 수행을 중단.  

### 협업 Tip  

절대 하지 말아야할 일  
- 팀원 각자가 마이그레이션 파일을 생성 -> 충돌 발생  

추천) 마이그레이션 파일 생성은 1명이 전담해서 생성  
- 생성한 마이그레이션 파일을 `버전관리`에 넣고, 다른 팀원들은 이를 받아서 migrate만 수행  

### Tip  

개발 시에 "서버에 아직 반영하지 않은" 마이그레이션을 다수 생성했다면?  
- 이를 그대로 서버에 Migrate 하지 말고, `하나의 Migration 으로 합쳐서 적용`하기를 권장  
-- 방법1) 서버로의 미적용 마이그레이션들을 모두 롤백하고 -> 롤백된 마이그레이션들을 모두 제거하고 -> 새로이 마이그레이션 파일을 생성  
-- 방법2) 미적용 마이그레이션들을 하나로 합치기 -> squashmigrations 명령  

<br>

---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊