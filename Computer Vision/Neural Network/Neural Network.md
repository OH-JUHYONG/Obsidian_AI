---
tags:
  - perceptron
  - neural-network
  - activation-function
  - bias
  - weight
  - sigmoid
  - ReLU
  - identity-function
  - softmax
  - linear-function
  - non-linear-function
  - ResNet
  - CNN
  - FC-layer
  - pooling
---
**cf**[^0]

- **퍼셉트론(perceptron)**: 다수의 신호를 입력으로 받아 하나의 신호(흐름)으로 출력
	⮚   복잡한 함수로 표현
	⮚   가중치 설정하는 작업을 수동으로 해야 한다는 단점

### 신경망

- 가중치 매개변수의 적절한 값을 데이터로부터 ==자동으로 학습==
	![[Pasted image 20231006102715.png]]
	
	⮚   신경망은 **입력층**, **은닉층**, **출력층**으로 구성
	⮚   각 층의 뉴런은 **활성화 함수**에 의해 ==입력값을 출력값으로 변환==   
	⮚   입력층에 가까운 층은 **단순한 패턴**, 출력층에 가까운 층은 **추상적**이고 **복잡**하다는 특징
	⮚   시작점에 따라 최저점이 달라지기 때문에 초기화(가중치 시작하는 위치)가 중요


### 활성화 함수(Activation function)

- 입력의 신호의 총합을 출력 신호로 변환하는 함수
- 입력 신호의 총합이 활성화를 일으키는지를 정하는 역할
	![[Pasted image 20231006103042.png]]
	
	⮚   위 그림을 보면 가중치 신호를 조합한 결과가 _a 노드_ 가 되고, 활성화 함수 _h()_ 를 통과하여 _y 노드_ 로 변화되는 과정을 볼 수 있음
	- **편향(bias)**
		- 뉴런이 얼마나 쉽게 활성화되는지를 제어
		- 각 층에서 이전 뉴런의 값에 영향을 받지 않은 정수
	- **가중치(weight)**
		- 각 입력 신호가 결과에 주는 영향력, 중요도
		- 미분 불가능한 경우 미분 가능하도록 비슷한 함수로 대체
		- 신경망의 각 층에서 입력값을 출력값으로 변환

##### 계단함수
```python
import numpy as np
import matplotlib.pylab as plt

"""
def step_function(x):
	if x > 0:
    	return 1
    else:
    	return 0
"""
# 넘파이를 활용하면 다음과 같이 한줄로 해결할 수 있음
def step_function(x):
    return np.array(x > 0, dtype=np.int) # 넘파이 배열에 부등호 연산을 수행해 bool 배열을 생성

X = np.arange(-5.0, 5.0, 0.1) # -5.0 ~ 5.0 전까지 0.1 간격의 넘파이 배열을 생성
Y = step_function(X)
plt.plot(X, Y)
plt.ylim(-0.1, 1.1)  # y축의 범위 지정
plt.show()
```
![[Pasted image 20231006104431.png]] 


##### Sigmoid 함수

![[Pasted image 20231006104451.png]]
-  0 ~ 1 사이를 mapping
- **확률값**
- 계산시 ReLU보다 오래 걸림
- 기울기 소실 문제 O
- 부드러운 곡선, 입력에 따라 출력이 **연속적으로 변화**
- 입력이 중요하면 출력이 1에 가까워지고, 입력이 중요하지 않으면 출력이 0에 가까워지는 구조
- 주로 **이진 분류**에서 사용
```python
import numpy as np
import matplotlib.pylab as plt

def sigmoid(x):
    return 1 / (1+np.exp(-x))

x = np.arange(-5.0, 5.0, 0.1)
y = sigmoid(x)
plt.plot(x, y)
plt.ylim(-0.1, 1.1) # y축 범위 지정
plt.show()
```
![[Pasted image 20231006104537.png]] 


##### ReLU
- max(z, 0)
	⮚   입력값이 0보다 작으면 0
	⮚   입력값이 0보다 크면 **입력값 그대로** 내보냄

- 학습이 빠름
- 기울기 소실 문제 X
```python
import numpy as np
import matplotlib.pylab as plt

def relu(x):
    return np.maximum(0, x)

x = np.arange(-5.0, 5.0, 0.1)
y = relu(x)
plt.plot(x, y)
plt.ylim(-1.0, 5.5) # y축 설정
plt.show()
```
![[Pasted image 20231006104659.png]] 


##### 항등함수(identity function)
- 입력을 그대로 출력(입력과 출력이 항상 같다는 뜻)
- 출력층에서 사용되는 함수
- 일반적으로 **회귀**(입력 데이터에서 **수치를 예측**하는 문제)에서 사용
![[Pasted image 20231006104734.png]] 


##### Softmax 함수
- 0.0 ~ 1.0 사이의 실수
- **다중 클래스 분류**에(데이터가 어느 클래스에 속하느냐)서 사용되는 함수
- 출력의 총합은 1 (이는 **확률로 해석**할 수 있음)
- 출력층의 각 뉴런이 모든 입력 신호에 영향을 받음
- **overflow 문제가 발생**할 수 있음, 이는 **입력 신호 중 최댓값을 빼주면** 올바르게 계산 가능
- 소프트 맥스 함수를 적용해도 **각 원소의 대소 관계는 변하지 않음**, 이는 지수 함수 y = exp(x)가 **단조 증가함수이기** 때문
![[Pasted image 20231006104910.png]]
```python
def softmax(a):
    c = np.max(a)
    exp_a = np.exp(a - c)  # 소프트맥스 함수 구현시 overflow 발생을 막기 위한 방법
    sum_exp_a = np.sum(exp_a)
    y = exp_a / sum_exp_a

    return y

"""
- overflow 대책을 안한 경우 [ nan  nan  nan] 값이 나와 제대로 계산 X
- overflow를 막은 후 [  9.99954600e-01   4.53978686e-05   2.06106005e-09]의 결과가 나옴
"""
print(softmax(np.array([1010, 1000, 990])))

# 소프트맥스 함수의 특징
a = np.array([0.3, 2.9, 4.0])
y = softmax(a)
print(y)  # [ 0.01821127  0.24519181  0.73659691]
print(np.sum(y))  # 1.0

>>>
[9.99954600e-01 4.53978686e-05 2.06106005e-09]
[0.01821127 0.24519181 0.73659691]
1.0
```


#### Why do neural networks use nonlinear rather than linear activation functions as activation functions?

- 위 함수들을 모두 비선형 함수
- **함수**: 어떤 값을 입력하면 그에 따른 값을 돌려주는 변환기
- **선형함수**: 변환기에 무언가를 입력했을 때 출력이 입력의 상수배만큼 변하는 함수
	⮚   층을 아무리 깊게 해도 은닉층이 없는 네트워크로도 똑같은 기능(표현)을 할 수 있다는 점이 문제
	 ex) _h(x) = kx_ 를 활성화 함수로 사용한 2층 네트워크를 구현
	-  식으로 나타내면 _y(x) = h(h(x))_, 이는 _y(x) = k * k * x = k^2 * x_ 로 표현
	-  이는 _y(x) = cx_ 와 똑같은 식, _c = k^2_ 라고 할 수 있기 때문에 은닉층이 없는 네트워크로 표현할 수 있음
	- 따라서 선형 함수를 이용해서 여러 층을 구성하는 이점을 살릴 수 없기에 층을 쌓는 혜택을 얻기 위해 **비선형 함수**를 사용

- **비선형 함수**: 선형이 아닌 함수, 즉 직선 1개로는 그릴 수 없는 함수


---

- [[ResNet]]
##### CNN
-  [[FC layer]]
-  [[Pooling]]









[^0]: https://ohanthony95.tistory.com/3
