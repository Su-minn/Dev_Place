

# 3. User_App



## 3.0 Replacing Default User

- 객체 상속

- 장고는 user application이 주어져 있다
  하지만 확장이 필요하다.
  ex. 프로필 사진 설정, 프로필 필드, 생년월일, 언어, 환율, super host, gender 등 추가

- config로 가서 settings.py에서 아래 코드 추가 (직접 적기보다는 찾아서 복붙)

  ~~~ python
  AUTH_USER_MODEL = 'myapp.MyUser'
  ~~~

  1) Django Documentation page 접속
  2) AUTH USER 검색
  3) Customizing authentication in Django 로 들어가서
  4) Substituting a custom User model 찾기

  [해당 링크](https://docs.djangoproject.com/en/3.0/topics/auth/customizing/)

  장고에서 커스터마이징하기위해 user model을 덮어쓰기 위해 사용

- 'model' 이란 데이터가 보여지는 모습을 말한다

- model 생성방법

  0) 우선, users 디렉토리의 models.py로 이동

  1) 기존의 Model을 상속 받아서 User라는 이름의 Class 생성

  ~~~ python
  class User(models.Model):
  ~~~

  나중에 rooms, reviews 등에서 위의 class 사용 예정
  지금은, 그 이상 모든 것을 포함한 user 모델을(admin) 만들기 위해
  다른 곳에서 import를 받는다

  맨 위에 다음과 같이 코드 작성하고 User Class 변경

  ~~~python
  from django.contrib.auth.models import AbstractUser
  from django.db import models // already exists 
  
  class User(AbstractUser):
  
      pass
  ~~~

  AbstractUser Class에는 username, email, is_active 등 다 존재

- 이제 장고 유저를 내가 만든 User로 대체
  1) User application을 settings.py에서 설치해야한다
  2) config 디렉토리의 settings.py로 들어가기

- 읽기 쉽게 세팅 변경
  DJANGO_APPS와 PROJECT_APPS를 만들어 준 후, 아래와 같이 코드 작성

  ~~~ python
  DJANGO_APPS = [
    move here in INSTALLED_APPS
  ]
  
  PROJECT_APPS = [
    "users.apps.UsersConfig",
  ]
  
  INSTALLED_APPS = DJANGO_APPS + PROJECT_APPS
  ~~~

  에러가 뜨는데, 이유는 이미 user app이 있는데 다시 만들어서 그렇다

  users에 있는 db를 삭제해 주면 된다

- settings.py의 아래 코드를 from에서 To로  변경

  ~~~ python
  AUTH_USER_MODEL = 'myapp.MyUser' (from)
  AUTH_USER_MODEL = 'users.User' (to)
  ~~~

  myapp은 users이고, MyUser이름은 User로(Class 이름) 만들었기 때문

- 다시 서버를 키면 User라고 불리는 dependency에 에러 발생

  ~~~ python
  python manage.py runserver
  ~~~

  이제는 장고가 User를 인식은 하지만, dependency에 migration이 없는 오류 발생
  다음 강의에서 이 문제 해결 예정



## 3.1 Introduction to the User Model

- 위 에러를 해결하기 위해 migration 파일을 생성하자

  ~~~ python
  python manage.py makemigrations
  ~~~

  Migration 파일이 생성된다

- 이렇게 생긴 파일을 migrate 해주면된다

  ~~~ python
  python manage.py migrate
  ~~~

- 다시 서버를 실행하면 에러가 사라지고 정상 실행이 된다

- 다시 superuser를 만들어보자

  ```python
  python manage.py createsuperuser
  ```

- 서버로 들어가서 로그인 해보면, 정상 로그인이 된다

  그런데 user가 사라져 있음 > default admin 을 다른 model을 대체했기 때문
  다시 admin을 만들어줘야함



---

### 발생 Error

~~~ python
python manage.py migrate
~~~

입력 시에,

~~~ 
django.db.migrations.exceptions.InconsistentMigrationHistory: Migration admin.0001_initial is applied before its dependency users.0001_initial on database 'default'. 
~~~

발생, 결과적으로 migration이 일어나지 않고,

~~~ 
You have 1 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): users.
Run 'python manage.py migrate' to apply them.
~~~

Error로 이어짐

> **해결방법**
> 1) Delete your db. 
> 2) Delete all files on your 'migrations' folder EXCEPT __init__.py 
> 3) Run makemigrations & migrate

출처 : [니꼴라스의 답변](https://academy.nomadcoders.co/courses/637659/lectures/11835420)

---



- 로그인하면 User가 사라져있다!
  
  디폴트 admin을 대체했기 때문
  
  > 그래서, admin을 만들어줘야함

- 그전에, 먼저 User model을 디자인해보자
  0001_initial.py를 보면 이미 많은 필드들이 존재하는데
  AbstarctUser을 class에 추가했기 때문

- User를 더 확장하기 위해, 다음과 같은 코드 작성
  설명은 추후 할 계획

  model을 admin에 연결하기
  : admin.py에 다음과 같은 코드 작성

  ~~~ python
  from django.contrib import admin // already exists
  from . import models
  
  @admin.register(models.User)
  class CustomUserAdmin(admin.ModelAdmin):
  		pass
    
  ~~~

  그리고 다시 접속하면 User가 생김 (admin 패널에 추가된 것)
  User에서 봤던 필드들도 모두 존재

- model에 추가로 작성하면 장고가 다 form으로 맞춰서 만들어줌

- ex) bio - 텍스트필드 로 생성 / 
  참고 - 필드 개념 및 옵션을 모르면 장고 documentation에서 model fields 검색
  models.py 에서 아래 코드 작성

  ~~~ python
  pass // 이 부분 삭제 후
  
  bio = models.TextField()
  ~~~

  저장하고 웹페이지를 리로드 하면 bio column이 없다는 Error 발생

  > migration을 안해줬기 때문임
  > database에게 변화를 알려주기 위해 migration 진행

  ~~~ python
  python manage.py makemigrations
  ~~~

  를 진행하면, default가 없다는 오류 발생
  다음과 같은 코드로 변경하여 해결

  ~~~ python
  bio = models.TextField(default="")
  ~~~

  이 후, makemigrations & migrate 진행하여 적용

- 서버를 재시작하고, admin 패널에 가서 새로고침을 하면 
  Bio 필드가 추가되어 있음

- model에 어느것을 넣든 admin패널에 추가됨
  database에 form으로 만들어줌

> 순서 : 
> 1) 필드 models.py User 클래스에 추가하고 
> 2) 필드를 보여주고
> 3) migration 폴더를 만들고
> 4) migrate 하면
>
> > admin패널에 form 생성됨





## 3.2 First Model Fields

- User를 확장해보자
  참고로, Pass 는 아무 의미가 없다.

- default의 중요성
  Ex. 100만 user가 있다할 때, 디폴트 값이 없다면 새로운 column이 추가되었을때
  데이터베이스가 어떤 값을 가져야할지를 모른다.

  ~~~ python
  bio = models.TextField(default="")
  ~~~

  

- 다른 옵션으로 null 값을 true 처리하는 방법도 존재한다
  -> 비어있어도 괜찮다는 것을 의미한다 / 즉, 비어있는 필드를 허용

  ~~~ python
  bio = models.TextField(null=True)
  ~~~

- Docstring : 파이썬에서 쓰는 표준 형식

  클래스를 작성할때마다 문구를 넣어서 무슨 클래스인지 알려주는 것

  ~~~ python
  class User(AbstractUser)
  
  		""" Custom User Model"""            << 이와 같이 작성
    
    	bio = models.TextField(default="")
  ~~~

  잘 작성되었는지 확인하려면 admin.py 파일에 가서 User에 마우스를 갖다대보면 알 수 있다. -> Custom User Model 이라고 나옴

- Admin.py에 위에 있는 코드 from . import models의 의미
  admin.py와 같은 폴더에 들어있는 models.py를 불러오는 것
  Models.User -> models안에 있는 User 클래스 사용

- User에 사진과 성(gender)을 추가해보자 아래코드 작성

  ~~~ python
  avatar = models.ImageField(null=True)
  gender = models.CharField[max_length=10, null=True]
  bio = models.TextField(default="")
  ~~~

- Pipenv로 pillow를 설치해한다
  pillow는 이미지 관련된 파이썬 라이브러리
  아래 코드 작성

  ~~~ python
  pipenv install pillow
  ~~~

- 이후 makemigrations & migrate 진행

- 다시 admin 패널의 User로 들어가보면 avatar, gender field가 생성되어있음

- Char field vs text field
  char field 는 한줄만 쓸 수 있고, 글자 수 제한이 있음. 대부분 char field로 작성
  text field는 여러줄을 쓸 수 있고 글자수 제한 없음

- Gender에 아무거나 적을 수 있는 상황이기에, 옵션 (choices)을 주자
  char field는 choices라 불리는 커스텀이 가능하다!
  아래와 같은 코드(상수(constant) 코드) 추가

  ~~~ python
  GENDER_MALE = "male"
  GENDER_FEMALE = "female"
  GENDER_OTHER = "other"
  
  GENDER_CHOICES = (
  	(GENDER_MALE, "Male"),
    (GENDER_FEMALE, "Female"),
    (GENDER_OTHER, "Other"),
  )
  
  avatar = models.ImageField(null=True) // already exists
  gender = models.CharField(choices=GENDER_CHOICES, max_length=10, null=True) // change
  ~~~

  GENDER_CHOICES에서 괄호() 를 tuple(튜플)이라고 부른다

  위와 같이 옵션을 주는 경우에는 데이터베이스에 영향을 주지않고 form만

  변경하는 것이므로 makemigration & migrate 할 필요가 없다

- 그런데, 이 상태로 web에서 user를 생성하려고하면
  필수 필드 avatar, gender, bio가 작성되지 않아서 생성할수 없다는 오류가 발생한다

  분명 null=True 옵션을 줬는데 왜 이런 오류가 발생할까?

  >  null은 데이터베이스에서 사용되는 것이고, form에는 blank가 따로 존재한다
  >
  > 그래서 각각 blank=True 를 적어줘야 정상적으로 작동된다

  코드

  ~~~ python
  avatar = models.ImageField(null=True, blank=True)
  gender = models.CharField(choices=GENDER_CHOICES, max_length=10, null=True, blank=True)
  bio = models.TextField(default="", blank=True)
  ~~~

  이러한 옵션에 대한 정보들은 모두 django documentation에 존재한다
  ==필요한 정보들을 알아서 찾아서 활용하는 능력 중요 !==
  


## 3.3 Finishing User Model

- Birthdate (생년월일)와 language, currency 추가해보자
  각각 Datefield 사용하기 / Charfield / Charfield사용하기
  	참고로 DateTimefield는 DateField와 다르다
  	DateTimeField는 시간까지 포함

  다음과 같이 코드 작성

  ~~~ python
  LANGUAGE_ENGLISH = "en"
  LANGUAGE_KOREAN = "kr"
  
  LANGUAGE_CHOICES = (
  		(LANGUAGE_ENGLISH, "English"),
   		(LANGUAGE_KOREAN, "Korean")
  )
  
  CURRENCY_USD = "usd"
  CURRENCY_KRW = "krw"
  
  CURRENCY_CHOICES = ((CURRENCY_USD, "USD"), (CURRENCY_KRW, "KRW"))
  
  avatar ~
  gender ~
  bio ~
  birthdate = models.DateField(null=True) // change from here
  language = models.CharField(choices=LANGUAGE_CHOICES, max_length=2, null=True, blank=True)
  currency = models.CharField(choices=CURRENCY_CHOICES, max_length=3, null=True, blank=True)
  ~~~

- 위의 LANGUAGE_CHOICES에서 
  LANGUAGE_ENGLISH가 데이터베이스로 가고
  English는 admin 패널 form 에서 보여지는 값이다

- Airbnb를 host가 super host인지 볼 수 있게 만들자
  sueprhost : 인증받은 host
  BooleanField 사용 : 참 거짓을 선택하는 Field
  아래와 같이 코드 추가

~~~ python
currnecy ~
superhost = models.BooleanField(default=False)
~~~

작성 후, makemigration과 migrate 진행하면 완벽 적용 완료~!



## 3.4 Falling in Love with Admin Panel

- 장고의 특징 - 여지껏 한 것과 같이
  장고의 룰을 따르면 장고가 알아서 configuration 해줌

- 장고 admin 설정 : admin은 웹마스터 / 프로그래머 용

- Admin.py에 추가하는 방식으로 바꾼다

- django documentation에서 model admin 검색
  The Django admin Site 들어가기

- admin에 model이 나타나게 만드는 두가지 방법
  1) decorator : class위에 있어야 작동함
  그래야 decorator가 알 수 있다
  Ex. 바로 아래 있는 custromUserAdmin으로 User를 컨트롤한다는 것

  ~~~ python
  @admin.register(models.User)
  class CustomUserAdmin(admin.ModelAdmin):
  	pass
  ~~~

  

  기본 틀 : admin.site.register(Author)

  ex) `admin.site.register(models.User, CustomUserAdmin)`
  앞에 모델, 뒤에 클래스를 넣어준다(admin 패널에서 컨트롤 할 수 있는 user 클래스)

  `@admin.register(models.User)` 코드와 동일

- 장고 admin 패널의 옵션

  1) list_display
  : admin의 list page의 변화에 따라 어느 field를 보여줄지 결정

  admin.py 에 다음과 같이 작성

  ```python
  class CustomUserAdmin(admin.ModelAdmin):
  
  	""" Custom User Admin """
  
  	list_display = ('username', 'gender', 'language', 'currency', 'superhost')
  ```
- 각 필드에 따라 user를 필터링 해서 보고 싶다면 다음과 같은 코드 추가

  `List_filter = ("language", "currency", "superhost")`

- rooms는 옵션이 많아서 admin panel이 더 복잡하게 될 것이다
- 장고는 기본으로 user를 위한 admin 패널이 존재하므로,
  그 admin 패널을 사용할 계획 > 추후에 커스텀 admin 패널 사용
- 다음 강의에서는 admin 패널 확장할 것
  

## 3.5 UserAdmin + CustomAdmin

- admin패널을 확장시켜보자
  admin.py 에서 다음과 같이 작성

  ~~~ python
  from django.contrib import admin
  from django.contrib.auth.admin import UserAdmin // change
  from . import models
  
  @admin.register(models.User) // change
  class CustomUserAdmin(UserAdmin): //change
    """ Custom User Admin """
    
    pass
  ~~~

  웹페이지를 확인해보면, 변경된 것을 알 수 있음
  원하던 것들이 확장되었지만
  이전에 model.py에서 추가적으로 만들었던 bio, avatar 등이 적용되지 않음
  UserAdmin은 우리가 만든 User필드를 아직 모르는 상태

- fieldset을 구현해보자 (웹페이지에서 파란색으로 묶인 필드의 그룹이 fieldset)

  admin.py에 아래와 같이 작성

  ~~~ python
  """ Custom User Admin"""
  
      fieldsets = (
  			(
  							"Custom Profile",
    						{
  									"fields" : (
  											"avatar",
  											"gender",
                      	"bio",
  											"birthdate",
                      	"langauge",
                      	"currency",
                      	"superhost",
                    )					
                }
        ),
  )
  ~~~

  fieldsets은 tuple을 가지고 있기에 위와 같은 notation을 가짐

- 위와 같이 저장하면 기존의 필드들이 사라짐
  아래와 같이 코드 추가 변경

  fieldsets = `UserAdmin.fieldsets +` (("Custom Profile", {"fields": ~~~ })) 
  그러면 정상적으로 원하는 결과를 얻게됨

- 장고의 authentication 시스템과 장고의 admin 패널, 장고 fieldset, 확장한 profile등을 모두 가져오게된 것

  > Users model은 거의 구현 완료

  

## 3.6 RECAP OMG!



- 복습 시간!

- sjeon@jeonsumcBookPro Airbnb_clone %                    config를 제외하고 / config는 마스터 폴더
  나머지, convsrsatations, lists, rooms, users 등은 모두 ==어플리케이션== 임
  
> 어플리케이션 : 함수 (function) 들의 집합(group)

users 어플리케이션 안에는
  model과 admin 등이 있고, 이제까지 구현 한 것임

- 장고는 우리가 작성한 코드를 사용함 / 우리가 장고를 사용하는 것과는 분명 다름

- 장고는 데이터베이스와 통신함
  장고는 ORM을 탑재하고 있음

  > ORM(Object Relational Mapping) : 
  > 우리의 파이썬 코드를 SQL문으로 바꿔서 데이터베이스가 알아듣도록 만드는 것

- models.py에 넣은 코드들을 장고가 알아서 데이터베이스 테이블로 바꿔줌

- model에는 fields로 이루어짐
  char field, image field, boolean field 등등
  필드는 이름 뿐만이 아니라, 더 편리하게 만들어주는 기능을 함
  ex. 데이터의 유효성 검사

- 모든 필드는 데이터 베이스로 들어가게됨 / 장고가 파이썬 코드를 SQL로 바꿔줌

  - 작업한 모델들은 admin 패널에서 볼 수 있었으며, 이 admin panel은 admin.py에서 작업

  - admin.py에서 model을 가져오려면 아래와 같은 register가 필요하고,

    ```python
    @admin.register(models.User) / decorator
    class CustomUserAdmin(UserAdmin)
    ```

    model을 register 해주려면 class가 필요
    class는 기본적으로 모델을 조정할 수 있다

- decorator은 그 다음에 어떤 코드를 쓰든 그걸 읽어 내려가는데,
  Class 위에 써주면 class 위치를 알게 되므로 위의 경우 models.User를
  CustomUser에 적용하게됨

- decorator 대신 다른 방법

  ```python
  admin.site.register(models.User, CustomUesrAdmin)
  ```

- admin.py에서 admin 패널의 구성을 바꿀 수 있다.
  그리고 장고는 이미 정해진 방식대로 움직이기 때문에
  register된 Model과 class를 찾은 다음 특정 키워드를 찾는다

- fieldset : admin 패널 화면상에서 파란색 바로 묶인 영역(파란 섹션)을 의미하며
  fieldset안에 다른 것을 추가할 수도 있다

- fieldset 바깥부분도 바꿀 수 있는데
  list_display : 보이는 리스트를 변경하기
  list_filter : list의 필터 변경

- class CustomUserAdmin(==UserAdmin==)
  위와 같이 들어가는 건 파이썬 상속의 개념
  이미 만들어둔 기능을 그대로 복사 붙여넣기 해준다고 생각

- 장고가 우리가 만든 폴더를 인식하려면
  settings.py 에 등록해줘야한다

- 코드를 간단히 하기위해 약간의 수정이 있었음

  ```python
  INSTALLED_APPS = DJANGO_APPS + PROJECT_APPS
  ```


  DJANGO_APPS는 디폴트로 주어진 admin 패널, auth, sessions 등

  PROJECT_APPS가 우리가 쓰려고하는 어플리케이션

- ==새로운 코드를 추가해보자==

  ~~~ python
  DJANGO_APPS = [
    "~~"
  ]
  
  THIRD_PARTY_APPS = [] // change
  
  INSTALLED_APPS = DJANGO_APPS + PROJECT_APPS
  ~~~

  THIRD_PARTY_APPS에는 나중에 다른 사람이 만든 applications를 넣을 공간 / 일단은 비워두기

- settings.py에서
  우리가 만든 User를 사용하기 위해 맨 마지막 부분 코드에

  ```python
  AUTH_USER_MODEL = "users.User"
  ```

  을 바꿔줬음

- 장고 User를 사용할 때는 바꿔줄 필요 없는 부분이다
  application마다 필요로하는 user model이 다를 수 있으며, 장고에서 기본으로 주는 User를 사용할때는 그대로 사용
  기본적인 것은 충분히 다 존재 / 대부분 수정할 필요 없음

- ==추가 진행 사항==

- Db.sqlite3 파일 (데이터베이스) 삭제하자

- \__pychache__ 폴더도 존재하면 삭제하기

- migrations 폴더의
  0001 / 0002 ~ 와 같은 파일들도 모두 삭제

- 다음 시작에 migration 없이 clean한 User model 상태로 시작할 예정
  다음에 이유를 알게 될 것

- Models.py 파일에 있는
  필드들의 null=True 코드도 모두 삭제
  ==date 필드==는 `(blank=True, null =True)` 로 두기

  textfiled의 default는 삭제

- db가 다시 생겼다면 재 삭제

- migrations는 항상 적게 유지하는게 좋다
  수업에서는 field와 admin을 보여주기위해 여러번의 Migration을 진행하였으나,
  Model 만들 때 migration은 한번 해주는게 좋다

- 그리고 다시 아래의 코드 makemigration & migrate 진행 후,
  데이터베이스를 삭제하며 super User가 사라졌으므로 재생성

  ```python
  python manage.py makemigrations
  python manage.py migrate
  
  python manage.py supercreate
  ```

   