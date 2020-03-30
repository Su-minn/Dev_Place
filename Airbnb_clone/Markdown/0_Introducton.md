# 0. Introduction

---



## 0.0 Introduction

- 에어비앤비의 주요 기능등을 클론하는 강의
- 예약, 달력기능, 후기 기능, 검색 기능, 구글맵 활용, 필터 기능 등을 다룸
- 장고(Django)를 활용할 계획
- 디자인은 tailwindCSS [utility-fisrt CSS 프레임워크] 활용 
  / 클래스가 많고, 사전에 디자인 되어있음

---



## 0.1 Theory Requirements

- html, css, 약간의 Python이 선행 조건, 약간의 Javascript
- 대부분 파이썬 사용 -> Function, classes, inheritance 등의 개념 필요
  장고와 파이썬은 객체지향을 강하게 따르므로, 클래스와 상속 중요
- 장고의 웹프레임워크는 대부분이 클래스와 상속으로 이루어짐
  부족하다면, 무료강의 참고
- html과 css 에서는 class, id, flex box 정도의 개념 필요

---



## 0.2 Software Requirements part One

- 컴퓨터에 깔려있어야하는 프로그램

- 코드 에디터 : VS code
  파이참은 파이썬만을 위한 IDE로 VS code 에서 파이썬을 지원하는 것보다 더 많은 부분을 지원하지만, 무료가 아님. 
  Pycharm 에는 두가지 버전이 존재하는데, 
  Community version 은 python 만을 지원하며 무료인 반면,
  Professional 버전에서만 장고를 지원하고, 이 버전은 무료가 아님.

  > 결론 : 장고지원은 Pycharm Professional 버전이 VS code 보다 좋지만,
  > 비용이 드므로 무료 버전인 VS code 를 사용한다

---



## 0.3 Software Requirements part Two

- 파이썬 설치 필요
  콘솔에서 python 을 입력했을 때, 설치가 되었다는게 나와야함
  python 2버전이 아닌 3버전을 설치해야함
- 맥에서는 이미 기존에 python 2가 설치되어있고, python2가 필요한 프로그램들이
  있으므로, 삭제하지말고 독립적으로 python3를 설치해야함
- 콘솔에서 python이라 쳤을때, 3가 안나온다면 python3 입력
- 파이썬을 홈페이지에서 받는 것보다, [brew](https://brew.sh/index_ko)로 설치하는 것 권장
  brew install python 검색

---



## 0.4 Django vs. Flask

- Flask, Pyramid 는 짧은 시간에 만들 수 있지만, 작고 가벼워서 많은 기능을 제공하지는 않음

  매우 미니멀하고 간결함

- 반면 장고는 거대한 프레임 워크, 많은 유틸리티 존재

- Flask 는 로그인, 로그아웃, 관리자 패널 등등을 모두 다시 만들어야함

- 반면 장고는 Django 가 이미 그러한 부분을 갖추고 있음
  모든 웹 애플리케이션에 공통적으로 쓰이는 것을 이미 만들어둠
  예로, 관리자 패널, 사용자 인증 이미 존재, 이미 만들어진 Form 도 존재
  아주 빠르고 신속하며, 커스터마이징도 쉬운편

- 장고는 큰 상자와 같아서, 안에 있는 것을 배워야함
  Flask 는 빈 상자와 같아서 채워넣어야함. 작은 기능을 구현할 때 좋기도 하다 
  (매우 짧은 코드로 서버 구현 가능)

---



## 0.5 Django vs. React

- 프론트엔드에서 React가 아닌 Django를 쓰는 이유

  1) 모든 프론트엔드에서 react를 쓸필요가 없기 때문
  2) Django의 거대한 템플릿 파트를 학습하기 위해

  ex) Django 사용 : 파이어폭스, 인스타그램, 핀터레스트 등
  인스타그램과 핀터레스트는 장고 프레임워크를 사용하고 있지는 않음 (백엔드 API 만 사용)

- 많은 상호작용이 필요하면 react 를 쓰지만 아닌 경우에는 안써도 되는 경우 존재한다.

> 결론 : 더 Django 에 대한 깊은 학습을 위해, 프론트엔드, 백엔드 모두 Django 로 진행!

---

