# Python Web Scraping





## 웹 스크래핑이란



### 웹 스크래핑 과 크롤링



- 크롤링

  - 크롤링이란 단어는 웹 크롤러(crawler)라는 단어에서 시작한 말이다.
  - 크롤러란 조직적, 자동화된 방법으로 월드와이드 웹을 탐색하는 컴퓨터 프로그램이다.
  - 크롤링은 크롤러가 하는 작업을 부르는 말로, 여러 인터넷 사이트의 페이지(문서, html 등)를 수집해서 분류하는 것이다.
  - 대체로 찾아낸 데이터를 저장한 후 쉽게 찾을 수 있게 인덱싱한다.

- 스크래핑

  - 스크래핑이란 HTTP를 통해 웹 사이트의 내용을 긁어다 원하는 형태로 가공하는 것이다.

    쉽게 말해 웹 사이트의 데이터를 수집하는 모든 작업을 뜻한다.

    크롤링도 일종의 스크래핑 기술이라고 할 수 있다.



## HTML



- Hyper Text Markup Language
- VScode에서 extension 추천
  - open in browser
  - VScode 안의 코드 바로 브라우져에서 열도록 해줌





### html 문법

- element
  - 생성 방법
    - 꺽쇠에 포함되는 부분이 element
    - 1) <html/>
      - 내용 없이 바로 닫을 때
    - 2) <html> 내용 </html>
      - 슬래쉬로 내용을 닫아준다고 생각
      - 여는 태그와 닫는 태그로 구성
    - 3) <input type ="password">
      - input 태그와 같이 닫는 태그가 없는 경우도 존재 
  - 구성 요소
    - 태그 이름
      - html, head, body와 같이 시작하는 부분 (용도)
    - attribute
      - element의 세부 속성
      - ex) input 태그의 type, value
- 태그 종류
  - head, body
    - 뼈대 역할
  - input
    - type attribute에 해당하는 내용 넣어줌
  - a (anchor - 링크)
    - href = "주소" attribute에 해당하는 주소로 이동
- 태그 사이의 관계
  - 부모 자식 관계
    - 상위 태그를 부모 태그
    - 하위 태그를 자식 태그라고 한다
  - 같은 depth에 있는 이전 노드, 다음 노드는 서로 형제 관계 라고 한다

- 예시

```html
	<html>
		<head>
      <meta charset="utf-8">
			<title>홈페이지 이름</title>
		</head>
		<body>
			<input type="text" value="아이디를 입력하세요">
			<input type="password">
			<input type="button" value="로그인">
      <a href="http://google.com">구글로 이동하기</a>
		</body>
  </html>
```

- 한글이 깨지는 경우
  - meta 태그로 utf-8 추가해주기
- html 추가 학습 필요시, [w3schools 활용](https://w3schools.com)





## XPath



- XPath란 XML Path Language를 의미

  - XPath는 XML 문서의 특정 요소(element)나 속성(attibute)에 접근하기 위한 경로를 지정하는 언어입니다.

  - XPath는 XML 문서의 일부분을 선택하고 처리하기 위해 만들어진 언어입니다.
  - html 내에 겹치는 태그와 내용이 많을 때 명확하게 구분짓고 지칭하기 위한 path
  - 약 200개 이상의 내장함수를 포함하고 있다

- 예시

  - /a학교/1학년/1반/학생[2]
    - a학교의 1학년 1반 2번째 학생 부르기
  - 문법을 사용하면 더 간단하게 줄일 수 있다
    - //*[@학번="1-1-5"]
      - 위의 학생의 학번이 1-1-5로 유니크하다면 문법을 사용하여 간단하게 표시할 수 있다
  - 예시 설명
    - / (슬래시) 한개
      - 현재 위치한 곳으로부터 한단계 아래 부분 의미
    - // (슬래시) 2개
      - 현재 위치로부터 하위 모든 element 포함
    - \* (asterisk)
      - 'element에 상관없이 모든 element에 관하여' 의 의미
    - @ (at)
      - 속성을 의미

- 실습 - 웹페이지에서 xpath 추출하기

  - 네이버 웹페이지에서 개발자 도구 켜기
  - Select mode 켜기 (Shift + Command + C)
  - 원하는 부분을 웹페이지에서 클릭한 후, 해당하는 코드 부분으로 가기
  - 오른쪽 마우스 클릭 후, Copy - Copy Xpath 클릭
    - `//*[@id="account"]/a` 가 복사됨을 확인할 수 있다
  - Copy - Copy full Xpath를 통해 축약되지 않은 Xpath도 추출할 수 있다
    - `/html/body/div[2]/div[3]/div[3]/div/div[2]/a`





## Chrome



- 구글에서 만든 브라우저
- 스크래핑 시에 유용한 도구
- 다운받아서 사용해야함





## Requests



