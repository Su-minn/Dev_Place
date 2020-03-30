

# 1. Environment_Setup



## 1.0 The Package installer for Python

- Pipenv 설치해야하는 이유
  Django 를 홈페이지에서 알려준대로만 설치하면 좋지 않음

  1) NodeJS를 안다는 가정하에 설명
  파이썬에는 공식 npm 이 없음 pip가 있으나 pip는 전역으로 설치해버림 -> 이는 좋지 않다

  pipenv 는 파이썬을 위한 npm + package.json 이라 볼 수 있음
  이를 사용하면 Django를 해당 프로젝트 환경에만 설치할 수 있음

  > 결론 pip 대신 pipenv를 사용해라

  2) NodeJs 모르는 경우
  서로 다른 버전을 사용하는 Django 프로젝트가 있을 수도 있으니, pip로 글로벌하게 설치하는건
  좋은 것이 아님
  즉, bubble (비눗방울) 처럼 각각 분리해서 지역적으로 설치해야함



## 1.1 Meet Pipenv

- 파이썬계의 npm 과같은건 pipenv

- pipenv 는 버블을 만들어준다

- 맥에서는 brew로 설치할 것

  ~~~ bash
  brew install pipenv
  ~~~

- 설치되었는지 확인하기
  터미널에서 pipenv를 쳤을 때, 설명이 나오면 됨


## 1.2 Creating our Env and Installing Django

- 폴더 하나 생성하기 ex. Airbnb-clone

- 폴더에 들어가서 pipenv 를 사용해서 독립된 개발환경(버블)을 만든다
  장고 등을 모두 여기에 설치

- pipenv 에 파이썬을 사용한다고 알려줘야함 (파이썬 3.7 기반 환경)
  장고는 파이썬 3에서 지원

  ~~~ bash
  pipenv --three
  ~~~

- 가상환경이 만들어지게 됨
  vscode 로 확인해보기

  ~~~ bash
  code .
  ~~~

  위 코드가 실행되기 위해서는 VScode 에서 간단한 [환경설정](https://medium.com/@han7096/visual-studio-code-%ED%84%B0%EB%AF%B8%EB%84%90%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0-9fc64fa6d608)이 필요하다

- VScode 에 들어가서 pipfile이 생성되어있으면 성공

- 현재는, 가상환경 bubble을 만들었으나, 그 버블 밖에 있으므로,
  버블 안에 들어가서 무엇인가를 설치하고, 환경 설정을 해주어야 함
  버블 안으로 들어가기 위한 커맨드

  ~~~ bash
  pipenv shell
  ~~~

- 버블의 안에 장고 설치
  터미널에서 장고 2.2.5 버전 설치

  ~~~ bash
  pipenv install Django==2.2.5
  ~~~

- pipfile 을 확인해보면 장고가 추가되는 것을 볼 수 있음

- 잘 설치되었는지 확인

  ~~~ bash
  django-admin
  ~~~

  입력했을 때, 문구들이 출력되어야함
  

## 1.3 Creating the Github Repository

- git repository 생성하기

  깃헙의 create new repository에서 airbnb-clone 이름으로 생성
  Public으로 생성하고, 초기화하지않고 생성후 URL 복사

  ~~~ bash
  git init
  git remote add origin [URL]
  git add .
  git commit -m "~~~"
  touch README.md
  touch .gitignore
  ~~~

  README.md 파일에 내용 적기

  #Airbnb Clone
  Cloning Airbnb with Python, Django, Tailwind and more ...

- .gitignore : github에 올리지 않고 싶은 파일 (보안, 굳이 필요없는 파일 등)

  .gitignore python 을 검색하 디폴트로 쓰이는 내용 복사
  Mac은 추가적으로 .gitignore에다가 .DS_Store 추가


## 1.4 Testing the Bubble

- 버블 작동 방식 알아보기
  새로운 탭에서 django-admin 을 실행하면
  Command not found 가 뜬다

  > 어떤 콘솔이든 간에 django로 무엇인가를 하려면 먼저, pipenv shell 부터 실행해야함

- Which django-admin 을 치면 어디에 있는지 나온다

- 창을 완전히 껐다면, pipenv shell 을 다시 해주어야함
  작업 전에 > 항상 django-admin을 확인해야함

- ~~~ bash
  exit
  ~~~

  을 입력해서 나갈 수도 있다