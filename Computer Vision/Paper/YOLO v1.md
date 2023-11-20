---
tags:
  - YOLOv1
  - FPS
  - 1-stage-detector
  - 2-stage-detector
  - DarkNet
  - NMS
---
**link**: https://arxiv.org/abs/1506.02640
**cf**[^0]


### Abstract

- YOLO = You Only Look Once: Unified, Real-Time Object Detection
- **2-stage detector**는 Regional Proposal과 classification을 수행하는 network 혹은 컴포넌트 분리
	- **각 task가 순차적으로 진행**되는 것을 의미
	- **병목현상 발생**하여 detection 속도 느려짐
- 반면 ==1-stage detector는 하나의 통합된 네트워크가 두 task를 동시에 진행==
	- **FPS**[^1]를 개선하여 real-time에 가까운 detection 속도를 보임![[Pasted image 20231004175020.png]]
		
		⮚ ==localization과 classification을 하나의 문제로 정의==하여 ==network가 동시에 두 task를 수행==하도록 설계
		⮚ 이미지를 지정한 grid로 나누고, 각 grid cell이 한번에 **bounding box**와 **class** 정보라는 2가지 정답을 도출하도록 만듬
		⮚ 각 **grid cell**에서 얻은 정보를 잘 feature map이 잘 encode할 수 있도록 독자적인 Convolutional Network인 **DarkNet**을 도입      
		⮚  얻어진 feature map을 활용하여 자체적으로 정의한 regression loss를 통해 전체 모델을 학습
		



### Main Idea

##### 1-stage detector
- 별도의 region proposals을 사용하지 않고 ==전체 이미지를 입력==하여 사용
	![[Pasted image 20231004194914.png]]
	
	1. 전체 이미지를 _S x S_ 크기의 grid로 나눠줌
	2. 객체 중심이 특정 grid cell에 위치하면, 해당 grid cell은 그 객체를 detect하도록 할당(responsible for)됨      
		⮚  위의 그림에서는 4행 3열의 grid cell이 왼쪽의 개를 예측하도록 할당     
		⮚  4행 4열의 grid cell이 오른쪽의 개를 예측하도록 할당     
		: 나머지 grid cell은 객체를 예측하는데 참여할 수 없음을 의미
		
	
	- 각각의 grid cell은 *B*개의 bounding box와 해당 bounding box에 대한 **confidence score**[^2]를 예측      
	- 각각의 bounding box는 box의 좌표 정보(x, y, w, h)와 confidence score라는 5개의 예측값을 가짐      
		⮚  __x, y__: grid cell의 경계에 비례한 box의 중심 좌표, grid cell 내에 위치하기 때문에 _0 ~ 1_ 사이의 값을 가짐  
		⮚  _w, h_: 높이, 너비 마찬가지로 grid cell에 비례한 값, 객체의 크기가 grid cell보다 더 클 수 있기 때문에 _1_ 이상의 값을 가질 수 있음		 
		 
		⮚ 하나의 bounding box는 하나의 객체만을 예측 , 하나의 grid cell은 하나의 bounding box를 학습에 사용
			ex) grid cell별로 B개의 bounding box를 예측한다고 할 때, confidence score가 가장 높은 1개의 bounding box만 학습에 사용
		![[Pasted image 20231004201907.png]]
		 
	
	- 각 grid cell은 *C*개의 conditional class probabilities인 _Pr( Classi | Object )_ 를 예측      
		⮚ 특정 grid cell에 객체가 존재한다고 가정했을 때, 특정 _class i_ 일 확률인 조건부 확률값   
		⮚  bounding box 수와 상관없이 하나의 grid cell마다 하나의 조건부 확률을 예측      
		⮚  ~~bounding box별로 class probabilities를 예측~~      
		⮚  grid cell별로 class probabilities를 예측 
			ex) _S = 7_, _B = 2_, _C = 20_ 이라면 이미지를 _7 x 7_ grid로 나누고 각 grid cell은 _2_ 개의 bounding box와 해당 box의 confidence score, 그리고 _C_ 개의 class probabilities를 예측
			(이미지별 예측값의 크기는 _7 x 7 x (2 x 5 + 20)_ )
			
	
	- 위 과정을 통해 bounding box의 위치와 크기, class에 대한 정보를 동시에 예측하는 것이 가능
	


##### DarkNet
- **1-stage detector** 을 통해 얻은 최종 예측값의 크기인 _7 x 7 x 30_ 에 맞는 feature map을 생성하기 위해  DarkNet이라는 Convolutional Network을 설계
	![[Pasted image 20231004212041.png]]
	- DarkNet은 ImageNet 데이터셋을 통해 학습
	- 이후 모델이 detection task를 수행할 수 있도록 _4_ 개의 conv layer와 _2_ 개의 FC layer를 추가    
		⮚ classification task를 위해 학습시킬 때는 _224 x 224_ 크기의 이미지를 사용     
		⮚ detection task를 위해 학습시킬 때는 _448 x 448_ 크기의 이미지를 사용
			detection task는 **fine grained**한 시각 정보를 필요하기 때문, 이미지 해상도(크기)를 높이면 각 픽셀이 더 작은 물체나 물체의 세부 정보를 나타낼 수 있음


##### Loss function
- 기존 R-CNN 계열의 모델이 classification, localization task에 맞게 서로 다른 loss function을 사용했던 것과 다르게 YOLO v1은 **SSE**(Sum of Squared Error)를 사용
	![[Pasted image 20231004214417.png]]
	- **Localization loss**, **Confidence loss**, **Classification loss**의 합으로 구성
		
		![[Pasted image 20231005142732.png]]
		![[Pasted image 20231005142745.png]]
		![[Pasted image 20231005142800.png]]


### Training YOLO v1

- 위에서 정의한 **DarkNet**에 이미지를 입력하여 _7 x 7 x 30_ 크기의 feature map을 loss function을 통해 학습시킴
	![[Pasted image 20231005143216.png]]



### Detection

- detection 시에는 최종 예측 결과에 [[NMS(Non-Maximum Suppression)]]을 적용, mAP 값이 2 ~ 3%정도 향상
- sliding window 방식이나 region proposal 기반의 모델과는 달리 ==YOLO v1 모델은 전체 이미지를 인지하여 맥락 정보(contextual information)을 학습==
- YOLO v1 모델은 ==일반화 가능한 표현(representations)를 학습==하여 ==새로운 도메인이나 예상치 못한 입력 이미지에 대해 상대적으로 robust한 모습==을 보임



### Limitation

- 각 grid cell은 2개의 boxes만 예측하고 1개의 class를 가질 수 있기 때문에 YOLO v1은 **bounding box 예측에 강력한 공간 제약**을 부과 
	⮚   이러한 공간 제약은 모델이 예측할 수 있는 주변 물체의 수가 제한
	
- 그룹으로 나타나는 작은 객체를 제대로 탐지 못함


















[^0]:
	- https://herbwood.tistory.com/13 
	- https://velog.io/@qtly_u/n4ptcz54 


[^1]: FPS
	- Fram Per Second
	- 1초당 몇 이미지 Frame이 우리에게 보여지는지
	
	![[Pasted image 20231004174821.png]] 
	

[^2]: confidence score
	- 해당 bounding box에 객체가 포함되어 있는지 여부와 box가 얼마나 정확하게 ground truth box를 예측했는지를 반영하는 수치
	- _Confidence score = Pr(Object) * IoU(truthpred)_ 
		- grid cell 내에 객체가 존재하지 않는다면 confidence score는 0이 됨
		- grid cell 내에 객체가 존재한다면 confidence score은 IoU 값과 같아짐

