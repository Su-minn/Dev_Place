# Unit 1. if 조건문과 while 반복문



## if 조건문

- 조건을 반대로 바꾸는 not 키워드

  not True ==False / not False == True
  Ex) `print(not 3 < 5)`

  ex) 

  ```python
  if not hobo.on_beeper():
  	hubo.drop_beeper()
  ```

- 조건 선택문이 많아지면 복잡해지기에,
  else if를 합친 elif 을 사용

  

## while 반복문

- While 반복문 : 주어진 조건이 참이면 앞에 있는 명령을 계속 사용

  Ex)

  ```python
  while not hubo.on_beeper()
  	hubo.move()
  ```

  

# Unit 2. if 와 while을 사용한 미로 탈출 예제

- 프로그램을 고치기 위해서는 사람이 먼저 코드를 이해해야한다
- 사람이 이해하기 좋게 코드를 작성해야한다
  

## 사람이 이해하기 좋은 코드를 작성하는 원칙

1) commment 영역 입력

```python
# This program lets the robot go arount her world
#	counter clockwise, stopping when he returns
# to the starting point.
```

2) 코드에 의미 있는 이름 붙이기

```python
hubo.drop_beeper()
while not hubo.front_is_clear() // 이렇게 작성하는 것 보다는

def mark_starting_point_and_move(): // 이와 같이 사람이 하이레벨로 이해할 수 있도록 의미 단위로 묶어주는 것이 좋다
	hubo.drop_beeper()
	while not hubo.front_is_clear(): hubo.turn_left()
	hubo.move()
```

3) 여러가지 동작들을 의미 있는 함수 이름으로 정의하고 프로그램을 Top down 형식으로 이해할 수 있도록 하는 것이 중요하다

> 아래의 상세한 부분을 보지 않아도 큰 그림을 볼 수 있도록 해줌



## 프로그램 작성 원칙

1) 간단하게 시작하기
2) 한번에 하나의 작은 작업만 수행하기

> Refinement(상세화) : 조금씩 증가시키기

3) 각각의 작업들이 이전 작업에 영향 주지 않기

> 새로 더한 작업이 이전 작업을 망치지 않도록 주의
> 이전 작업을 확인 후, 조금씩 보수적으로 프로그램을 개선

4) 알기 쉬운 유용한 주석 달기

> 프로그램 명령들을 있는 그대로 쓰지 않기

5) 의미를 이해하기 쉬운 이름 붙이기