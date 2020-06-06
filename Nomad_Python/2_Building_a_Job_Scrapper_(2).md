# 2. Building a Job Scrapper_(2)



### 이전 markdown

- [2_Building_a_Job_Scrapper_(1)](/Users/sjeon/Desktop/For_min/Dev_Place/Nomad_Python/2_Building_a_Job_Scrapper_(1).md) 



## 2.9 StackOverflow Pages



> ```python
> pages = soup.find("div", {"class":"s-pagination"}).find_all("a")
> ```
>
>  현재, web page의 소스코드에서  
> class가 pagination 에서 s-pagination으로 바뀌었다는 점 참고



- 이제, stackoverflow를 진행하기 전에,  
  main.py의 코드를 indeed.py에 모아주도록 하자

- indeed.py로 가서 함수를 하나 만들어주고 main.py의 코드를 가져오자

- get_jobs라는 이름으로 함수를 만들어주고,  
  last_indeed_page의 이름을 last_page로,  
  ,indeed_jobs의 변수 이름을 jobs로 바꾸어주고  
  jobs를 return하자

- 관련 코드들이 모두 indeed.py라는 파일 안에 있으므로,  
  변수나 함수 이름에 indeed가 들어간 부분을 빼주도록 하자  
  extract_indeed_pages도 get_last_page로 변경하고,  
  extract_indeed_jobs도 extract_jobs로 변경해주자

  ```python
  def extract_jobs(last_pages):
  	jobs = []
    for page in range(last_page):
      print(f"Scrapping page {page}")
      result = requests.get(f"{URL}&start={page*LIMIT}")
      soup = BeautifulSoup(result.txt, "html.parser")
      results = soup.find_all("div", {"class": "job search-SerpJobCard"})
      for result in results:
        job = extract_job(result)
        jobs.append(job)
    return jobs
   
  def get_jobs():
  	last_page = get_last_page()
  	jobs = get_extract_jobs(last_page)
    return jobs
  ```

- 이제 main.py 파일에 가서, get_jobs를 호출해주자

  ```python
  from indeed import get_jobs as get_indeed_jobs
  
  indeed_jobs = get_indeed_jobs()
  
  print(indeed_jobs)
  ```

- error가 뜨는데, 에러의 내용은 company에 Nonetype이 있다는 내용이다  
  즉, company class를 갖고 있지 않는 span이 있다는 이야기이고,  
  extract_job 함수에 조건문을 추가하여 해결해보자

  ```python
  def extract_job(html):
  	title = html.find("div", {"class" : "title"}).find("a")["title"]
  	company = html.find("span", {"class" : "company"})
  	if company:
      company_anchor = company.find("a")
      if company_anchor is not None:
        company = str(company_anchor.string)
      else:
        company = str(company.string)
      company = company.strip()
     else:
      company = None
    location = html.find("div", {"class":"recJobLoc"})["data-rc-loc"]
  	job_id = html["data-jk"]
  	return {'title':title, 'company': company, 'location': location, "link": f"https://www.indeed.com/viewjob?jk={job_id}"}
  ```

  

- 이제 stackoverflow 내용을 담기 위한 새로운 파일을 만들어보자

- 파일명을 so.py로 하고, indeed.py의 상단에 있는 코드를 복사 붙여넣기하고,  
  URL을 stackoverflow에 맞게 바꿔주자  
  페이지를 바꾸면서 변하는 부분을 보면, url의 뒷부분 규칙이  
  `sort=i & ~~` 와 같이 변함을 알 수 있고, 이를 url에 반영하자

- url을 관찰하면, LIMIT은 필요하지 않으므로 삭제하자

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  URL = f"https://stackoverflow.com/jobs?q=python&sort=i"
  ```

- flow를 다시 정리해보면,  
  1) 페이지 가져오기  
  2) request 만들기  
  3) job 추출하기  
  와 같다

- 페이지를 가져오기 위해, 개발자도구를 통해 pagiantion 부분의 소스코드를 확인하자  

- `div class pagination`이 존재하고 이 부분을 살펴보자

- 이 전의 indeed webpage와 구조가 거의 동일하다

  - 마지막에 next 존재
  - anchor로 구성  

  function을 하나 만들어서 공통되는 부분을 함수화해도 아주 좋다  
  주의 할 점은, 만약 한 곳의 웹페이지가 변경된다면 function이 동작을 안할 수도 있으므로,  
  여기서는 따로 만들 예정

- 이전에 indeed에서 한것과 마찬가지로 so.py에 다음과 같이 코드를 작성하자

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  URL = f"https://stackoverflow.com/jobs?q=python&sort=i"
  
  def get_last_page():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    print(soup)
    
  def get_jobs():
  	last_page = get_last_page()
    return []
  # 임시로 list 반환
  ```

- main.py에 가서 다음과 같은 작업을 하자  
  지금은 stackoverflow관련 코드를 test 해나갈 예정이므로, indeed 부분은 주석처리하기

  ```python
  from indeed import get_jobs as get_indeed_jobs
  from so import get_jobs as get_so_jobs
  
  # indeed_jobs = get_indeed_jobs()
  so_jobs = get_so_jobs()
  ```

- 먼저 잘 실행되는지 위의 코드를 확인해보면, soup가 잘 출력됨을 알 수있다  
  다음으로는 pagination에 대한 print test를 진행하며 한단계씩 나아가자

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  URL = f"https://stackoverflow.com/jobs?q=python&sort=i"
  
  def get_last_page():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pagination = soup.find("div", {"class":"pagination"})
    print(pagination)
    
  def get_jobs():
  	last_page = get_last_page()
    return []
  # 임시로 list 반환
  ```

- 다음으로는 anchor에 대한 test 진행  
  pagination 변수에 find_all method를 추가하고, 변수 이름을 pages로 변경하여 출력하였다

  ```python
  import requests
  from bs4 import BeautifulSoup
  
  URL = f"https://stackoverflow.com/jobs?q=python&sort=i"
  
  def get_last_page():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pages = soup.find("div", {"class":"pagination"}).find_all("a")
    print(pages)
    
  def get_jobs():
  	last_page = get_last_page()
    return []
  # 임시로 list 반환
  ```

  

## 2.10 StackOverflow extract_jobs



- indeed 페이지에서 했던 작업과 마찬가지로,  
  페이지 숫자를 추출하기위해, next를 없애주고 가장 큰 숫자를 가져올 것이다

  ```python
  def get_last_page():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pages = soup.find("div", {"class":"pagination"}).find_all("a")
    last_pages = pages[-2]
    print(last_pages)
  # result : <div class="pagination"> ~~ <span>101</span> ~~ </div>
  ```

  index에서 -1이 마지막이고, -2가 그전 숫자이므로,  
  -1은 next이고 index -2가 우리가 원하는 마지막 숫자이다  
  pages[-2] 뒤에 .string으로 출력을 해보면 None이 출력된다  
  우리가 원하는 건 마지막 숫자이므로 get_text로 출력해보자

  ```python
    last_pages = pages[-2].get_text()
    print(last_pages)
  # result : 101
  ```

- 원하는 결과가 나왔으나, whitespace가 포함되어있다  
  get_text의 경우, 속성으로 strip을 아래와 같이 적용시킬 수 있다  
  doc에서 찾아보면 알 수 있다  

  ```python
    last_pages = pages[-2].get_text(strip=True)
    print(last_pages)
  # result : 101
  ```

  이 결과를 return해주고, get_jobs() 함수에서 출력해보도록 하자  
  같은 결과가 나와야한다

  ```python
  def get_last_page():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pages = soup.find("div", {"class":"pagination"}).find_all("a")
    last_pages = pages[-2].get_text(strip=True)
    return last_pages
    
  def get_jobs():
  	last_page = get_last_page()
  	print(last_page)
    return []
  # result : 101
  ```

  같은 결과가 나옴을 확인할 수 있다

- job을 추출하기 위한, extract_jobs라는 함수를 새로 만들어주자  

  ```python
  def get_last_page():
  	result = requests.get(URL)
    soup = BeautifulSoup(result.text, "html.parser")
    pages = soup.find("div", {"class":"pagination"}).find_all("a")
    last_pages = pages[-2].get_text(strip=True)
  	return int(last_page)
  
  def extract_jobs(last_page):
  	jobs = []
  	for page in range(last_page):
  		print(page)
  
  def get_jobs():
  	last_page = get_last_page()
  	jobs = extract_jobs(last_page)
  	return jobs
  ```

  get_text로 last_pages를 추출했으므로, string의 결과를 갖게되고,  
  range를 적용하기 위해서는 return 시에, int로 형변환을 해줘야한다   
  extract_jobs의 print 결과로 0에서 100까지의 숫자가 출력된다  
  우리가 원하는건 101까지의 숫자이고, 이 후에, +1의 작업을 추가하면 된다

- ```python
  def extract_jobs(last_page):
  	jobs = []
  	for page in range(last_page):
  		result = requests.get(f"{URL}&pg={page+1}")
      print(result.status_code)
  # result : 200이 100개 출력됨
  ```

  이 때, URL 뒤의 &pg 부분은 stackoverflow web page에서 변하는 부분을  
  check하여 변수와 함께 적어준다  
  보통 첫 페이지는 변하는 부분에 아무것도 없는 경우가 종종 있는데,  
  url 뒷부분을 page=1 또는 page=0으로 변경하여 이동해도  
  대부분 같은 결과가 나오게 된다

  ```python
  def extract_jobs(last_page):
  	jobs = []
  	for page in range(last_page):
  		result = requests.get(f"{URL}&pg={page+1}")
  		soup = BeautifulSoup(result.text, "html.parser")
      results = soup.find_all("div", {"class" : "-job"})
      for result in results
      	print(result["data-jobid"])
  ```

  result의 내용을 soup에 담고, 웹 사이트에서 개발자 도구를 통해  
  해당하는 class를 찾아서 코드에 넣어주자

- print를 통해, test를 진행할 때, result의 모든 결과를 가져오면 지저분하므로,  
  attribute "data-jobid"만 우선적으로 가져오자



## 2.11 StackOverflow extract_job



- indeed web page를 추출할때와 마찬가지로,  
  일자리 정보를 가져오기 위해, html 이름을 가진 soup object 를 인자로 받는  
  함수를 만들어주고, 기존의 함수인 extract_jobs와 연결해주자

  ```python
  def extract_job(html):
    	title = html.find("div", {"class":"-title"}).find("span")["data-ga-label"]
      print(title)
  
  def extract_jobs(last_page):
  	jobs = []
  	for page in range(last_page):
  		result = requests.get(f"{URL}&pg={page+1}")
  		soup = BeautifulSoup(result.text, "html.parser")
      results = soup.find_all("div", {"class" : "-job"})
      for result in results
      	job = extract_job(result)
      	jobs.append(job)
     return jobs
  ```

  일자리의 직무 부분을 가져오기위해, 어떤 class에 속해있는지 개발자 도구를 통해 살펴보자  
  div class이름이 title인 부분, 그 중 span 부분의 속성이 data-ga-label 인 부분을 가져오자   
  값이 나오긴 하나, 결과 값에 원하지 않는 숫자들이 같이 포함되어 나오므로,  
  다른 방법을 찾아보자.

- 소스코드를 살펴보면, title안에 h2가 있고, 그 안에 anchor에 있는 정보에 원하는 값이 있으므로  
  이 값을 가져와보자.

  ```python
  ~~~
  
  def extract_job(html):
    	title = html.find("div", {"class":"-title"}).find("h2").find("a")["title"]
      print(title)
      
  ~~~
  ```

  깔끔하게 원하는 결과가 출력됨을 알 수 있다  
  결과 값을 반환하는 return을 적어주자

- 다음으로, 회사의 정보를 가져오자  
  소스코드를 살펴보면, div의 class 이름이 -company인 부분에 관련 정보가 있고,  
  그 중, span 안에 원하는 데이터가 있음을 알 수 있다  

  ```python
  
  def extract_job(html):
    	title = html.find("div", {"class":"-title"}).find("h2").find("a")["title"]
      company_row = html.find("div", {"class":"-company"}).find_all("span", recursive=False)
      print(company_row)
      return {'title':title}
  ```

  결과 값을 보면, 첫 번째 span의 내용이 company, 두 번째 span의 내용이 location이다

  - 주의 해야 할 점으로, 웹사이트의 소스코드를 자세히 보면,  
    company의 span안에 다른 span이 또 존재함을 알 수 있고,  
    find_all method를 사용하면, 겹겹이 싸여있는 이 결과를 전부 가져오게 된다  
    이 경우, find_all에 `recursive=False`를 추가해주면,  
    가장 바깥의 결과만 가져오게 된다

- 파이썬에서, 리스트에 요소가 두개인지 이미 알고 있을 때,   
  각각을 따로 변수에 넣어 줄 수 있다.  
  2개의 item을 가진 list company_row가 있을 때,

  ```python
  company = company_row[0]
  company = company_row[1]
  ```

  대신

  ```python
  company, location = html.find("div", {"class":"-company"}).find_all("span", recursive=False)
  ```

  와 같이 사용한다  
  이것을 unpacking value라고 한다

  ```python
  company, location = html.find("div", {"class":"-company"}).find_all("span", recursive=False)
  print(company.get_text(strip=True), location.get_text(strip=True))
  # result : 회사, 위치 의 원하는 결과가 나옴
  ```

  변수 company와 location을 그대로 출력하면 span에 둘러싸여 있으므로,  
  .get_text(strip=True) 을 추가하여 필요한 부분만 출력해보자  
  company와 location 사이에 존재하는 " - " 를 제외하고 원하는 대로 출력됨을 볼 수 있다  
  다음 강의에서 " - " 를 제거해보자.

  

## 2.12 StackOverflow extract_job part Two

 

- 코드를 보기 좋게 정리하고, "-" 와 같은 불필요한 부분을 제거하자

  ```python
  company, location = html.find("div", {"class":"-company"}).find_all("span", recursive=False)
  company = company.get_text(strip=True)
  location = location.get_text(strip=True).strip("-").strip("\n")
  print(company, location)
  ```

  로 test를 하였을 때, new line이 제거가 되지는 않음  
  location 부분을  

  ```python
  location = location.get_text(strip=True).strip("-").strip(" \r").strip("\n")
  ```

  으로 strip("\r")을 추가하였을 때 원하는 결과가 나왔다.  
  new line을 \n이 아닌 \r로 표현하기도 한다는 점 참고

- 이제 return을 추가해주자

  ```python
  company, location = html.find("div", {"class":"-company"}).find_all("span", recursive=False)
  company = company.get_text(strip=True)
  location = location.get_text(strip=True).strip("-").strip(" \r").strip("\n")
  return {'title': title, 'company' : company, 'location' : location}
  ```

  

## 2.13 StackOverflow Finish



- 이제 일자리를 클릭하면 나오는, url을 가져오자

- 임의의 일자리 하나를 클릭해서 링크로 이동해보자  
  url을 확인해보면, url이 길게 나오는데 id 이후의 부분을 지워보자

- 지워도 같은 페이지로 이동함을 확인할 수 있고,  
  이와 같이 어느 링크에 우리가 필요한 내용을 담은 핵심 url이 어디까지인지를 파악하는 것도  
  매우 중요함을 알 수 있다 (코딩의 난이도에 큰 영향을 준다)

- 다시 이동할 때, 눌렀던 부분의 소스코드를 개발자 도구를 이용해서 확인해보면,  
  div 이름이 data_jobid인 부분에 관련 내용이 들어있음을 확인할 수 있다

- 이 내용을 job_id 변수에 넣어주고, Return에 추가해주자

  ```python
  company, location = html.find("div", {"class":"-company"}).find_all("span", recursive=False)
  company = company.get_text(strip=True)
  location = location.get_text(strip=True).strip("-").strip(" \r").strip("\n")
  job_id = html['data-jobid']
  return {'title': title, 'company' : company, 'location' : location, "apply_link" : f"https://stackoverflow.com/jobs/{job_id}"}
  ```

- 다시 print를 통해 test를 해보자

  ```python
  def extract_jobs(last_page):
  	jobs = []
  	for page in range(last_page):
  		result = requests.get(f"{URL}&pg={page+1}")
  		soup = BeautifulSoup(result.text, "html.parser")
      results = soup.find_all("div", {"class" : "-job"})
      for result in results
      	job = extract_job(result)
        print(job)
        # test결과가 잘 나옴을 확인할 수 있다
      	jobs.append(job)
     return jobs
  ```

- 이제 모든 결과를 main.py에 반환하고,  
  indeed와 stackoverflow의 결과를 함께 확인해보자  
  두 결과를 리스트로 만들어서, jobs라는 이름의 변수에 넣어주자

  ```python
  from indeed import get_jobs as get_indeed_jobs
  from so import get_jobs as get_so_jobs
  
  so_jobs = get_so_jobs()
  indeed_jobs = get_indeed jobs()
  jobs = so_jobs + indeed_jobs
  print(jobs)
  ```

  

- extract_jobs 함수 부분에도 결과를 더 잘보기 위해  
  print 를 추가해주자

  ```python
  def extract_jobs(last_page):
  	jobs = []
  	for page in range(last_page):
      print(f"Scrapping SO : Page: {page}")
  		result = requests.get(f"{URL}&pg={page+1}")
  		soup = BeautifulSoup(result.text, "html.parser")
      results = soup.find_all("div", {"class" : "-job"})
      for result in results
      	job = extract_job(result)
        print(job)
        # test결과가 잘 나옴을 확인할 수 있다
      	jobs.append(job)
     return jobs
  ```

  그리고 실행시켜보자

  잘 해결됨을 알 수 있다

## 2.14 What is CSV



## 2.15 Saving to CSV



## 2.16 OMG THIS IS AWESOME