# 1. Theory



## 1.0 Data Types of Python



- 변수(Variable) : 정보를 넣는 곳, 데이터 저장하는 곳  
  ex) a =2, b= 3

- Print 함수 : 결과값을 출력하는 함수  
  ex) print(a+b)

- 변수에 저장 할 떄, 문자열(string)은 따옴표나 쌍따옴표로 둘러싸야함  
  ex) `a_string = "like this"`

- 변수에 넣을 수 있는 것
  숫자, 문자열(String), 참(True) /거짓(False) 등

- 참 거짓
  True / False -> 문자열과는 다르다
  즉, c = False 와 c = "False" 는 완전히 다름  
파이썬에서 참거짓의 첫 문자는 대문자로 적어야함 ( True , False (o) // true, false (x) )
  
- float는 소숫점이 있음 . 다음에 숫자가 떠다닌다고해서 float임

- 자료형이 궁금할 때, type() 을 이용
  ex) `print(type(a_string))`

- None type : 참 거짓이 아닌 없다는 의미 -> 존재하지 않는다는 의미 / python에만 있음

- Python 변수 명명 암묵적 약속
  변수 이름을 길게 적어야할 때,   
  1) 모두 소문자이고,   
2) 단어끼리 분리시키되 _로 연결
  뱀 같이 생겼다고하여 이러한 표기법을 'snake case'라 한다
  ex) super_long_variable = "abcdefghizklmnop"
  
- Code
  
  ```python
a_string = "like this"
  a_number = 3
  a_float = 3.12
  a_boolean = False
  a_none = None
  ```
  
  

## 1.1 Lists in Python



- Python에는 열거형 타입(sequence type)이 존재

- List와 tuple

- 이번 시간에는 list학습
  List : 많은 값(value)을 열거하여 하나로 저장하고 싶을 때 사용

- 사용 법 : 대괄호로 묶고, 콤마로 구분  

  ```python
  days =  ["Mon", "Tue", "Wed", "Thur", "Fri"]
  ```

- print(type(days))로 type을 확인하면
  <class 'list'> 라는 결과가 나온다 - type이 list라는 의미

- 파이썬 학습 시 파이썬이 어떻게 동작하는 지에 대한 Document를 알고싶다면,   
  [Python Standard library](https://docs.python.org/3/library/) 사이트를 참고할 것

  > 검색창에 python standard library를 검색해서 들어가거나
  > Docs.python.org를 주소창에 입력

- List의 연산자에는 common & mutable sequence operations가 있음 
  1) common operations  

  - ex) x in s   
    : s(sequence)안에 x가 있는가  
  
    result : True (존재하는 경우) // False (존재하지 않는 경우)
  
    - `print("Mon" in days)`
  
      > 결과 True
  
    - `print("Man" in days)`
  
      > 결과 False
  
  - ex) s[i] (index)
    
  
  : s에서 i번째 item을 가리킴  
  
    cf) index는 0부터 시작  
  
    - `print(days[0])`
    
      > 결과 Mon
    
  - ex) len(s) function
  : s(sequence)의 길이를 알고 싶을 때 사용
    
  - `print(len(days))`
    
      > 결과 5
  
  2) Mutable operations
  : 값을 변경할 수 있다는 의미 -> 값을 변경하는 연산자  
  cf) immutable은 값 변경 불가  
  값을 바꾸고 싶지 않을 때는 immutable sequence에 넣어야함  
  ==list는 mutable sequence이다==
  
  - ex) days.append(x)  
      : list (days)에 x를 추가
  
    ```python
    days = ["Mon", "Tue", "Wed", "Thur", "Fri"]
    print(days) 
    # result : ['Mon', 'Tue', 'Wed', 'Thur', 'Fri']
    days.append("Sat")
    print(days) 
    # result : ['Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat']
    ```
  
  - ex) days.reverse()
    : list의 순서를 역방향으로 재정렬
  
    ```python
    days.reverse()
    print(days)
    # result : ['Sat', 'Fri', 'Thur', 'Wed', 'Tue', 'Mon']
    ```
  
    
  
  
  

## 1.2 Tuples and Dicts



- tuple
  : immutable sequence type(수정 불가능)
  list는 common and mutable sequence operations 모두 가능하지만,
tuple은 common operations만 가능하다
  
  - 사용법 : ( )로 감싸고 ,로 구분한다
  
    ```python
    days = ("Mon", "Tue", "Wed", "Thur", "Fri")
    
    print(type(days))
    # result : <class 'tuple'>
    ```
  
    리스트와 같은 구조를 갖고 있지만, 변경할 수는 없다
    이에, 약 list의 50%정도 일들을 처리할 수 있다
  
    
  
- dictionary (사전과 유사)
  : key와 value로 구성
  변수는 둥둥떠있고 어디 한곳에 모여있는게 아니다
  그래서 dictionary를 사용하여 하나의 object를 만든다

  - 사용법 : {}로 감싸고
    "key" : value, "key" : value 나열하여 작성 / ,(comma) 를 이용하여 구분

  ```python
  nico = {
  	"name" : "Nico",
  	"age" : 29,
  	"korean" : True,
  	"fav_food" : ["kimche", "bulgogi"]
  }
  
  print(nico)
  # result : {'name' : 'Nico', 'age' : 29, ~~~}
  print(nico["name"])
  # result : Nico
  
  # dictonary에 새로운 값을 추가하고 싶을 때 아래와 같이 코드 작성
  nico["handsome"] = True
  print(nico)
  # result : {'name' : 'Nico', 'age' : 29, ~~~, 'handsome' : True}
  
  ```

  

- cf) [repl.it](https://repl.it/) : online으로 python(그 외 언어도 가능)을 complie 할 수 있는 website  
  Tip) repl.it에서 코드 깔끔하게 정리하기 - 마우스 오른쪽 format doucment 클릭



## 1.3 Built-in Functions



- 함수(function)
  : 어떤 행동(기능)을 갖고 있고, 반복가능한 것

- print()와 type() 같은 것은 python의 기본 함수
  [Python Standard library](https://docs.python.org/3/library/)에서 built-in functions part를 참고하면  
  기본적으로 사용가능한 python 내장 함수들을 확인할 수 있다.  

- ex) 타입 변환 함수  
  Int(), float() 등

  ```python
  age = "18"
  print(type(age))
  # result : <class 'str'>
  
  n_age = int(age)
  print(type(n_age))
  # result : <class 'int'>
  
  ```

  

## 1.4 Creating a Your First Python Function



 - 함수 사용법 
   : ()로 끝나거나, ()안에 무엇인가가 들어감

 - python에서는 함수를 만든다하지않고, 정의(define)한다고 함

 - 사용법 
   : `def function_name():`
   파이썬은 { } (bracket)로 함수의 시작과 끝을 판단하지 않고,
   indentation (들여쓰기)로 function의 시작과 끝을 판단한다 (tab 이용)
one tab(= 4칸 공백) 을 통해서 function의 body임을 알려줌
   
 - 함수 호출(실행) 시에는 function_name() 으로 호출  
function_name 뒤의 ()를 button을 누르는 행위와 같다고 생각하면 기억하기에 편하다  
   function_name은 button이고, ()가 button을 누르는 행위
   
   ```python
   def say_hello():
   	print("hello")
   	print("bye")
   
   say_hello()
   # result : hello bye
   ```





## 1.5 Function Arguments



- Arguments - 함수에 전달하는 인자
: 함수의 ()안에 값을 넣어서 전달
  유효한 값이라면 무엇이든 전달할 수 있다
  
  - Positional Arguments 와 Keyworded Arguments 로 나누어짐
  - Positional Argument
    : 위치에 의존적인 인자
    인자를 받는 순서대로 함수가 받는 값에 대응됨
  
- 지금은 Positional argument를 먼저 알아보자.

  ```python
  def say_hello(who):
  	print("hello", who)
  
  say_hello("Nico")
  # result : hello Nico
  
  say_hello()
  # result : error
  ```

- default 설정

  - 사용법
    : 인자(argument) = value
  - 함수가, 전달 받아야하는 인자를 받지 못하는 경우 에러를 출력하게 됨
    인자를 받지 않는 경우, 기본 값(default)을 설정할 수 있음

  ```python
  def say_hello(who):
  	print("hello", who)
  
  say_hello()
  # result : error 출력
  
  def say_hello(who="anonymous"):
  	print("hello", who)
  
  say_hello("Nico")
  # result : hello Nico
  
  say_hello()
  # result : hello anonymous
  ```

   

- 인자 2개인 경우

  ```python
  def plus(a, b):
  	print(a + b)
  	
  plus (2, 5)
  # result : 7
  
  def minus(a, b=0): # b가 주어지지 않는 경우, default를 0으로 설정
    print(a - b)
    
  minus(2)
  # result : 2
  ```
- print() 도 function이며, 무한대의 인자를 가질 수 있다.



## 1.6 Returns



- 값을 단순히 출력하는 것(print)과 반환하는 것(return)에는 큰 차이가 존재한다
- 실제로 함수를 다루다보면 input으로부터 return 하게되어 나온 그 값(output)을
  주로 사용하게된다.
- 아래 실제 코드를 통해 비교해보자. 

  ```python
def p_plus(a, b):
	print(a + b)
	
	# result : 5
	
	def r_plus(a, b):
		return a + b
	
	p_result = p_plus(2, 3)
	r_result = r_plus(2, 3)
	
	print(p_result, r_result)
	
	# result : None, 5
	```
- return 
  : function을 호출 할 때, function을 return 값으로 치환시켜준다

  - 여러 개의 값을 반환할 수 있으며, 이 경우에는 튜플이 반환된다 (언패킹)

  - 파이썬에서는 괄호 없이 값을 콤마로 구분하면 튜플

  - ```python
    return 1, 2
    
    return (1, 2)
    
    # 같은 표현
    ```

  - 또한, return은 함수를 종료시킨다




```python
 def r_plus(a,b):
  	return a + b
  	print("This sentence is not printed")
  	
  r_result = r_plus(2,4)

  print(r_result)

# result : 6

# r_plus 함수에서 return 이후의 내용은 실행되지 않는다
```

 



## 1.7 Heart



Nico Says,

> I just wanted to say: Thank you for being here and watching our lectures.
> See you on the next video!



## 1.8 Keyworded Arguments



- 지금까지 사용한 인자는 Positional Argument  
  : 위치에 의존적인 인자  
  위치에 따라 쌍을 이뤄서 대입 됨  

- Keyword Argument  
  : 위치가 아닌, 이름으로 쌍을 짓는 인자  
  인자가 많아질 때, 순서와 상관없이 input 가능하므로 유용함  
더 나은 방법임
  
  ```python
  def plus(a, b):
  	return a - b
  	
  result = plus(b=30, a=1)
  print(result)
  # result : -29
  ```

- return 값이 string인 경우 
  : `return "string"` 의 형식으로 작성

  ```python
def say_hello():
return "Hello"
  ```

say = say_hello()

print(say)
# result : Hello
  ```

  - return 값이 변수를 포함한 string인 경우,  
    : 방법 1) 앞에 f (format)을 앞에 적어주고, 변수 들을 {}로 감싼다 (추천)  
      방법 2) 변수는 string 밖으로 분리하고, + 를 통해 연결 한다  

  ```python
  def say_hello(name, age):
  	return f"Hello {name} you are {age} years old"
  # 또는
  	return "Hello " + name + " you are " + age + "years old"
  # 로 작성
  
  hello = say_hello(name="nico", age="12")
  
  print(hello)
  # result : Hello nico you are 12 years old
  ```

  

  

## 1.9 Code Challenge!



- 7가지 function 만들기
  주의 해야할 점 : user는 완벽하지 않다!

  ```python
  def plus(a, b):
  	return a + b
  	
  plus(12, "10")
  # error 발생하므로, 예외처리 하기 / hint : type변환
  ```
  
  
  - plus
  
  - minus
  
  - times
  
  - division
  
  - negation
  
  - power
  
  - Reminder
    
    
## 1.10 Conditionals Part One



- 조건문을 알아보자

- if -else는 조건문이라고 부르며, 기본적으로 소프트웨어 logic을 컨트롤하는 방법
  대부분, 거의 모든 프로그래밍 언어는 조건문을 가지고 있음
  
- ```python
  if CONDITION:
  	1~~
else:
    2~~
  ```

   과 같이 구성

  - 뒤에 항상 : 를 붙여야함에 유의
  - CONDITION이 TRUE라면 if절에 걸린 내용 (1~~) 이 실행되고
    FALSE라면 else절에 걸린 내용 (\~\~)이 실행된다
  
- 예시

```python
def	plus(a,b):
	if type(b) is int or type(b) is float:
		return a + b
	else :
		return None
	
print(plus(12, 1.2))
# result : 13.2
```

- is
  : truth value testing 앞과 뒤의 객체가 서로 같은지 확인 (object identity)   

  Python Standard Library에서 Truth Value Testing 참고  

- or 
  : A or B 에서, 둘 중 하나 이상이 참인 경우 전체 문장의 값은 참(true)이 되고,  
  하나라도 거짓인 경우, 전체 문장의 값은 거짓(false)이 된다

  cf) str : string의 type

  

## 1.11 if else and or



- 조건문에서는 여러 Boolean Operations를 사용

  - and
    : x and y에서
    x와 y가 둘다 true 인 경우에만, true
  - or
    : x or y에서
    x와 y 중 하나 이상이 true이면, true
  - not
    : not x에서
    x가 false인 경우 true

- ```python
  def age_check(age):
  	print(f"you are {age}")
  	if age < 18:
  		print("you cant drink")
  	else:
  		print("enjoy your drink")
  		
  age_check(18)
  ```

  

- if를 여러개 쓰고 싶을 때 (중첩 if 문)
  elif 를 사용한다

- elif = else if

- if와 elif의 모든 condition에 속하지 않는 경우,
  else에 속한 내용이 실행된다

  ```python
  def age_check(age):
  	print(f"you are {age}")
  	if age < 18:
  		print("you cant drink")
  	elif age == 18:
  		print("you are new to this!")
  	elif age > 20 and age < 25:
  		print("you are still kind of young")
  	else:
  		print("enjoy your drink")
  		
  age_check(18)
  ```

  

## 1.12 for in



-   반복문
    : 어떤 행위를 반복적으로 하고 싶을 때 사용

- 사용법 
: for x(변수 이름) in days(sequence)
  
> 배열의 요소 (such as a string, tuple or list) 를 순차적으로 가리키고 싶을 때 사용한다

  ```python
  days = ("Mon", "Tue", "Wed", "Thu", "Fri")
  
  for day in days:
  	if day is "Wed":
  		break
  	else
  		print(day)
# result : Mon Tue
  # 변수 day는 for문이 실행될 때 생성됨
  
  for x in [1, 2, 3, 4, 5]:
    print(day)
  # result : 1 2 3 4 5
  
  for letter in "nicolas":
    print(letter)
  # result : n i c o l a s
  ```

  

## 1.13 Modules



- 파이썬에는 module 이라는게 내장되어있다
  module   
  : 기능의 집합이며, import해서 사용한다  
기본적으로 제공하는 모듈은 설치할 필요 없이 Import해서 사용  
  import만 해주면, module이 제공하는 함수를 사용할 수 있다  
  
- 누가 만든 것을 다운로드 받아서 import 할 수 도있고,   
  기본 제공 모듈을 가져올 수도 있으며,   
  내가 생성한 파일에서 정의한 기능을 import 할 수도 있음  

- ex) ceil : 올림, fabs : 절댓값

  ```python
  import math
  
  print(math.ceil(1.2))
  # result : 2
  
  print(math.fabs(-1.2))
  # result : 1.2
  ```

- import의 주의사항  
  : math를 import하면 모든 math의 기능을 다 가져오는데, 이건 비효율적임   
  필요한 함수만 import해서 사용

  ```python
  from math import ceil, fsum
  
  print(ceil(1.2))
  # result : 1.2
  
  print(fsum([1, 2, 3, 4, 5, 6, 7])) 
  # result : 28.0
  ```

- 사용하려는 모듈 함수의 이름을 변경할 수 도 있다
변경법 : `함수 이름 as 변경하고 싶은 이름`
  
  ```python
  from math import fsum as sexy_sum
  
  print(sexy_sum([1, 2, 3, 4, 5, 6, 7]))
  # result : 28.0
  ```

- 기존에 calculator.py 라는 파일이 있고, 이 파일에 존재하는 함수를 쓰고 싶을 때,
  아래와 같이 사용

  - 기존에 존재하는 파일 calculator.py

    ```python
    def plus(a, b):
    	return a + b
    
    def minus(a, b):
    	return a - b
    ```

  - calculator의 함수를 import하여 사용하는 파일 main.py

    ```python
    from calculator import plus, minus
    
    print(plus(1, 2), minus(1, 2))
    # result : 3 -1
    ```

    