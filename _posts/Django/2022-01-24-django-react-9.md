---
title: "[Django] 장고 Views를 활용한 HTTP 요청 처리 - 3"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-24
last_modified_at: 2022-01-24
---

## 뷰 장식자  

### 장식자(Decorators)  

어떤 함수를 감싸는 (Wrapping 함수)  

```python
from django.contrib.auth.decorators import login_required
from django.shortcuts import render

@login_required
def protected_view1(request):
    return render(request, 'myapp/secret.html')

def protected_view2(request):
    return render(request, 'myapp/secret.html')

protected_view2 = login_required(protected_view2)
```  

### 몇 가지 장고 기본 Decorators  

django.views.decorators.http  
- require_http_methods, require_GET, require_POST, require_safe  
-- 지정 method가 아닐 경우, HttpResponseNotAllowed 응답 (상태코드 405) 반환  

django.contrib.auth.decorators  
- user_passes_test : 지정 함수가 False를 반환하면 login_url로 redirect  
- login_required : 로그아웃 상황에서 login_url로 redirect  
- permission_required : 지정 퍼미션이 없을 때 login_url로 redirect  

django.contrib.admin.views.decorators  
- staff_member_required : staff member가 아닐 경우 login_url로 이동  

<https://docs.djangoproject.com/en/3.0/topics/http/decorators/>  

### CBV에 장식자 입히기 - 1  
- 가독성이 좋지 않다.  

요청을 처리하는 함수를 Wrapping 하기  

```python
from django.contrib.auth.decorators import login_required
from django.views.generic import TemplateView

class SecretView(TemplateView):
    template_name = 'myapp/secret.html'

view_fn = SecretView.as_view()

secret_view = login_required(view_fn) # 이미 생성된 함수에 장식자 입히기
```  

### CBV에 장식자 입히기 - 2  
- dispatch 재정의  

```python
from django.contrib.auth.decorators import login_required
from django.utils.decorators import method_decorator
from django.views.generic import TemplateView

class SecretView(TemplateView):
    template_name = 'myapp/secret.html'
    
    # 클래스 멤버함수에는 method_decorator를 활용
    @method_decorator(login_required)
    def dispatch(self, *args, **kwargs):
        return super().dispatch(*args, **kwargs)

secret_view = SecretView.as_view()
```  

### CBV에 장식자 입히기 - 3  

```python
from django.contrib.auth.decorators import login_required
from django.utils.decorators import method_decorator
from django.views.generic import TemplateView

# 클래스에 직접 적용
@ method_decorator(login_required, name='dispatch')
class SecretView(TemplateView):
    template_name = 'myapp/secret.html'

secret_view = SecretView.as_view()
```

## 장고 기본 CBV API (Generic data views)  

### Built-in CBV API  

Base views  
- View, TemplateView, Redirect View  

Generic display views  
- DetailView, ListView  

`Generic date views`  
- ArchiveIndexView, YearArchiveView, MonthArchiveView, WeekArchiveView, DayArchiveView, TodayArchiveView, DateDetailView  

Generic editing views  
- FormView, CreateView, UpdateView, DeleteView  

<https://docs.djangoproject.com/en/3.0/ref/class-based-views>  

### Generic Date Views  

ArchiveIndexView : 지정 날짜필드 역순으로 정렬된 목록  

YearArchiveView : 지정된 year년도의 목록  

MonthArchiveView : 지정 year/month 월의 목록  

WeekArchiveView : 지정 year/week 주의 목록  

DayArchiveView : 지정 year/month/day 일의 목록  

TodayArchiveView : 오늘 날짜의 목록  

DateDetailView : 지정 year/month/day 목록 중에서 특정 pk의 detail  
- DetailView와 비교 : URL에 year/month/day를 쓰고자 할 경우에 유용  

<https://docs.djangoproject.com/en/3.0/ref/class-based-views/generic-date-based/>  

### 공통 옵션  

allow_future (디폴트 : False)  
- False : 현재시간 이후의 Record는 제외  

### ArchiveIndexView  
- 지정 날짜필드 역순으로 정렬된 목록 -> 최신 목록을 보고자 할 때  

필요한 URL 인자 : 없음  

옵션  
- model  
- date_field : 정렬 기준 필드  
- date_list_period (디폴트: "year")  

디폴트 template_name_suffix: "_archive.html"  

Context  
- latest: QuerySet  
- date_list : 등록된 Record의 년도 목록  

```python
from django.views.generic import ArchiveIndexView
from .models import Post

post_Archive = ArchiveIndexView.as_view(model=Post, date_field='created_at')
```  

### YearArchiveView  
- 지정 year년도의 목록  

필요한 URL 인자 : "year"  

옵션  
- model, date_field  
- date_list_period(디폴트: "month")  
-- 지정 년도에서 month 단위로 Record가 있는 날짜 리스트  
- make_object_list(디폴트: "False")  
-- 거짓일 경우, onject_list를 비움  

디폴트 template_name_suffix: "_archive_year.html"  

Context  
- year, previous_year, next_year  
- date_list: 전체 Record의 월 목록  
- object_list  

```python
urlpatterns = [
    re_path(r'^archive/(?P<year>\d{4}/$', ...),
]
  
from django.views.generic.dates import YearArchiveView
from .models import Post

class PostYearArchiveView(YearArchiveView):
    model = Post
    date_field = 'created_at'
    # make_object_list = False
```  

### MonthArchiveView  
- 지정 year/month 월의 목록  

필요한 URL 인자  
- "year", "month"  

옵션  
- month_format(디폴트: "%b")  
-- 숫자 포맷은 "%m"  

디폴트 template_name_suffix: "_archive_month.html"  

Context  
- month, previous_month, next_month  
- date_list: 전체 Record의 날짜 목록  
- object_list  

```python
urlpatterns = [
    re_path(r'^archive/(?P<year>\d{4})/(?P<month>\d{1,2})/$', ...),
]

from django.views.generic.dates import MonthArchiveView
from .models import Post

class PostMonthArchiveView(MonthArchiveView):
    model = Post
    date_field = 'created_at'
    month_format = '%m'
```  

### WeekArchiveView  
- 지정 year/week 주의 목록  

필요한 URL 인자  
- "year", "week"  

옵션  
- week_format  
-- "%U" (디폴트) : 한 주의 시작을 일요일로 지정  
-- "%W" : 한 주의 시작일을 월요일로 지정  

디폴트 template_name_suffix: "_archive_week.html"  

Context  
- week, previous_week, next_week  
- date_list: 전체 Record의 날짜 목록  
- object_list  

```python
urlpatterns = [
    re_path(r'^archive/(?P<year>\d{4})/week/(?P<week>\d{1,2})/$', ...),
]

from django.views.generic.dates import WeekArchiveView
from .models import Post

class PostMonthArchiveView(WeekArchiveView):
    model = Post
    date_field = 'created_at'
    week_format = '%m'
```

## 적절한 HTTP 상태코드로 응답하기  

## URL Reverse를 통해 유연하게 URL 문자열 및 응답 생성하기  



<br>
---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊