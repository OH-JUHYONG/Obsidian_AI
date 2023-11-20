---
tags:
  - linear-classification
  - classification
  - binary-classification
  - threshold
  - dummy-feature
  - feasible-region
  - 0-1-loss
  - loss-function
  - GD
  - SGD
  - stochastic-gradient-descent
  - surrogate-loss-function
  - linear-regression
  - sigmoid
  - sigmoidal
  - log-linear
  - perceptron
  - cross-entropy
  - logistic-regression
  - decision-boundary
  - multiclass-classification
  - one-of-K
  - one-hot-vector
  - softmax
  - softmax-regression
  - multiclass-logistic-regression
  - gradient-checking
  - learning-rate
  - oscillation
  - training-curve
  - mini-batch
  - convex
  - convex-sets
  - convexity
  - XOR-function
---
: 일차원 혹은 다차원 데이터들을 linear model을 이용하여 클래스들로 분류


- **Binary classification**: predicting a binary-valued target
	- 선형 분류가 가능하면 직선이 그려짐
	- 선형 분류가 가능하지 않다면 직선으로 그려지지 않음

### Binary linear classifiers

- **Classification**: predicting a discrete-value target
- **binary**: predict a binary target _t ∈ {0, 1}_ / postivie & negative
- **linear**: model is a linear function of _x_, followed by a **threshold**(기준값)
	![[Pasted image 20231028104606.png]]
	- _w_: weight vector
	- _b_: scalar-valued bias

- **training set** consist of a set of _N_ pairs _(x^(i), t^(i))_ 
	- _x^(i)_: input
	- _t^(i)_: the binary-valued target or label 

#### goal
- correctly classify all the training cases
- compute a linear function of the inputs, and determine whether or not the value is larger than some **threshold** _r_ 
- **Eliminating the threshold**
	![[Pasted image 20231028104724.png]]
	![[Pasted image 20231028104930.png]]
	-  bias를 _b-r_, _r = 0_ 으로 설정하여 일반성을 잃지 않게 함

- **Eliminating the bias**
	![[Pasted image 20231028110616.png]]  
	![[Pasted image 20231028110636.png]]
	-  bias를 삭제하고 항상 1의 값을 취하는 **dummy feature** 인 _x0_ 를 추가, 이는 효과적으로 bias의 역할을 함

#### Exapmles
##### _NOT_
- training set
	![[Pasted image 20231028112231.png]]

##### _AND_
- training set
	![[Pasted image 20231028112136.png]]


#### The geometric picture
##### Data space
- focus on two-dimensional input and weight spaces
- Each point in **data space**(or **input space**) correspond to a possible **input vector**
	- postive region / negative region <-- 이 두 지역을 나누는 **decision boundary**(wT x + b = 0)

#### _NOT example_
- **Input space & Data space**
	![[Pasted image 20231028120126.png]]
	-  Hypotheses _w_ can be represented by **half-space**
	![[Pasted image 20231028120225.png]]

- **Weight space**
	![[Pasted image 20231028121059.png]]
	- **feasible region**: 모든 제약 조건을 만족하는 영역, 만약 이 영역이 비어 있지 않으면 feasible함


#### Choosing a cost function
- 데이터 세트가 선형적으로 분리될 수 없는 경우 어떻게 합리적인 학습 기준을 정할 수 있는지
	- 잘못 분류된 훈련 예제의 수를 최소화하는 것
	- 
##### 0-1 loss
- **loss function**      
	![[Pasted image 20231028175836.png]]
	![[Pasted image 20231028175940.png]]
	- the error rate
	 ![[Pasted image 20231028180329.png]]
	![[Pasted image 20231028181730.png]]
	- 0-1 loss function은 **SGD 기반 optimization이 불가능**하기 때문에 0-1 loss를 근사하는 연속함수를 고려
	- **surrogate loss function**: 신뢰하는 손실 함수를 덜 신뢰하지만 최적하기 쉬운 다른 손실 함수로 대체하는 것

##### [[Linear Regression]]
- 0-1 loss의 surrogate loss로 squared error loss 고려
	![[Pasted image 20231028182424.png]] ^24af15
	- error에 대해서 penalty를 더 크게 주기 위해 | | 이 아닌 _1/2( )^2_ 로 두는 것
	![[Pasted image 20231028182740.png]]
	- class score _y_ 가 positive이면 penalty가 적어야 하는데 의도와 다르게 커짐
	- 정답일 수록 error가 커지는 문제

##### Logistic Activation Function
- a kind of **sigmoidal**
	![[Pasted image 20231028183908.png]]
- **log-linear**: logistic nonlinearity을 가진 선형 모델  
	![[Pasted image 20231028184004.png]]
	![[Pasted image 20231028184523.png]]![[Pasted image 20231028184316.png]]


##### Logistic regression
- **CE**(cross-entropy): 두 확률분포가 얼마나 유사한지를 측정, 두 확률 분포의 차이를 구하기 위해 사용
	![[Pasted image 20231028204521.png]]
	- 더 높은 confidence를 가진 예측에 대해 틀린 경우 더 많은 penalty를 부과

- logistic activation function(sigmoid)를 cross-entropy loss와 결합하면 **logistic regression**
	- sigmoid function을 사용하여 y값이 0~1 사이로 제한되어 linear regression보다 값의 변화가 적어 overfitting에 유리
	- data가 decision boundary에서 멀리 떨어졌을 경우 data는 0 or 1 값에 가까이 가게 되고 이때의 기울기는 0과 가까워 decision boundary가 민감하게 반응하지 않게 됨
	![[Pasted image 20231028204840.png]]
	![[Pasted image 20231028204855.png]]

#####  Comparsion of loss function
	![[Pasted image 20231028205057.png]]




### Multiclass Classification
##### target을 표현하는 방법: one-hot vector
- **one-of-K encoding** 이라고 불림
- 모델의 linear part을 고려하기 위해 _K x D_ **weight matrix** 와 **_K_-dimensional bias vector** 필요
	- _K_: output
	- _D_: input  
	![[Pasted image 20231031133052.png]]
	![[Pasted image 20231031133208.png]]

##### Activation function: softmax function
- a multivariate generalization of the logistic function
- **input**: _zk_ are called the **logits**
- **output**: nonneagtiva and **sum to 1**
- _K_ 클래스에 대한 확률 분포로 해석이 가능(class score을 확률로 변환) 
	- _K = 2_ 인 경우 logistic function과 같음 
- _zk_ 가 다른 값들보다 월등하게 클 경우 (exp를 취하기 때문에 다른 값들과 격차가 엄청 벌어짐) _zk_ 에 해당하는 값만 1이 되고 나머지는 0이 되는 **argmax**로 볼 수 있음
	![[Pasted image 20231031133609.png]]

##### the loss function: Cross-entropy
- Cross-entropy can be generalized to the multiple-output case
	![[Pasted image 20231031133855.png]]
	-  one-hot vector 이므로 _tk_ 중 하나만 1이고 나머지는 0
		![[Pasted image 20231031140756.png]]

위에 요소들을 모두 합쳐서 **multiclass logistic regression**(or **softmax regression**)을 표현
	![[Pasted image 20231031134257.png]]

- softmax와 cross-entropy function는 서로 잘 상호작용하므로 수치 안전성을 위해 항상 single softmax-cross-entropy function인 _L_SCE_ 로 결합
	![[Pasted image 20231031141407.png]]


### Gradient Checking
- 구한 기울기가 맞는지 아닌지 판단하는 방법
	![[Pasted image 20231031154736.png]]
	- 기울기를 잘못구한 경우 최적화에 어려움이 있음, 학습이 안될 수 있음 


### Learning Rate
- the learning rate **α** is a hyperparameter
	- **α** too small: 너무 느리게 학습됨, 시간이 오래 걸림
	- **α** too large: **oscillation**(수렴이 안됨)
	- **α** much too large: 학습 자체가 불안정해짐
		- 적당한 값은 _0.001 ~ 0.1_ 
	![[Pasted image 20231031155433.png]]


### Training Curve
- To diagnose optimization problem, it's useful to look at training curves
	![[Pasted image 20231031155818.png]]
	- training curve 만으로 최적화가 잘 이루어지고 있는지 아닌지 말하기 어려움, 상대적으로 설정한 parameter들을 조정해야 함을 판단할 수 있음

### Stochastic Gradient Descent
- the cost function _J_ 
	![[Pasted image 20231031160540.png]]
	![[Pasted image 20231031161046.png]]
	- 각 data들의 loss를 gradient를 구해서 평균  --> **batch training**
	- **SGD**: update the parameters based on the gradient for a single training example, chosen **uniformly at random**
		- variance 변화 없이 바로 원하는 방향으로 학습(시간이 오래 걸림)되는 GD와 달리 SGD는 학습이 빠름(대신에 variance 커짐, noisy가 발생)
		- data가 무수히 많은 경우에 효율
		- SGD is an unbiased estimate of the batch gradient, 각 data들의 gradient에 대한 평균을 구하면 cost function에 대한 미분이랑 같음 
		![[Pasted image 20231031161645.png]]


#### mini-batch
- GD와 SGD의 중간지점, 표준 학습 방
	- **mini-batch**를 사용함으로써 SGD의 방식의 variance를 줄일 수 있음
- compute the gradients on a randomly chosen medium-sized set of training example
	![[Pasted image 20231031162322.png]]
	- the mini-batch size _|M|_ is a hyperparameter
		- Too large: 계산하는데 시간이 많이 걸림, 메모리 커짐
		- Too small: vector화된 성능을 활용 못함
		- _|M| = 100_ 이 적당한 크기

#### SGD Learning Rate
- 일반적으로 학습 초기에는 learning rate을 크게 해 수렴에 가까워지도록 설정 후 점점 learning rate을 점점 줄여 안정화 시키는 작업을 취함
	![[Pasted image 20231031163201.png]]
 
### Convex Sets
- **Convex**: 집합 _S_ 에서 임의의 두 점을 연결하는 선분이 S 안에 있으면 두 점을 연결하는 선분이 S 안에 완전히 위치하는 경우를 말함
	![[Pasted image 20231031142641.png]]

##### Convex Function
- 다음을 만족하는것이 **convex function**(<--> concave function)
	![[Pasted image 20231031143601.png]]
	-  Convexity가 중요한 이유??
		-  All critical points are minima
		- Gradient descent finds the optimal solution(will always converge to the global minimum)



### Limits of Linear Classification
##### XOR function
- XOR이 linear separable 하지 않음을 증명하는 방법(linear decision boundary를 찾을 수 없음)
	- **선형으로 분리가 된다면** decision boundary 기준으로 data space를 쪼개면 half space는 **convex한 성질**을 갖춰야 함
	- 해당 성질을 XOR에 대입을 하면 교차점이 두 개의 half space에 놓여져 있기 때문에 선형 분리가 안됨을 증명
	![[Pasted image 20231031153637.png]]
- 해당 문제를 해결하기 위해 3차원을 확장, _x1_ **x** _x2_ 값을 새로운 feature로 추가 
	![[Pasted image 20231031145749.png]]
	- ex)...입력 데이터를 선형으로 분리할 수 있게 변환해 주는 **신경망**

