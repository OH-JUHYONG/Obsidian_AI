---
tags:
  - R-CNN
  - mAP
  - region-proposal
  - CNN
  - localize
  - auziliary-task
  - domain-specific-fine-tunning
  - OverFeat
  - selective-search
  - hierarchical
  - multi-stage-process
  - warp
  - linear-SVM
  - SVM
  - NMS
  - bounding-box-regressor
  - category-independent-region
  - feature-extraction
  - translationequivariance
  - hard-negative-mining
---
: Rich feature hierarchies for accurate object detection and semantice segmentation

**link**:  https://arxiv.org/abs/1311.2524 
**cf**[^6]

### Abstract

- Object detection은 지난 몇 년간 정체됨 
- R-CNN은 지금까지 가장 성능이 좋았던 VOC 2012보다 **mAP**[^7]를 30% 이상 높임
- R-CNN이 제안하는 것
	- Localize와 segmentation을 위해 **Region proposal**[^0]에 CNN 적용
	- Labeled training data가 부족할 때, **auxiliary task**[^1]을 위해 superviese pre-training을 수행, **domain-specific fine tunning**[^2]을 적용해 성능 향상
- R-CNN은 sliding window detector 기반의 객체 탐지 알고리즘인 OverFeat의 성능을 뛰어 넘음
	- OverFeat model은 bounding box가 정확하지 않고, 모든 sliding window에 CNN을 적용하기 때문에 연산량이 많다는 문제점을 가지고 있음
- Object detection을 수행하기 위해 딥러닝을 최초로 적용했다는 점에서 의의
- **But**, Selective search를 사용하여 이미지 한 장당 2000개의 regional proposal를 추출하다보니, 학습 및 추론 속도가 매우느리다는 단점 / Fine tuned AlexNet, linear SVM, Bounding box regressor라는 3가지 모델을 사용해 전체 구조와 학습 과정이 복잡하다는 문제제


### Introduction

- 지난 몇 년 동안 SIFT와 HOG에 의해 다양한 인식 분야가 발전됨
- 인식은 여러 단계 아래에서 이루어지는데 이는 시각적 인식에 훨씐 더 많은 정보를 제공하는 특징을 계산하는 **hierarchical**, **multi-stage process**가 있음을 시사
- CNN이 훌륭한 성과를 내면서 이미지넷에서 CNN 분류 결과가 PASCAL VOC Challenge 객체 탐지에 적용할 수 있는지에 대한 논의 이루어짐
	- Imgae classification과 object detection 사이의 간극을 메우기 위해 2가지 문제에 집중
		- ==Localizing object with deep network== 
		- ==Training a high-capacity model with only a small quantity of annotated[^3] detection data== 
		
	- Imgae classification과 달리 object detection은 이미지 내의 다수의 object를 localization 해야 함
		- Localization을 regression problem으로 간주
		- Sliding-window detector를 구축, Sliding-window 접근 방식 채택 
		
![[Pasted image 20230930162816.png]]
#### Object detection system overview
1. input image에 대해 **Selective Search**[^4]을 사용해 객체가 있을 법한 위치인 후보 영역(category-independent region proposal[^0]) 2,000 개를 추출
2. 각각의 region proposal에 227 x 227 크기로 **warp**(=resize) 시켜 줌
3. Warp된 모든 region proposal을 Fine tune된 AlexNet에 입력하여 2000 x 4096 크기의 feature vector 추출
4. 추출된 feature vector를 **linear SVM** 모델과 **Bounding box regressor** 모델에 입력하여 각각 confidence score와 조정된 bounding box 좌표를 얻음
5.  [[NMS(non-maximum suppression)]]을 적용해 최소한의, 최적의 bounding box 출력


### Object detection with R-CNN

- 크게 3가지 module로 구성
	1. 각 클래스별로 영역 제안
	2. 제안된 영역으로부터 fixed-length feature vector를 추출하는 대규모 CNN
	3. 클래스별 linear SVMs 집합

#### Module design
##### Region proposal
- 객체의 위치를 추정하기 위해 ==Selective Search==를 사용해 ==category-independent region== 생성
	- 비슷한 영역을 그룹핑하면서 후보 영역을 제안
	- 색상, 무늬, 명암 등의 다양한 기준으로 non-object based segmentation을 수행해 픽셀을 grouping
	- Bottom-up 방식으로 small segemented area를 합쳐 더 큰 segmented area를 만듬(점차 통합시켜 객체가 있을법한 위치를 bounding box 형태로 추천)
	- 단일 이미지에 약 2,000개의 후보 영역을 추출한 뒤, CNN 모델을 입력하기 위해 227 x 227 크기로 warp(=resize) 시켜 줌
	![[Pasted image 20230930165207.png]]
		- **input**: Single image
		- **process**: region proposal by Selective search & warp
		- **output**: 227 x 227 sized 2000 region proposals

##### Feature extraction
- Selective Search를 통해 도출된 각 region proposal(2000개)로부터 ==Fine tune된 AlexNet==에 입력하여 4,096 차원의 feature vector 추출
	
	**Fine tuning pre-trained AlexNet**
	- region proposal은 배경을 포함할 수도 있고 객체를 포함할 수도 있음
	- fine tune 시 예측하려는 객체의 수가 N개라고 할 때 ==배경을 포함하여 (N+1)개의 class 를 예측하도록 모델을 설계==하고 객체와 배경을 모두 포함한 학습 데이터를 구성
		- PASCAL VOC 데이터셋에 Selective search 적용해 객체와 배경이 모두 포함된 후보 영역을 추출하여 학습데이터로 사용
		
	1. PASCAL VOC 데이터셋에 Selective Search 적용해 후보 영역 추출
	2. 후보 영역과 ground truth box와 IoU 값을 구함
	3. IoU 값 
		- 0.5 이상인 경우 positive sample(객체)로 저장
		- 0.5 미만인 경우 negative sample(배경)로 저장
	4. postive sample = 32, negative sample = 96로 mini batch(=128)을 구성하여 pre-trained된 AlexNet에 입력하여 학습을 진행해 feature vector 추출
		- feature들은 5개의 convolutional layers와 2개의 fully connected layer로 전파
		- 이때 CNN의 입력으로 사용되기 위해 각 region은 227 x 227 RGB의 고정된 크기로 변환(warping)
		- warping 하기 전에, region proposal 주변 배경을 16 pixel만큼 살릴 때 성능이 가장 좋음
		![[Pasted image 20230930213813.png]]
			- **input**: 227 x 227 sized 2000 region proposal
			- **process**: feature extraction by fine tuned AlexNet
			- **output**: 2000 x 4096 sized feature vector
	
	![[Pasted image 20230930214204.png]]
	![[Pasted image 20230930214228.png]]


##### Classification by linear SVM
- 특정 class에 해당하는지 여부만을 판단하는 이진 분류기(binary classifier)인 linear [SVM](https://ko.wikipedia.org/wiki/%EC%84%9C%ED%8F%AC%ED%8A%B8_%EB%B2%A1%ED%84%B0_%EB%A8%B8%EC%8B%A0)(Support Vector Machine)은 2000 x 4096 feature vector를 입력 받아 class를 예측하고 confidence score를 반환
- N개의 class를 예측한다고 할 때, 배경을 포함한 (N+1)개의 독립적인 linear SVM 모델을 학습시켜야 함
	
	**Training linear SVM using fine tuned AlexNet**
	1. 객체와 배경을 모두 학습하기 위해 PASCAL VOC 데이터셋에 Selective search 적용해 region proposal 추출
	2. AlexNet 모델을 fine tune할 때와 달리 오직 ground truth box만을 positive sample로, IoU 값이 0.3 미만인 예측 bounding box를 negative smaple로 저장
	3. postive sample = 32, negative sample = 96로 mini batch(=128)을 구성하여 fine tuned AlexNet에 입력하여 feature vector 추출하고, 이를 linear SVM에 입력하여 학습시킴
		- 하나의 linear SVM 모델을 측정 class에 해당하는지 여부를 학습하기 때문에 output unit = 2
	4. 학습이 한 차례 끝난 후, [[Hard Negative Mining]]기법을 적용하여 재학습
	![[Pasted image 20230930215644.png]]
		- **input**: 2000 x 4096 sized feature vector
		- **process**: class prediction by linear SVM
		- **output**: 2000 classes and confidence scores


##### Detailed localization by Bounding box Regressor
- Selective search을 통해 얻는 객체의 위치는 다소 부정확할 수 있기에 이를 해결하기 위해 ==bounding box의 좌표를 변환하여 객체의 위치를 세밀하게 조절==해주는 bounding box regressor 모델을 적용
	![[Pasted image 20230930235436.png]]
	- 회색 box는 Selective search에 의해 예측된 bounding box
	- 빨간 테두리는 ground truth box
	- Bounding box regressor는 예측한 bounding box의 좌표 _p = (px, py, pw, ph)(center X, center Y, width, height)_ 가 주어졌을 때, ground truth box의 좌표 _g = (gx, gy, gw, gh)_ 로 변환되도록 하는 Scale invariant Transformation을 학습
	![[Pasted image 20230930235925.png]]
	: 즉, Bounding box regressor 모델은 di(P)가 ti가 되도록 Lreg를 통해 학습시킴
	
	
	 **Training linear SVM using fine tuned AlexNet**
	 1. PASCAL 데이터셋에 Selective search을 적용하여 얻은 region proposal를 학습 데이터로 사용
	 2. IoU 값이 지나치게 작거나 겹쳐진 영역이 없는 경우, 모델을 통해 학습시키기 어렵기 때문에 별도의 negative sample은 정의하지 않고 IoU 값이 0.6 이상인 sample을 positive sample로 정의
	 3. postivie sample을 fine tuned된 AlexNet에 입력하여 얻은 feature vector를 Bounding box regressor에 이력하여 학습시킴
	 4. 추론 시 Bounding box regressor는 feature vector를 입력 받아 조정된 bounding box 좌표값(output unit=4)을 반환
	![[Pasted image 20231001000553.png]]
		- **input**: 2000 x 4096 sized feature vector
		- **process**: boundign box coordinates transformation by Bounding box regressor
		- **output**: 2000 bounding box coordinates


##### Non maximum Suppression
- linear SVM 모델과 Bounding box regressor을 통해 얻은 2000개의 bounding box를 전부 다 표시할 경우 하나의 객체에 대해 지나치게 많은 bounding box가 겹칠 수 있음, 이로 인해 객체 탐지의 정확도가 떨어질 수 있음
- 이러한 문제를 해결하기 위해 bounding box 중에서 비슷한 위치에 있는 box를 제거하고 가장 적합한 box를 선택하는 [[NMS(Non-Maximum Suppression)]]] 을 적용
	![[Pasted image 20231001001953.png]]
	- **input**: 2000 bounding box
	- **process**: removing unnecessary boxes with NMS
	- **output**: optimal bounding boxes wih classes












[^0]: Region proposal
	- 영역 제안

[^1]: auxiliary task
	- 보조 task
	- 본 task는 아니지만 본 task에서의 성능이 더 잘 나올 수 있도록 도와줌

[^2]: domain-specific fine tuning
	- 영역별 미세 조정

[^3]: annotated
	- 객체 레이블 정보, 경계 박스 위치 정보 등을 포함하는 메타 정보
	- 경계 박스란 객체 위치를 표시하는 bounding box

[^4]: Selective search
	- Sliding window와는 아예 다른 방식
	- Bounding box를 random하게 많이 생성을 하고 이들을 조금씩 Merge 해나가면서 물체를 인식해나가는 방식
	- 입력 영상에 대해 segmentation을 실시해 이를 기반으로 후보 영역을 찾기 위한 seed 설정
	- 초기에 엄청나게 많은 후보들이 만들어짐
	- 이를 적절하게 통합해 나가면, segmentation은 후보 영역의 개수가 줄어들고 결과적으로 이를 바탕으로 Bounding box 후보 개수도 줄어듬
	- R-CNN에서는 모델 추론 시에도, AlexNet을 fine tune 할 때도 사용

[^5]: low dimensional
	- 상대적으로 feature 수가 적은 것을 의미

[^6]:
	- [https://herbwood.tistory.com/5](https://herbwood.tistory.com/5) 
	- [https://bkshin.tistory.com/entry/%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-R-CNN-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0](https://bkshin.tistory.com/entry/%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-R-CNN-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0)
	- [https://ganghee-lee.tistory.com/35](https://ganghee-lee.tistory.com/35)
	- [https://velog.io/@jaehyeong/R-CNNRegions-with-CNN-features-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0](https://velog.io/@jaehyeong/R-CNNRegions-with-CNN-features-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0)
	- [https://velog.io/@skhim520/R-CNN-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0](https://velog.io/@skhim520/R-CNN-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0)


[^7]: mAP
	- Mean Average Precision
	- AP는 정확도
	- mAP는 CNN을 평가할때 사용하는 지표


