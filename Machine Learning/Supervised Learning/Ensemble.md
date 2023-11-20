---
tags:
  - ensemble
  - bagging
  - bootstrap
  - boosting
  - aggregration
  - categorical-data
  - continous-data
  - random-forest
  - decision-tree
  - AdaBoost
  - bias
  - variance
sticker: lucide//plus
---
**cf**[^0]

: 앙상블 기법(Ensemble Learning)은 ==여러 개의 개별 모델을 조합하여 최적의 모델로 일반화==하는 방법

- 여러 개의 weak classifier[^1]를 결합하여 strong classifier[^2]를 만드는 것
	ex) 주로 여러 개의 [[Decision Trees]] 를 결합하여 하나의 결정 트리보다 더 좋은 성능을 내는 머신러닝 기법
- Ensemble Learning에서는 **Bagging**과 **Boosting**이 있음

##### Bias and Variance
- **Bias**: (unknown) true function과 hypothesis(or model) _h(x)_ 들의 평균 예측의 차이
- **Variance**: hypothesis가 training set에 따라 동일한 입력 x에 대해 변화하는 정도(training set이 달라지면서 x에 대한 예측값의 변화량)
	![[Pasted image 20231025111820.png]]
	- bias가 크면 model 단순해지고 성능이 떨어지는 underfitting이 발생
	- variance가 크면 model이 복잡해지고 overfitting이 발생

##### Bayes Optimality
- 특정 상황에서 가장 좋은 의사 결정을 내릴 때, 베이지안 추론과 확률론을 사용하여 최적한 결정을 내리는 것을 의미
	![[Pasted image 20231025110556.png]]
	- 위 식의 최소값일때 y값을 구하는것이 목표
		- 첫 번째 항은 음수가 아니며, _𝑦 = 𝑦∗_ 로 설정하여 0으로 만들 수 있음
		- 두 번째 항은 대상의 **inherent unprdictability**(예측할 수 없는 불확실성), or **noise**, of the targets, and is called the **Bayes error**  
	![[Pasted image 20231025111311.png]]
	![[Pasted image 20231025111405.png]]
	- **bias**: how wrong the expected prediction is
	- **variance**: 예측의 변동성 정도
	- **Bayes error**: 가능한 최소의 오류, 예측할 수 없는 불확실성, 통제 범위 벗어남
	![[Pasted image 20231025111909.png]]


- **Bayes Optimal Classifier**: 주어진 train 데이터셋을 바탕으로, 전에 보지 못한 새로운 데이터가 주어졌을 때, 가장 그럴 듯한 예측을 계산해주는 확률 모델
	- P(A|B): **사후확률(posterior)**, 사건 B가 발생한 후 갱신된 사건 A의 확률, 주어진 데이터를 고려한 후에 어떤 결정이 가장 가능성 있는지 나타냄
	- P(A): **사전확률(prior)**, 사건 B가 발생하기 전에 가지고 있던 사건 A의 확률, 각 결정이 얼마나 가능성이 있는지를 나타냄



### Bagging

- **Bootstrap Aggregation**
- Train classifiers **independently** on **random** subsets of the training data
	- sample을 여러 번 뽑아(Bootstrap, ==전체 데이터 기준으로 랜덤하게 복원추출을 진행==) 각 모델을 학습시켜(==개별적인 모델은 독립적==) 결과물을 집계(Aggregration)하는 기법
- variance을 줄여주는 효과
	- bias를 줄여주지 않음 
- Boosting과 다르게 **각 모델들은 서로 독립적**이고 **병렬로 학습**
	![[Pasted image 20231013150857.png]]
	- **Categorical Data**: 투표 방식(voting)으로 결과를 집계, 전체 모델에서 예측한 값 중 가장 많은 값을 최종 예측값으로 선정
	- **Continuous Data**: 평균으로 집계

- 문제점: the datasets are not independent, so we don't get the _1/m_ variance reduction
	![[Pasted image 20231025113118.png]]
	- 위에 식은 sampled prediction을 나타낸것
	- _𝜌_: data들의 상관관계, Bayes error 처럼 통제 x
	- data set을 완전히 독립적으로 sampling 하는 것을 어려우므로 다른 종류의 모델들을 사용하여 예측의 독립성을 높임, 즉 위의 식에서 𝜌를 줄임

##### Random Forest
- [[Decision Trees]] 가 모여 Random Forest를 구성, 이는 overfitting의 문제점을 해결
	- 수많은 Feature를 기반으로 label를 예측하면 높은 확률로 overfitting이 발생
	- 여러 개의 작은 Decision Trees가 예측한 값들 중 가장 많은 값(분류) or 평균값(회귀)를 최종 예측 값으로 정함
	![[Pasted image 20231013151924.png]]
	- **n_estimators**: random forest 안의 결정 트리 갯수
		- 클수록 좋지만 그만큼 메모리와 시간이 증가
	- **max_feature**: 무작위로 선택할 Feature 개수
		- _bootstrap = True_ 인 경우 전체 Feature에서 복원 추출해서 트리를 만듬

---

### Boosting

- 가중치를 활용하여 weak classifier를 strong classifier로 만드는 방법
- bias을 줄여주는 효과
	- variance는 보장을 못함
- Bagging과 다르게 **각각의 모델의 연관성**이 있고 **순차적으로 학습**
	- 순차적으로 앞 모델이 예측한 결과에 따라 데이터에 가중치가 부여되고, 부여된 가중치가 다음 모델에 영향을 줌
	- 잘못 분류된 데이터에 집중하여 새로운 분류 규칙을 만드는 단계를 반복
	- 고정된 학습 데이터
	![[Pasted image 20231013153204.png]]
	- D2를 보면 D1에서 잘 분류된 데이터는 크기가 작아짐(가중치가 작아짐), 잘못 분류된 데이터는 크기가 커짐(가중치가 커짐)

- Bagging에 비해 error가 적어 성능이 좋음
- 속도가 느리고 overfitting 될 가능성이 있음
	- 즉, 개별 결정 트리의 낮은 성능이 문제인 경우 Boosting이 적합
	- overfitting이 문제인 경우 Bagging이 적합

##### Weak Classifier
- 약 분류기, 단일 분류 규칙 또는 모델로 구성
- 상대적으로 낮은 성능, 높은 정확도를 기대하기 힘듬
	![[Pasted image 20231026142453.png]]
	- 위 사진은 weak classifier
	- horizontal and vertiacl half space로 구성되어 있는 decision stumps
	![[Pasted image 20231026142649.png]]
	- 최대 _1/2 − γ_ 의 값
	- err: 틀린 경우를 1로 보고(indicator function) 틀린 경우에 _x Wi_ 를 해줌


##### AdaBoost
- 각각의 분류기는 이전의 mistake에 집중해 ==bias를 줄여줌== 
- AdaBoost는 여러개의 stump로 구성
	- **stump**: 하나의 node에 2개의 leaf를 가진 tree
	- 트리와 다르게 stump는 정확한 분류를 하지 못함
	- stump는 단 하나의 질문으로 데이터를 분류해야 하므로 weak learner

- Key steps
	1. At each iteration we re-weight the training samples by assigning larger weights to samples that were classified incorrectly
	2. Train a new weak classifier based on the re-weighted samples
	3. Add this weak classifier to the ensemble of classifiers(new classifier)
	4. Weight each weak classifier in the ensemble with somw weights
	5. repeat the process many times
	![[111.jpg]]
	![[222.jpg]]
	![[333.jpg]]
	![[444.jpg]]
	![[555.jpg]]
	![[Pasted image 20231026152914.png]]

- AdaBoost Minimize the Training Error
	![[Pasted image 20231026153254.png]]


- 각각의 트리가 동등한 가중치를 가지는 Random Forest와 달리 AdaBoost에서는 특정 stump가 다른 stump보다 더 중요
	![[Pasted image 20231013201319.png]]
	- 크기가 큰 것은 가중치가 더 높은 stump를 뜻함
	- 가중치가 높다는 것은 **Amount of Say**가 높다고 표현, 결과에 미치는 영향이 크다는 뜻

AdaBoost의 특징
	1. weak learner로 구성, weak learner은 stump의 형태
	2. 어떤 stump는 다른 stump보다 가중치가 높음
	3. 각 stump의 error는 다음 stump의 결과에 영향을 줌







[^0]: https://bkshin.tistory.com/entry/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-11-%EC%95%99%EC%83%81%EB%B8%94-%ED%95%99%EC%8A%B5-Ensemble-Learning-%EB%B0%B0%EA%B9%85Bagging%EA%B3%BC-%EB%B6%80%EC%8A%A4%ED%8C%85Boosting
	- https://medium.com/dawn-cau/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%EC%95%99%EC%83%81%EB%B8%94-%ED%95%99%EC%8A%B5-%EC%9D%B4%EB%9E%80-cf1fcb97f9d0
	- https://daje0601.tistory.com/120
	- https://bkshin.tistory.com/entry/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-14-AdaBoost

[^1]: weak classifier
	- 약 분류기
	- 단일 분류 규칙 또는 모델로 구성
	- 상대적으로 낮은 성능, 높은 정확도를 기대하기 힘듬

[^2]: strong classifier
	- 강한 분류기
	- 여러 약한 분류기의 조합으로 구성
	- 이를 위해 일반적으로 ensemble 학습 기술을 사용 
		- AdaBoost
		- Random Forest