---
tags:
  - hard-negative-mining
  - hard-negative
  - binary-classification
  - R-CNN
---
: in machine learning for improving the perfomance of **binary classification model**

- ==hard negative==: 실제로는 negative인데 positive라고 잘못 예측하기 쉬운 데이터

- the model is trained on a set of positive and negative example, and then the examples that the model gets wrong, i.e, false negatives, are identified and added to the training set as hard negative example
- 모델이 예측에 실패하는 어려운(hard) sample들을 모으는 기법으로, hard negative mining을 통해 수집된 데이터를 활용하여 모델을 보다 robust하게 학습시키는 것이 가능해짐

![[Pasted image 20230930221232.png]]

#### When use Hard Negative Mining in [[R-CNN]]
- 객체 탐지 시, 객체의 위치에 해당하는 postive sample 보다 배경에 해당하는 negative sample이 훨씬 많은, 클래스 불균형(class imbalance)으로 인해 발생
- 이러한 문제를 해결하기 위해 모델이 잘못 판단한 False Positive sample을 학습 과정에서 추가하여 재학습하면 모델은 보다 robust 해지며, False Positive라고 판단하는 오류가 줄어듬


