# 11. HomeView



## 11.0 HomeView Intro



- 이 섹션의 목표 : View 최적화
  all_rooms가 현재 모든 Room을 보여주고 있으나,
  10개 단위로 자르고 페이지를 넘기는 방식으로 변경할 것

  

- 장고를 활용하면 쉽게 할 수 있지만,
  어려운 방법부터 하나씩 하고 쉬운 방법 설명 예정

- 3 가지 방법 설명
  1) 파이썬으로만 100% 수동 작성
  2) django 도움을 받아 작성
  3) 거의 코드 없이 작성

- 장고를 활용하는 법보다, 장고의 작동과정과 원리를 이해하는 것이 중요
  

## 11.1 Pagination with Limit and Offset



- 우리의 페이지에는 list의 모든 것이 나오고 있고 데이터베이스에 안좋으므로
  제한할 필요가 있음

- 만약 5개로 제한하고 싶다면
  다음과 같은 코드 추가 [:5]
  rooms - view.py 에서

  ```python
  def all_rooms(request):
  	all_rooms = models.Room.objects.all()[:5]
  	return render(request, "rooms/home.html", {"rooms": all_rooms})
  ```

- [:5]에서
  [a:b] > a는 offset - 건너뛰고 싶은 수(offset) (list의 시작 넘버) 
  / 만약 생략되어있다면 0을 의미
  b는 보여주고 싶은 list의 끝
  10개씩 페이지마다 보여주고 싶다면
  1페이지는 [0:10] / 2페이지는 [10:20] / ~

- 장고의 작동원리는
  Models.Room.objects.all()[:5]가 있다면
  모든 object를 가져온후 [:5] 를 제한하는 것이 아니고,
  한번에 [:5]로 제한된 object를 가져온다 > DB의 낭비가 없음
  SQL 상으로 [a:b]는 (OFFSET a LIMIT 5) 와 같다

- 이제 페이지를 변경하는 방법을 알아야함
  페이지리를 변경하는 데 가장 많이 사용하는 방법은 컨벤션 사용
  Url 맨뒤에 ?page=1 / ?page=2 등등이 붙는 것

- request object를 이용해야함

- Url에서 오는 모든 것은 get request 이다
  페이지로 오는 것은 get이고 그 말은 우리가 할 수 있다는 것을 의미

  ```python
  def all_rooms(request):
  	print(request.GET) // 장고는 이 request를 결과로 보여줌
  ```

  웹의 주소창에 작성한 url을 QueryDictionary로 결과를 보여주게 된다

- request object를 더 자세히 알고 싶다면

  장고 documentation에서 request object에서 Http request를 찾아보자
  get, post, Cookies 등의 많은 정보가 있음

- Tip) GET은 get이라는 method를 가지고 있는데, 이와 같이 어떤 메소드를 갖고 있는지, 어떤 하위 내용을 갖고 있는지를 확인할 때, print를 활용하면 좋다

  ```python
  print(vasrs(request.GET)) > 여기서는 vars(변수)가 아니어서 작동 x
  print(dir(request.GET)) > 관련 method 들을 볼 수 있다
  
  print(request.GET.keys()) > 나온 결과를 이와 같이 다시 print하면 이 method 에 대한 결과값도 확인 가능
  
  request.GET.get("page", 1) > page 값이 없을 때(None) default를 page=1으로 설정
  ```

  아래 코드로 수정

  ```python
  def all_rooms(request):
  	page = int(request.GET.get("page", 1)) // request를 str형으로 하므로 아래 계산들을 위해서는 int형으로 바꿔줘야함
  	page_size = 10
  	limit = page_size * page
  	offset = limit - page_size
  	all_rooms = models.Room.objects.all()[offset:limit]
  	return render(request, "rooms/home.html", {"rooms": all_rooms})
  ```

- 다음 강의에서는 에러페이지 처리와 navagation 설정 예정

## 11.2 Pages List Navigation



## 11.3 Next Previous Page Navigation



- 장고 템플릿에서는 많은 로직을 허용하지 않지만,

  template tag 와 filter를 사용하면 상당 부분을 해결할 수 있다

## 11.4 Using Django Paginator



## 11.5 get_page vs page



## 11.6 Handling Exceptions



## 11.7 Class Based Views



- list view : class based view로부터 왔다

- class based view
  : view도 추상화해서 여러 곳에 사용함

- 이전의 rooms-view.py에서의 def all_rooms를 class로 변경하자
  views.py의 내용들을 지우고 다음과 같이 작성

  ```python
  from django.views.generic import ListView
  from . import models
  
  class HomeView(ListView):
  	
    """ HomeView Definition """
    
    model = models.Room
  ```

  core - urls.py의 path에서 room_views.all_rooms 부분을
  `room_views.HomeView.as_view()` 로 변경

  url은 함수만 path로 인식하는 데, listview에는 함수로 바꿔주는 method가 있기에 as_view를 사용하면 class인 homeview를 함수로 변경시킨다

- templates 디렉토리의 rooms에 들어가서 home.html을 room_list.html로 변경
  listview는 자동으로 template를 찾는데, 요구하는 *_list.html로 바꿔줘야함 

- 장고 documentation을 참고하면 listview는 우리가 액세스 할 수있는 object list를 제공한다

- room_list.html로 가서

  ```python
  {% for room in page.object_list %} // 부분을
  {% for room in object_list %} // 로 변경
  ```

  정상 작동함

- Class based view의 단점은 보기가 힘들다는 것인데
  누군가 이를 위해 좋은 사이트를 만들어줬다
  Https://ccbv.co.uk
  대부분의 class based view를 보여준다

## 11.8 Class Based Views part Two

