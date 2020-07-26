

# 오픈소스를 참여를 위한 git/github 기본실습





## 들어가기 전에

- CLI와 GUI는 용도가 다르다
  - GUI로 할 수 있는건 CLI로 다 할 수 있다
  - 하지만, CLI로 하는 것은 다 GUI로 할 수 있는 것은 아니다
  - 개발자로서, CLI를 잘 활용하는 것은 강점
  - 트레이닝 차원에서도 CLI가 더 연습하기에 좋다
    - 무엇을 모르는지 보다 정확하게 알 수 있기 때문
- 그래서, 수업은 CLI에 집중해서 진행할 계획





## 수업 내용



### 초기 환경 설정



- 구름 ide - 대시보드에서 새 컨테이너 생성 클릭
- 컨테이너 : 리눅스 (오리지널 리눅스는 아니다)  
  컨테이너를 만드는 것은 - 새 리눅스 환경을 만드는 것과 유사  
  작업할 수 있는 환경을 만드는 것
- 예시는 pytorch로 진행할 계획
- 리눅스(컨테이너) 타입으로 pytorch 선택
- 환경설정 세팅을 하는데에 오래걸리는 것을, 컨테이너로 하면 굉장히 빠르게 할 수 있다
- github에서 pytorch에 들어가보자  
  github.com/pytorch/pytorch 로 들어가서 파일을 보면  
  Dockerfile이 존재
- 이 Dockerfile은 pytorch 환경설정을 해주는 파일
- 이 스크립트를 돌리면 환경설정을 할 수 있다
- 구름 ide도 이와 같은 방식을 활용한다





### 오픈 소스 프로젝트 복사 / 다운 받기



- GitHub.com/pytorch/examples로 들어가자
- commit이란 : 소스파일의 변화분
- fork : github상에서 복사하는 것
- 원본에서 작업을 할 수는 없으니, fork를 통해 가져와서 진행
  - github 오른쪽 위의 fork 선택
  - fork 하는 이유
    - 원본에는 push 권한이 없다
    - 협업을 해나가는데에 있어서, 원본을 git clone으로 하여 로컬로 작업을 진행하면  
      나의 결과를 다른 사람에게 보여줄 수 없다 
- 이렇게 fork 받은 것을 clone 한다 (오리지널 소스를 clone 하지 않는다)
- Clone : 소스코드를 local에 복사 

- pytorch/examples에 있는 내용을 fork 후 clone 받자

  ```shell
  cat README.md
  ```

  을 하면, MNIST example을 진행하기 위해 해야할 환경설정, 사용법이 나온다

  ```shell
  # README.md 의 결과
  
  pip install -r requirements.txt
  # pytorch 환경설정을 해주며, 우리는 구름 ide의 컨테이너를 통해 이미 환경설정을 마쳤으므로 진행되지 않는다
  
  python main.py
  # 구름 ide에서 초기 설정되어있는 index.py를 실행시킨 결과와 동일한 결과가 나온다
  ```





### 개발자가 오픈소스를 읽는 방법

- 단순 오픈소스 사용이 목적이라면, git clone해서 사용하는게 다일 수 있으나,  
  오픈소스에 기여하는 개발자의 입장에서는 history와 log를 보고 접근해야한다



1) 누가 제일 많이 개발할까?

```bash
# 해당 오픈소스에서 "누가 제일 개발을 많이 할까? / 기여를 많이 했을까"
# 참고 : nl 명령은 파일의 line number 명시 (순위표시용으로 사용)
# less는 상위 10개만 보도록 설정
$git shortlog -sn | nl | less
# merge 커밋을 제외한, 순수하게 작업되고 수정된 commit만을 보도록 명령할 수 있다 / merge commit : 합쳐지는 것을 위해서 만든 빈 Commit
$git shorlog -sn --no-merges | nl | less
```



2) 전체 소스파일수정 내역(commit) 및  갯수는?

```bash
# 전체 소스파일 수정내역(commit) 갯수 세기
$git log --oneline | wc -l

# 참고 : wc -l 명령은 (파일) 라인 수 갯수 측정
# wc는 word count로 갯수를 측정하며 줄, 단어, 문자, 바이트를 세준다 / -l은 그 중 line을 의미

# 전체 소스파일 수정내역 리스트 / 제목만 나옴
$git log --oneline

# 마찬가지로, merge commit을 제외하고 볼 수 있음 
$git log --oneline --no-merges
# 참고 : 'q' 키 눌러서 나가기
```



3) 소스파일 수정내역(commit)의 ID란?  
: 소스파일 수정내역의 고유한 ID

```bash
$git log --oneline
...
6c8e2ba fix warnings and failures
3c032e8 Add CI to run examples via github action
19ae7a3 Merge pull request
...

# 위의 commit log 중 하나를 확인해보자
$git show 6c8e2ba

# 참고 'q' 키 눌러서 나가기
```

- git show를 통해 diff의 결과를 볼 수 있다
- 빨간색으로 '-' 로 나오는 부분은 지워진 부분
- 초록색으로 '+'로 나오는 부분은 추가된 부분
- git은 이러한 소스코드의 변화 히스토리를 저장해두는 역할
  - 누가 수정했고, 왜 수정했고, 어떻게 수정했는지를 보여준다
- 이와 같은 관리의 장점은,  
  이러한 히스토리를 통해 협업적으로도, 백업에서도 장점이 있다



### 미션 : 6c8e2ba commit을 통해 수정된 소스파일의 갯수를 알아보자

- 변화된 파일을 알려주는 공통된 부분을 찾아보자

```bash
# 해당 commit(6c8e2ba)의 수정한 파일들 확인하기
$ git show 6c8e2ba | grep "diff --git"
diff --git a/dcgan/main.py b/dcgan/main.py
diff --git a/fast_neural_style/download_saved_models.py b/fast_neural_style/download_saved_models.py
...
...

# 해당 commit의 수정한 파일 갯수 확인
$ git show 6c8e2ba | grep "diff --git" | wc -l
4
```



4) Merge commit 이란?  
: 수정 내역이 없는 병합 진행 시에 생긴 commit  
오픈 소스 참여자 입장에서는 빈 커밋이라고 이해해도 된다

```bash
$git show 19ae7a3
```



- 오픈소스 프로젝트를 파악할 때,  
  Top down 방식으로 접근해서 분석하는 것은 좋은 접근법
- 가장 먼저 oneline으로 리스트 갯수를 보며, 누가 제일 많이 기여했는 지,  
  총 commit 갯수는 몇 개 인지,  
  commit list의 제목을 보며 조금 더 자세히 무슨 commit이 이루어졌는지,  
  구체적인 수정 내역은 무엇인지 순으로 파악



5) 전체 소스파일 수정내역(commit) 자세히 보기

```bash
# 전체 소스파일 수정내역(commit) 자세히 보기
$git log -p
```



6) 특정 소스파일기준 수정내역 (commit) 리스트

```bash
# 오픈소스 작업 폴더로 이동
$cd /workspace/pytorch/examples/

# 특정 폴더를 기준으로 소스 수정내역(commit) 리스트 확인하기
$git log --oneline -- mnist/
```



- git을 잘활용하면 소스코드 리딩하는 데에도 큰 도움을 받을 수 있다
- 예로, C program 의 함수가 1000개 정도 된다고하면, 하나씩 다 보는 것은 매우 힘든 일
- 전체적인 구조나 변화를 파악하기 위해 특정 디렉토리의 git log를 보는 것도 좋은 전략이 될 수 있음

- 총 commit의 갯수 중, 특정 디렉토리의 commit수의 비율을 보며 비중을 가늠해 볼 수도 있다

```bash
$git log --oneline -- mnist/ | wc -l
```



7) 특정 날짜기준 수정내역(commit) 리스트

```bash
# 2020년 1월부터 2020년 6월 30일까지 소스 수정내역 리스트 확인
$ git log --oneline --after=2020-01-01 --before=2020-06-30

# 갯수를 알고 싶다면 뒤에 | wc -l 을 추가하면 된다
```

- 이와 같이 최근 날짜 기준 commit 갯수나, 그 내역을 확인 할 수 있다는 것은  
  매우 좋은 통계자료가 될 수 있다
- 개발자의 입장에서, 해당 오픈소스의 어떤 부분이 핫한지, 어떻게 읽어야할지를 파악할 수 있음
- 지금까지의 내용들은 소스코드 자체를 읽는 것이 아닌,  
  git 을 통해 오픈소스를 어떻게 읽을 수 있을지에 관한 이야기
- 이러한 내용을 익히면, 더 넓은 시야를 가질 수 있는 좋은 인사이트를 가질 수 있다
- 참고로 이러한 명령어들은 .git이라는 파일을 지우면 작동하지 않는다
- .git안에 히스토리가 모두 들어있고, 핵심은 .git이다
- push도 .git을 통해서 해당 소스코드 들을 서버에 보내는 것



8) 특정 날짜 + 파일 기준 수정내역 (commit) 리스트

```bash
# 2020년 6월 한달간 mnist/ 폴더 기준 소스파일 수정내역 확인하기
$git log --oneline --after=2020-06-01 --before=2020-06-30 -- minst/
59423c5 Delete unnecessary blank lines
```



9) 옛날것부터 소스파일 수정내역(commit) 보기

```bash
# 소스파일 수정내역(commit) 옛날 것 부터 살펴보기
$git log --reverse
```

- 이 프로젝트의, 이 디렉토리의 최초의 commit은 뭘까 에 대한 해결 방법
- 물론 이외에도, 여러가지에 활용 가능하다





### 오픈소스 개발참여준비 Git 설정

- Git config
- 컨셉은 오픈소스 개발참여준비



1) 오픈소스 개발 참여 준비 Git 설정하기

- 구름 IDE를 이용하면 리눅스를 새로 설정한 것이기에 캐싱데이터가 존재할 수 없음
- 한 노트북에서 계정이 2개인 경우 (회사, 개인 계정) 충돌이 발생할 수 있음  
  캐싱데이터(미리 읽어 놓은 데이터)가 존재하는 경우
- 혹은 다른 사람의 노트북이나, 데스크탑의 경우 캐싱데이터를 삭제해야하는 경우들이 있다

```bash
# Github ID/PW 캐싱데이터 삭제 (삭제 시 문제 없음)
# 다른(사람) Github 계정과의 충돌 방지
$git config --global --unset credential.helper
$git config --system --unset credential.helper

# Github 계정 이메일 주소 및 본인영문이름
# 차후 소스코드 파일수정 내역(commit) wjwk(author)정보
$ git config --global user.email "본인 메일 적기"
$ git config --global user.name "본인 이름 적기"

# 메일은 github계정 적기 / 이름은 영어로 적기
```

- 위와 같은 정보를 입력해야하는 이유
  - Commit(소스코드 수정 내역)을 만든다는 것은, 내가 소스코드를 수정한다는 의미이고  
    누가 수정했는지에대한 저자 정보를 적는 것임
  - 오픈소스를 참여하면서, 누가 수정했는지에 대한 정보는 필수



2) Git 기본 편집기 설정

```bash
# Git commit(소스파일 수정내역) message(설명 글) 수정할 기본 편집기 설정
# 원하는 편집기 설정 가능 (vim, emacs, nano, notepad 등)
# 꼭 nano로 진행할 필요는 없음
$git config --global core.editor nano

# nano 편집기 사용시 설치 필요
$ sudo apt install -y nano
```



3) git 설정 내용 확인하기

```bash
# git 설정 내용 확인하기
# 참고 : git 설정 수정은 이전 실습명령 동일하게 입력시 수정
$git config --list
```





### 오픈소스 개발참여를 위한 수정(commit) 작업

- Git branch & commit

- 참여할 오픈소스에 대한 기본적인 reading을 진행하였고,  
  개인 git 설정을 완료하였다면, 이제 소스코드 수정 작업을 진행할 차례
- 이 때, branch가 반드시 들어가야한다



![image-20200726005532281](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726005532281.png)



- 우리는 fork해온 이전의 commit 위에다가 새로운 commit을 추가할 예정
- 이 때, 기존에 존재하던 commit을 base라고 하며,  
  우리가 commit을 하면, 기존의 base와의 diff값인 우리가 새로 수정한 내용만 올라가게된다



1) 오픈소스 개발 참여를 위한 수정(commit) 작업 : 작업의 시작 "브랜치(branch) 생성"

```bash
# 오픈 소스 프로젝트 폴더로 이동
$cd /workspace/pytorch/emamples/

# Branch 생성
# 작업내용을 대표하는 키워드로 branch 명 생성 추천
$ git checkout -b fix-mnist
```

- add : 히스토리를 남기기 위한, 수정 사항을 commit 하기 위한 준비
- commit : 히스토리를 남기는 것
- branch : 같은 폴더인데도 다른 세상처럼 운영할 수 있게 해주는 것  
  - "같은 폴더, 다른 세상"
- 만약 backup을 위해 새로운 디렉토리로 복사하거나, zip 파일로 남겨 놓는 다면 발생하는 문제
  - 용량 문제 : 변경할 때마다 backup 파일을 만든다면 용량 문제가 생김
  - 경로 문제 : 새로운 디렉토리를 만들면, 경로나 환경설정 상 엉키는 문제들이 발생할 가능성이 생김
- 이 어려운 문제점을 해결해주는 것이 =="branch"==
- 개발을 하다보면 왜 branch가 필요하고, 나오게 되었는지 자연스럽게 알 수 있음

```bash
# "같은 폴더 다른 세상" 브랜치 테스트
$ touch hi.txt

# 새롭게 생성한 fix-mnist 브랜치에서
# 새로운 파일 hello.txt 수정내역(commit) 만들기
$ git add hi.txt
$ git commit -m "test: add hi.txt file"

# 브랜치를 master 브랜치로 변경
$ git checkout master

# 같은 폴더 내 파일 내용확인하기
# hello.txt 파일 존재여부 확인
$ ls

# 브랜치 fix-mnist 로 변경 후
# hello.txt 파일 존재여부 확인
$ git checkout fix-mnist

# 다시 master 브랜치로 변경 후 확인
$ git checkout master
$ ls
```

- git은 브랜치 운영과 속도가 큰 강점이다
- branch를 딸 때, 작업내용을 대표하는 키워드로 branch 명 생성 추천
  - pull-request 보낼 때도 깔끔하고,
  - 이전 작업 추적하기에도 좋고,
  - 다른 사람들이 보기에도 좋다
  - branch 를 운영하는 방식에 따라서 다르게 명명할 수도 있으나, 위의 방식을 추천



2) 브랜치 삭제하기

```bash
# master 브랜치로 변경후
$ git checkout master

# branch 삭제
$ git branch -d fix-mnist
```

- 브랜치 명에 fix와 같이 조금 더 구체적으로 적는 것이 좋다
- 예로, update와 같은 추상적인 단어를 사용하면, 잘못된 것을 수정한건지, 원래 되던걸 더 발전시킨건지, 버그를 수정한건지 헷갈리게 된다
- fix : 잘못된 것을 수정
- enhance or improve : 개선시킨 것 (3초 걸리던 것을 1초 걸리도록 수정)



3) 현재 소스파일 상태(status) 확인 하기

```bash
# 현재 소스파일 상태 (status) 확인하기
$git status
```

- nothing to commit, working tree clean : 수정할 내역이 없다는 의미



- 정리해보면, 오픈소스 작업을 하는 순서는
  - 참여하려는 오픈소스에서 Fork를 뜨고, clone을 받아서 master를 가져온 후,  
  - branch를 따는 것이 시작이다.



4) 소스파일 수정 후 최신역사와의 차이점 확인하기

```bash
# 원하는 편집기 (vim, emacs 등)로 mnist/main.py 파일 수정하기
$ nano mnist/main.py

# mnist/main.py 소스 파일 수정한 내용 확인하기
$ git diff
diff --git a/mnist/main.py b/mnist main.py
# ~~~ diff 의 결과가 나옴
```

- git diff 명령어를 통해서 확인한다



5) 수정내역(commit) 만들기 전 준비작업(add)

```bash
# 수정한 파일 git commit 할 준비하기
$git add mnist/main.py

# 수정한 파일 git commit 준비 완료상태 확인
$git status
# changes to be committed:
#		modified: mnist/main.py 출력
```

- 아직 commit 된 것은 아니고, 변경될 부분에 대한 준비작업이 된 것



6) 수정내역(commit) 만들고 확인하기

```bash
# git commit(수정내역) 만들고
# commit을 만든 이유 작성
# help 내용 안에 잘못된 default 값 수정했다는 설명 적기
$ git commit -m "Correct typo in default value within help"

# 내가 작성한 commit 확인하기
$ git show

# 참고: 'q' 키 누르고 나가기
```

- 이후 다시, `git staus` 로 현재 상태를 확인하면,  
  이미 최신 내용을 commit(수정) 했기에 수정할 부분이 없다고 나온다.



7) 수정내역(commit) 1개 더 추가하기  

- step1) 소스파일 수정
  - nano를 통해 소스파일 수정
  - `git diff` 를 통해 수정 내용 확인
- step2) `git add` 를 통해 commit 준비
  - `git status` 를 통해 commit 준비 완료상태 확인
- step3) commit 만들기
  - `git commit` 을 통해 커밋 진행
  - `git show` 를 통해 내가 작성한 commit 확인



```bash
# 진행한 commit 전체적으로 보기
$git log --oneline

# 최신 2개만 볼 수도 있다
$git log --oneline -2
```



### 오픈소스 프로젝트에 내가만든 commit 작업 제출하기

- Git push & Github Pull-request

- 여기서 pull-request는 github의 기능이다  
  이에, push를 먼저 진행하지 않으면, 즉 서버에 먼저 업로드를 해놓지 않으면  
  pull-request를 진행할 수 없다
- pull-request  
   : 내가 Fork한 저장소에, push 해놓은 내용들을 오리지널에 pull(당기기) 해달라고 요청

![image-20200726175553724](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726175553724.png)



- Pull-request 버튼을 누르면, 원본에 바로 들어가는 것이 아니라,  
  게시판으로 이동하게 됨
- 보통, 공식적인 오픈소스를 upstream이라 부르고,  
  내가 fork 뜬 부분을 origin 이라 한다



1) 내가 만든 수정내역(commit) 제출 준비하기

```bash
# 나의 작업 브랜치 fix-mnist 확인하기
$ git branch

# 내가 작성한 2개의 commit을
# 나의 fork 저장소 github에 업로드해서
# 내가 만든 수정내역(commit) 제출(pull-request) 준비하기
$ git push origin fix-mnist
```

- GitHub id와 password를 입력해서 진행
- 이와 같이, push를 진행하면 github에 해당내용이 올라가게 된다
- 이렇게 내가 fork 뜬 repository에 올린 내용은 pull-request를 진행할 수 있다



2) Fork한 저장소(프로젝트) github에서 commit 제출하기

- 첫번째 방법

![image-20200726182722221](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726182722221.png)



- 두번째 방법

![image-20200726182745299](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726182745299.png)



3) Pull-request 게시판에서 내가 제출한 commit 확인

- 이렇게 진행한 PR은 Pull requests 게시판에서 확인할 수 있다

![image-20200726182848421](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726182848421.png)





### Git 도구 : 소스코드 파일 다루기 - 기본 실습



1) 파일 수정하고 수정내용(diff) 확인하기

```bash
# 원하는 편집기로 mnist/main.py 파일 수정 후
$ nano mnist/main.py

# 수정한 내용 확인하기
$ git diff
```

- diff 는 가장 최신역사로부터 달라진점을 보여준다
- add를 하고나서 보면 안보임



2) 수정 내용 잠시 저장(stash)해두기

``` bash
# 수정한 파일 확인하기
$ git status

# 수정한 내용 잠시 저장(stash)하기
$ git stash

# 현재 소스폴더 상태 확인하기: 아무 수정분 없음을 확인
$ git status

# 잠시 저장(stash)해둔 내용 복구
$ git stash pop

# 복구된 수정한 파일 확인하기
$ git status
```

- commit하기에는 애매하고, 잠시 임시저장할 때 사용
- 작업을 하다가, 테스트를 해봐야 할때도 자주 사용
- 특히, 지금 작업을 하다가 branch를 이동해서 다른 작업을 하려할 때,  
  현재 작업분이 있으면 branch를 옮기는게 잘 안될 때가 있다
- 이럴 때, 임시저장을 해두고 branch를 옮겨서 작업을 한 후,  
  다시 돌아와서 stash pop으로 임시 저장 분을 꺼내서 작업



3) 수정한 파일 복구하기

```bash
# 수정한 내용 확인하기

$ git diff
# diff 결과 출력

# 파일 수정한 내용 최신 역사를 기준으로 복구하기
# checkout 의미 local git 저장소 에서 "가져오다 / 대출받다" 의미
# .git 이라는 로컬 저장소 도서관에서 히스토리 내용을 가져온다는 것과 유사하다
$ git checkout -- mnist/main.py

# 최신 역사 기준으로 파일내용 복구 후 내용 확인
$ git diff
# 변화 없기에 출력결과 없음

$ nano mnist/main.py
# 내용 확인해보면 최신 역사로 복구 되어 있음
```



4) 파일 복구하기 (checkout) vs 수정 내용 저장 (stash) 의 차이점

- checkout은 수정분을 완전히 날리고, 최신 역사로 돌아감
- stash는 수정분을 임시로 저장하고, 최신 역사로 돌아감



5) commit을 준비하는 add 명령 취소하기 (reset)

```bash
# 수정한 내용 기준으로 commit 할 준비하기
$ git add mnist/main.py

# commit 할 준비완료 상태 확인
$ git status
# changes to be committed: 결과가 출력됨

# add 명령 취소
$ git reset

# commit 할 준비취소 후 상태 확인
$ git status
# Changes not staged for commit :
# 커밋 준비가 안되었다는 내용이 출력됨
```



6) 소스 수정내역 (commit) 삭제 하기

```bash
# 수정한 내용 기준으로 commit 할 준비하기
$ git add mnist/main.py

# commit 만들기
$ git commit -m "Add import json"

# 생성한 Commit 정보 확인하기
$ git show
$ git log --oneline -1

# commit 정보 삭제하기
# 참고 : HEAD~1 은 가장 위에서 첫번째 내용을 삭제한다는 의미
$ git reset --hard HEAD~1

# 삭제 후 가장 최신 commit 확인하기
$ git log --oneline -1
```



7) 라이센스 서명 넣어서 commit message 적기

```bash
# 수정한 내용 기준으로 commit 할 준비하기
$ git add mnist/main.py

# commit 만들기
# -s 옵션 포함시 라이센스 서명을 의미하는 Signed-off-by 내용을
# commit message 안에 포함하게 된다.
$ git commit -sm "Add import requests"

# commit message 안에 Signed-off-by 확인
$ git show
```

- signed-off-by는 라이센스를 잘 의미했다는 서명 표시
- 위에 뜨는 Author 저자 표시와는 의미가 좀 다르다
- 오픈 소스 입장에서는, 간혹 오픈소스임에도 본인 코드에 대한 소유권을 주장하고 소송하는 경우가 있었음
- 실제 실습에서는 CLA라고하는, Contributor Lisence Agreement 라는  
  내가 이 라이센스를 이해하고 진행한다는 회원가입 인증절차 밟게 될 것
- 요즘은 서명인 -s를 안넣고 CLA 회원가입을 통해서 진행할 수 있음
- 프로젝트마다 조금씩 다르기에 참고해야함  
  어떤 프로젝트에는 서명을 넣는게 필수인 경우도 존재한다



8) commit 수정하기 (amend)

```bash
# 최신 Commit 수정이전 commit ID 확인하기
$ git log --oneline -1
106d167 (HEAD -> fix-mnist) Add import requests

# 수정한 내용 commit 할 준비하기
$ git add mnist/main.py

# 최신 commit 수정하기
$ git commit --amend

# 최신 commit 수정 이후 commit ID 확인하기
$ git log --oneline -1
9786c82 (HAED -> fix-mnist) Add import requests
```

- 수정을 하고 싶은데, commit을 새로 하고 싶지는 않은 경우,  
  최신 히스토리 기준으로 수정분을 더할 수 있다
- 여기서 amend를 하면, 수정에 대한 commit ID는 변경된다
  - 수정 시간이든, 내용이든 어떠한 부분이라도 변경되면 commit ID가 변경된다
- 여기서 HEAD는 가장 최근 히스토리를 의미한다





### Git 도구 : 소스코드 파일 다루기 - 고급실습

- 오픈 소스 입장에서, 메인테이너보다는 컨트리뷰터로서 더 자주 사용하는 내용들을 위주로 학습



1) 나의 Fork 저장소와 오픈소스 Github 저장소 설정

```bash
# 오픈소스 공식 github 프로젝트 URL
# upstream 으로 등록 하기
$ git remote add upstream https://github.com/taeung/pytorch-example

# origin: 나의 Fork 저장소 Github URL
# upstream: 오픈소스 공식 github URL (또는 팀프로젝트 URL)
$ git remote -v
origin https://github.com/gentlelinuxer/pytorch-example.git (fetch)
origin https://github.com/gentlelinuxer/pytorch-example.git (push)
upstream		https://github.com/taeung/pytorch-example (fetch)
upstream		https://github.com/taeung/pytorch-example (push)
```

- remote가 두개 존재한다
  - original 과 upstream
  - remote 주소가 꼭 github은 아니다 - 회사 레포지토리 일 수도, IP일 수도 있다
- 보통 origin만 remote로 존재한다면 backup용으로 사용하고 있는 것
- upstream이 필요한 경우는, rebase를 사용해야할 경우 때문이다
- 다른 사람이 진행한 내용이 먼저 merge가 되었을때, 다시 rebase를 받아야한다
- 이게 혼란스러우면, 모두 collabrator로 등록하고 push 권한을 열수도 있겠지만,  
  오픈소스 개발 방향과는 맞지 않는 이야기



![image-20200726210650057](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726210650057.png)



![image-20200726210702215](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726210702215.png)



![image-20200726210710148](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726210710148.png)



- 다른 사람의 commit이 먼저 merge된 경우, base가 바뀐 것과 마찬가지이므로  
  이것을 토대로 작업을 진행 해야한다
- 메인테이너는, PR을 받는 입장에서 종종 컨트리뷰터가 commit 을 날리면  
  commit은 좋은데, rebase를 먼저 진행하고 오라는 이야기를 함

- Pull은 fetch + merge 이다
- pull 받는 것 보다, 디테일하게 들어가려면 fetch를 먼저 하는 방법이 있다
- 이렇게 local에 받은 내용은 내가 fork했던 repository에 다시 push 되어야함
- rebase를 받는 경우, 바뀐 base와 내가 작업했던 내용이 겹쳐서 충돌이 일어나는 경우에는,  
  수동으로 작업을 해줘야함  
  git이 이 부분까지 자동으로 해결할 수는 없음
- rebase 과정을 조금 더 구체적으로 보면,  
  중간에 commit이 끼어 들어간 것이 아니라,  
  rewind를 통해 기존 base로 되감기를 하였다가, 그 위에 새로운 base를 갱신하고,  
  작업한 부분을 쌓는 방식으로 진행된다

![image-20200726211711879](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726211711879.png)



- github이 꼬여있는 경우, force push를 해야하는 경우도 있음
  - github은 서버 저장소일 뿐이기에,  
    꼬여있는 경우에는 local에서 정리를 잘 한다음에, force push하는 방식으로 진행
- 컨트리뷰터 입장에서 fork한 저장소에 force push하는 경우가 많음
- 하지만, upstream은 여러명이 사용하는 곳이서 force push하는 경우는 거의 없음
- 이렇게 push를 하면, 이전에 보낸 pull-request에 대하여 github이 자동으로 갱신하여  
  반영시켜준다

![image-20200726211728584](/Users/sjeon/Library/Application Support/typora-user-images/image-20200726211728584.png)



2) 오픈소스 Github 최신 소스수정내역(commit)으로 Base 업데이트 : Rebase

```bash
# 공식 upstream 저장소에서 최신 Commit history 가져오기
$ git fetch upstream master

# 최신 commit history 기준으로 베이스 갱신 (rebase)
$ git rebase upstream/master

# Fork한 저장소(Github)도 수정하기 (PR 자동 갱신)
$ git push --force origin fix-mnist
```

- 여기서 upstream/master는 fetch로 가져오게되면 생기는 local branch의 명칭이다
- 기존에서 master라는 이름의 branch가 존재하므로 구분하기 위해 이와같이 명명된다
- rebase가 정상적으로 되었다면,  
  `git log --oneline`을 해보았을 때, 내가 commit한 부분 아래에 base가 잘 교체되어있다



3) 수정내역(commit) 과거 시점으로 되감기 : rewind

```bash
# Rebase 실습할 github 저장소 소스폴더 다운받고 이동
$ cd /workspace
$ git clone https://github.com/taeung/git-training
$ cd git-training

# 가장 오래된 commit 두번째로 되감기
# 두번째로 가장 오래된 commit 의 "pick" 글자를 "edit"으로 수정하기
# nano 편집기 저장 : Ctrl + O
# nano 편집기 나가기 : ctrl + x
$ git rebase -i --root

# 되감기(rewind)한 commit 리스트 확인하기
$ git log --oneline

# rebase intreactive 상태 (되감은 상태) 확인
$ git status

# 참고: --root 대신 최신 commit 기준으로 HEAD~10 10개중에서 선택도 가능하다
```



4) 수정내역(commit) 다시 현재시점으로 풀기(continue)

```bash
# 되감은 내용 풀기(continue) 이전 상태 확인
$ git log --oneline

# 되감은 내용 풀기 (continue)
$ git rebase --continue

# 되감은 내용 풀기(continue) 이후 상태 확인
$ git log --oneline

# 참고: rebase 과정을 취소하려면 --abort 옵션을 사용할 수 있다
```

- `git rebase -i --root` 에서 pick을 edit으로 여러 군데를 수정하게 되면,  
  각각 break point 역할을 한다
- 즉, rewind는 가장 오래된 edit 지점으로 가고 continue를 할때 마다,  
  edit으로 수정된 부분으로 이동하게 된다
- 가장 최신상태로 돌아오려면 continue를 edit으로 변경한 숫자만큼 진행해줘야한다.
- 오픈소스 작업을 하다보면, 리뷰나 디스커션이 자주 일어나게 되고,  
  그러면 commit을 수정해야하는 경우들이 많이 발생하게 된다
- 예로 Commit을 10개했는데, 메인테이너가 3번째, 4번째의 Commit 수정을 요구한다면,  
  rewind를 통해 3번째로 감아서, amend로 수정을 하고 돌아오는 방법이 가능하다
- amend는 최신 히스토리만 수정가능하지만, rewind로 이전 과거로 돌아갈 수 있다면,  
  중간 내용도 모두 수정할 수 있게 된다



5) 특정 소스코드 라인에서 버그가 있을 때 Commit 정보 찾기 : blame

```bash
# 해당 소스파일을 누가 수정했고 언제 수정했는지
# 소스 라인 기준으로 Commit 정보를 찾아 낼 수 있다

$ git blame cmds/record.c
...
edf805aad cmd-record.c (Taeung Song 2018-04-16 ... 102)
edf805aad cmd-record.c (Taeung Song 2018-04-16 ... 103)
...

# 103 번째 라인은 누가 수정했고, 언제 수정했는지 왜 수정했는지
# Commit 정보를 확인할 수 있다.
# blame은 최신 Commit 기준으로 보여준다
$ git show edf805aad
```

 

