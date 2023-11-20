---
tags:
  - decision-tree
  - classification-tree
  - regression-tree
  - entropy
  - information-theory
  - overfitting
  - information-gain
  - KL-divergence
  - k-nearest-neighbors
  - K-NN
  - missing-value
  - discrete-data
  - continous-data
---
: 트리 구조에 따라 여러 속성을 ==재귀적으로 분할하여 예측==


### When to Consider Decision Trees

- Instances decribable by attribute-value pairs
- Target function is **discrete valued**
- **Disjunctive hypothesis** may be required
- Possibly noisy traning data


### Decision Tree Construction Algorithm

- Algorithm마다 decision boundary가 만들어지는 방식이 다름
- ==Leaf node are output(prediction)==
- Branching is determined by attribute value
- Simple, greedy, recursive approach, build up tree node-by-node
	1. Pick an attribute to split at a non-terminal node(중간 노드드)
	2. split examples into groups based on attribute value
	3. for each group:
		- if no example - return majority from parent
		- Else if all examples in same class - return class
		- Else loop to step 1
	![[Pasted image 20231004150746.png]]  


### Decision Tree: Classification and Regression

##### Classification tree
- **discrete output**
	- 별도의 값 또는 구분된 값을 가지는 데이터
	- 연속적이지 않으며 각 값 사이에 간격이 존재
	- 변수가 가질 수 있는 값의 개수가 유한개
- leaf value _ym_ typically set to the most common value in $$\{ t^{(m_{1})}, ... t^{(m_{k})} \}$$
##### Regression tree
- **continuous output**
	- 연속적인 범위 안에서 모든 가능한 값을 가질 수 있는 데이터
- leaf value _ym_ typically set to the mean value in  $$\{ t^{(m_{1})}, ... t^{(m_{k})} \}$$
### Problem

- Exponentially less data at lower levels
	⮚ Decision tree는 트리가 깊어질수록 각 노드에서 사용할 수 있는 데이터가 기하급수적으로 줄어듬, 이는 상위 노드에서의 분할 기준을 만족하는 데이터만이 해당 하위 노드로 이동하기 때문   
	
- 지나치게 큰 tree는 overfitting 문제가 발생
- Greedy algorithm은 반드시 global optimum을 찾아주지 않음


### Choosing a Good Split

- 좋은 decision tree를 만들기 위해 ==enthropy를 줄이는 방향으로 분기==해야 함
	![[Pasted image 20231004152006.png]] 
	- 위 예시를 보면 leaf node 왼쪽은 class 1이라고 볼 수 있지만, 오른쪽은 class 1과 class 0이 1 차이 밖에 안남에도 class 1이라 분류하기 때문에 불확실성이 큼
	- 이에 우리는 **information theory** 기법을 사용
		- 정보이론의 핵심 척도는 **Entropy**
		- 정보이론은 정보의 정량화, 저장 및 통신에 대한 수학적 연구


### Entropy

- Information theory의 핵심 척도
- 무작위 변수의 값 또는 무자위 프로세스의 결과와 관련된 ==불확실성의 양을 정량화==
	- 주로 _0 ~ 1_ 사이의 범위	
	- **Low Entropy**
		- 0에 가까운 값
		- 불확실성이 낮음
		- 분포가 더 예측 가능, 어떤 상황에서 일어날 가능성이 더 높음
		- Distribution of variable has many peaks and valleys
		- Overfitting될 가능성이 큼
	![[Pasted image 20231004153030.png]]
	
	- **High Entropy**
		- 1에 가까운 값
		- 불확실성이 큼, 무질서함, 예측하기 힘듬
		- 많은 정보를 가짐, 다양함
		- 학습데이터로 좋음
		- 단순하게 정보가 많다고 보면 안되고 이는 변수들 간의 분포가 고르기 때문에, 변수들 간 구분이 힘들기 때문에 예측하기 힘듬
			- 즉, decision tree를 분기할 때 entropy 값을 줄이는 방향으로 가야 함
		- Variable has a **uniform like distribution**, 변수들 간의 분포가 고르다
			- **uniform distribution**: 모든 확률변수에 대해 균일한 확률을 가짐
	
	![[Pasted image 20231004153052.png]]
	

- 모델의 신뢰성이 높다의 의미(모델이 높은 confidence를 가지고 분류)는 entropy값이 작다는 의미
	![[Pasted image 20231004153441.png]]
	⮚  오른쪽이 왼쪽보다 entropy가 더 크다(정보의 양이 많다)
	
![[Pasted image 20231024163253.png]]


### Entropy of a Joint Distribution
	![[Pasted image 20231004154134.png]]  


### Specific Conditional Entropy
	![[Pasted image 20231004154248.png]]  


### Conditional Entropy
- 특정 event가 일어났을 때 다른  event가 일어날 확률
	![[Pasted image 20231004154516.png]]  

##### Properties of Conditional Entropy
- _H_ is always **non-negative**
- Chain rule
	⮚  _H(X, Y) = H(X|Y) + H(Y) = H(Y|X) + H(X)_
	
- X와 Y가 독립인 경우
	⮚  _H(Y|X) = H(Y)_
	
- X와 Y가 종속
	⮚  _H(Y|X) = 0_
	 


### Information Gain

- Information gain = ==entropy(parent, 상위노드) - [average entropy(children, 하위노드)]
- Decision tree에서 entropy를 계산 후, 어떤 노드를 선택하는 것이 옳은지 따져볼 때 사용하는 기댓값
	⮚  ==Information gain이 높은 쪽(변별력이 좋다)을 선택해 다음 가지 생성==
	- Decision tree는 entropy 값이 작아지는 방향으로 가야하기 때문에 Children entropy 값은 Parent entropy 값보다 더 값이 좋아야 되기 때문에 Children entropy 값은 낮을 것이며 이 둘의 차는 높아질수록 잘 분리된 값으로 판단
	![[Pasted image 20231004155533.png]]
	- 1/3 = 50 / (100 + 49)
	- 2/3 = (50+49) / (100 + 49)
	 

##### KL-divergence
- **Kullback-Leibler divergence**, 쿨백-라이블러 발산
- ==두 확률분포의 차이를 계산==하는 데에 사용하는 함수
- 어떤 이상적인 분포에 대해, 그 분포를 근사하는 다른 분포를 사용해 샘플링을 한다면 발생할 수 있는 **정보 엔트로피** 차이를 계산
	- relative entropy
	- information gain
	- information divergence 라고도 함
	![[Pasted image 20231013120111.png]]
	![[Pasted image 20231013115612.png]]

- 두 분포의 차이는 다음과 같이 나타냄
	![[Pasted image 20231013115740.png]]
	![[Pasted image 20231013120211.png]]

- **KL-divergence 성질**
	![[Pasted image 20231013120255.png]]
	- ==KL-divergence는 거리 개념(distance metric)이 아니다==(asymmertic 하다) 
		![[Pasted image 20231013121846.png]]
		- KL-divergecne에서 p와 q를 바꾼 값과 원래의 값이 다르다는 점이 비대칭적이다.
		- 대칭적이라면 결과값이 같아야 함

### Comparison to k-NN
- Decision tree가 k-NN보다 좋은 점?
	- Good with discret attribute, 불연속 속성에 적합
	- **결측값(missing value)** 을 쉽게 처리, 다른 값으로 처리 가능
		ex) **nominal feature**(빠진 feature), 즉 categorical feature인 경우 새로운 class로 취급 
	- Robust to scale of inputs
	- Fast at test time
	- More interpretable

- k-NN이 Decision tree보다 좋은 점?
	⮚  복잡한 방식으로 상호 작용하는 속성/특징을 처리
	⮚  일반적으로 실제로 더 나은 예측을 할 수 있음

