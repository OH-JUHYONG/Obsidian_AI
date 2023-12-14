

## Pre training & Fine tuning
### Pre training
- 사전 학습 모델, 기존에 **Xavier**등 임의의 값으로 초기화하던 모델의 가중치들을 다른 문제(task)에 학습시킨 가중치들로 초기화 하는 방법
- **pre-trained model**: 이미 학습이 완료되어 output 도출이 가능한 모델
	ex) 텍스트 유사도 예측 모델을 만들기 전 감정 분석 문제를 학습한 모델의 가중치를 활용해 텍스트 유사도 모델의 가중치로 활용하는 방법
	- 하위 문제(**downstream task**): 사전 학습한 가중치를 활용해 학습하고자 하는 본 문제
	- 사전 학습 문제(**pre-train task**): 사전 학습한 모델

### Transfer Learning
- 사전에 학습된 모델이 pre-trained model이고, 이를 활용하여 새로운 데이터셋을 학습하는 과정을 transfer learning
- [Domain Adaptation](Domain%20Adaptation.md)이라 부르기도 함

### Fine tuning
- 사전 학습된 모든 가중치와 더불어 **downstream task**를 위한 최소한의 가중치를 추가해서 모델을 추가로 학습(미세 조정) 하는 방법
- **얻고자 하는 결과값이 다르기 때문**에 마지막 output layer를 삭제하고 다른 layer를 붙여서 써서 모델을 다시 훈련
- 이미 잘 학습된 pre-trained model을 사용하는 것은 이미지가 어떻게 생겼는지에 대한 구조를 이해할 수 있기 때문에 학습 알고리즘이 새로운 모델에서도 더 잘 작동하는 것에 도움이 됨, 적은 데이터셋으로도 효과를 극대화 시킬 수 있음
	ex) 사전 학습 방법인 감정 분석 문제에 사전 학습시킨 가중치와 더불어 텍스트 유사도를 위한 부가적인 가중치를 추가해 텍스트 유사도 문제를 학습하는 것이 미세 조정 방법





## Imbalanced Data

- 데이터 불균형으로 인한 과적합 문제가 발생
- 데이터가 불균형하다면 분포도가 높은 클래스에 모델이 가중치를 많이 두고 더 예측하려고 하기 때문에 Accuracy가 높아질 수 있지만, 반대로 분포도가 낮은 값에 대해서 Precision이 낮을 수 있어 분포가 작은 클래스의 재현율이 낮아지는 문제가 발생

### Solution
#### Undersampling(Downsampling) for majority class
- 데이터 분포가 높은 값을 낮은 값으로 맞춰주는 작업
	- 유의미한 데이터만 남길 수 있지만 정보가 유실되는 문제가 생길 수 있음

#### Oversampling for minority class
- 적은 수의 데이터를 복원 추출해 class 간 데이터 수의 군형을 맞추는 방법
- 모델이 학습해야하는 데이터 수가 증가하기 때문에 학습 시간이 많이 소요됨
	- 복원 추출된 데이터가 새로운 데이터가 아니기 때문에 해당 데이터 셋에 오버피팅 될 수 있음

#### Weighting in Loss Function
- class 별로 loss function에 weight를 주는 방식과 sample 별로 weight를 주는 방식이 존재
- 둘 다 minority class에 속한 데이터 샘플에 더 집중해 학습할 수 있도록, loss 값에 weight를 부여하는 방법



## Class 분포
- train / test label 분포 고려해야 함
- weight cross entropy
- 조명 증강
- pseudo lableing  

- 소프트보팅
- 하드보팅