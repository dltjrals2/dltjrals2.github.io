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

### DayArchiveView  
- 지정 year/month/day 일의 목록  

필요한 URL 인자  
- "year", "month", "day"  

옵션  
- month_format(디폴트: "%b")  
-- 숫자 포맷은 "%m"  

디폴트 template_name_suffix: "_archive_day.html"  

Context  
- day, previous_day, next_day  
- date_list: 전체 Record의 날짜 목록  
- object_list  

```python
urlpatterns = [
    re_path(r'^archive/(?P<year>\d{4}/'
            r'(?P<month>\n{1,2}/(?P<day>\d{1,2})/$', ...),
]

from django.views.generic.dates import DayArchiveView
from .models import Post

class PostDayArchiveView(DayArchiveView):
    model = Post
    date_field = 'created_at'
    month_format = '%m'
```  

### TodayArchiveView  
- 오늘 날짜의 목록  

필요한 URL 인자 : 없음  

DayArchiveView와 유사하게 동작하지만,  
- year/month/day 인자를 받지 않는다.  
- previous_day, next_day 미제공  

```python
from django.views.generic.dates import TodayArchiveView
from .models import Post

post_today_archive = TodayArchiveView.as_view(model=Post, date_field='created_at')
```

## 적절한 HTTP 상태코드로 응답하기  

### HTTP 상태코드  

웹서버는 적절한 상태코드로 응답해야 한다.  

각 HttpResponse 클래스마다 고유한 status_code가 할당  

REST API르 만들 때, 특히 유용  

```python
# django/http/response.py
class HttpResponseRedirect(HttpResponseRedirectBase):
    status_code = 302
    
from django.http import HttpResponse

def test_view(request):
    # Return a "created" (201) response code.
    return HttpResponse(status=201)
```  

### 대표적인 상태 코드  

200번대 : 성공  
- 200 : 서버가 요청을 잘 처리했다 -> OK  
- 201 : 작성됨. 서버가 요청을 접수하고, 새 리소스를 작성했다.  

300번대 : 요청을 마치기 위해, 추가 조치가 필요하다.  
- 301 : 영구 이동, 요청한 페이지가 새 위치로 영구적으로 이동했다.  
- 302 : 임시 이동, 페이지가 현재 다른 위치에서 요청에 응답하고 있지만, 요청자는 향후 원래 위치를 계속 사용해야 한다.  

400번대 : 클라이언트측 오류  
- 400 : 잘못된 요청  
- 401 : 권한없음  
- 403(Forbidden) : 필요한 권한을 가지고 있지 않아서, 요청을 거부  
- 404 : 서버에서 요청한 리소스를 찾을 수 없다.  
- 405 : 허용되지 않는 방법. POST 방식만을 지원하는 뷰에 GET요청을 할 경우  

500번대 : 서버측 오류  
- 500 : 서버 내부 오류 발생  

### 200 응답하는 몇 가지 예  

```python
from django.http import HttpResponse, JsonResponse
from django.shortcuts import render

def view1(request):
    return HttpResponse('Hello World')

def view2(request):
    return render(request, 'template.html')

def view3(request):
    return JsonResponse({'hello': 'World'})
```  

### 302 응답하는 몇 가지 예  

```python
from django.http import HttpResponseRedirect
from django.shortcuts import redirect, resolve_url

def view1(request):
    return HttpResponseRedirect('/shop/')

def view2(request):
    url = resolve_url('shop:item_list') # URL Reverse 적용
    return HttpResponseRedirect(url)

def view3(request):
    # 내부적으로 resolve_url 사용
    # 인자로 지정된 문자열이 url reverse에 실패할 경우,
    # 그 문자열을 그대로 URL로 사용하여, redirect 시도
    return redirect('shop:item_list')
```  

### 404 응답하는 몇가지 예  

```python
from django.http import Http04, HttpResponseNotFound
from django.shortcuts import get_object_or_404
from shop.models import Item

def view1(request):
    try:
        item = Item.objects.get(pk=100)
    except Item.DoesNotExist:
        raise Http04

def view2(request):
    item = get_object_or_404(Item, pk=100) # 내부에서 raise Http04
    # 생략

def view3(request):
    try:
        item = Item.objects.get(pk=100)
    except Item.DoesNotExist:
        return HttpResponseNotFound()   # 잘 쓰지 않는 방법
    # 생략
```  

### 500 응답하는 몇 가지 예  

뷰에서 요청 처리 중에, 뷰에서 `미처 잡지못한 오류`가 발생했을 경우  
- IndexError, KeyError, django.db.models.ObjectDoesNotExist 등  

```python
from shop.models import Item

def  view1(request):
    # IndexError
    name = ['Tom', 'Steve'][100]

    # 지정 조건의 Item 레코드가 없을 때, Item.DoesNotExist 예외
    # 지정 조건의 Item 레코드가 2개 이상 있을 때, Item.MultipleObjectReturned 예외
    item = Item.objects.get(pk=100)
```  

### 다양한 HttpResponse 서브 클래스  
- 지정 상태코드의 응답이 필요할 때  

HttpResponseRedirect : 상태코드 302  

HttpResponsePermanentRedirect : 상태코드 301 (영구 이동)  

HttpResponseNotModified : 상태코드 304  

HttpResponseBadRequest : 상태코드 400  

HttpResponseNotFound : 상태코드 404  

HttpResponseForbidden : 상태코드 403  

HttpResponseNotAllowed : 상태코드 405  

HttpResponseGone : 상태코드 410  

HttpResponseServerError : 상태코드 500

## URL Reverse를 통해 유연하게 URL 문자열 및 응답 생성하기  

### URL Dispathcer  
- urls.py 변경만으로 "각 뷰에 대한 URL"이 변경되는 유연한 URL 시스템  

```python
# "/blog/", "/blog1/1/" 주소로 서비스하다가
urlpatterns = [
    path('blog/', blog_views.post_list, name='post_list'),
    path('blog/<int:pk>/', blog_views.post_list, name='post_detail'),
]

# 다음과 같이 변경을 하면,
# 이제 "/weblog/", "/weblog/1/" 주소로 서비스하게 된다.
urlpatterns = [
    path('weblog/', blog_views.post_list, name='post_list')
    path('weblog/<int:pk>/', blog_views.post_detail, name='post_detail')
]
```  

### URL Reverse의 혜택  

개발자가 일일이 URL을 계산하지 않고, URL이 변경되더라도 URL Reverse가 변경된 URL을 반영  

### URL Reverse를 수행하는 4가지 함수 - 1  

url 템플릿태그  
- 내부적으로 reverse 함수를 사용  

reverse 함수  
- 매칭 URL이 없으면 NoReverseMatch 예외 발생  

resolve_url 함수  
- 매칭 URL이 없으면 "인자 문자열"을 그대로 리턴  
- 내부적으로 reverse 함수를 사용  

redirect 함수  
- 매칭 URL이 없으면 "인자 문자열"을 그대로 URL로 사용  
- 내부적으로 resolve_url 함수를 사용  


### URL Reverse를 수행하는 4가지 함수 - 2  

```python
# 문자열 URL
{% raw %}
{% url "blog:post_detail" 100 %}
{% url "blog:post_detail" pk=100 %}
{% endraw %}

# 문자열 URL
reverse('blog:post_detail', args=[100])
reverse('blog:post_detail', kwargs={'pk': 100})

# 문자열 URL
resolve_url('blog:post_detail', 100)
resolve_url('blog:post_detail', pk=100)
resolve_url('/blog/100/')

# HttpResponse 응답 (301 or 302)
redirect('blog:post_detail', 100)
redirect('blog:post_detail', pk=100)
redirect('/blog/100/')
```  

### 모델 객체에 대한 detail 주소 계산  

매번 다음과 같은 코드로 할 수도 있지만  

```python
resolve_url('blog:post_detail', pk=post.pk)
redirect('blog:post_detail', pk=post.pk)
{% raw %}
{% url 'blog:post_detail' post.pk %}
{% endraw %}
```

다음과 같이 사용할 수도 있다.  
```python
resolve_url(post)
redirect(post)
{{ post.get_absolute_url }}
```  

### 모델 클래스에 get_absolute_url() 구현  

resolve_url 함수는 가장 먼저 get_absolute_url() 함수의 존재여부를 체크하고, 존재할 경우 reverse를 수행하지 않고 그 리턴값을 즉시 리턴  

```python
# django/shortcuts.py

def resolve_url(to, *args, **kwargs):
    if hasattr(to, 'get_absolute_url'):
        return to.get_absolute_url()
    # 중략
    try:
        return reverse(to, args=args, kwargs=kwargs)
    except NoReverseMatch:
        # 생략
```  

### resolve_url/redirect를 위한 모델 클래스 추가 구현  

```python
from django.urls import reverse

class Post(models.Model):
    # 중략
    def get_absolute_url(self):
        return reverse('blog:post_detail', args=[self.pk])
```  

### 그 외 활용  

CreateView / UpdateView  
- success_url을 제공하지 않을 경우, 해당 model 객체의 get_absolute_url 주소로 이동이 가능한지 체크하고, 이동이 가능할 경우 이동  
- 생성/수정하고나서 Detail 화면으로 이동하는 것은 자연스러운 시나리오  

특정 모델에 대한 Detail 뷰를 작성할 경우  
- Detail 뷰에 대한 URLConf설정을 하자마자, 필히 get_absolute_url설정을 하자. 코드가 보다 간결해진다.  


<br>
---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊