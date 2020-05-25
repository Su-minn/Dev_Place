

# 10. Introduction to Views and Urls



## 10.0 Introduction to Urls and Views

- Url : 요청하는 방법 (request)
  ex) urlpatterns = [path(=="admin/"==, admin.site.urls)]
  Rooms/ , login/

- view : 요청에 반응하는 방법 (answer)

  view는 function
  ex) urlpatterns = [path("admin/", ==admin.site.urls==)]

- room의 list를 보여주고 싶은 Function은
  rooms application에 두면 된다
  rooms의 views.py에 작성

- ```python
  def all_rooms(request):
  	pass
  ```

- config의 url.py로 가서 view import하기

  ```python
  from django.contrib import admin
  from django.urls import path
  from django.conf import settings
  from django.conf.urls.static import static
  from rooms import views as room_views
  
  urlpatterns = [path("", room_views.all_rooms), path("admin/", admin.site.urls)]
  ```

   와 같이 작성하고, 웹페이지에 들어가면 view가 아무것도 리턴하지 않는다고 에러가 뜸

- url은 작성하면서 길어지므로, divide and conquer 을 해야한다

- /rooms로 시작하는 url 들은 rooms의 파일로 보내고,

- /users로 시작하는 url들은 users의 파일로 가게한다

- / 와 같이 아무것으로도 시작하지않는 home, login, logout 같은 것은
  core로 가게 한다

- 분할 정복을 위해 위의 코드에서 방금 작성한 부분 다시 삭제

  ```python
  from django.contrib import admin
  from django.urls import path
  from django.conf import settings
  from django.conf.urls.static import static
  
  
  urlpatterns = [path("admin/", admin.site.urls)]
  ```

- core의 urls.py로 이동

- Urls의 path를 import하고 다음 코드 작성
  urlpattenrs는 옵션이 아니라 필수다!

  ```python
  from django.urls import path
  from rooms import views as room_views
  
  urlpatterns = [
  	path("", room_views.all_rooms)
  ]
  ```

   그리고 이것을 config의 url에 넣어줘야한다

- config의 urls.py에 다음과 같이 입력

  ```python
  urlpatterns = [path("", include("core.urls")), path("admin"/), admin.site.urls] // 이와 같이 변경
  ```

- core의 urls.py에 돌아와서 room view에 이름을 주자
  다음과 같이 변경

  ```python
  urlpatterns = [path("", room_views.all_rooms, name="home")]
  ```

- config의 urls.py에 namespace 추가

  ```python
  urlpatterns = [path("", include("core.urls", namespace="core")), path("admin"/), admin.site.urls]
  ```

  name과 namespace의 설명은 추후에 진행 예정

  include의 namespace를 지정할 때 app name필요

- core의 urls.py에 와서 다음 코드 추가

  ```python
  from django.urls import path
  from rooms import views as room_views
  
  app_name = "core"
  
  urlpatterns = [path("", room_views.all_rooms, name="home")]
  ```

  core - urls.py의 app_name과 config-urls.py namespace의 core가 같아야함

- 그리고 웹페이지에 들어가보면아직 error가 발생함을 알 수 있다
  다음 강의에서 해결 예정


## 10.1 HttpResponse and render



-  그 URL로 매번 들어갈때마다 매번 http Request를 생성하는 것

- 그 Request에 대해 Http Response를 응답하는게 브라우저의 구성방법임

- 장고에 포함된 것이 아닌 인터넷에 있는 기능

- 위의 에러는 아직 HttpResponse를 하지않았기에 발생

- Request를 받을 때마다 장고는 이것을 python object로 변환하여 모든 view에 대해 첫번째 인자로 전달한다

- HttpResponse를 위해 rooms-views.py에서 HttpeResponse를 import하자

  ```python
  from django.shortcuts import render
  from django.http import HttpResponse
  
  def all_rooms(request):
  	return HttpResponse(content="hello")
  ```

  위 코드 맨 위의 render를 import하면
  HttpResponse 안에 html을 넣어서 보내줄 수 있다

- datetime을 import해서 넣어보자

  ```python
  from datetime import datetime
  from django.shortcuts import render
  from django.http import HttpResponse
  
  def all_rooms(request):
  	now = datetime.now()
  	return HttpResponse(content=f"<h1>{now}</h1>")
  ```

   의 코드를 저장하고 웹페이지를 열면 시간이 나옴

- 실제로 매번 이와 같이 코드를 사용하지는 않고, 템플릿을 렌더링을 함
  다음과 같이 코드 변경

  ```python
  from django.shortcuts import render
  
  def all_rooms(request):
  	return render(request, "all_rooms.html")
  ```

  렌더링에는 request가 필요 (요청에 대한 응답이기 때문)
  렌더링할 templete을 다음 시간에 만들어보자
  


## 10.2 Introduction to Django Templates



- Httpresponse를 수동으로 매번 보내고 싶지 않기에 템플릿을 렌더링 할 것임

- template는 html이고 차이점은, 파이썬이 대신 컴파일 해준다는 것

- Template를 위한 디렉토리를 app과 병렬적인 위치에 생성 
  이름은 "templates", 이 디렉토리 하위에 all_rooms.html 생성

- Config의 settings.py에 가서 templates폴더를 등록해줘야함

- TEMPLATES라 적힌 곳으로 이동해서 DIR 코드에 추가

  ```python
  "DIRS": [os.path.join(BASE_DIR, "templates")],
  ```

  템플릿의 매우 멋진 점은 
  context : 변수를 보내는 방법

- rooms의 view.py에서 다음과 같이 작성

  ```python
  from datetime import datetime
  from django.shortcuts import render
  
  def all_rooms(request):
  	now = datetime.now() // 변수
  	hungry = True // 변수
  	return render(request, "all_rooms.html", context={"now": now, "hunghry" : hungry})
  ```

- all_rooms.html 로 이동해서 다음과 같은 코드 작성
  다른 문법의 html 작성 / 
  장고 템플릿이기에 로직도 작성 가능

- 장고 html 문법
  1) 변수는 {{ }} 로 감싼다
  2) 조건문 반복문은 {% %} 로 감싼다

  ```django
  <h1>Hello!!</h1>
  
  <h4>The time right now is: {{now}} </h4>
  
  <h6>
    {% if hungry%}I'm hungry{% else %}i'm okay{% endif %}
  ```



## 10.3 Extending Templates part One



- All_rooms view에서 모든 방을 보여주도록 해보자
  Rooms - views.py 에 다음 코드 작성

  ```python
  from django.shortcuts import render
  from . import models // models을 import
  
  def all_rooms(request):
  	all_rooms = models.Room.objects.all()
  	return render(request, "all_rooms.html", context={"rooms" : all_rooms})
  ```

- all_rooms.html 로 이동

- 장고 Template 문법 작성 도와주는 확장프로그램 : django snippets

- 아래 코드 작성

  ```django
  {% for room in rooms %}
  	<h1>{{room.name}} / ${{room.price}}</h1>
  {% endfor %}
  ```

- 주의 사항 3가지
  1) views.py에서 return render(request, =="home.html"==, ~) 템플릿 name은 반드시
  Templates 디렉토리의 html 이름과 같아야 함
  2) views.py 의 method " def ==all_rooms==(request) " 는 
  urls.py의 urlpatterns = [path("", room_views.==all_rooms==, name="home")] 
  연결 부분과 같아야함
  3) views.py의 context={"==rooms==" : all_rooms}) 는 
  home.html의 사용 변수 이름과 같아야함

- Tip) home.html과 같은 html 파일에서 기본 html 틀을 만들고 싶다면
  html 확장 프로그램 설치 후, ==html:5== 타이핑

- 주의 사항: 위의 틀을 만들 때는 입력모드를 html로 변경하고,
  다시 장고 template 문법을 적을 때는 입력모드를 django template로 변경해야함

- html에서 공통적으로 사용되는 부분은 base.html로 만들어보자

- templates 디렉토리로 이동하여, 하위에 base.html을 만들고, rooms라는 디렉토리도 생성하여 rooms 디렉토리안에 home.html을 이동시키자
  home.html에서 base로 옮겨서 골격으로 쓸 부분은 지우고 핵심만 남기기

  ```django
  {% for room in potato %}
  	<h1>{{room.name}} / ${{room.price}}</h1>
  {% endfor %}
  ```

- Base.html은 앞으로 rendering 하지는 않지만, 다른 모든 html은 base를 부모로 가짐
  마치 타임스탬프 모델과 유사

- Base.html에 title, head, body 등의 기본 틀을 만들어주고 > html:5 타이핑

- home.html로 와서 base를 확장시켜보자

  ```django
  {% extends "base.html" %} // 추가하여 base를 확장
  {% for room in rooms %} // already exists
  	<h1>{{room.name}} / ${{room.price}}</h1>
  {% endfor %}
  ```

- rooms - views.py에서 home.html의 위치를 변경했으므로 코드 수정이필요

  ```python
  from django.shortcuts import render
  from . import models // models을 import
  
  def all_rooms(request):
  	all_rooms = models.Room.objects.all()
  	return render(request, "rooms/home.html", context={"rooms" : all_rooms}) // change
  ```

- 이 상태로 웹에 들어가면 base는 연동 되었지만 home part가 나오지 않는 문제 발생
  다음 강의에서 해결 예정

  

## 10.4 Extending Templates part Two and Includes



- base에 home을 가져오려면 블럭의 개념이 필요

- block : 자식 템플릿이 부모 템플릿에게 넘겨주는 창

- 사용법 : {% block "blockname" %} {% endblock %}
  1) base.html(부모 템플릿)의 원하는 위치에 넣고
  2) home.html(자식  템플릿)의 원하는 내용을 위의 블럭으로 감싸서 사용

- Ex) title 뒤에 | Nbnb는 부모 템플릿으로 공통으로 사용하고 싶고
  그 다음 제목만 바꾸고 싶을 때.
  Base.html 에 아래와 같이 작성

  ```django
  <title>{% block page_name %}{% endblock page_name %} | Nbnb </title>
  ```

  home.html로 이동하여 다음 코드 작성

  ```django
  {% block page_name %}
  	Home
  {% endblock page_name %}
  ```

- html도 크기가 계속 커지므로 분할 정복이 필요

- templates 디렉토리안에 partials 라는 하위 디렉토리를 만들고
  footer.html과 header.html 을 생성하여 관련 내용을 집어넣자

- Base.html에 분할한 내용들을 집어넣자

  ```django
  <body>
  	{% include "partials/header.html" %}
  	
  	{% block content %}{% endblock %}
  	
  	{% include "partials/footer.html" %}
  </body>
  ```

- 현재 room의 view에서는 models.Room.objects.all()로 모든 object를 호출하고 있는데
  데이터베이스 입장에서 매우 좋지않음
  다음 강의에서 이것을 해결할 예정

