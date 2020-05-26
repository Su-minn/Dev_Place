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
  
  2) Mutable operations은 값을 변경할 수 있다는 의미  
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
one tab을 통해서 function의 body임을 알려줌
   
 - 함수 호출(실행) 시에는 function_name() 으로 호출  
function_name 뒤의 ()를 button을 누르는 행위와 같다고 생각하면 기억하기에 편하다  
   function_name은 button이고, ()가 button을 누르는 행위
   
   ```python
   def say_hello():
   	print("hello")
   	print("bye")
   	
say_hello()
   ```
   
   

## 1.5 Function Arguments

- 함수에 전달하는 인자

  ```python
  def say_hello(who="anonymous"): // default 설정
  	print("hello", who)
  
  say_hello("Nico") // result : hello Nico 출력
  say_hello() // result : hello anonymous 출력
  ```

- 인자 2개인 경우

  ```python
  def plus(a, b):
  	print(a + b)
  	
  plus (2, 5)
  
  def minus(a, b=0): // b가 없는 경우, default를 0으로 설정
    print(a - b)
    
  minus(2) // result : 2 출력
  ```

  

## 1.6 Returns

```python
def p_plus(a, b):
	print(a + b) // 5 출력

def r_plus(a, b):
	return (a + b)

p_result = p_plus(2, 3)
r_result = 5

print(p_result, r_result) // result None, 5 출력
```

- return은 함수를 종료한다
  

## 1.7 Heart


Nico Says,

> I just wanted to say: Thank you for being here and watching our lectures.
> See you on the next video!



## 1.8 Keyworded Arguments

- 지금까지 사용한건 Positional Argument이다
  인자인데 위치에 의존적인 인자라는 의미
  위치에 따라 쌍을 이뤄서 대입 됨

- Keyword Argument : 위치가 아닌, 이름으로 쌍을 짓는 인자
  인자가 많아질때, 순서와 상관없이 input 가능하므로 유용함
  더 나은 방법임

  ```python
  def plus(a, b):
  	return a - b
  	
  result = plus(b=30, a=1)
  print(result) // result : -29 출력
  ```

  ```python
  def say_hello(name, age):
  	return f"Hello {name} you are {age} years old"
  // 또는
  	return "Hello " + name + " you are " + age + "years old"
  // 로 작성
  
  hello = say_hello(name="nico", age="12")
  print(hello)
  ```

- string안에 변수를 포함시키고 싶으면 앞에 f (format)을 앞에 적어주고,
  변수를 {}로 감싼다
  아니면 아래 방법 처럼, 변수를 string 밖으로 빼내야한다

  

## 1.9 Code Challenge!

- 7가지 function 만들기
  주의 해야할 점 : user는 완벽하지 않다!

  ```python
  def plus(a, b):
  	return a + b
  	
  plus(12, "10") // error 발생하므로, 예외처리 하기 / hint : type변환
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

- if ~~ :
  else : 과 같이 구성
  뒤에 : 를 붙여야한다

- Python Standard Library에서 Truth Value Testing을 보면

  is 존재 // is : object identity
  str : string의 type

```python
def	plus(a,b):
	if type(b) is int or type(b) is float:
		return a + b
	else :
		return None
	
print(plus(12, 1.2)) // result : 13.2
```



## 1.11 if else and or

- x or y  / x and y 등의 여러 조건문 존재
- elif = else if

## 1.12 for in

-   반복문

- 사용법 : for x(변수 이름) in days(sequence)

  > 배열의 요소를 순차적으로 가리키고 싶을 때 사용한다

  ```python
  days = ("Mon", "Tue", "Wed", "Thu", "Fri")
  
  for day in days:
  	if day is "Wed":
  		break
  	else
  		print(day) // result : Mon Tue 출력
  ```

  

## 1.13 Modules

- 파이썬에는 module 이 내장되어있다
  module : 기능의 집합 / import해서 사용
  설치할 필요 없이 Import해서 사용 / 기본적으로 제공하는 모듈에 한해

- 누가 만든 것은 다운로드 받아서 import 할 수 도있고, 기본 제공 모듈을 가져올 수도 있으며, 내가 만든 것을 가져올수도 있음

- ex) ceil : 올림

  ```python
  import math
  
  print(math.ceil(1.2)) // result : 2
  ```

- import의 주의사항, 예로 math를 import하면 모든 math를 다 가져오므로, 필요한 함수만 import해서 사용

  ```python
  from math import ceil, fsum
  
  print(ceil(1.2))
  print(fsum([1, 2, 3, 4, 5, 6, 7]))
  ```

- as로 이름을 바꿔줄 수도 있음

  ```python
  from math import fsum as sexy_sum
  
  print(sexy_sum([1, 2, 3, 4, 5, 6, 7]))
  ```

  