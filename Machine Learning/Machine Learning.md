---
tags:
  - machine-learning
  - supervised-learning
  - unsupervised-learning
  - reinforcement-learning
  - decision-tree
  - ensemble
  - nearest-neighbor-upsampling
  - NN
  - hard-negative-mining
  - classification
  - regression
  - linear-regression
  - clustering
  - k-means
  - ANN
  - deductive-learning
  - inductive-learning
  - hypothesis-space
  - generalization
  - normalization
  - training-set
  - validation-set
  - test-set
  - hyper-parameter
  - the-curse-of-dimensionality
  - manifold
  - Lp-norm
  - L1-norm
  - L2-norm
  - outlier
  - huber-loss
  - parameter
  - hyperparameter
  - input-vector
---
#### Machine learning 학습 방법
### 지도학습(Supervised Learning)
- 정답이 있는 데이터를 활용해 데이터 학습
- 입력 값이 주어지면 입력 값에 대한 label를 주어 학습시킴  
	- [[Decision Trees]] 
	- [[Ensemble]]  
	- 
- **분류(classification)**: 주어진 데이터를 정해진 카테고리(label)에 분류  
	-  [[Nearest Neighbors]]
	- [[Hard Negative Mining]] 
	- [[Linear Classification]] 
  

-  **회귀(regression)**: 어떤 데이터들의 feature를 기준으로, 연속된 값(그래프)를 예측하는 문제로 주로 어떤 패턴이나, 트렌드, 경향을 예측할 때 사용
	-  [[Linear Regression]]


### 비지도학습(Unsupervised Learning)
- 정답 라벨이 없는 데이터를 비슷한 특징끼리 군집화하여 새로운 데이터에 대한 결과를 예측하는 방법
- 지도학습에서 적절한 feature를 찾아내기 위한 전처리 방법으로 비지도 학습을 사용하기도 함
	- Clustering
	- K-means
	- [[Generative Model]]

### 강화학습(Reinforcement Learning, RL)
- 분류할 수 있는 데이터가 존재 x
- 데이터가 있어도 정답이 따로 정해져 있지 않음
- 자신이 한 행동에 대한 보상(reward)을 받으며 학습


#### What is the difference between Deep learning and Machine learning?
- Deep learning은 여러 층을 가지는 ==인공신경망==(Artificial Neural Network, ==ANN==)을 사용하여 머신러닝 학습을 수행하는 것으로, 심층학습이라고도 부름(엄밀히 말하면 머신러닝에 포함된 개념)

- 머신러닝에서는 학습하려는 데이터의 여러 특징 중에서 어떤 특징을 추출할지 사람이 직접 분석하고 판단하는 반면, 딥러닝에서는 기계가 자동으로 학습하려는 데이터에서 특징을 추출하여 학습
	⮚   특징 추출에 사람이 개입(feature engineering)하면 머신러닝, 개입하지 않으면 딥러닝
	![[Pasted image 20231001004737.png]] 



#### What is the difference Deductive learning(연역적 학습) and Inductive learning(귀납적 학습)
**Deductive learning**
- 연역적 추론을 통한 학습
- **이미 알고있는 판단을 근거**로 새로운 판단을 유도하는 추론을 통해 주어진 문제를 해결하려는 접근 방식 


**Inductive learning**
- 데이터로부터 일반적인 **규칙이나 패턴을 발견**하려고 노력하는 접근 방식
- 데이터에 대한 패턴을 식별하고 이를 기반으로 미래의 데이터의 대한 예측을 시도하는 것을 포함
- Given a training set of example of the form (_x, f(x)_)
	⮚  _x_ is the input, _f(x)_ is the output

- Return a function _h_ (학습하려는 모델) that approximates _**f**_ (target function)
	⮚  _h_ is called **hypothesis**

- find a _h_ that agrees with _f_ on a training set
	- _h_ is consistent if it agrees with _f_ on all examples

- finding a consistent hypothesis is not always possible
	⮚  100% 신뢰할 수 있는 data는 없음, 어느 정도 ==noisy== 끼어 있다고 가정
	⮚  Insufficient hypothesis space


#### Hypothesis Space(가설 공간)
- set of all hypothesis _h_ that the learner may consider
- 가중치 w를 변경하여 얻은 모든 예측자 score의 집합


#### Parameters vs Hyperparamter
**parameter**
- 모델 내부에서 결정되는 변수
- 데이터로부터 결정
- 사용자에 의해 조정되지 않음

**hyper paramter**
- 모델링할 때 사용자가 직접 세팅해 주는 값
ex)... learning rate, loss function, mini batch size...


#### Generalization(일반화) vs Normalization(정규화)
**Generalization**
- 모델이 훈련 데이터에서 학습한 패턴을 **새로운 데이터에 적용할 수 있는 능력**
- 일반화가 잘 되면 모델은 새로운 데이터에서도 정확하게 예측을 수행할 수 있음
- a good hypothesis will generalize well

**Normalization**
- 데이터를 특정 범위로 변환
	⮚ 주어진 데이터의 범위나 스케일을 조정하여 데이터를 더 쉽게 처리하거나 모델을 훈련하는데 도움을 줌
	⮚ 데이터의 각 feature가 서로 다른 범위나 단위를 가질 때 발생하는 문제를 해결
	![[Pasted image 20231004135541.png]]
	- 각 feature을 **평균이 0**이고 **표준편차가 1**인 표준 정규 분포에 가깝도록 변환

- 어떤 문제에서는 특정 스케일이 중요한 정보를 나타낼 수 있으므로 주어진 문제를 특성을 고려하여 조절해야 함


#### Data set 종류
**Traning set**
- 훈련, 학습 데이터
- 매개변수 학습

**Validation set**
- 검증 데이터
- Hyper parameter 성능 평가
- 학습에 참여하지 않은 data
- 성능 평가할 때 시험 데이터를 사용해서 안됨

**Test set**
- 시험 데이터
- 신경망의 범용 성능 평가
- 선택한 **모델의 최종 성능 평가**
- 학습과 검증이 완료된 모델의 성능을 평가하기 위한 dataset
	![[Pasted image 20231004133804.png]]


#### The Curse of Dimensionality(차원의 저주)
- **데이터의 차원이 증가**할수록 **공간의 크기(변수 개수)가 기하급수적으로 증가**하여 데이터간 거리가 기하급수적으로 멀어지고 희소한 구조를 갖게 되는 현상
- 학습데이터 수가 차원의 수보다 적어져 성능이 저하
	- **차원**: 어떤 시스템의 독립변수의 개수
	- 한 데이터를 여러 독립 차원을 이용하여 표현
	- 데이터에 대한 정보가 많으면 많을 수록 더욱 정밀하게 표현 가능
	- 한 데이터를 표현하기 위해 수 많은 차원이 필요한 경우 그 모든 차원에서 균일하게 데이터가 분포해 있으려면 늘어난 차원 수 보다 더 많은 양의 데이터가 필요 
- 이를 해결하기 위해 ==차원을 증가시킨만큼 더 많은 데이터를 추가==하거나, PCA, LDA, LLE, MDS와 같은 ==차원 축소 알고리즘으로 차원을 줄여== 해결
	![[Pasted image 20231024142028.png]]
	![[Pasted image 20231024141712.png]]
	![[Pasted image 20231004134232.png]]
	- Class가 다른 건(고차원) --> 비슷한 Class(저차원), 이때 필요한 정보들만 가지고 불필요한 정보를 날림(압축)
	- **Manifold**: 고차원의 데이터를 저차원으로 비선형 축소(embedding)할 때 데이터를 잘 설명하는 집합의 모형
	![[Pasted image 20231004134442.png]] 


#### Norm
- **vector가 얼마나 큰지**를 알려주는 것
- vector의 차원의 크기가 아닌, **구성요소의 크기**를 표현
##### Lp-norm
	![[Pasted image 20231014014014.png]]
##### L1-norm
- vector의 요소에 대한 절댓값의 합
- L2-norm와 다르게 outlier[^0](이상치)의 영향을 크게 받지 않음
	![[Pasted image 20231014014407.png]]
##### L2-norm
- n차원 좌표평면에서의 vector 크기
	![[Pasted image 20231014014555.png]]

##### Huber Loss
- L1, L2의 장점을 취하면서 단점을 보완하기 위해 제안된 것
- 모든 지점에서 미분이 가능
- outlier에 robust한 성격을 지님
	![[Pasted image 20231014014910.png]]
	![[Pasted image 20231014014921.png]]


#### Input Vector
- Machine learning algorithms need to handle lots of types of data: images, text, audio, waveform, credit card transaction, etc
- Common strategy: represent the input as an **input vector** in R^d
	- Representation: 계산하기 쉬운 다른 공간으로 mapping
	- vector are a great representation since we can do linear algebra


#### Parametric vs Non-parametric
##### parametric model
- 모델의 파라미터 수가 정해져 있음
- 데이터가 특정 분포를 따른다고 가정
- 우리가 학습을 하면서 결정해야 하는 파라미터의 종류와 수가 명확하게 정해져 있기 때문에 데이터 수에 상관 없이 결정해야 할 파라미터의 수는 변하지 않음
- 데이터가 많을수록 정확도가 올라감
ex) [[linear regression]], [[Linear Regression|logistic regression]], bayes inference, [[neural network]] ...


##### non-parametric model
- 파라미터의 수가 학습 데이터의 크기에 따라 달라짐
- 데이터가 특정 분포를 따른다는 가정이 없기 때문에 학습에 따라 튜닝해야 할 파라미터가 명확하게 정해져 있지 않음, flexible 함
- 속도가 느리고, 많은 데이터를 필요로 하지 않음, 모델의 형태에 대한 명확한 설명이 어려움
ex) [[Decision Trees|decision tree]], [[Ensemble|random forest]], [[Nearest Neighbors|K-NN]]




[^0]: outlier
	- 관측된 데이터의 범위에서 많이 벗어난 아주 작은 값이나 큰 
	- https://gannigoing.medium.com/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%9D%B4%EC%83%81%EC%B9%98-outlier-%EC%9D%98-%EA%B8%B0%EC%A4%80%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-f11f60bf901a

