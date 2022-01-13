---
title: "[Django] Django Overview"

categories:
  - Django
tags:
  - [Python, Django, React]

toc:  true
toc_sticky: true

date: 2022-01-12
last_modified_at: 2022-01-13
---

## 파이썬 설치(윈도우)  

### 파이썬 설치, 다양한 파이썬 배포판  

공식 배포판도 있지만, 개발용 머신에서는 Anaconda Python 추천  
- PyPi보다 다양한 C/C++ 라이브러리 바이너리를 Anaconda 측에서 배포.  
- 일반적인 파이썬 가상환경보다 강력한 Conda의 Environment  

실제 웹서비스에서는 대게 리눅스 운영체제를 사용하여, 이때에는 대게 운영체제 배포판에서 제공해주는 파이썬을 사용하게 된다.  

1. Anaconda 다운로드 사이트 접속  
    site : <https://www.anaconda.com/products/individual>에 접속해서 Download를 받아요.  

2. All Users (requires admin previleges)를 선택 후, Next 버튼을 눌러주세요.  
    ![image](https://user-images.githubusercontent.com/37467408/149074600-9ea0f0dc-098f-41e2-917c-2fad6e0f4c34.png)  
    두 개의 차이는, Just Me는 개인폴더(문서)에 저장되게 되고, All users는 프로그램 영역에 설치가 되게 된다.  
    All users를 선택한 이유는, 다운로드 설치 경로가 간단해지게 되서 선택을 했다.  

3. 프로그램 설치 경로 확인 후, Next 버튼 클릭  
    ![image](https://user-images.githubusercontent.com/37467408/149075167-56fee18d-f89a-4574-9ece-43ed1bfdc1f3.png)

4. Add Anaconda to the system PATH environment variable 선택 후, Install 클릭  
    ![image](https://user-images.githubusercontent.com/37467408/149075389-aa67fc98-31c8-4516-98b2-6814ad8c594f.png)  

5. cmd 창에 `conda --version`을 입력해 정상 설치 되었는지 확인한다.  

### 가상환경 설정 및 Django 설치  

1. 가상 환경 설정  
    `conda create --name PROJECT_NAME python=PYTHON_VERSION`  
    
2. 가상환경 실행  
    `conda activate PROJECT_NAME`  

### Django 설치  

Django 공식 소스 저장소  
- <https://github.com/django/django>  

Django Versions  
- LTS(Loong Term Support) 버전 : 대개 3년동안 업데이트를 지원  
- 비동기를 공식 지원하는 3.0 버전은 2019년 12월에 정식 릴리즈 (완전한 비동기 지원은 4.0에서)  

설치 : 파이썬 패키지 매니전 pip를 이용  
- pip install "django~=3.0.0" >> 3.0.* 버전 중에 최신 버전 설치를 시도  

## 장고 프로젝트 생성  

### 장고 프로젝트 생성  

위에서 만든 가상환경을 실행시킨 후, 가상환경에서 폴더를 만든 후, 아래의 명령어 둘 중 하나 입력  

`django-admin startproject PROJECT_NAME`  or `python -m django startproject PROJECT_NAME`  

### 기본 생성된 파일/디렉토리 목록  

기본 템플릿 : django/conf/project_template  

PROJECT_NAME : 프로젝트 명  
- manage.py : 명령행을 통해 각종 장고 명령을 수행  
- PROJECT_NAME : 프로젝트명으로 생성된 디렉토리, 이 이름을 참조하고 있는 코드 몇 개 있기에 함부로 수정 X  
-- __init__.py : 모든 파이썬 패키지에는 __init__.py을 둔다. 패키지를 임포트할 때의 임포트 대상  
-- settings.py : 현재 프로젝트에서 장고 기본설정(django/conf/global_settings.py) 을 덮어쓰고, 새롭게 지정할 설정들  
-- urls.py : 최상위 URL 설정  
-- wsgi.py : 실서비스에서의 웹서비스 진입점  

### 프로젝트 초기화 작업 및 개발서버 구동  

파이썬 명령은 각 머신의 파이썬3 세팅에 따라, python, python3 혹은 python3.7 등이 될 수 있다.  

```
django-admin startproject PROJECT_NAME
cd PROJECT_NAME
python manage.py migrate
python manage.py createsuperuser

ID, PW 입력을 통해 계정을 만들자. 이메일은 선탟사항이므로 입력하지 않고 엔터를 쳐도 된다.

python manage.py runserver
```  

이제 웹 브라우저를 띄워, http://localhost:8000 으로 접속해 로그인해보자.  
- 윈도우의 컴퓨터이름이나 계정명이 한글일 경우, 서버 구동에 실패할 수도 있으니 실패한 경우 컴퓨터 이름이랑 계정명을 확인해보자.  
- http://localhost:8000/admin 으로 이동해 위에서 만든 계정으로 로그인 해보자.  

### 크롬이나 Firefox 사용 권장  

웹서비스 개발 시의 웹브라우저는 가급적 Google Chrome이나 Mozilla Firefox를 쓰는 것이 좋다.  
- 최신 웹기술 지원  
- 웹브라우저 개발자 도구 지원이 좋다.  
- 다양한 개발용 확장  

## Visual Studio Code 설치  

1. Visaul Studio Code 설치 사이트 이동 후 다운로드  
    <https://code.visualstudio.com/>  

2. 아래 단계에서, "Code(으)로 열기" 작업을 Windows 탐색기 디렉터리의 상황에 맞는 메뉴에 추가 선택 후, 다음을 눌러주세요.  
    ![image](https://user-images.githubusercontent.com/37467408/149082895-2e8f0031-6cf4-4230-9b19-ed0ecd94fc46.png)  

3. PROJECT_DIR로 가서 우클릭 후, Open wite Code 선택  
4. Visual Code의 EXTENSIONS으로 가서 Python 선택 및 다운로드  
5. Command Palette에 Python: Select interpreter 선택 후, 로딩 되면 자신이 사용활 환경 선택  
6. Command Palette에 Terminal: Select Default Profile 선택 후, Command Prompt 선택  

## 장고 주요 구성요소  

### 장고 주요 기능들 (1)

1. Function Based Views : 함수로 HTTP 요청 처리  
2. Models : 데이터베이스와의 인터페이스  
3. Templates : 복잡한 문자열 조합을 보다 용이하게, 주로 HTML 문자열 조합 목적으로 사용하지만, 푸쉬 메세지나 이메일 내용을 만들 때에도 쓰면 편리  
4. Admin 기초 : 심플한 데이터베이스 레코드 관리 UI  
5. Logging : 다양한 경로로 메세지 로깅  
6. Static files : 개발 목적으로의 정적인 파일 관리  
7. Message framework : 유저에게 1회성 메세지 노출 목적  

### 장고 주요 기능들 (2)  

1. Class Based View : 클래스로 함수 기반 뷰 만들기  
2. Forms : 입력폼 생성, 입력값 유효성 검사 및 DB로의 저장  
    - validators & Fields & Widgets  
3. 테스팅  
4. 국제화 & 지역화  
5. 캐싱  
6. Geographic : DB의 Geo 기능 활용 (PostgreSQL 중심)  
7. Sending Emails  
8. Syndication Feeds (RSS/Atom)  
9. Sitemaps  

### 웹 어플리케이션 기본 구조  

![image](https://user-images.githubusercontent.com/37467408/149084740-64dd75f9-70ca-49c7-b5f0-33cee5bc4665.png)  

### 장고 기본 구조  

![image](https://user-images.githubusercontent.com/37467408/149084898-9f58c627-fe79-456d-8c4f-6b55f56daaf5.png)  

### 따라해보기  

### 장고 앱의 필요성  

현재 프로젝트의 블로그 기능을 `다른 프로젝트에서도 사용하려한다면???`  
- 블로그를 장고앱 형태로 격리해서 만들어둔다면, 다른 프로젝트에도 적용하기 편리  

1. 가상환경 프로젝트에서 아래 명령어 실행  
    python manage.py startapp blog1  
2. blog1 폴더 아래에 urls.py를 만들고, 아래처럼 입력해준다.  

    ```python
    urlpatterns = [
    ]
    ```  
   
### 장고 앱  

`재사용성`을 목적으로한 파이썬 패키지  
- 재사용성을 목적으로 둔 것이 아니라면, 하나의 장고 앱에서 현재 프로젝트의 거의 모든 기능을 구현해도 무방하다.  
- 앱을 하나의 작은 서비스로 봐도 무방  

하나의 앱이름은 현재 프로젝트 상에서 `유일`해야한다.  

새롭게 생성한 장고앱이나 외부 라이브러리 형태의 장고앱은 `필히 settings.ISTALLED_APPS 에 등록`을 시켜줘야만 장고앱으로서 대접을 받는다.  
- 앱의 URLConf 를 제외한 많은 부분(모델, Template, static 등) 들이 자동으로 등록된다.  

1. PROJECT_NAME 폴더에 settings.py로 이동해 ISTALLED_APPS를 수정해준다.  
    ```python
    INSTALLED_APPS = [
        # django Default Apps..
        # 'blog1',
    ]
    ```  

2. blog1 폴더 models.py로 이동해 아래 내용 추가  
    ```python
    class Post(models.Model):
        title = models.CharField(max_length=100)
        content = models.TextField
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    ```  

3. blog1 migration 진행  
    ```
    python manage.py makemigrations blog1
    python manage.py migrate blog1
    ```  

4. blog1 admin.py 수정  
    ```python
    from .models import Post

    admin.site.register(Post)
    ```  

5. PROJECT_NAME 폴더 urls.py 수정  
    ```python
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('blog1/', include('blog1.urls')),
    ]
    ```  

6. blog1/view.py 수정  
    ```python
    from django.shortcuts import render
    from .models import Post

    def post_list(request):
        qs = Post.objects.all() # QuerySet
        return render(request, 'blog1/post_list.html', {
            'post_list' : qs,
        })
    ```  

7. blog1/templates/blog1 폴더 만든 후, 폴더 내에 post_list.html 생성 후 수정  
    ```html
    <h1>Post List</h1>
    {% raw %}{% for post in post_list %}{% endraw %}
        {% raw %}<h2>{{ post.title }}</h2>{% endraw %}
        {% raw %}{{ post.content }}{% endraw %}
    {% raw %}{% endfor %}{% endraw %}
    ```  

8. blog1/urls.py 수정  
    ```python
    from . import views

    urlpatterns = [
        path('', views.post_list, name='post_list'),
    ]
    ```  

---
**🐢 현재 공부하고 있는 `파이썬/장고 웹서비스 개발 완벽 가이드 with 리액트 - 이진석 강사` 의 강의를 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊