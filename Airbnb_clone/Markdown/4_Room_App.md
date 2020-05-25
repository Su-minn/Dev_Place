

# 4. Room_App



## 4.0 TimeStampedModel



- Rooms Model을 만들어보자!

- 가장 먼저, rooms application을 settings.py에 등록해줘야함

- settings.py에 다음과 같이 코드 작성

  ```python
  PROJECT_APPS = ["users.apps.UsersConfig", "rooms.apps.RoomsConfig"]
  ```

  models.py에 다음과 같이 코드 작성

  ```python
  from django.db import models // already exists
  
  class Room(models.Model):
  
  	""" Room Model Definition """
  
  	pass
  ```

  admin.py 로 가서

  ```python
  from django.contrib import admin // already exists
  from . import models
  
  @admin.register(models.Room)
  class RoomAdmin(admin.ModelAdmin):
  
  	pass
  ```

- 웹의 홈스크린으로 이동해서 보면 ROOMS가 생겼음

  아직 migration을 안했기에 클릭하면 당연히 에러 발생

- rooms가 만들어질 때나, 업데이트 될때를 알 수 있으면 유용하다
  Created 필드, updated 필드를 datetime field로 생성	

```python
created = models.DateTimeField()
updated = models.DateTimeField()
```

이 필드 들은 rooms뿐 아니라, reviews, reservations, lists에도 들어갈 건데,
복사해서 사용하면 비효율적 > 프로그래머라 할 수 없다

> 새로운 application > 이름 : core 생성

pipenv shell로 들어가서
`Django-admin startapp core` 입력

core 안에 다른 app에서 재사용 가능한 common 파일을 만들 것
core 안에 model을 만들고 이 model을 이용하여 rooms를 확장
Users를 제외한 모든 app은 전부 이 model.Model 한 가지 model로 부터 확장

- core 디렉토리의 models.py 에 model 생성

  ```python
  from django.db import models // already exists
  
  class TimeStampedModel(models.Model):
  
  	""" Time Stamped Model """
  	
  	created = models.DateTimeField()
  	updated = models.DateTimeField()
  ```

  같은 내용을 복사 붙여넣기 하지 않기 위해 이렇게 Core model에
  created와 updated 필드 생성

- 코드 작성 후 core 등록
  settings.py로 이동하여 다음과 같이 작성

  ```python
  PROJECT_APPS = ["core.apps.CoreConfig", ~~ "~~~"]
  ```

  에러 확인 > 서버돌아가는 상태(runserver) 에서 저장했을때 오류가 발생하는지 확인

- 주의 사항 core에 Model을 생성하면 db로 이동하게 되는데, 이는 원치 않는 방향
  우리는 이 model을 쓰는 다른 model만 db로 가기를 희망함

  > 기타 사항을 적어줄 수 있는 Meta class 생성

  ~~~ python
  class Timestamped Model
  	created = models.DateTimeField()
  	updated = models.DateTimeField()
  
    class Meta:
      abstract = True
  ~~~
  
  abstract라는 property를 true로 넣어준다
abstract model : model이지만 DB에는 나타나지 않는 model
  대다수의 abstract model은 확장하기 위해 사용
  
- User app에서 models.py를 보면 맨위에 AbstractUser를 import하는 것을 볼 수 있음
  위의 TimeStampedModel도 Abstract를 사용하는 이유임

- Rooms의 model에는 timestamped를 지우고 위와 같이 작성

  ~~~ python
  from django.db import models // already exists
  from core import models as core_models
  
  class Room(core_models.TimeStampedModel)
  
   """ Room Model Definition"""
    
    pass
  ~~~

  Timestampedmodel은 user를 제외한 다른 app에서도 많이 사용될 것

  AbstractUser는 이미 로그인, 가입을 기록하는 기능이 있어서, User는 사용 x
  

## 4.1 Room Model part One



- Room model을 만들어보자
  migration없이 할 수 있는 부분까지 한번에 갈 예정 / field 부터.

- room name 생성 - char field
  필수 입력 사항이므로, blank true나 null true 설정은 하지 않음

- Description 생성 - Textfield

- Country 생성 - choice
   나라를 전부 복붙하기는 힘들고, 아주 좋은 라이브러리 패키지 django countries 이용

  pipenv shell 들어간 상태에서
  `pipenv install django-countires` 입력하여 설치
  설치되는 동안 INSTALLED_APPS 가서 Django-countries 등록 
  config - settings.py - THIRD_PARTY_APPS에 추가하기

  ~~~ python
  THIRD_PARTY_APPS = [
    "django_countries"
  ]
  ~~~

  사용법은, import해주고 :
   `from Django_countries.field import CountryField`

  `country = CountryField()` 로 사용


  ~~~ python
  from django.db import models // already exists
  from django_countries.fields import CountryField
  from core import models as core_models
  
  class Room(core_models.TimeStampedModel):
    
    """ Room Model Definition """
  
  	name = models.CharField(max_length=140)
    description = models.TextField()
    country = CountryField()
    city = models.CharField(max_length=80) // 여기부터 아래에서 진행
    price = models.IntegerField()
    address = models.CharField(max_length=14)
    guests = models.IntegerField()
    beds = models.IntegerField()
    bedrooms = models.IntegerField()
    baths = models.IntegerField()
  	check_in = models.TimeField()
    check_out = models.TimeField()
    instant_book = models.BooleanField(default=False)
    
  ~~~

- 장고에서의 좋은 코딩 방법 (import)
  1) django와 관련된 걸 전부 import
  2) 외부 패키지 import
  3) 내가 만든 패키지 import

- 예를 들어 파이썬에서 import를 하는 경우, 가장 위에다 써준다
  1) python 관련한 것 import 
  2) 다음 django
  3) 외부 패키지(third_party_apps)
  4) 마지막 만든 application import

- 다음으로 city 생성 - char field (패키지 없음)
  옵션 최대글자 80자

- price 생성 - IntegerField

- address 생성 = CharField

- guests, beds, bedrooms, baths 필드 생성 - IntegerField

- Check_in 생성 - TimeField
  dateField, datetimeField와 다르게, TimeField는 0시에서 24시 사이에서 고르는 것

- Instant_book 생성 - BooleanField
  즉시 예약이 되는지

- host 생성 : host는 user이어야 하기에, 연결해야함

- foreign key : Model과 다른 model을 연결하는 방법

  foreign key는 커넥션이 필요하다 > 여기서는 room과 user

  User를 import 하자

  ```python
  from core import models as core_models // already exists
  from users import models as user_models
  ```

  다음 code 입력

  ```python
  ~~
  instant_book = models.BooleanField(default=False) // already exists
  host = models.ForeignKey(user_models.User, on_delete=models.CASCADE)
  ```

  위의 코드를 통해, user와 room이 연결됨
  설명에 제일 좋은 방법은 admin 패널에서 직접 보는 것

  그러므로, admin 패널에 연결해주자

  -> 	admin.py 파일에 decorator와 RoomAdmin class가 적혀있다면,
  연결이 된 것이다

- 마지막으로, auto now auto now add를 알아보자
  datetime에는 좋은 기능이 있는데,
  Auto now를 true해주면 필드가 model을 save 할 때 date랑 time을 계속 기록한다
  auto now add를 true해주면 필드는 model을 생성할 때마다 수시로 업데이트 됨

  ```python
  class Timestampled Model
  	~~~
    
  	created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)
    
    class Meta:
      abstract = True
  ```

  이제 migration 진행
  `Python manage.py makemigrations`
  `Python manage.py migrate`

  python manage.py runserver 로 서버 실행후 Rooms로 들어가면
  room이 없다고 나옴 > 다시해보면
  host와 User가 연결되었다!!!
  


## 4.2 Foreign Keys like a Boss



- 웹을 확인해보면, 위 코드에 작성한 것들이 그대로 적용되었음을 알 수 있다

- Room과 Host를 연결한 것이 가장 중요!
  장고는 User를 수정하는 기능도 제공하는 데 아주 유용하다!
  user를 추가하는 기능도 있다
  
- 이렇게 User를 연결하면 user도 바뀌는게 있는지확인해보자

  > 없음 -> User는 연결에 대한 정보를 갖지 않음

- 일대다 관계
  여기서, ForeignKey는 Many-to-one 관계를 연결해주며,
여러 room은 하나의 user를 가리킴
  
- 데이터베이스로 확인해보면
  room은 필드로 user (id)라는 foreign key를 갖는다
  그리고 그 필드는 user DB에도 존재
  


## 4.3 ManyToMany like a Boss



- room의 admin을 설정해줘야함

- 장고나 파이썬의 작동 방식

  클래스를 가지고, 그 클래스를 string으로 만들어준다 
  장고에 있는 class들이 공통적으로 가지고 있는 method

- \__str__ method
  파이썬이 class를 발견하면, class를 마치 string처럼 보여주는데
  이 method를 파이썬에서 \__str__ 로 표현

- rooms의 models.py 에 작성

  ```python
  ~~
  instant_book = models.BooleanField(default=False) // already exists
  host = models.ForeignKey(user_models.User, on_delete=models.CASCADE) // already exists
  
  def __str__(self):
  	return self.name
  ```

- 다대다 관계 : 여러 entity가 관계를 갖는 것
  룸과 룸에 들어가는 Item (Amenity)는 다대다 관계

- rooms의 models.py에 Item class를 Abstract로 만들어보자
  이후 RoomType class 생성

  ```python
  ~
  from users import models as user_models
  
  class AbstractItem(core_models.TimeStampedModel):
  
  	""" Abstract Item """
   
  	name = models.CharField(max_length=80)
   	
   	class Meta:
   		abstract= True
   		
   	def __str__(self):
   		return self.name
    
  class RoomType(AbstractItem):
    
  	pass
  ```

  Room class에 RoomType 추가해주기

  ```python
  ~
  host = models.ForeignKey(user_model.User, on_delete=models.CASCADE)
  room_type = models.ManyToManyField(RoomType, blank=True)
  ```

   다음으로, makemigrations & migrate 진행

  `Python manage.py makemigrations`
  ` Python manage.py migrate`

- 런서버 실행하기
  `Python manage.py runserver`

- 페이지를 보면, Roomtype이 반영되었지만, 아직 세부사항은 없다
  아직 Room Type을 import 해주지 않았기 때문!

- Rooms의 admin.py 에 가서 다음 코드 작성

  ```python
  ~
  from . import models // already exists
  
  @admin.register(models.RoomType)
  class ItemAdmin(admin.ModelAdmin):
    
    """ Item Admin Definition """
    
  	pass
  
  @admin.register(models.Room) // already exists
  ~
  ```

- 그리고 다시 페이지에서 새로고침을 하면
  roomtype을 추가할 수 있는 버튼이 생성된다

- Admin panel Room Type에서
  Entire place / Private room / Shared room / Hotel room 추가해보기
  ctrl이나 commnad를 누르면서 클릭하면 중복 선택이 가능하다

- 여기서는 예시로 만든 것이고, 사실 room은 하나의 type을 가지므로
  다음 강의에서 foreignkey로 변경 예정

- 이번 강의 정리
  1) rooms 처럼 오직 한 명의 host를 가질 수 있다면
  foreign key !
  2) 여러개를 가질 수 있다면
  ManyTomany !

  

## 4.4 on_delete, Amenity, Faciliy, HouseRule Models



- on_delete 설명
  보여주기 가장 좋은 방법은 admin panel을 보여주는 것

- Rooms app의 model 에서
  `host = models.ForeignKey(user_models.User, on_delete=models.CASCADE)`
   의 의미는
  Room 모델이 host라는 Key로 User에게 연결되었다는 의미

- `On_delete=models.CASCADE` (cascade : 폭포수) 의 의미는
  host(user)가 삭제 될 때, user의 room도 폭포수 효과로 삭제된다는 것을 의미

- on_delete의 다른 행동들은 Django documentation에서
  Foreign key를 검색 후, model field referenced에서
  foreign key를 검색해서 찾아볼 수 있다.
  Ex) protect 등, 보통 cascade 혹은 protect 사용

- On_delete는 오직 foreign key만을 위한 속성, foreign key는 한가지에만 연결되기 때문

- AbstractItem class를 만든 이유
  Roomtype(방 유형)뿐만 아니라, Amenity Type, Facility, House Rule 등 병렬적으로 필요한 Item들이 많기 때문

  ```python
  class RoomType(AbstractItem):
  
  	""" RoomType Object Definition """ // 추가로 작성
  
  	pass
  	
  class Amenity(AbstractItem): // 추가 작성
  	
    """ Amenity Object Definition """
  	
  	pass
  
  class Facility(AbstractItem):
    
    """ Facility Model Definition """
    
    pass
  
  class HouseRule(AbstractItem):
    
    """ HouseRule Model Definition """
  
  	pass
  ```

  

- 어떤 기능을 admin panel에 넣어야 할지 (사용자가 언어추가를 직접 가능하도록 설정),
  코드로만 만들어야 할지(프로그래머가 코드로 만든 언어만 사용가능)
  어떻게 관리할 것인가에 대한 고민 필요

- Room class에서 roomtype 필드 변경해주자
  하나의 룸타입에 여러 룸이 될 수 있으므로
  Foreignkey 사용 / 룸타입을 삭제해도 해당 룸들을 삭제하진 않을 것이기에 on_delete속성 null 설정

- Amenities 필드 설정
  한 방에 여러 amenitiy가 있고, 한 amenity가 여러 방에 들어 갈 수 있으므로 다대다관계

  ```python
  ~
  host = models.ForeignKey(user_model.User, on_delete=models.CASCADE)
  room_type = models.ForeignKey(RoomType, on_delete=models.SET_NULL, null=True) // change
  amenities = models.ManyToManyField(Amenity, blank=True)
  facilities = models.ManyToManyField(Facility, blank=True)
  house_rules = models.ManyToManyField(HouseRule, blank=True)
  ```

  makemigration & migrate 진행

- 웹을 보면 반영은 되었지만, 아직 내용을 추가할 수는 없게 되어있다
  admin에 등록되지 않았기때문

- rooms의 Admin.py에서

  ```python
  ~
  from . import models // already exists
  
  @admin.register(models.RoomType, models.Facility, models.Amenity, models.HouseRule) // 변경
  class ItemAdmin(admin.ModelAdmin):
  	pass
  
  @admin.register(models.Room) // already exists
  ~
  ```

- 위의 코드 작성 후,
  admin panel에서 Rooms administraion에서 Rooms를 보면
  등록 되어 있는것을 알 수 있다

- 장고는 자동으로 위에 등록한 모델들에 s를 붙여주는데,
  Amenitys와 같이 잘못 붙일 수 있다

- Meta class를 이용하여, 다음 수업에서 변경할 계획
  

## 4.5 Meta Class and Photos Model



- Meta class는 모델 내의 모든 class 들 안에 있는 class
  많은 것을 변경해서 설정 할 수 있음

- Option 중 verbose_name_plural 이 있음
  따로 설정하지 않으면, 장고는 기본적으로 verbose_name에 + "s" 를 붙인다

- Verbose_name은 주어지지 않으면 기본적으로 모델의 이름을 사용한다

- rooms의 models.py 에서

  ```python
  class RoomType(AbstractItem):
  
  	""" RoomType Object Definition """ // 추가로 작성
  
  	class Meta:
      verbose_name = "Room Type"
  	
  class Amenity(AbstractItem):
  
  	""" Amenity Model Definition """
  	
  	class Meta:
  		verbose_name_plural = "Amenities"
  		
  class Facility(AbstractItem):
  
  	""" Facility Model Definition """
  	
  	class Meta:
  		verbose_name_plural = "Facilities"
  	
  class HouseRule(AbstractItem):
  
  	""" HouseRule Model Definition """
    
    class Meta:
  		verbose_name = "House Rule"
  ```

- Meta class 에서 할 수 있는 다른 부분
  ordering 변경 가능
  ex) Roomtype을 들어가보면, 처음에는 생성순으로 나열되어있지만 변경 가능

- Documentation 에서 ordering을 검색해서 보면 여러 옵션이 있다

```python
class RoomType(AbstractItem):

	""" RoomType Model Definition """
	
	class Meta:
		verbose_name = "Room Type"
		ordering = ["created"] // 이와 같이 추가 가능
		
```

- 위 코드를 작성하면 생성순으로 정렬된다
- 외에도 meta class 활용 용도는 많다
- Photo part
- Photo model을 생성하자

```python
class Photo(core_models.TimeStampedModel):

	""" Photo Model Definition """
	
	caption = models.CharFiled(max_length=80) // 사진에 달리는 설명 캡션
	file = models.ImageField() // 사진 이미지는 파일을 거쳐서 온다
	room = models.ForeignKey(Room, on_delete=models.CASCADE) // Room은 Photo와 연결되어야함 	

	def __str__(self):
		return self.caption

class Room(core_models.TimeStampedModel): // already exists
~
```

- 위와 같이 작성하면, 문제 발생
- python은 상하 수직으로 코드를 읽어 나가기 때문에, photo에서 foreignkey로 사용하려는 room은 아직 정의되지 않은 상태
- 해결책
  1) Photo class의 위치를 room 아래로 옮긴다
  2) strrings을 하는 방법

 String만 있으면, django는 어떤 모델을 말하고 있는지 알 수 있다

```python
from users import models as user_models // 삭제 > string으로 변경하면 import 할 필요가 사라짐
~

room = models.ForeignKey("Room", on_delete=models.CASCADE) // Room을 "Room"과 같이 string 으로 처리

~

host = models.ForeignKey("users.User", on_delete=models.CASCADE) // Rooms 내에 있는 User를 찾는게 아니라, users 의 User를 가져오는 것이므로, users.User라 작성
room_type = models.ForeignKey("RoomType", on_delete=models.SET_NULL, null=True)
amenities = models.ManyToManyField("Amenity", blank=True)
facilities = models.ManyToManyField("Facility", blank=True)
house_rules = models.ManyToManyField("HouseRule", blank=True)
```

```python
""" Room Admin Definition """
```

와 같이 명확하게 정의를 해주면, 협업할 때에 class를 명확히 해줘서 좋다

- 사진 model을 어드민에 등록하자

  ```python
  @admin.register(models.RoomType, models.Facility, models.Amenity, models.HouseRule)
  class ItemAdmin(admin.ModelAdmin):
  	pass
  
  @admin.register(models.Room) // already exists
  ~
  
  @admin.register(models.Photo)
  class PhotoAdmin(admin.ModelAdmin):
  
  	""" """
  	
  	pass
  ```

  와 같이 작성 후, makemigration & migrate 실행

- 다음 강의는 이어서, review model 을 만들며, 우선적으로 모든 model을 다 만들 계획

