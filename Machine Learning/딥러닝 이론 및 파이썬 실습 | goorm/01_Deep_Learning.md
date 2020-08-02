

# 01_Deep Learning







## 딥러닝 이론 소개



### 딥러닝 ver. brain



- Deep learning idea is from our brain
  - 다양한 머신러닝 기술 중 하나
  - 우리의 뇌의 작동방식에서 착안
- 우리의 뇌 약 1000억 개의 뉴런이 존재

![image-20200730002923189](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730002923189.png)

- Dendrite (수상돌기)  
  :  signal receiver  

  시그널 받는 부분, 한 개의 뉴런에 여러 개가 달려 있다

  - 즉, 뉴런은 여러개 신호를 받게 됨

- Cell body  
  : Sum and threshold signals  

  여러 개의 시그널을 복합적으로 계산하여 하나의 신호를 만들어내어 Axon으로 다른 뉴런 들에게 전달을 함

- Axon  
  : Signal sender to other neurons  
  Axon은 시그널을 보내는 역할, 다른 뉴런의 dendrite에 연결됨

- Synapse  
  : point of contact (axon - dendrite)  
  액손과 dendrite의 결합점



### 딥러닝 ver. code

![image-20200730003540397](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730003540397.png)

- activation function은 여러 종류가 존재
  - 결과 값이 기대치 이상인지 아닌지에 따라 다른 신호를 보내는 함수







## 싱글 인풋 뉴론



![image-20200730003807748](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730003807748.png)



### Single Input Neuron

- 자극이 있으면, 뉴런에 그 자극을 계산하고 다음 뉴런으로 보내는 뉴런



![image-20200730003923896](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730003923896.png)



- 딥러닝에서 구현하면 위와 같다
- 자극이 오면 weight 가중치와 함께 곱해주고,  
  이 결과값을 activation 함수에 인자로 넣어주면 activation은 기댓값 이상인 경우 높은 값을  
  기댓값 이하인 경우 낮은 값을 리턴



![image-20200730004053339](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730004053339.png)





![image-20200730004134721](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730004134721.png)



### Activation functions

- Step function, Sign function, Sigmoid function, Linear function 이외에도  
  훨씬 더 많은 함수 존재



![image-20200730004319595](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730004319595.png)



- activation function을 보다 잘 활용할 수 있도록 bias 추가
- bias 를 추가시켜주는 것 만으로도, activation 함수의 parameter 값을 쉽게 조절할 수 있다



![image-20200730004511237](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730004511237.png)



- bias가 더해지면 위와 같이 표현이 된다









## 멀티 인풋 뉴론 (퍼셉트론)





### single input neuron

- input (p\) 가 들어왔을때, 가중치 (w)와 곱해준 다음, bias (b)를 더해주면, net input (n)이 된다.  
- 이 net input을 activation function에 넣어주면, 결과값으로 1 또는 -1 같은 방식으로 threshold 보다 높은 값이 들어왔을 때 높은 값을, 낮은 값이 들어왔을 때 낮은 값을 activation으로 return 한다.



### real neuron has multiple input

- 우리의 실제 뉴런은 한 개 이상의 input을 받는다.

![image-20200730132109198](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132109198.png)




### 예시

- 눈이 안보이는 로봇에게 사과와 공을 구분하게하기
- 먼저, input 정의하기
  - 눈이 안보이기에   
  - 로봇이 받는 input p1 = shape , p2 = texture 
- input 특성 정의
  - shape = 1 when shape is round
  - shape = 0 when shape is not round
  - texture = 1 when texture is smooth
  - texture = 0 when texture is not smooth
  - define input as [shape, texture]
    - apple : [1, 1]
    - ball : [1, 0]
- output 정의 하기
  - 1 = apple / -1 = ball

![image-20200730132127530](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132127530.png)

![image-20200730132137879](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132137879.png)

- 웨이트나 바이어스도 정해야하지만, 쉬운 예시를 위해 지정함



![image-20200730132207306](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132207306.png)



- 그래프로 표현하면 위와 같은 결과를 얻게 됨

- perceptron only recognize linearly separable pattern

  - 퍼셉트론은 선으로 나누어지는 상황에서만 작동

  

### ex) AND operation

- true와 false 구분을 잘 해낸다

  

- ![image-20200730132222156](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132222156.png)

  

  ![image-20200730132242468](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132242468.png)

  





### ex) OR operation

![image-20200730132252679](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132252679.png)



![image-20200730132302799](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132302799.png)





### perceptron NOT works XOR operation

- 한가지 문제점 발생
  - exclusive OR (배타적 논리합) 에서 작동하지 않는다
- 멀티 인풋 뉴런은 이 문제를 해결할 수가 없다
- 즉 single perceptron은 XOR issue 해결 불가
- linearly separable한 issue에서만 작동
- 하지만 여러개의 퍼셉트론이라면 해결 가능하다







## 멀티 레이어 퍼셉트론(MLP) 소개 및 XOR 풀이





### Multiple neurons in brain

- 실제 우리 뇌에서 아이디어를 얻음
- 우리 뇌에는 많은 뉴런이 존재하고, axon을 통해서 각각의 output을 return함
- 이 output은 다른 뉴런의 input으로 들어감
- 이전에 있던 뉴런들을 하나의 layer (층)이라고 한다
- output을 input으로 받는 뉴런을 현재의 layer라 한다
- 이것이 multi layer perceptron의 간단한 concept



### XOR operation

- single perceptron은 XOR issue 해결 불가

- linearly separable한 issue에서만 작동하기 때문

- 이 문제를 해결하기 위해서, multi layer perceptron 이 필요함
  ![image-20200730132414812](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132414812.png)

  

- XOR 문제를 해결하기위해 weights, bias, activation의 threshold 설정

![image-20200730132430192](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132430192.png)




### input layer

![image-20200730132442858](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132442858.png)



### hidden layer

![image-20200730132455070](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132455070.png)



### output layer

![image-20200730132505201](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132505201.png)



- 이것을 MLP (Multi Layer Perceptron) 이라 한다
- 또는, Feed Forward Neural Network 라 한다







## 텐서 (Tensor)



### tensor 란?

- **어떻게 바라보아도 그 본질이 변하지 않는 것**
  - 좌표의 변형(change of coordinate)에도 그 본성[크기와 방향]을 잃지 않는 객체로서,  
    변형된 좌표계에 따른 고유성분을 가지며(special components),  
    그 성분을 표현할 수 있는 [변환의]방법(predictable way)을 가진 객체
- 다차원 숫자 배열(Multidimensional array of numbers) 혹은 격자(grid)




### tensor의 종류

| rank | type     | example                       |
| ---- | -------- | ----------------------------- |
| 0    | scalar   | [1]                           |
| 1    | vector   | [1,1]                         |
| 2    | matrix   | [[1,1], [1,1]]                |
| 3    | 3 tensor | [[[1,1],[1,1]],[[1,1],[1,1]]] |
| n    | N tensor |                               |

- rank 1 tensor (Vector) : rank0 tensor를 item으로 갖는 tensor
- rank 2 tensor (Matrix) : rank1 tensor를 item으로 갖는 tensor
- rank 3 tensor (3 d tensor) : rank2 tensor를 item으로 갖는 tensor
- rank N tensor : rank N-1 tensor를 item으로 갖는 tensor



### tensor example in NLP

- 자연어 처리 예시
  <img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132625209.png" alt="image-20200730132625209" style="zoom: 33%;" />

  

- 각각의 단어들을 one hot encoding vector로 표현

- 각 word 들은 같은 길이를 가지면서도, 서로 구분되는 vector 값을 가짐

- word를 vector로 표현한 후, 문장도 vector로 표현할 수 있게 됨

- 이 문장 vector representation을 Deep Learning model에 input으로 넣는다

- input을 넣을 때, 각 문장 하나씩 넣어주는 것보다 보통 mini batch로 넣어줌

- mini batch : input으로 하나씩 넣는게 아닌, 뭉치로 넣는 것

![image-20200730132700963](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132700963.png)



- 첫번째 숫자

  - Sample dimension : 몇 개의 sample을 갖고 있는가
    <img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132723521.png" alt="image-20200730132723521" style="zoom:50%;" />

    

- 두번째 숫자

  - max length of sentence : 그 문장에 있는 단어의 갯수가 몇개 인가
    <img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132744458.png" alt="image-20200730132744458" style="zoom:50%;" />

  

- 세번째 숫자

  - word vector dimension : 그 word들이 몇 개의 숫자로 표현 되는가
    <img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132804178.png" alt="image-20200730132804178" style="zoom:50%;" />

  

### Tensor example in image

- image processing에서의 tensor 예시
  ![image-20200730132823352](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132823352.png)

  

- 3d tensor

![image-20200730132838944](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132838944.png)



- 4d tensor



### Tensor example in rgb color video

![image-20200730132850973](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132850973.png)



- 위처럼 각 image는 5 * 5의 row와 column을 가진다고 가정









## 선형과 비선형의 차이 (머신러닝, 딥러닝)



### What is linear in machine learning?

- If linear combinations of coefficients in the model,  
  we call it linear model
- If linear combinations of weights in the model,  
  we call it linear model 
 - 머신러닝 공식에서 계수, 가중치들이 선형결합 관계에 있을 때, 선형 모델이라고 한다
 - 선형결합은 간단히 말하면, 두 벡터의 합이라 볼 수 있다

![image-20200730132933002](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132933002.png)





### regression 에서의 계수

![image-20200730132946489](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730132946489.png)



### Deep Learning 에서의 계수

![image-20200730133000723](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730133000723.png)



- 가중치들이 계수이고, 이 계수들이 선형결합 관계에 있을 때, 선형 모델이라고 한다.



### Linear model example

- log(x), (x_1)^2 등 이 곱해져있다고 하더라도,  
  w1, w2 , ...,  w_n이 선형결합 관계에 있다면 선형모델이다
  <img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200730133014522.png" alt="image-20200730133014522" style="zoom:50%;" />

  

- Can linear model has curve shape?

  - Yes
  - since linear means linear combination of weights, not the shape of function
  - 선형 모델은, 직선을 의미하는 것은 아니다

  

### non-linear model

- ex) logistic regression

- sigmoid 함수를 사용하므로, 비선형 모델
  ![image-20200730133037323](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730133037323.png)

  

- ex) deep learning
  ![image-20200730133049569](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730133049569.png)

  

  - x값이 w와 곱해지는 것 까지는 선형이지만,  
    뒷쪽에서 보통 비선형 함수가 적용됨





## 퍼셉트론 (뉴론, 노드) 텐서플로우로 구현하기



### 기본세팅 참고

- tensorflow 2.0 튜토리얼



### 실제 실습 구현

- 딥러닝에는 노드 (or 뉴런 or 퍼셉트론)이 존재
- 특히, 다층으로 존재하는 경우,  
  MLP, 다층 레이어 퍼셉트론이라고 한다

<img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200730212957018.png" alt="image-20200730212957018"  />



- 이번 실습에서는 하나의 뉴런에만 포커스를 둘 계획
- 뉴런을 이해하면서 레이어를 이해하고, 레이어를 이해해서 딥러닝을 이해하는 방식



### 뉴런 (퍼셉트론)의 구조



![image-20200730213135485](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730213135485.png)



- Activation Function은 이번 실습에서는 step function을 사용할 계획
- 전통적으로는 step function을 사용해왔다
  - x >= 0 인 경우, 1 return
  - x < 0 인 경우, 0 return

- 하나의 뉴런으로도 할 수 있는 것들이 존재
  - AND / OR Operation



### Practice with Tensorflow

```python
import tensorflow as tf
```



### Constants

- We will practice perceptron with AND, OR, XOR operation.  
  For Truth table and bias of the perceptron, we created constant here.

```python
T = 1.0
F = 0.0
bias = 1.0
```



### Collect Data

- Here is the truth table we will solve using perceptron.

![image-20200730214427435](/Users/sjeon/Library/Application Support/typora-user-images/image-20200730214427435.png)



```python
def get_AND_data():
	X = [
	[F, F, bias],
	[F, T, bias],
	[T, F, bias],
	[T, T, bias]
	]
	
	Y = [
			[F],
			[F],
			[F],
			[T]
	]
	
	return X, Y
	
def get_OR_data():
	X = [
	[F, F, bias],
	[F, T, bias],
	[T, F, bias],
	[T, T, bias]
	]
	
	Y = [
			[F],
			[T],
			[T],
			[T]
	]
	
	return X, Y
	
def get_XOR_data():
	X = [
	[F, F, bias],
	[F, T, bias],
	[T, F, bias],
	[T, T, bias]
	]
	
	Y = [
			[F],
			[T],
			[T],
			[F]
	]
	
	return X, Y
```



- Choose date for your practice.

```python
X, Y = get_AND_data()
# X, Y = get_OR_data()
# X, Y = get_XOR_data()
```



### Initialize weights

- We will initialize weight with random number.
- since the truth table has two inputs with one bias,  
  and we just have single neuron, the shape of weight is [3, 1]

```python
W = tf.Variable(tf.random_normal([3, 1]))
```



### Activation Function

- Perceptron uses step function as its activation function.
- step(x) = { 1 if x > 0 ; 0 otherwise }

```python
def step(x):
	return tf.to_float(tf.greater(x, 0))
```



### Loss Function

- We will simply use Mean Square Error as its loss function

```python
f = tf.matmul(X, W)
output = step(f)
error = tf.subtract(Y, output)
mse = tf.reduce_mean(tf.square(error))
```





## 다층퍼셉트론(MLP) 텐서플로우 구현_ 1. XOR







## 다층퍼셉트론(MLP) 텐서플로우 구현_ 2. MNIST





## 조기 종료(Early Stopping) 텐서플로우 실습







## 드랍아웃(Drop Out) 이해 및 텐서플로우 구현







## CNN(Convolutional Neural Network, 합성곱 신경망)





## 텐서플로우 CNN 구현





## RNN 기초 (순환신경망 - Vanila RNN)





## LSTM 쉽게 이해하기