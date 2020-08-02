# Lec 01 : 기본적인 Machine Learning의 용어와 개념 설명





## 학습 목표



- 기본적인 머신러닝의 용어와 개념에 대해 알아본다.



## 핵심 키워드



- 머신러닝 (Machine Learning)
- 지도학습 (Supervised Learning) / 비지도학습 (Unsupervised Learning)



## 학습 내용



### Machine Learning

- Limitations of explicit programming

  - Spam filter : many rules
  - Automatic driving : too many rules
  - explicit programming : 하나씩 Rule 들을 명시적으로 알려주는 것

  > 이에 대한 한계로 machine learning 발전

- Machine Learning : "Field of stduy that gives computers the ability to learn without being  
  explicitly programmed"

  - Arthur Samuel (1959)

  >  개발자가 알려주는 것이 아니라, 프로그램이 알아서 학습



### Supervised / Unsupervied Learning



- 프로그램은 머신러닝을 위해 학습을 해야하고, 학습하는 방식에 따라 구분



1) Supervised Learning

- learning with labeled examples - training set

  > 데이터를 가지고 학습

- ex) Image labeling - learning from tagged images  
  Email spam filter - learning from labeled (spam or ham) email  
  Predicting exam score - learning from previous exam score and time spent



- Types of supervised learning
  - Regression
  - Classification
    - binary classification
    - multi-label classification



2) Unsupervised Learning

- ex) Google news grouping, Word clustering

  > 데이터 없이 스스로 학습





# Lec 02 : Simple Linear Regression





## 학습 목표



-  선형회귀 (Linear Regression)의 개념에 대해 알아본다.



## 핵심 키워드



- 선형회귀 (Linear Regression)
- 가설 (Hypothesis)
- 비용함수 (Cost function)



## 학습 내용



### Regression

- Regression toward the mean

  > 데이터들은 전체 평균으로 되돌아가려는, 회귀하려는 속성이 있다는 통계적 원리
  >
  > Francis Galton





### Linear Regression

- 데이터를 가장 잘 대변하는 직선의 방정식을 찾는 것

- Hypothesis : H(x) = Wx + b
  - W는 weight, b는 bias





### Cost

- How fit the line to our (training) data
  - 우리의 가설과 실제 데이터와의 차이
- loss or error 라고도 한다
- 우리가 하려는 것은 이 Cost를 최소하는 방법에 관한 것





### Cost function

- 비용 함수 : 오차 제곱의 평균

<img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200729235245515.png" alt="image-20200729235245515" style="zoom:50%;" />





### Goal : Minimize cost

- 우리의 목적은 cost function을 최소화하는 w와 b를 찾는 것

<img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200729235424585.png" alt="image-20200729235424585" style="zoom: 50%;" />



- Machine Learning 과, Deep Learning에서 Learning의 목적은  
  Cost를 정의하고, 이 cost를 minimize 하는 것





# Lab 02 : Simple Linear Regression를 TensorFlow로 구현하기 





## 학습 목표



- 선형회귀(Linear Regression)을 코드로 구현한다





## 핵심 키워드



- 선형회귀 (Linear Regression)
- 가설 (Hypothesis)
- 비용함수 (Cost function)





## 학습 내용



### Hypothesis and Cost

- Hypothesis : 가설 or 모델 or 예측

  - H(x) = Wx + b

- Cost

  - <img src="/Users/sjeon/Library/Application Support/typora-user-images/image-20200729235245515.png" alt="image-20200729235245515" style="zoom:50%;" />

    

  - cost function은 에러 제곱의 평균 값

  - 학습 : cost가 최소화 되는 w와 b를 구하는 것





# Lec 04 : 





## 학습 목표



- 



## 핵심 키워드



- 



## 학습 내용





# Lec 00 : 





## 학습 목표



- 



## 핵심 키워드



- 



## 학습 내용

