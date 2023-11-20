---
tags:
  - linear-regression
  - weight
  - bias
  - loss-function
  - cost-function
  - score
  - residual
  - chain-rule
  - partial-derivatives
  - direct-solution
  - critical-point
  - feature-mapping
  - numerical-differentitation
  - gradient
  - gradient-descent
  - GD
  - eta
  - learning-rate
  - parameter
  - hyper-parameter
  - fixed-point
  - η
---
:  종속 변수 y와 하나 이상의 독립 변수 x와의 선형 상관 관계를 모델링 하는 기법

- ==학습 데이터와 가장 잘 맞는 하나의 직선을 찾는 일==
- **단순 선형 회귀**: _y = Wx + b_
	- _W_ : 가중치(weight)
	- _b_ : 편향(bias)
	- _y_ : prediction, output

- What is Linear?
	- the target must be predicted as a linear function of the inputs
	![[Pasted image 20231027005245.png]]


#### Loss(objective, error) function
- 실제 값과 예측 값에 대한 오차에 대한 식
- **score**(예측자) = 가중치 벡터 x feature map
	![[Pasted image 20231013223822.png]]
	- _y - t_: **residual**, 값이 작아지는 방향으로 학습
	- 1/2은 나중에 미분할때 계산이 편하라고 놓는 값


#### Cost function
- loss function **averaged** over **all** training examples
	![[Pasted image 20231027005415.png]]

### Solving the optimization problem
- Partial derivatives(편미분)이 중요
- **the chain rule**을 편미분에 적용하면 다음과 같음
	![[Pasted image 20231027013558.png]]
	![[Pasted image 20231027010110.png]]

#### Direct solution
- **Critical Point**(임계점): 도함수가 0이 되는 지점, 최소값은 부분 도함수가 0이 되는 지점에서 발생
- **Direct solution**: 편미분을 0으로 설정하고 매개변수를 푸는 것
	![[Pasted image 20231027012436.png]]

### Feature mapping
- Use a feature mapping that linear regression to learn nonlinear dependencies(비선형 종속)
	![[Pasted image 20231027125527.png]]
	-  polynomial regression을 예로 들음
	- 위 과정(feature mapping)을 통해 linear gression 처럼 할 수 있음


### Generalization
- 최종적으로 모델이 훈련 데이터에서 학습한 패턴을 **새로운 데이터에 적용**할 수 있게 generalization하는게 목표
	![[Pasted image 20231027125944.png]]

### Regularization
- 데이터를 특정 범위로 변환
- 모델 복잡도에 대한 penalty로 정규화는 overfitting을 예방하고 generalization 성능을 높이는데 도움을 줌줌

##### L2 Regularization
- 기존의 cost function에 가중치의 제곱을 포함하여 더함으로써 가중치가 너무 크지 않은 방향으로 학습
- **weigth decay**라고도 함
	![[Pasted image 20231027144418.png]]
	
	![[Pasted image 20231027175530.png]]
	![[Pasted image 20231027175544.png]]
	- 학습이 진행되면서 최신 w에 대한 이전 iteration의 w의 비중은 점점 작아짐


##### L1 vs L2 Regularization
- **L1 norm**: 특정 parameter 값을 0을 많이 만드는 방향
	- feature selection이 가능하기 때문에 sparse model에 적합
	- convex(아래로 볼록) optimization에 유용 
	- 밑에 그림에서 처럼 미분 불가능한 점이 있기 때문에 gradient-base learning에는 주의가 필요
- **L2 norm**: 전체가 작아지는 방향
	- 각각의 벡터에 대해 항상 unique한 값을 낼 수 있음
	![[Pasted image 20231027175837.png]]


---

### 수치 미분(numerical differentiation)

- 경사법에서는 기울기(경사) 값을 기준으로 나아갈 방향을 정함

##### 미분
- 특정 순간의 변화량
	![[Pasted image 20231013224353.png]] 
	- 위 식의 좌변은 _f(x)_ 의 _x_ 에 대한 미분(_x_ 에 대한 _f(x)_ 의 변화량)을 나타낸 기호

```python
import numpy as np
import matplotlib.pylab as plt

# x: 함수 f에 넘길 인수
def numerical_diff(f, x):
    h = 1e-4 # 0.0001
    return (f(x+h) - f(x-h)) / (2*h) # 오차를 줄이기 위해 (x+h)와 x의 차분인 전방 차분이 아닌 (x+h)와 (x-h)ㅇ일 때의 함수 f의 중심(중앙) 차분을 사용

def function_1(x):
    return 0.01*x**2 + 0.1*x 

def tangent_line(f, x):
    d = numerical_diff(f, x)
    print(d)
    y = f(x) - d*x
    return lambda t: d*t + y
     
x = np.arange(0.0, 20.0, 0.1) # 0에서 20까지 0.1 간격으로 배열 x를 만듬
y = function_1(x)
plt.xlabel("x")
plt.ylabel("f(x)")

tf = tangent_line(function_1, 5)
y2 = tf(x)

plt.plot(x, y)
plt.plot(x, y2)
plt.show()

>>>
0.1999999999990898
```

	![[Pasted image 20231013224542.png]] 


##### 편미분(Partial derivatives)
- 변수가 여럿인 함수에 대한 미분
- 변수가 하나인 미분과 마찬가지로 특정 장소의 기울기를 구하는데 여러 변수 중 목표 변수 하나에 초점을 맞추고 다른 변수는 값을 고정
	![[Pasted image 20231013224852.png]]
```python
# x0 = 3, x1 = 4 일 때, x0에 대한 편미분 ∂f/∂x0 을 구하라
def function_tmp1(x0):
    return x0*x0 + 4.0**2.0
numerical_diff(function_tmp1, 3.0)

>>>
6.00000000000378
```



##### 기울기(gradient)
- 모든 변수의 편미분을 vector로 정리한 것
```python
def numerical_gradient(f,x):
    h = 1e-4
    grad = np.zeros_like(x)
    
    # 넘파이 배열 x의 각 원소에 대해서 수치 미분
    for idx in range(x.size):
        tmp_val = x[idx]
        
        x[idx] = tmp_val + h
        fxh1 = f(x)
        
        x[idx] = tmp_val - h
        fxh2 = f(x)
        
        grad[idx] = (fxh1-fxh2)/(2*h)
        x[idx] = tmp_val
        
    return grad
    
print(numerical_gradient(function_2, np.array([3.0,4.0])))
print(numerical_gradient(function_2, np.array([0.0,2.0])))
print(numerical_gradient(function_2, np.array([3.0,0.0])))

>>>
[6. 8.]
[0. 4.]
[6. 0.]
```
	![[Pasted image 20231013225113.png]] 
	- 기울기는 각 지점에서 낮아지는 방향을 가리킴
	- 기울기가 가리키는 쪽은 각 장소에서 함수의 출력 값을 가장 크게 줄이는 방향


##### 경사법 하강법(gradient descent)
- 현 위치에서 기울어진 방향으로 일정 거리만큼 이동, 기울어진 방향으로 나아가기를 **반복**/함수의 값을 점차 줄이는 방법   
	- GD는 훨씬 더 광범위한 모델에 적용할 수 있음
	- direct solution보다 구현하기 쉬움
	- 고차원의 회귀의 경우 direct solution보다 효율
- **gradient**: 함수가 커지는 방향
	![[Pasted image 20231027122834.png]]
	- **𝐽**: 주어진 위치에서 함수의 값이 최대가 되는 방향을 나타내는 vector  
	- **∂𝐽/∂w** > 0: increasing w, increases 𝐽
	- **∂𝐽/∂w** < 0: increasing w, decreases 𝐽
	![[Pasted image 20231027123127.png]]

	![[Pasted image 20231013225354.png]]
	- **η**(eta): 한 번의 학습으로 얼마만큼 학습해야 할지, 즉 매개변수 값을 얼마나 갱신하느냐를 정함
		- 신경망 학습에서는 **learning rate**(학습률)이라고 함,
		- 학습률이 너무 크면 발산, 너무 작으면 거의 갱신이 되지 않음
	
	- **parameter**
		- 모델 내부에서 결정되는 변수
		- 데이터로부터 결정
		- 사용자에 의해 조정되지 않음
	- **hyper parameter**
		- 모델링할 때 사용자가 직접 세팅해 주는 값
		ex) learning rate, loss function, mini batch size...
	
	- **fixed points**(points where the iterate doesn't change)를 찾는데 도움을 줌
		- _divergence_ or _local optima_ 문제로 최적의 해를 못찾을 수 있음

```python
# f(x0, x1) = x0^2 + x1^2의 최솟값

import numpy as np
import matplotlib.pylab as plt

def gradient_descent(f, init_x, lr=0.01, step_num=100):
    x = init_x # 초기값
    x_history = []

    # step num: 경사법에 따른 반복 횟수
    for i in range(step_num):
        x_history.append( x.copy() )

        grad = numerical_gradient(f, x)
        x -= lr * grad
        
        # lr: learning rate, 학습률

    return x, np.array(x_history)

def function_2(x):
    return x[0]**2 + x[1]**2

# 초기값을 (-3.0, 4.0)으로 설정한 후 경사법 사용해 최솟값 탐색
init_x = np.array([-3.0, 4.0])    

# lr은 미리 정해야 함, 적절한 값을 설정하는게 핵심
lr = 0.1
step_num = 20
x, x_history = gradient_descent(function_2, init_x, lr=lr, step_num=step_num)

plt.plot( [-5, 5], [0,0], '--b')
plt.plot( [0,0], [-5, 5], '--b')
plt.plot(x_history[:,0], x_history[:,1], 'o')

plt.xlim(-3.5, 3.5)
plt.ylim(-4.5, 4.5)
plt.xlabel("X0")
plt.ylabel("X1")
plt.show()

>>>
array([-6.11110793e-10, 8.14814391e-10]) 으로 거의 (0, 0)에 가까운 결과로 정확한 결과를 얻었음을 알 수 있음
```
	![[Pasted image 20231014140154.png]] 
