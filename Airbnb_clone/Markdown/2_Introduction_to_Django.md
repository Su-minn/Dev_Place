

# 2. Introduction_to_Django



## 2.0 Creating a Django Project

- Django는 django-admin에 startproject라는 명령어를 갖고 있다
  프로젝트 생성할때 사용

- Documentation 에서는

  ~~~ bash
  django-admin startproject mysite
  ~~~

  로 프로젝트를 만들라고 설명되어있지만,
  튜터리얼 내지 초심자 용이다.
  규모를 확장하기에는 별로이기 때문.

- 실제 장고 애플리케이션을 다루기 위한 최상의 방법

  1) 작업할 디렉토리로 들어간다

  2) 해당 커맨드 실행

  ~~~ bash
  django-admin startproject config
  ~~~

  > 결과로, 디렉토리 2개가 생성된다

  3) 생긴 config 라는 디렉토리(바깥 디렉토리)를 Aconfig로 바꾼다 
  (밖으로 꺼낼 때 중복이름 피하기 위해)

  4) Aconfig 내의 config와 manage.py를 디렉토리 밖으로 꺼낸다

  5) 이 후, Aconfig 는 삭제

  > 결과적으로, 최상위폴더에 config 디렉토리와 manage.py가 존재

  6) Manage.py 를 선택했을때, 오른쪽 아래에 여러 알림들이 뜨지 않는다면,
  extension에서 python 설치할 것

  7) VScode 왼쪽 아래에 파이썬 버전이 표시되어있음 
  오른쪽 아래 알림창의 Select Python Interpreter 클릭해서 3.7의 pipenv가 있는 버전 선택

  

## 2.1 Setting up VSCode and Python Like a PRO

- 파이썬은 컴파일 언어가 아니다

  컴파일언어 컴파일하며 프로그램이 시작되기 전에 에러를 잡아준다
  파이썬은 runtime 언어이기에 에러있으면 터진다.
  그래서 Linter를 사용하게 됨

- Linter 는 미 에러가 생길 부분을 감지해준다.
  또한, 파이썬에서 널리쓰이는 관습을 잡아준다
  python pep > 파이썬 코드를 쓸 때의 권장사항 (PEP8 - 스타일 가이드)

- 권장하는 Linter flake8
  1) 오른쪽 아래 Select Linter에서 flake8 선택
  2) settings.json 에 linting 관련된 내용이 생긴다
  flake8 활성화, Linting 활성화

- Pipfile을 열어보 [dev-packages]와 [packages]라는게 생겨있다

  Packages는 웹 애플리케이션이 동작할때 필요한 패키지
  Dev-packages는 개발자가 개발할때만 필요한 패키지

- Linter는 에러를 알려주고, Formatter는 자동으로 수정 포매팅해줌
  추천하는 formatter 는 black

- 오른쪽 아래의 Use black을 클릭하거나, piping install black 입력

  > 자동으로 되는 것들은 모두 python extension이 작동하기 때문
  >
  > 설치가 완료되면 json에 formatting.provider 이 black으로 되어있음
  > 안되어 있다면 json에 아래 코드 추가

  ~~~ json
  "python.formatting.provider": "black"
  ~~~

  flake8 + black은 최고의 조합
  이제는 저장 (커맨드 + s)를 누르면 아래에 자동 Formatting 되는 표시가 나옴

  만약 나오지 않는다면 manage.py에서 왼쪽 아래 설정에 들어가서
  format on save를 검색한 후 Format on Save 에 체크박스 표시할 것

  만약 없다면 settings.json에 아래 코드 추가

  ~~~ json
  "editor.formatOnsave":true
  ~~~

- config 디렉토리에 Settings.py에 들어가보면
  빨간줄이 있고, 왼쪽 탐색기에는 숫자 4가 있음 > Linter가 오류 표시해주는 것

- 여기에 뜨는 오류는 라인의 길이가 길다는 에러 이지만, 예전에 스크린 크기가 작았을때만
  의미있고, 현재는 크게 불필요 (flake8(E501))

- Linter의 설정을 수정하기위해서는, settings.json에 들어가서 아래 코드 작성

  ~~~ json
  "python.linting.flake8Args": ["--max-line-length=88"]
  ~~~

  하나 남은 에러는 길이가 91이라서 그런 것, 어쩔수 없는건 그냥 두자
  

## 2.2 Falling in love with Django part One

- Manage.py 추후에 설명

- Config 안에는
  Init.py : 파이썬에 필요한 파일, 장고 관련된 파일 아님
  새폴더 만들때 파이썬에서 그 폴더의 파일들을 써야할 때는 
  항상 그안에 \_\_init\_\_.py를 둬야함 / import 라 생각하면 된다.
  즉, init.py 이 있어야, manage.py에서 "config.settings"와 같은것을 사용 가능

  Settings.py 는 애플리케이션을 만드는데 필요한것들이 담겨있음
  장고가 제공한 것
  ex. Admin, contenttypesemd, 템플릿, 데이터베이스 등등
  Settings 에 있는 코멘트와 링크들은 모두 중요

- Django Documentation 문서 사이트들 중 단연코 최고라 생각
  참고할 만한 자료가 매우 친절하게 다 존재


## 2.3 Falling in love with Django part Two

- manage.py의 명령어를 실행해보자

  코드 위에 마우스를 올려놓고 command 키를 꾹 누르면 
  함수가 정의가 된 곳으로 갈 수 있다 > IntelliSense 라는 기능
  python extension 설치되어있으면 사용가능하다
  파이참에서는 더 완벽하게 작동

- 1) pipenv shell을 실행시켰는지 확인
  2) 아래 코드 입력

  ~~~ bash
  python manage.py runserver
  ~~~

  3) 주는 주소로 들어가면 > 성공적으로 설치되었다 라고 나옴
  4) 웹사이트가 만들어진 것이다 축하
  settings 에서 저장을 하면 콘솔창에서 재시작됨

- db.sqlite3 이라는 파일은 개발용 데이터베이스이다 > 자동적으로 생성됨

- 아까 나온 주소창에 /admin을 덧붙이면 에러페이지 및 관리자 내용들을 보여준다

- 서버 끄는법은 " Ctrl + Z"

- 서버가 구동하지 않는 상태에서 아래 코드 입력

  ~~~ bash
  python manage.py migrate
  ~~~

  이후 서버 재실행

  ~~~ bash
  python manage.py runserver
  ~~~

  이후 다시 서버에 들어가서, /admin 붙인 후 새로고침을 하면

  Django administration 페이지가 뜬다 > 성공적으로 작동된 것

- 이 페이지는 위의 runserver 코드를 입력해야만 열린다

- Error: That port is already in use. 가 나오는 경우

  ~~~ bash
  sudo lsof -t -i tcp:8000 | xargs kill -9
  ~~~

  입력해서 kill that port !! 후 재 연결한다 ~

- 관리자 패널 로그인 하기

  ~~~ bash
  python manage.py createsuperuser
  ~~~

  1) 계정 설정하기
  2) 다시 서버에 들어가서 만든 계정으로 로그인
  3) 관리자 패널 사용 가능


## 2.4 Django Migrations

- DB SQlite 삭제 > runserver red text 뜨도록하기위해

  이상태로 다시 runserver 를 작동하면 오류가 뜬다

- 다시 장고코드를 이용해서 작성할 계획

- 오류가 뜨는건 데이터베이스와 장고가 연동되지 않았기 때문

- SQLite3는 테스트할때나, 소규모 프로젝트 시에 많이 사용함
  무엇을 따로 설치할 필요 없기 때문

- 반면 PostgreSQL과 같은 SQL system은 보다 전문적이고 백업기능 존재

- 장고는 Migration system을 가지고 있음
  데이터 베이스의 경우, 하나의 상태에서 다른 상태로 바꾸는 것을 의미

- 아래의 코드를 작성하면 장고는 데이터 models을 확인하고
  Migration 파일을 생성함

  ~~~ bash
  python manage.py makemigration
  ~~~

- 장고의 엔진 동작 방식
  일부 데이터 유형을 반경하고, 장고를 사용해서 migration
  그 다음 장고를 사용해서 데이터베이스를 업데이트
  먼저 migration을 생성하고 migrate한 / SQL이 필요없어짐
  장고와 동일한 데이터 유형을 동기화하기 위해 데이터베이스를 업데이트 하는 코드

  ~~~ bash
  python manage.py migrate
  ~~~

  결과적으로, 데이터베이스가 장고의 기본 데이터베이스 정보를 갖게 됨
  장고는 기본적으로 갖고있는 login, session 등 들이 있기 때문

  

## 2.5 Django Applications



## 2.6 Creating the Apps!



## 2.7 Explaing the Apps



