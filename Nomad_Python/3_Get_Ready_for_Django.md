

# 3. Get Ready for Django



## 3.0 Django is AWESOME

- 웹사이트 등을 만들 때, 파이썬을 활용해서 프론트와 백엔드를 모두 만들 수 있는 프레임워크
  

## 3.1 *args **kwargs

- 장고에서 이해해야할 두가지 중요한 컨셉

- 1) arguments & keyword arguments

  2) 객체지향 프로그래밍 : Class, Inheritance (상속), method

- 1) arguments & keyword arguments

- 무한대로 arguments를 넣고 싶다면.
  1) *args 이용 -> position arguments를 받을 수 있음
  2) **kargs 이용 -> keyword arguments를 받을 수 있음

- Arguments 에는 두가지 종료 존재
  1) position arguments : 순서대로 받는 인자 -> 순서 중요
  2) keyword arguments : 이름 명명

  ```python
  def plus(*args):
  	result = 0
  	for number in args:
  		result += number
  	print(result)
  	
  plus(1, 2, 1, 1, 1, 1, 1 ,1 ,1 ,1, 2, 3) // plus에 인자를 무수히 넣었을 때도 계산이 가능한 함수를 만드는 예제 -> *args로 받으면 모든 position 인자를 다 받을 수 있다
  ```

  ```python
  def plus(a, b, *args, **kwargs):
  	print(args) 
  // tuple 형태로 (1, 1, 1, 1, 1, 1, 1, 1 , 1)의 결과를 가짐
  	print(kwargs) 
  // 딕셔너리 형태로 {'hello' : True, 'aa' : True, ~~~}
  	return a + b
  	
  plus(1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, hello=True, aa=True, df=True, ~~)
  ```

  

## 3.2 Intro to Object Oriented Programming

- 객체지향 프로그래밍은 장고를 사용할 때 많이 보게 됨
  코드를 정리하는 방법 중 하나

- class : 설계도

- 사용법 : class name():

  ```python
  class Car():
  	wheels = 4 // class의 properties
  	doors = 4
  	windows = 4
  	seats = 4
  	
  	porche = Car() // Instantiation
  	porche.color = "Red" // Car의 class를 확장
  	
  	ferrari = Car()
  	ferrari.color = "Yellow"
  	
  	mini = Car()
  	mini.color = "White"
  	
  ```

- instance : 설계도로 만든 복제품(결과물)

  위의 예시에는, porche, ferrari, mini가 instance에 해당

- Instantiation : 설계도를 가지고 와서 Instance를 만드는 것



## 3.3 Methods part One

- Method는 class 안에 있는 function

- 만든 모든 method의 첫번째 argument는 그 method를 호출한 인자(argument)이다!

  ```python
  class Car():
  	wheels = 4
  	doors = 4
  	windows = 4
  	seats = 4
  	
  	def start(self): // method (class 안에 존재)
  		print(self.doors)
  		print("I started")
  
  porche = Car()
  porche.color = "Red Sexy Red"
  porche.start()
  // porche.start(porche) 즉 자기자신(instance)을 넣은 상태로 호출하는 것과 같다
  
  
  /*
  def start(): // function (class 밖에 존재)
  	print("I started")
  */
  ```

  

- 파이썬은 모든 함수를 하나의 argument와 함께 사용한다 : self (이름 변경 가능)
  모든 method의 첫번째 argument는 method를 호출하는 instance 자기 자신

  

  

## 3.4 Methods part Two

- 이미 있지만 사용하지 않았던 method들이 존재

- dir(Car)을 살행해보자
  dir은 class안에 있는 모든 것들을 리스트로 보여준다

  ```python
  class Car():
  	
    def __init__(self, *args, **kwargs): // 이미 존재하는 mehtod인 __init override
  		self.wheels = 4
      self.doors = 4
      self.windows = 4
      self.seats = 4
      self.color = kwargs.get("color", "black")
      self.price = kwargs.get("price", "$20") // kwargs는 dictionary 자료구조이고, dictionary는 get이라는 것을 가지고 있음, get(k, d)에서 k는 key d는 default
  			
    def __str__(self): // 기존에 존재하던 __str__ 을 overriding
    	return f"Car with {self.wheels} wheels"
  
  print(dir(Car)) // result : class, __str__ 등 여러 기본적으로 포함하는 method 들이 나옴
  
  porche = Car(color="green", price="$40")
  print(porche) 
  // result : Car with 4 wheels -> print가 porche를 str로 바꿔서 출력하려고 하고, 이는 __str__ method를 자동으로 호출하여 실행됨
  
  mini = Car()
  print(mini.color, mini.price) // result : black $20 (default 값)
  ```

- 결과로 나오는 것들은 class 에 있는 모든 properties

- \__str__은 호출될 때마다 그 class의 instance 출력

- Override : 기존에 있는 것을 재정의 하고 싶을 때 사용



## 3.5 Extending Classes



- 어떤 method가 class에는 어울리지 않으나 (모든 객체에 적용되지 않으나),
  특정 객체에서는 사용될 때

- 상속(Inherit) or 확장(Extend)
  : 기존에 있는 class를 받아서 확장

  ```python
  class Car():
  	
    def __init__(self, *args, **kwargs):
  		self.wheels = 4
      self.doors = 4
      self.windows = 4
      self.seats = 4
      self.color = kwargs.get("color", "black")
      self.price = kwargs.get("price", "$20")
  			
    def __str__(self):
    	return f"Car with {self.wheels} wheels"
  
  class Convertible(Car):
  
   	def __init__(self, **kwargs):
      super().__init__(**kwargs) 
  // 여기서 super은 Car class, argument를 넘겨줘야함
      self.time = kwargs.get("time", 10)
  
  	def take_off(self)
    	return "taking off"
    
  class Something(Convertible):
  ```

- 기존 특정 method를 받아서 확장하고 싶을 때 super 사용

- super()는 부모클래스를 호출하는 함수

- 장고는 좋은 class들의 모음집임, 이것을 잘 활용하여 extend 하고 super등을 활용하여, method 활용할 것 !
  

## 3.6 Whats Next

- Django 와의 연계 추천