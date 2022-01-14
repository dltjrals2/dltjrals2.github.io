---
title: "[Django] 장고 Models를 활용한 데이터베이스 처리 - 2"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-14
last_modified_at: 2022-01-14
---

## 장고가 media 파일을 다루는 방법  

### Static & Media 파일  

Static 파일  
- 개발 리소스로서의 정적인 파일(js, css, image 등)  
- 앱 / 프로젝트 단위로 저장 / 서빙  

Media 파일  
- FileField / ImageField를 통해 저장한 모든 파일  
- DB필드에는 저장경로를 저장하며, 파일은 파일 스토리지에 저장  
-- `실제로 문자열을 저장하는 필드`  
- 프로젝트 단위로 저장/서빙  

1. instagram/models.py 수정  

    ```python
    from django.db import models
    
    class Post(models.Model):
        message = models.TextField()
        photo = models.ImageField(blank=True)
        is_public = models.BooleanField(default=False, verbose_name='공개여부')
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    
        # Java의 toString
        def __str__(self):
            return self.message
    ```  

2. pillow 라이브러리 설치  

    ```commandline
    pip install pillow
    ```  

3. 라이브러리 requirements.txt 파일 만들기  

    ```text
    diango~=3.0.0
    pillow
    ```  
   
    추후, 패키지를 install 할 경우, 버전 관리에 용이  

    설치 방법  
    ```commandline
    pip install -r requirements.txt
    ```  

4. migration 진행  

    ```commandline
    python manage.py makemigrations instagram
    python manage.py migrate instagram
    ```  

### Media 파일 처리 순서  

1. HttpRequest.FILES를 통해 파일이 전달  
2. 뷰 로직이나 폼 로직을 통해, 유효성 검증을 수행하고,  
3. FileField/ImageField 필드에 "경로(문자열)"를 저장하고,  
4. settings.MEDIA_ROOT 경로에 파일을 저장합니다.  

### 기본 settings  

1. PROJECT_NAME/settings.py 수정  

    ```python
    STATIC_URL = '/static/'
    # STATIC_ROOT = '' # TODO
    
    
    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```  
   
### FileFiend와 ImageField  

FileField  
- File Storage API를 통해 파일을 저장  
-- 장고에서는 File System Storage만 지원. django-storages를 통해 확장 지원.  
- 해당 필드를 옵션 필드를 두고자 할 경우, blank=True 옵션 적용  

ImageField(FileField 상속)  
- Pillow(이미지 처리 라이브러리)를 통해 이미지 width/height 획득  
-- Pillow 미설치 시에, ImageField를 추가한 makemigration 수행에 `실패`한다.  

위 필드를 상속받은 커스텀 필드를 만들 수도 있다.  
- ex) PDFField, ExcelField 등  

### 사용할 만한 필드 옵션  

blank 옵션  
- 업로드 옵션처리 여부  
- 디폴트 : False  

upload_to 옵션  
- settings.MEDIA_ROOT 하위에서 저장한 파일명/경로명 결정  
- 디폴트 : 파일명 그대로 settings.MEDIA_ROOT에 저장  
-- `추천) 성능을 위해, 한 디렉토리에 너무 많은 파일들이 저장되지 않도록 조정하기`  
- 동일 파일명으로 저장 시에, 파일명에 더미 문자열을 붙여 파일 덮어쓰기 방지  

1. PROJECT_NAME/urls.py 수정  

    ```python
    from django.conf.urls.static import static
    from django.contrib import admin
    from django.urls import path, include
    
    # from django.conf import global_settings
    # from askcompany import settings
    from django.conf import settings
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('blog1/', include('blog1.urls')),
        path('instagram/', include('instagram.urls')),
    ]
    
    if settings.DEBUG:
        urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```  

2. instagram/admin.py 수정  

    ```python
    from django.contrib import admin
    from django.utils.safestring import mark_safe
    from .models import Post
    
    @admin.register(Post) # wrapping
    class PostAdmin(admin.ModelAdmin):
        list_display = ['id', 'message', 'message_length', 'is_public', 'created_at', 'updated_at']
        list_display_links = ['message']
        list_filter = ['created_at', 'is_public']
        search_fields = ['message']
    
        def photo_tag(self, post):
            if post.photo:
                return mark_safe(f'<img src="{post.photo.url}" style="width: 72px;" />')
            return None
    
        def message_length(self, post):
            return f"{len(post.message)} 글자"
    ```  

3. instagram/urls.py 수정  

    ```python
    from django.db import models
    
    class Post(models.Model):
        message = models.TextField()
        photo = models.ImageField(blank=True, upload_to='instagram/post/%Y/%m/%d')
        is_public = models.BooleanField(default=False, verbose_name='공개여부')
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    
        # Java의 toString
        def __str__(self):
            return self.message
    ```  
   
### 파일 업로드 시에 HTML Form enctype  

form method는 필히 POST로 지정  
- GET의 경우 enctype이 "application/x-www-form-urlencoded"로 고정  

form enctype을 필히 "multipart/form-data"로 지정  
- "application/x-www-form-urlencoded"의 경우, 파일명만 전송  

### upload_to 인자  

`파일 저장 시에` upload_to 함수를 호출하여, 저장 경로를 계산  
- 파일 저장 시에 upload_to 인자를 변경한다고 해서, DB에 저장된 경로값이 갱신되진 않는다.  

인자 유형  
- 문자열로 지정  
-- 파일을 저장할 "중간 디렉토리 경로"로서 활용  
- 함수로 지정  
-- "중간 디렉토리 경로" 및 "파일명"까지 결정 가능

### 파일 저장 경로 / 커스텀 (upload_to 옵션)  

한 디렉토리에 파일을 너무 많이 몰아둘 경우, OS 파일찾기 성능 저하. 디렉토리 Depth가 깊어지는 것은 성능에 큰 영향 없음.  

필드 별로, 다른 디렉토리 저장경로를 가지기  

- 대책 1) 필드 별로 다른 디렉토리에 저장  
-- photo = models.ImageField(upload_to="blog")
-- photo = models.ImageField(upload_to="blog/photo")

- 대책 2) 업로드 시간대 별로 다른 디렉토리에 저장  
-- upload_to에서 strftime 포맷팅을 지원  
-- photo = models.ImageField(upload_to="blog/%Y/%m/%d")  

예시) uuid를 통한 파일명 정하기  

```python
import os
from uuid import uuid4
from django.utils import timezone

def uuid_name_upload_to(instance, filename):
    app_label = instance.__class__._meta.app_label  # 앱 별로
    cls_name = instance.__class__.__name__.lower()  # 모델 별로
    ymd_path = timezone.now().strftime('%Y/%m/%d')  # 업로드하는 년/월/일 별로
    uuid_name = uuid4().hex
    extension = os.path.splitext(filename)[-1].lower() # 확장자 추출하고, 소문자로 변환
    return '/'.join([
        app_label,
        cls_name,
        ymd_path,
        uuid_name[:2],
        uuid_name + extension,
    ])
``` 

### 템플릿에서 media URL 처리 예시  

필드의 .url 속성을 활용한다.  
- 내부적으로 settings.MEDIA_URL과 조합을 처리  
```
{% raw %}<img src="{{ post.photo.url }}" %}" />{% endraw %}
```

- 필드에 저장된 경로에 없을 경우, .url 계산에 실패함에 유의. 그러니 안전하게 필드명 저장유무를 체크  
```
{% raw %}{% if post.photo %}{% endraw %}  
    {% raw %}<img src="{{ post.phoro.url }}" %}">{% endraw %}  
{% raw %}{% endif %}{% endraw %}  
```  

참고  
- 파일 시스템 상의 절대경로가 필요하다면, .path 속성을 활용하세요.  
-- settings.MEDIA_ROOT와 조합  

### 개발환경에서의 media 파일 서빙  

static 파일과 다르게, 장고 개발서버에서 서빙 미지원  

개발 편의성 목적으로 직접 서빙 Rule 추가 가능  

```python
from django.conf import settings
from django.conf.urls.static import static

# 중략

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## 장고 쉘  

### 모델을 통한 조회(기초)  

데이터베이스 질의 인터페이스를 제공  

디폴트 Manager로서 ModelCls.objects가 제공

```python3  
# 생성되는 대강의 SQL 윤곽 -> SELECT * FROM app_model
ModelCls.objects.all()

# 생성되는 대강의 SQL 윤곽 -> SELECT * FROM app_model ORDER BY id DESC LIMIT 10
ModelsCls.object.all().order_by('-id')[:10]

# 생성되는 대강의 SQL 윤곽 -> INSERT INTO app_model (title) VALUES ("New Title")
ModelCls.objects.create(title="New Title")
```

### QuerySet  

SQL을 생성해주는 인터페이스  

순회가능한 객체  

Model Manager를 통해, 해당 Model에 대한 QuerySet을 획득  
- Post.objects.all() 코드는 "SELECT * FROM post ..."  
- Post.objects.create(...) 코드는 "INSERT INTO..."  

### QuerySet은 Chaining을 지원  

Post.objects.all().filter(...).exclude(...).filter(...) -> QuerySet  

QuerySet은 Lazy한 특성  
- QuerySet을 만드는 동안에는 DB접근을 하지 않는다.  
- 실제로 데이터가 필요한 시점에 접근을 한다.  

데이터가 필요한 시점은 언제인가?  
1. queryset  
2. print(queryset)  
3. list(query)  
4. for instance in queryset: print(instance)  

### 다양한 조회요청 방법(SELECT SQL 생성)  

조건을 추가한 Queryset, 획득할 준비  
- queryset.filter(...) -> queryset  
- queryset.exclude(...) -> queryset  

특정 모델객체 1개 획득을 시도  
- queryset[숫자인덱스]  
-- 모델객체 혹은 예외발생(IndexError)  
- queryset.get(...)  
-- 모델객체 혹은 예외발생(DoesNotExist, MultipleObjectsReturned)  
queryset.first() -> 모델객체 혹은 None  
querset.last() -> 모델객체 혹은 None  

### filter <-> exclude(SELECT 쿼리에 WHERE 조건 추가)  

인자로 "필드명 = 조건값" 지정  

1개 이상 인자 지정 -> 모두 AND 조건으로 묶임  

OR 조건을 묶을려면, django.db.models.Q 활용  

### 필드 타입별 다양한 조건 매칭(데이터베이스에 따라 생성되는 SQL이 다르다.)  

숫자/날짜/시간 필드  
- 필드명__lt = 조건값 -> 필드명 < 조건값  
- 필드명__lte = 조건값 -> 필드명 <= 조건값  
- 필드명__gt = 조건값 -> 필드명 > 조건값  
- 필드명__gte = 조건값 -> 필드명 >= 조건값  

문자열 필드  
- 필드명__startswith = 조건값 -> 필드명 LIKE "조건값%"  
- 필드명__istartswith = 조건값 -> 필드명 ILIKE "조건값%"  
- 필드명__endswith = 조건값 -> 필드명 LIKE "%조건값"  
- 필드명__iendswith = 조건값 -> 필드명 ILIKE "%조건값"  
- 필드명__contains = 조건값 -> 필드명 LIKE "%조건값%"  
- 필드명__icontains = 조건값 -> 필드명 ILIKE "%조건값%"  

예제)  
```python
# shop/views.py

from django.shortcuts import render
from .models import Item

# 중략 ...
def item_list(request):
    qs = Item.objects.all()
    q = request.GET.get('q', '')
    if q:
        qs = qs.filter(name__icontains=q)
    return render(request, 'shop/item_list.html', {
        'item_list': qs,
        'q': q,
    })

# shop/views.py
# 중략...
urlpatterns = [
    path('', views.item_list)
]
```  

```html
<-- shop/templates/shop/item_list.html -->
<form action="" method="GET">
    <input type="text" name="q" value="{{ q }}"/>
    <input type="submit" value="검색"/>
</form>

{% for item in item_list %}
    <div>
      <h3>{{ item.name }}</h3>
      <h4>가격: {{ item.price }}</h4>
      {% if item.desc %}
        <p>{{ item.desc }}</p>
      {% endif %}
    </div>
{% endfor %}
```

---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊