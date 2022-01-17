---
title: "[Django] 장고 Models를 활용한 데이터베이스 처리 - 3"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-17
last_modified_at: 2022-01-17
---

## QuerySet을 통한 간단 검색 구현  

예제) Item 목록/간단검색 페이지  

```python
# shop/view.py  
from django.shortcuts import render
from .models import Item

# 중략...

def item_list(request):
    qs = Item.objects.all()
    
    q = request.GET.get('q', '')
    if q:
        q = qs.filter(name__icontains=q)
    return render(request, 'shop/item_list.html', {
        'item_list': qs,
        'q': q,
    })

# shop/views.py
# 중략...

urlpatterns = [
    path('', viwes.item_list),
]
```  

```html
<!- shop/templates/shop/item_list.html -->
<form action="" method="GET">
    <input type="text" name="q" value="{{ q }}" />
    <input type="submit" value="검색" />
</form>

{% for item in item_list %}
    <div>
        <h3>{{ item.name }}</h3>
        <h4>가격: {{ item.price }}</h4>
        {% if item.decc %}
            <p>{{ item.desc }}</p>
        {% endif %}
    </div>
{% endfor %}
```  

## QuerySet에 정렬 조건 추가  

### 정렬 조건 추가(SELECT 쿼리에 "ORDER BY" 추가)  

정렬 조건을 추가하지 않으면 일관된 순서를 보장받을 수 없음.  

DB에서 다수 필드에 대한 정렬을 지원  
- 하지만, 가급적 단일 필드로 하는 것이 성능에 이익  
- 시간순/역순 정렬이 필요한 경우, id 필드를 활용해볼 수 있음  

정렬 조건을 지정하는 2가지 방법  
1. (추천) 모델 클래스의 Meta 속성으로 ordering 설정 : list 지정  
2. 모든 queryset에 order_by(...)에 지정  

### 정렬 지정하기 1  

```python
class Item(models.Model):
    name = models.CharField(max_length=100)
    desc = models.TextField(blank=True)
    price = models.PositiveIntegerField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        ordering = ['id']
```  

```commandline
>>> python manage.py shell_plus --print-sql
```

### 슬라이싱을 통한 범위조건 추가(SELECT 쿼리에 "OFFEST/LIMIT" 추가)  

str/list/tuple에서의 슬라이싱과 거의 유사하나, 역순 슬라이싱은 지원하지 않음  
- 데이터베이스에서 지원하지 않기 때문  

객체[start:stop:step]  
- OFFSET -> start  
- LIMIT -> stop - start  
- (주의) step은 쿼리에 대응되지 않는다. 사용을 비추천  

## django-debug-toolbar  

현재 request/responses에 대한 다양한 디버깅 정보를 보여줌  

다양한 Panel 지원  
- SQLPanel을 통해, 각 요청 처리 시에 발생한 SQL 내역 확인 가능  
- Ajax 요청에 대한 지원은 불가  

### django-debug-toolbar 설치  

설치 공식문서  
- <https://django-debug-toolbar.readthedocs.io/en/latest/installation.html>  

주의사항  
- 웹페이지의 템플릿에 필히 "<body>" 태그가 있어야만, django-debug-toolbar가 동작.  
- 이유: dbt의 html/script 디폴트 주입 타겟이 </body> 태그 (INSERT_BEFORE 설정 디폴트: "</body>")  

### 코드를 통한 SQL 내역 확인  

QuerySet의 query 속성 참조  
- ex) print(Post.objects.all().query) -> 실제 문자열 참조 시에 SQL 생성  

settings.DEBUG = True 시에만 쿼리 실행내역을 메모리에 누적  

```python
from django.db connection, connections

for row_dict in connection.queries:
    print('{time} {sql}'.format(**row_dict))

connections['default'].queries
```

쿼리 초기화  
- 메모리에 누적되기에, 프로세스가 재시작되면 초기화  
- django.db.reset_queries() 통해서 수동 초기화도 가능  

### 그 외 : django-querycount  

SQL 실행내역을 개발서버 콘솔 표준출력  

Ajax 내용도 출력 가능  

## 관계를 표현하는 모델 필드(ForeifnKey)  

### 주의 사항  

ORM은 어디까지나, SQL 생성을 도와주는 라이브러리  

ORM이 DB에 대한 모든 것을 처리해주진 않는다.  

보다 성능높은 어플리케이션을 만들고자 한다면, 사용하실 DB엔진과 SQL에 대한 높은 이해가 필요하다.  

### RDBMS에서의 관계 예시(설계하기 나름)  

1 : N 관계 -> models.ForeignKey로 표현  
- 1명의 유저(User)가 쓰는 다수의 포스팅(Post)  
- 1명의 유저(User)가 쓰는 다수의 댓글(Comment)  
- 1개의 포스팅(Post)에 다수의 댓글(Comment)  

1 : 1 관계 -> models.OneToOneField로 표현  
- 1명의 유저(User)는 1개의 프로필(Profile)  

M : N 관계 -> models.ManyToManyField로 표현  
- 1개의 포스팅(Post)에는 다수의 태그(Tag)  
-- 1개의 태그(Tag)에는 다수의 포스팅(Post)  

### ForeignKey  

1 : N 관계에서 N측에 명시  
- ex) Post:Comment, User:Post, User:Comment,  

ForeignKey(to, on_delete)  
- to : 대상 모델  
-- 클래스를 직접 지정하거나,  
-- 클래스명을 문자열로 지정, 자기 참조는 "self" 지정  

- on_delete : Record 삭제 시 Rule  
-- `CASCADE` : ForeignKey로 참조하는 다른 모델의 Record도 삭제  
-- PROTECT : ProtectedError (IntegerityError) 를 발생시키며, 삭제 방지  
-- SET_NULL : null로 대체, 필드에 null=True 옵션 필수.  
-- SET_DEFAULT : 디폴트 값으로 대체. 필드에 디폴트값 지정 필수  
-- SET : 대체할 값이나 함수 지정. 함수의 경우 호출하여 리턴값을 사용  
-- DO_NOTHING : 어떠한 액션 X, DB에 따라 오류가 발생할 수도 있다.  

### 올바른 User 모델 지정  

```python
# django/contrib/auth/models.py
Class User(AbstractBaseUser):
    # ...

# blog/models.py
class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL,
                               on_delete=models.CASCADE)
    title = models.CharField(max_length=100)
```  

```commandline
>>> user.post_set.all()
```  

### FK에서의 reverse_name  
- reverse 수행 접근 시의 속성명: 디폴트 -> "모델명소문자_set"  

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()

class Comment(models.Model):
    post = models.ForeignKey(Post, on_delete=models.CASCADE)
    message = models.TextField()
```  

```commandline
>>> comment.post
>>> post.comment_set.all() <-> comment.objects.filter(post=post)
```

### reverse_name 이름 충돌이 발생한다면?  

reverse_name 디폴트 명은 앱이름 고려 X, 모델명만 고려  

다음의 경우, user.post_set 이름에 대한 충돌  
- blog앱 Post모델, author = FK(User)  
- shop앱 Post모델, author = FK(User)  

이름이 충돌이 날 때, makemigrations 명령이 실패  

이름 충돌 피하기  
- 1. 어느 한 쪽의 FK에 대해, reverse_name을 포기 -> related_name='+'  
- 2. 어느 한 쪽의 (혹은 모두) FK의 reverse_name을 변경  
-- 1. ex) FK(User, ..., related_name="blog_post_set")  
-- 2. ex) FK(User, ..., related_name="shop_post_set")  

### ForeignKey.limit_choices_to 옵션  

Form을 통한 Choice 위젯에서 선택항목 제한 가능  
- dict/Q 객체를 통한 지정 : 일괄 지정  
- dict/Q 객체를 리턴하는 함수 지정 : 매번 다른 조건 지정 가능  

ManyToManyField에서도 지원  

```python
staff_member = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    limit_choices_to={'is_staff': True},
)
```

## 관계를 표현하는 모델(OneToOneField)  

### OneToOneField  

1 : 1 관계에서 어느 쪽이라도 가능  
- User:Profile  

ForeignKey(unique=True)와 유사하지만, reverse 차이  
- User:Profile FK로 지정한다면 -> profile.user_set.first() -> user  
- User:Profile O2O로 지정한다면 -> profile.user -> user  

OneToOneField(to, on_delete)  

```python
# django/contrib/auth/models.py
class User(AbstractBaseUser):
    # ...

# accounts/models.py
class Profile(models.Model):
    author = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```  

### O2O에서의 related_name  
- reverse 접근 시의 속성명 : 디폴트 -> 모델명소문자  

```python
# accounts/models.py
class Profile(models.Model):
    author = models.OneToOneField(settings.AUTH_USER_MODEL,
                                  on_deleted=models.CASCADE)
    phone = models.CharField(max_length=11, blank=True)
    birth = models.DateField(null=True)
```  

```commandline
>>> profile.author
>>> user.profile
```


---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊