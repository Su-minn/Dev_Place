# 2. Building a Job Scrapper



## 2.0 What is Web Scrapping



- Web Scraping
  : 웹 상의 데이터를 추출하는 것
  - ex) 페이스북에 기사 url을 복사 붙여넣기 하면
    url로부터 제목과 상단 첫 이미지를 가져와서 페이스북 preview를 보여줌
  - web index mining, data mining 등이 유사한 표현으로 쓰임 



## 2.1 What are We Building



- 진행하려는 프로젝트
  : Scrapper를 통해, 구인사이트 Indeed와 Stackoverflow에서  
   Python 개발자를 찾는 일자리 관련 정보들을 모두 엑셀 시트에 옮기기



## 2.2 Navigating with Python



- 프로젝트는 repl.it 으로 진행하기로 하자

- 프로젝트의 알고리즘을 분할하여 생각해보자
  1) 정보가 있는 웹사이트의 파이썬을 사용하여 url에 접근
  2) 페이지가 몇개나 존재하는지 알아야한다
  페이지에 하나씩 들어가서 접근해야하기 때문

- Indeed에서 Advanced Job Search (고급 검색)에 들어가서  
  맨 아래 Display의 results per page를 50으로 바꾸자  
  이 결과 나온 url을 사용할 예정

- Indeed와 stackoverflow에 있는 정보들을 가져올 예정인데,  
  Indeed의 정보를 모두 가져온 후, stackoverflow의 정보를 가져오는 식으로 진행할 예정이다  

- 이제 코드를 작성해보자

- 파이썬 내장 모듈인 urllib를 통해서도 url을 가져올 수 있다  
  하지만, 더 강력한 모듈을 온라인 다운을 통해 사용할 수 있고,  
  그 모듈이 requests !

- requests 모듈을 import하자  
  requests  
  : 파이썬에서 요청을 만드는 기능을 모아 놓은 것  
  사용하기 위해서는, 다운로드가 필요하다

- repl.it 에서 다운로드를 받는 방법은, 일반적인 방법과 조금 다르다  
  왼쪽의 package에 들어가서 requests라는 package를 검색하고  
  두번째에 있는, requests: Python HTTP for Humans 클릭 후, + (추가 버튼)을 눌러서 설치  

- 설치가 완료되면, main.py로 이동하여 requests를 import

- requests의 자세한 사용법은 구글링하면 친절하게 설명되어 있다
  지금은 이 [깃허브 링크](https://github.com/psf/requests)를 참고하며 진행해보자

- ```python
  import requests
  
  indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
  
  print(indeed_result)
  # 테스트를 위한 print
  # result : <Response [200]>
  
  print(indeed_result.text)
  # result : 해당 url의 html 전부를 가져오게 된다
  ```

- requests.get 처럼 .을 붙여서 사용하는 것을 method라고 함

- method : object 안에 있는 function이며, 추후 자세히 설명할 예정이다.

- `print(indeed_result.text)` 의 결과로 나온 html에서 정보를 추출할 예정  
  원하는 정보는 '페이지의 숫자'들이다

- 수동으로 가져오면 많은 시간이 걸리기에, beautiful soup라는 library를 활용할 예정이다  

-  Beautiful Soup : html에서 정보를 추출하기에 매우 유용한 package

- 설치하기위해 repl.it의 package에 들어가서 'beautifulsoup4' 검색 후  
  screen-scraping library 다운  
  일반적인 다운로드 방법은 `pip install beautifulsoup4`

  

## 2.3 Extracting Indeed Pages



- beautiful soup에서 Documentation을 확인해보자  
  [beautiful soup 페이지](https://www.crummy.com/software/BeautifulSoup/)에서 [Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 클릭  
  [한국어 번역 페이지](https://www.crummy.com/software/BeautifulSoup/bs4/doc.ko/)도 존재한다

- Quick start를 보면 html doc이 있는데, html은 우리가 이전에 이미 가져온 상태이다

- code 작성 시에, beautifulsoup를 import 하자  
  여기서 soup는 data extractor (데이터를 추출하는 역할)이다

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
   
  indeed_soup = BeautifulSoup(indeed_result.text, "html.parser")
  
  print(indeed_soup)
  ```

- beautiful soup documentation을 참고하며 진행하면,  
  soup.title로 제목을, soup.p로 paragraph를 찾을 수 있고,  
  `soup.find_all('a')` 와 같이 find_all() 안에 들어있는 부분의 리스트를 찾게 할 수도 있다  
  여기서 'a'는, anchor 요소를 모두 찾으라는 의미이다

- find_all을 이용하여 원하는 부분을 추출할 예정이고, 지금은 페이지번호를 가져올 예정이다

- indeed web site에서 개발자도구 (option + command + i) 를 켠 후, 페이지 하단에 있는 page number 목록의 코드를 확인하면  

  ```html
  <div class="pagination" onmousedown="polka(event);">
  ```

  와 같이 pagination의 Class name을 가진 Div로 되어있음을 알 수 있고,  

  이 안에는 anchor 링크들이 요소로 들어있다

- 이 내용 들을 pagaination 이라는 변수 안에 넣어보자  
  soup.find 를 사용 시에, name과 attribute를 적어줘야하는데  
  지금의 경우 name은 div이고, attribute는 class이름이 pagination이기에 아래와 같이 코드를 작성한다

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
   
  indeed_soup = BeautifulSoup(indeed_result.text, "html.parser")
  
  pagination = indeed_soup.find("div", {"class" : "pagination"})
  
  print(pagination)
  # result : pagination에 관한 내용의 결과 확인 가능
  ```

- cf) print는 작성한 코드에 대해 결과물이 잘 나오는지,  
  디버깅을 위해 확인하는 용도로 사용  
  원하는 결과가 나왔다면 지운 후 다음 코드 작성

- pagination안의 링크인 'a'(anchor)들을 찾아서 pages 변수 안에 넣어준다 

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
   
  indeed_soup = BeautifulSoup(indeed_result.text, "html.parser")
  
  pagination = indeed_soup.find("div", {"class" : "pagination"})
  
  pages = pagination.find_all('a')
  
  print(pages)
  # result : <a>와 관련된 내용의 결과가 잘 나옴을 확인할 수 있다
  ```

- <a> 태그 안에 있는 내용을 개발자도구를 열어서 확인해보면   

  ```html
  <a href="~~" ~~> 
  	</a><span class="pn">2</span>
  </a>
  ```

  와 같이 span으로 구성되어있는 것을 알 수있다

- 리스트에 있는 각 anchor의 span을 찾아보자

- indeed_soup에서 find를 이용하여 찾은 결과를 pagination 변수에 넣어줬고,  
  이 pagination 변수에서 <a>에 해당하는 리스트를 만들어서 pages 변수에 넣어준 상태이다

- 이 결과에서 span만 출력해보면 다음과 같다

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
   
  indeed_soup = BeautifulSoup(indeed_result.text, "html.parser")
  
  pagination = indeed_soup.find("div", {"class" : "pagination"})
  
  pages = pagination.find_all('a')
  
  for page in pages:
  	print(page.find("span"))
  # result : <span class = "pn">2</span>
  # <span class = "pn">3</span>
  # <span class = "pn">4</span>
  # <span class = "pn">...</span>
  # <span class = "pn">20</span>
  # <span class = "pn"><span class="np">Next </span></span>
  ```

- 여기서 맨 마지막 Next 부분을 지워주기 위해 다음과 같이 작성

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
   
  indeed_soup = BeautifulSoup(indeed_result.text, "html.parser")
  
  pagination = indeed_soup.find("div", {"class" : "pagination"})
  
  pages = pagination.find_all('a')
  spans = []
  for page in pages:
  	spans.append(page.find("span"))
  spans = spans[:-1]
  ```

  - spans라는 빈 array를 생성한 후에, 마지막에 있는 next를 제외한 span들을 붙여보자
  - index -1은 마지막에서부터 시작해서 첫 item을 나타낸다
  - [a:b]는 리스트의 a번째 index 부터 시작해서 b전까지의 (b 포함 x) 내용을 의미한다
    ex) spans[0:-1]   
    첫번째 (index : 0) 부터 마지막 전(마지막 index 포함 x)까지의 내용을 가리킨다  
    [:-1] 와 같이 앞 부분이 생략되면 처음부터와 같은 의미이다   
    [0:-1] = [:-1]

- 여기까지하여, indeed 페이지를 성공적으로 추출하였다!

   

## 2.4 Extracting Indeed Pages part Two



- 간단하게 이전 내용을 복습해보면,  
  - soup : 어떤 데이터를 찾아주는 object.  
    방대한 양의 html에서 데이터를 탐색하고 추출할 수 있도록 해준다
  - pagination 변수
    : 일종의 soup로 name이 div이고 class가 pagination인 내용을 담음
  - pages 변수
    : 링크('a')를 모두 찾아서 리스트 만들어 주었다
    - 앞으로 이해를 보다 편하게 하기 위해 links로 변수 이름 변경
  - spans
    : 각 링크 안에 있는 span을 넣어준 리스트   
    미리 빈 리스트를 만든 후 mutable operation을 통해 값을 추가했다
    - 이해를 보다 편하게 하기 위해 pages로 변수 이름 변경
- 여기서 텍스트(string)만 가져오는 코드를 사용하여,  
  span 속의 string만 추출하도록 하자

```python
import requests
from bs4 import BeautifulSoup

indeed_result = requests.get("https://www.indeed.com/jobs?q=python&limit=50")
 
indeed_soup = BeautifulSoup(indeed_result.text, "html.parser")

pagination = indeed_soup.find("div", {"class" : "pagination"})

links = pagination.find_all('a')
pages = []
for link in links:
	pages.append(link.find("span").string)
pages = pages[0:-1]

print(pages)
# result : ['2', '3', '4', ... '20']
```

- <a> link에 관한 코드를 다시 살펴보면,

  ```python
  <a href="~~" ~~> 
  	</a><span class="pn">2</span>
  </a>
  ```

  안 에서, string의 요소에는 '2' 하나만 존재한다는 것을 알 수 있다.  
  즉, 이 경우에서 span 속에서 string을 가져오는 것과   
  링크 안에서 string을 가져오는 것은 같은 결과를 갖게 된다.

- 위의 코드에서 `pages.append(link.find("span").string)` 부분을      
  `pages.append(link.string)` 으로 간단하게 적어도 같은 결과를 갖게 된다

- 지금 가져온 숫자는 string으로 가져왔으므로, integer로 변환해보자  
  link.string에 int형으로 변환하는 함수인 int()를 적용시키고  
  span이 아닌 link에서 string을 가져오도록 코드를 변경했으므로  
  리스트의 마지막 요소인 next를 포함하지 않도록 반복문의 links 부분을 아래와 같이 수정한다

  ```python
  for link in links[:-1]:
  	pages.append(int(link.string))
  
  print(pages)
  # result : [2, 3, 4, 5, ..., 20]
  ```

- 가장 큰 숫자 (= 마지막 숫자)를 찾아서 max_page라는 변수에 저장해두자

  ```python
  for link in links[:-1]:
  	pages.append(int(link.string))
  
  max_page = pages[-1]
  ```

- 다음으로, 페이지를 계속해서 request 하는 방법을 알아볼 것이다

- 마지막에 구한, 최대 page 숫자를 활용해서 (여기서는 20), 20개의 request를 만들 예정



## 2.5 Requesting Each Page



- 최대 페이지 수만큼 request를 보내보자

- range 함수를 활용해서 만들어보자  
  range()   
  : () 안에 넣은 수만큼의 크기의 배열을 만들 때 사용한다

  ```python
  max_page = pages[-1]
  
  print(range(max_page))
  # result : range(0, 20)
  # 현재, max_page는 20
  ```

- indeed web page의 url을 살펴보면  
  "https://www.indeed.com/jobs?q=python&limit=50&start=50"에서  
  맨 뒤 start 부분이 페이지의 index에 따라 50씩 늘어나는 규칙을 가짐을 알 수 있다  
  ex) page 1 -> start=0, page 2 -> start=50, ..., page 20 -> start=950

- range를 활용해서 다음과 같이 코드를 작성해보자

  ```python
  max_page = pages[-1]
  
  for n in range(max_page):
  	print(f"start={n*50}")
  # result : start=0 start=50, ..., start=950
  ```

- 지금까지 작성한 코드가 길어지고 있으므로, 이 부분을 function으로 만들어주자

- indeed.py라는 파일을 새로 생성한 후, 다음 코드 부분을 복사 붙여넣기하여 아래와 같이 만들어준다  
  indeed page를 추출하는 부분을 함수로 만들어주고, request로 받는 url 부분을 변수로  
  따로 빼준다, 강조를 위해 대문자로 URL로 변수 생성

- 파일 이름이 indeed 이므로 변수 이름에 indeed를 제거하여 코드를 정리하자

- 이 후, 맨 마지막에 `return max_page` 추가

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  URL = "https://www.indeed.com/jobs?q=python&limit=50"
  
  def extract_indeed_pages():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pagination = soup.find("div", {"class" : "pagination"})
  
    links = pagination.find_all('a')
    pages = []
    for link in links[:-1]:
      pages.append(int(link.string))
  
    max_page = pages[-1]
    return max_page
  ```

- main.py로 이동하여, 따로 분리해준 함수를 import하고 print해보자

  ```python
  from indeed import extract_indeed_pages
  
  max_indeed_pages = extract_indeed_pages()
  
  print(max_indeed_pages)
  # result : 20
  ```

- 다음으로, indeed page를 입력받아서,   
  필요한 만큼 request를 받는 (지금은 20번) 함수를 만들어주자

- indeed.py에 다음과 같은 함수 작성  
  input으로 마지막 페이지 넘버를 받는다

  ```python
  
  ~~~
    max_page = pages[-1]
    return max_page
    
  def extract_indeed_jobs(last_pages):
    for page in range(last_page):
      print(f"&start={page*50}")
  ```

- 지금 설정한 50이라는 limit은 바뀔 수 있는 부분이기에,  
  함수를 더 유연하게 만들기 위해 limit을 따로 변수로 만들어주자

- 맨위에 LIMIT 변수를 만들어주고, 50으로 설정한 부분들에 LIMIT 변수를 넣어주자  
  URL 마지막 부분 변경, `print(f"&start={page*50}")` 부분 변경

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  LIMIT = 50
  URL = f"https://www.indeed.com/jobs?q=python&limit={LIMIT}"
  
  def extract_indeed_pages():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pagination = soup.find("div", {"class" : "pagination"})
  
    links = pagination.find_all('a')
    pages = []
    for link in links[:-1]:
      pages.append(int(link.string))
  
    max_page = pages[-1]
    return max_page
    
  def extract_indeed_jobs(last_pages):
    for page in range(last_page):
      print(f"&start={page*LIMIT}")
  ```

- 다시, main.py에 가서 방금 만든 함수인 extract_indeed_job를 import 하고,  
  함수를 호출해보자

  ```python
  from indeed import extract_indeed_pages, extract_indeed_jobs
  
  last_indeed_pages = extract_indeed_pages()
  
  extract_indeed_jobs(last_indeed_page)
  ```

- 지금은 extract_indeed_jobs 함수에 print를 실행시켰으므로,  
  원하는 결과가 출력됨을 확인할 수 있다

- 페이지를 요청하기 위해, indeed.py 파일에 있는 extract_indeed_jobs 함수의  
  print부분을 request.get으로 바꿔주고, URL을 넣어준 후, 변수에 넣어주자

  ```python
  def extract_indeed_jobs(last_pages):
    for page in range(last_page):
      result = request.get(f"{URL}&start={page*LIMIT}")
      print(result.status_code)
  # 동작하는지 확인하는 용도로 status_code 사용
  # result : 200이 20개 출력됨
  ```

- 다음으로, 각 페이지로 이동하며, 일자리 정보를 추출해서, 어딘가에 담고  
  모든 일자리를 반환하는 작업을 할 계획이다

- main.py로 이동하여, extract_indeed_jobs의 반환값을 변수에 넣어주자

  ```python
  from indeed import extract_indeed_pages, extract_indeed_jobs
  
  last_indeed_pages = extract_indeed_pages()
  
  indeed_jobs = extract_indeed_jobs(last_indeed_page)
  ```

- 다음으로, indeed.py 파일에 가서, extract_indeed_jobs 함수에  
  jobs라는 이름의 빈 array를 만들어주자  
  이 함수에서 일자리를 추출한 후에, jobs에 담아서 반환할 계획이므로  
  `return jobs`도 추가해주자

  ```python
  def extract_indeed_jobs(last_pages):
  	jobs = []
    for page in range(last_page):
      result = request.get(f"{URL}&start={page*LIMIT}")
      print(result.status_code)
    return jobs
  ```

  

## 2.6 Extracting Titles



> cf) indeed 웹사이트 수정으로 인해, 아래의 div 부분이 h2로 수정된 부분 존재
>
> ```python
> for result in results: 
> 	title = result.find("div", {"class": "title"}).find("a")["title"]
> ```
>
> 부분을
>
> ```python
> for result in results: 
>   title = result.find("h2", {"class": "title"}).find("a")["title"]
> ```
>
> 으로 수정해야 작동한다고 한다



- 이제 원하는 일자리에 대한 data를 html에서 추출해보자

- soup를 활용하여 data를 추출하자

- indeed.py 파일에서 extract_indeed_jobs 함수에 soup관련 코드 추가하기

  ```python
  def extract_indeed_jobs(last_pages):
  	jobs = []
    for page in range(last_page):
      result = request.get(f"{URL}&start={page*LIMIT}")
  		soup = BeautifulSoup(result.txt, "html.parser")
    return jobs
  ```

  soup 적용후, indeed website에서 개발자도구를 통해 원하는 일자리 data에 속하는  
  name과 class를 찾자

- find_all을 사용해서 div를 찾을 것이고, class이름은 job search-SerpJobCard 이다  
  이 결과를 results라는 변수에 저장

  ```python
  def extract_indeed_jobs(last_pages):
  	jobs = []
    for page in range(last_page):
      result = request.get(f"{URL}&start={page*LIMIT}")
  		soup = BeautifulSoup(result.txt, "html.parser")
  		results = soup.find_all("div", {"class": "job search-SerpJobCard"})
    return jobs
  ```

- 결과적으로, job의 정보를 포함한 list가 results에 저장된다

- Tip) 위의 경우에서 print를 통해 test를 할 경우, 20개의 페이지가 모두 실행되므로,  
  임시적으로 아래와 같이 변경하여 하나의 페이지에 대해서만 테스트를 하며 진행하자

- 반복문을 주석처리하고, 변수인 page를 상수 0 으로 변경하였다

  ```python
  def extract_indeed_jobs(last_pages):
  	jobs = []
    # for page in range(last_page):
    result = request.get(f"{URL}&start={0*LIMIT}")
    soup = BeautifulSoup(result.txt, "html.parser")
    results = soup.find_all("div", {"class": "job search-SerpJobCard"})
    print(results)
    return jobs
  ```

- 여기서 results는 html list이자, soup의 list이다

- results의 list에 있는 각 result의 title 들을 추출해보자

  ```python
  def extract_indeed_jobs(last_pages):
  	jobs = []
    # for page in range(last_page):
    result = request.get(f"{URL}&start={0*LIMIT}")
    soup = BeautifulSoup(result.txt, "html.parser")
    results = soup.find_all("div", {"class": "job search-SerpJobCard"})
    for result in results:
  		print(result.find("div", {"class" : "title"}))
    return jobs
  ```

- 위의 결과를 보면, title안에 또 anchor가 존재하여 원하지 않는 부분들도  
  같이 딸려옴을 확인할 수 있다

- 테스트를 반복적으로 진행하며, 필요한 부분을 추출하는 것이 중요 !

- 아래의 코드로 anchor 안의 string을 뽑아보면, 또 원하지 않는 부분들이 딸려나온다

  ```python
  for result in results:
  	title = result.find("div", {"class" : "title"})
  	print(title.find("a").string)
  ```

- 효율적으로 data를 뽑을 수 있는 방법을 고안하기 위해, website의 코드를  
  천천히 보면, anchor 코드 안에 이미 우리가 필요한 정보인 title이 들어있고,  
  이 부분에 직무가 적혀있음을 찾을 수 있다!

- anchor안의 attribute가 title인 부분을 anchor라는 변수에 저장하고, print해보자  
  `find("a")["title"]`와 같이 ("a") 뒤에 [ ]를 붙여서 attribute를 추가할 수 있다

  ```python
  for result in results:
  	title = result.find("div", {"class" : "title"})
  	anchor = title.find("a")["title"]
  	print(anchor)
  # result : Python Developer / Software Engineer / ...
  ```

  원하는 결과가 정상적으로 출력됨을 확인할 수 있기에, 위의 코드에서 title이  
  중복되었으므로, 한줄로 간결하게 코드를 줄여보자

  ```python
  for result in results:
  	title = result.find("div", {"class" : "title"}).find("a")["title"]
  	print(title)
  ```

- 두 줄로 하거나, 한줄로 하거나 둘 다 상관없다



## 2.7 Extracting Companies



- 이전 강의에서 타이틀을 추출했으니, 이번에는 회사 이름을 가져와보자

- 마찬가지로, 크롬에서 개발자도구를 켜서 (option + command + i)  
  우리가 원하는 company부분의 소스코드를 확인해보자

  ```html
  <span class="company">
   <a ~~~~></a>
  </span>
  ```

  안에 관련 내용이 존재하므로, 먼저 class 이름이 company인 span에 해당하는 내용을 가져오자

-  company라는 변수를 추가하여 내용을 넣어주자

  ```python
  ~~~
  for result in results:
  	title = result.find("div", {"class" : "title"}).find("a")["title"]
  	company = result.find("span", {"class" : "company"})
    print(company)
  # result : company의 결과값 출력 / company class안에 anchor가 포함되어 출력됨
  return jobs
  ```

- find와 find_all의 차이

  - find : 첫번째 찾은 결과를 보여준다
  - find_all : 해당하는 리스트의 전부를 가져온다

- company class안에 anchor가 포함되어있기에, 관련내용이 함께 포함되서 나온다

- indeed 페이지의 소스코드를 살펴보면, title을 추출할때와는 다르게  
  anchor안에 company에 대한 정보가 없음을 알 수 있다

- 그리고, 웹페이지에서 회사 이름을 보여주는 소스코드들을 살펴보면  
  어떤 부분은 anchor(링크) 가 있는 반면, 어떤 부분은 anchor(링크) 없이  

  ```html
  <span class="company">
  	Cybernetic Search</span>
  ```

  위 처럼 구성된 경우도 존재함을 알 수 있다..!

- 그래서 아래와 같은 코드로 print를 하면 곳곳에 `none`이 함께 출력되게 된다  
  아래는, company class중 anchor부분만 반복분을 통해서 출력하는 코드

  ```python
  for result in results:
  	title = result.find("div", {"class" : "title"}).find("a")["title"]
  	company = result.find("span", {"class" : "company"}).find("a")
    print(company)
  ```

- 이와 같이, 어떠한 경우에는 링크가 있고, 어떠한 경우에는 링크가 없는 경우에는  
  반복문을 통해서 해결해보도록 하자

- 알고리즘은 이와 같다  
  만약(if) company가 anchor(링크)를 갖고 있는 경우 (None이 아닌 경우)에는  
  company의 anchor안에 있는 string을 가져오고,  
  company가 anchor를 갖고 있지 않은 경우 (None)인 경우에는,  
  company의 span에서 string을 가져온다  
  코드로 작성하면 아래와 같다

  ```python
  ~~~
  
  for result in results:
  	title = result.find("div", {"class" : "title"}).find("a")["title"]
  	company = result.find("span", {"class" : "company"})
    if company.find("a") is not None:
    	print(company.find("a").string)
    else:
    	print(company.string)
  return jobs
  ```

   결과가 잘 출력됨을 볼 수 있고, 코드를 좀 더 정리해보자  
  company.find("a")를 company_anchor 라는 이름의 변수로 만들자

  ```python
  ~~~
  
  for result in results:
  	title = result.find("div", {"class" : "title"}).find("a")["title"]
  	company = result.find("span", {"class" : "company"})
  	company_anchor = company.find("a")
    if company_anchor is not None:
    	print(company_anchor.string)
    else:
    	print(company.string)
  return jobs
  ```

  그리고, 같은 결과가 나오는지 다시 확인  
  코드를 변경할 때 마다, 원하는 결과가 나오는지 체크하는 습관을 잘 들이는 것이 중요!

- 지금까지 나온 결과를 보면, 원하는 회사 이름은 잘출력되지만, 딸려오는 빈칸이 많은 상황이다

- 결과를 str() 함수를 이용하여 string으로 변경출력해보자  
  print(~~) 부분을 print(str(\~\~))로 변경

  ```python
  if company_anchor is not None:
    	print(str(company_anchor.string))
    else:
    	print(str(company.string))
  ```

- 아쉽게도, 같은 결과가 출력되어, 원하는 결과가 나오지 않았다..

- 일단 이에 대한 해결은 잠시 미뤄두고, test를 위해 print 처리해두었던 부분을  
  company 변수에 담아주자   

- result.find의 결과가 담긴 company는 soup인 상태였고,  
  이 변수에 다른 내용을 다시 담으면 새롭게 덮어써지게 되므로 상관없다  

  ```python
  if company_anchor is not None:
    	company = str(company_anchor.string)
    else:
    	company = str(company.string)
  ```

- 지금과 같이 원하는 결과에 빈칸이 많이 껴있는 경우,  
  string이 아닌 strip을 사용해야한다  
  string에도 다양한 operation이 있는데, 그 중 하나가 strip이다

- strip()은 ()안에 있는 내용을 지워준다

  - 사용법 : string.strip("f") - 해당 string에서 f를 지운다
    string.strip() - 안에 담긴 것이 없으면 공백(빈칸)을 지운다

  ```python
  if company_anchor is not None:
    	company = str(company_anchor.string)
    else:
    	company = str(company.string)
    company = company.strip()
    print(company)
  return jobs
  ```

- 마지막으로, title과 company가 같이 원하는대로 잘 출력되는지 확인해보자

  ```python
  if company_anchor is not None:
    	company = str(company_anchor.string)
    else:
    	company = str(company.string)
    company = company.strip()
    print(title, company)
  return jobs
  ```

   결과가 잘 나오며, 원하는 정보를 추출하는데에 성공했다!

  

## 2.8 Extracting Locations and Finishing up



## 2.9 StackOverflow Pages



## 2.10 StackOverflow extract_jobs



## 2.11 StackOverflow extract_job



## 2.12 StackOverflow extract_job part Two



## 2.13 StackOverflow Finish



## 2.14 What is CSV



## 2.15 Saving to CSV



## 2.16 OMG THIS IS AWESOME