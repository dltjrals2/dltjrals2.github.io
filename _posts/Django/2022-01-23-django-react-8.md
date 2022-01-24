---
title: "[Django] 장고 Views를 활용한 HTTP 요청 처리 - 2"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-23
last_modified_at: 2022-01-23
---

## 클래스 기반 뷰 시작하기  

### View  
- 호출가능한 객체(Callable Object)  

함수 기반 뷰(Function Based View)  
- View 구현의 기본 -> FBV로 구현할 줄 알아야 한다.  
- 공통 기능들은 장식자 문법으로 적용  

```python
@api_view(['GET'])
@throttle_classed([OncePerDayUserThrottle])
def my_view(requset):
    return Response({"message": "Hello for today!"})
```  

클래스 기반 뷰(Class Based View)  
- 공통 기능들은 상속 문법으로 적용  

```python
class MyView(APIView):
    throttle_classed = [OncePerDayUserThrottle]
    
    def get(self, request):
        return Response({"meesage": "Hello for today!"})
```  

### Class Based View  

View 함수를 만들어주는 클래스  
- as_view() 클래스 함수를 통해, View 함수를 생성  
- 상속을 통해, 여러 기능들을 믹스인  

장고 기본 CBV 패키지  
- django.views.generic  
- <https://github.com/django/django/tree/3.0.2/django/views/generic>  

써드파티 CBV  
- django-braces  
- <https://django-braces.readthedocs.io>  

<https://docs.djangoproject.com/en/3.0/topics/class-based-views>  

## CBV 컨셉 구현해보기  

### 1. FBV  

```python
from django.shortcuts import get_object_or_404, render

def post_detail(request, id):
    post = get_object_or_404(Post, id=id)
    return render(request, 'blog/post_detail.html', {
        'post': post,
    })

def article_detail(request, id):
    article = get_object_or_404(Article, id=id)
    return render(request, 'blog/article_detail.html', {
        'article': article,
    })

# urls.py
urlpatterns = [
    path('post/<int:id>/', post_detail),
    path('article/<int:id>/', article_detail)
]
```  

### 2. 함수를 통해, 동일한 View 함수 생성  

```python
def generate_view_fn(model):
    def view_fn(request, id):
        instance = get_object_or_404(model, id=id)
        instance_name = model._meta.model_name
        template_name = '{}/{}_detail.html'.format(model._meta.app_label, instance_name)
        return render(request, template_name, {
            instance_name: instance
        })
    return view_fn

post_detail = generate_view_fn(Post)
article_detail = generate_view_fn(Article)
```  

### 3. Class로 동일한 View 함수 구현  

```python
class DetailView:
    def __init__(self, model):
        self.model = model

    def get_object(self, *args, **kwargs):
        return get_object_or_404(self.model, id=kwargs['id'])

    def get_template_name(self):
        return '{}/{}_detail.html'.format(
            self.model._meta.app_label,
            self.model._meta.model_name
        )

    def dispatch(self, request, *args, **kwargs):
        object = self.get_object(*args, **kwargs)
        return render(request, self.get_template_name(), {
            self.model._meta.model_name: object,
        })

    @classmethod
    def as_view(cls, model):
        def view(request, *args, **kwargs):
            self = cls(model)
            return self.dispatch(request, *args, **kwargs)
        return view

post_detail = DetailView.as_view(Post)
article_detail = DetailView.as_view(Article)
```  

### 4. 장고 기본 제공 CBV 활용  

```python
from django.views.generic import DetailView

post_detail = DetailView.as_view(model=Post, pk_url_kwarg='id')
article_detail = DetailView.as_view(model=Article, pk_url_kwarg='id')

# pk_url_kwarg 인자를 "pk"로 지정했다면
post_detail = DetailView.as_view(model=Post)
article_detail = DetailView.as_view(model=Article)

urlpatterns = [
    path('post/<int:pk>/', post_detail),
    path('article/<int:pk>/', article_detail),
]
```  

```python
# 상속을 통한 CBV 속성 정의
from django.views.generic import DetailView

class PostDetailView(DetailView):
    model = Post
    pk_url_kwarg = 'id'

post_detail = PostDetailView.as_view()  
```

### CBV는 ~  

CBV가 정한 관례대로 개발할 경우, 아주 적은 양의 코드로 구현  
- 그 관례에 대한 이해가 필요 -> FBV를 통한 개발경험이 도움  
-- 필요한 설정값을 제공하거나, 특정 함수를 재정의하는 방식으로 커스텀 가능  
-- 하지만, 그 관례를 잘 이해하지 못하고 사용하거나, 그 관례에서 벗어난 구현을 하고자 할 때에는 복잡해지는 경향이 있다.  

CBV를 제대로 이해하려면~  
- 코드를 통한 이해가 지름길  
-- 파이썬 클래스에 대한 이해가 필요 (특히 상속, 인자 packing, unpacking)  
- <https://github.com/django/django/tree/2.1/django/views/generic>  

CBV 코드를 동일하기 동작하는 FBV로 구현해보는 연습을 추천  

## 장고 기본 CBV API (Base Views)  

### Built-in CBV API  

Base views  
- View, TemplateView, RedirectView  

Generic display views  
- DetailView, ListView  

Generic date views  
- ArchiveIndexView, YearArchiveView, MonthArchiveView, WeakArchiveView, DayArchiveView, TodayArchiveView, DateDetailView  

Generic editing views  
- FormView, CreateView, UpdateView, DeleteView  

<https://docs.djangoproject.com/en/2.1/ref/class-based-views/>  

### Base Views  
- django/views/generic/base.py  

View  

TemplateView  
- TemplateResponseMixin  
- ContextMixin  
- View  

RedirectView  
- View  

### View  

모든 CBV의 모체  
- 이 CBV를 직접 쓸 일은 거의 없다.  

http method별로 지정 이름의 멤버함수를 호출토록 구현  

CBV.as_view(**initkwargs)  
- initkwargs 인자는 그대로 CBV 생성자로 전달  

```python
def __init__(self, **kwargs):
    for key, value in kwargs.items():
        setattr(self, key, value)
```  

```python
class View:
    def __init__(self, **kwargs):
        # ...
    
    @classonlymethod
    def as_view(cls, **initkwargs):
        # ...
        return self.dispatch(request, *args, **kwargs)
    #...
    return view

    def dispatch(self, request, *args, **kwargs):
        # ...
        # request.method.lower() 이름의 멤버함수를 호출
        handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        return handler(request, *args, **kwargs)

    def http_method_not_allowed(self, request, *args, **kwargs):
        # ...
        return HttpResponseNotAllowed(self._allowed_methods())

    def options(self, request, *args, **kwargs):
        # ...
        return response

    def _allowed_methods(self):
        return [m.upper() for m in self.http_method_name if hasattr(self, m)]
```  

### TemplateView  

```python
class ContextMixin:
    extra_context = None
    
    def get_context_data(self, **kwargs):
        kwargs.setdefault('view', self)
        if self.extra_context is not None:
            kwargs.update(self.extra_context)
        return kwargs

class TemplateResponseMixin:
    template_name = None
    template_engine = None
    response_class = TemplateResponse
    context_type = None

    def render_to_response(self, context, **response_kwargs):
        response_kwargs.setdefault('contene_type', self.content_type)
        return self.response_class(
            request = self.request,
            template = self.get_template_name(),
            context = context,
            using = self.template_engine,
            **response_kwargs
        )

    def get_template_names(self):
        if self.template_name is None:
            raise ImproperlyConfigured(
                "TemplateResponseMixin requires either a definition of "
                "'template_name' or an implementation of 'get_template_names()'"
            )
        else:
            return [self.template_name]

class TemplateView(TemplateResponseMixin, ContextMixin, View):
    def get(self, request, *args, **kwargs):
        context = self.get_context_date(**kwargs)
        return self.render_to_response(context)
```  

### RedirectView  

Permanent (디폴트: False)  
- True: 301 응답 (영구적인 이동) - 검색엔진에 영향  
- False : 302 응답 (임시 이동)  

url = None
- URL 문자열  

pattern_name = None
- URL Reverse를 수행할 문자열  

query_string = False
- QueryString을 그대로 넘길 것인지 여부  

```python
class RedirectView(View):
    permanent = False
    url = None
    pattern_name = None
    query_string = False

    def get_redirect_url(self, *args, **kwargs):
        if self.url:
            url = self.url % kwargs
        elif self.pattern_name:
            url = reverse(self.pattern_name, args=args, kwargs=kwargs)
        else:
            return None

    def get(self, request, *args, **kwargs):
        url = self.get_redirect_url(*args, **kwargs)
        if url:
            if self.permanent:
                return HttpResponsePermanentRedirect(url)
            else:
                return HttpResponseRedirect(url)
        else:
            logger.warning(
                'Gone: %s', request.path,
                extra={'status_code':410, 'request': request}
            )
            return HttpResponseGone()

    # head, post, options, delete, put, patch 모두 같은 구현
    def head(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

```  

## 장고 기본 CBV API (Generic display views) - 1  

### Built-in CBV API  

Base views  
- View, TemplateView, Redirect View  

Generic display views  
- DetailView, ListView  

Generic date views  
- ArchiveIndexView, YearArchiveView, MonthArchiveView, WeekArchiveView, DayArchiveView, TodayArchiveView, DateDetailView  

Generic editing views  
- FormView, CreateView, UpdateView, DeleteView  

<https://docs.djangoproject.com/en/2.1/ref/class-based-views/>  

### Generic display views  

DetailView  
- SingleObjectTemplateResponseMinin  
-- TemplateResponseMinin  
- BaseDetailView  
-- SingleObjectMinin  
-- View  

ListView  
- MultipleObjectTemplateResponseMinin  
-- TemplateResponseMinin  
- BaseListView  
-- MultipleObjectMinin - ContextMinin  
-- View  

<https://docs.djangoproject.com/en/2.1/ref/class-based-views/generic-display/>  

### DetailView  

1개 모델의 1개 Object에 대한 템플릿 처리  
- 모델명소문자 이름의 Model Instance 템플릿에 전달  
-- 지정 pk 혹은 slug에 대응하는 Model Instance  

```python
from django.views.generic import DetailView
from .models import Post

post_detail = DetailView.as_view(model=Post)

class PostDetailView(DetailView):
    model = Post

post_detail2 = PostDetailView.as_view()
```  

### DetailView 상속관계  
- django.views.generic.detail.DetailView  

SingleObjectTemplateResponseMinin  
- template_name이 지정되지 않았다면, 모델명으로 템플릿 경로 유추  
- TemplateResponseMixin  

BaseDetailView  
- SingleObjectMixin : url_kwarg로 지정된 model Instance 획득  
-- contextView  
- View  

## 장고 기본 CBV API (Generic display views) - 2  

### ListView  

1개 모델에 대한 List 템플릿 처리  
- 모델명소문자_list 이름의 QuerySet을 템플릿에 전달  

페이징 처리 지원  

```python
from django.views.generic import ListView
from .models import Post

post_list1 = ListView.as_view(model=Post)

post_list2 = ListView.as_view(model=Post, paginate_by=10)

class PostListView(ListView):
    model = Post
    paginate_by = 10

post_list3 = PostListView.as_view()

class PostListView(ListView):
    model = Post
    paginate_by = 10

    def get_queryset(self):
        qs = super().get_queryset()
        qs = qs.filter(...)
        return qs

post_list4 = PostListView.as_view()
```  

### ListView 상속관계  
- django.views.generic.list.ListView  

MultipleObjectTemplateResponseMixin  
- template_name이 지정되지 않았다면, 모델명으로 템플릿 경로 유추  
-- TemplateResponseMixin  

BaseListView  
- MultipleObjectMixin : Paginator가 적용된 QuerySet 획득  
-- ContextMixin  
- View  





## 장고 기본 CBV API (Generic display views) - 3


<br>
---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊