---
tags:
  - Fast-R-CNN
  - warp
  - RoI
  - RoI-pooling
  - max-pooling
  - selective-search
  - RoI-projection
  - sub-sampling
  - multi-task-loss
---
**link**: https://arxiv.org/abs/1504.08083
**cf**[^0]


### Abstract

- 기존 R-CNN 모델보다 속도면에서 큰 개선을 보임
	⮚   R-CNN은 2000장의 region proposal를 CNN 모델에 입력시켜 각각에 대하여 독립적으로 학습시켜 많은 시간 소요
	⮚   Fast R-CNN은 이 문제를 개선하여 1장의 이미지를 입력 받으며, region proposal 를 warp(= resize) 시킬 필요 없이 **RoI pooling** 을 통해 ==고정된 크기의 feaure vector를 fully connected layer에 전달==



![[Pasted image 20231002231507.png]]

### Main idea

##### Rol(Region of Interest) Pooling
- RoI pooling은 feature map에서 region proposals에 해당하는 **관심 영역(Region of Interest)** 을 **지정한 크기의 grid**로 나눈 후 **max pooling**을 수행
	⮚   각 channel별로 독립적으로 수행
	⮚   이 방법을 통해 ==고정된 크기의 featur map을 출력==
	⮚   RoI Pooling의 장점
	1. 공간적 정확성 유지
		- 원본 이미지의 region proposals을 효율적으로 feature map과 mapping할 수 있고, 이는 ==원하는 객체나 영역의 공간적인 위치 정보를 유지==할 수 있음
		
	2. 크기 불변성
		- ==입력 이미지나 특성 맵의 크기에 관계없이 일정한 크기의 출력 특성 맵을 얻음==
		- 다양한 크기의 객체를 처리하거나 다양한 해상도의 이미지에서 객체를 감지하는 데 도움이 됨
		
	3. 계산 효율성
		- max pooling을 통해 간단한고 효율적으로 수행
		
	![[Pasted image 20231003113035.png]]
	![[Pasted image 20231002231549.png]]
	1. 원본 이미지를 CNN 모델에 통과시켜 feature map을 얻음
		⮚   _800 x 800_ 크기의 이미지를 VGG 모델에 입력하여 _8 x 8_ 크기의 feature map을 얻음
		⮚   sub-sampling[^1] ratio = 1/100 이라고 할 수 있음
		
	2. 동시에 원본 이미지에 대하여 **Selective Search**[^2]를 적용해 region proposal을 얻음
		⮚   원본 이미지에 Selective search를 적용해 _500 x 700_ 크기의 region proposal을 얻음
		
	3. feature map에서 각 region proposal에 해당하는 영역을 추출
		⮚   이 과정은 **RoI Projection**[^3]을 통해 가능
		⮚   Selective search를 통해 얻은 region proposal는 [[Pooling|sub-sampling]] 과정을 거치지 않지만, 원본 이미지의 feature map은 sub-sampling 과정을 여러 번 거쳐 크기가 작아짐
		⮚   작아진 feature map에서 region proposal이 encode(표현)하고 있는 부분을 찾기 위해 작아진 featur map에 맞게 region proposal를 투영해주는 과정 필요
		⮚   이는 region proposal의 크기와 중심 좌표를 sub sampling ration에 맞게 변경시켜줌으로써 가능
		
		⮚   Region proposal의 중심점 좌표, width, height와 sub-sampling ratio을 활용하여 feature map으로 투영시켜 줌
		⮚   feature map에서 region proposal에 해당하는 _5 x 7_ 영역을 추출
		
	4. 추출한 RoI feature map을 **지정한 sub-window의 크기**에 맞게 grid로 나눠줌
		⮚   추출한 _5 x 7_ 크기의 영역을 지정한 _2 x 2_ 크기에 맞게 grid를 나눠줌
		
	5. gride의 각 셀에 대하여 max pooling을 수행하여 고정된 크기의 feature map을 얻음
		⮚   각 grid 셀마다 max pooling을 수행하여 _2 x 2_ 크기의 feature map을 얻음
	
	⮚  이처럼 미리 지정한 크기의 sub-window에서 max pooling을 수행하다보니 region proposal의 크기가 서로 달라도 고정된 크기의 feature map을 얻을 수 있음  
	

##### Multi-task loss
- feature vector를 multi-task loss를 사용하여 Classifier와 Bounding box regressor을 동시에 학습습
- 모델을 개별적으로 학습시킬 필요 없이 한 번에 학습



[^0]:
	- https://herbwood.tistory.com/8
	


[^1]: subsampling
	- pooling을 거치는 과정
	


[^2]: Selective Search
	- Sliding window와는 아예 다른 방식
	- Bounding box를 random하게 많이 생성을 하고 이들을 조금씩 Merge 해나가면서 물체를 인식해나가는 방식
	- 입력 영상에 대해 segmentation을 실시해 이를 기반으로 후보 영역을 찾기 위한 seed 설정
	- 초기에 엄청나게 많은 후보들이 만들어짐
	- 이를 적절하게 통합해 나가면, segmentation은 후보 영역의 개수가 줄어들고 결과적으로 이를 바탕으로 Bounding box 후보 개수도 줄어듬
	- R-CNN에서는 모델 추론 시에도, AlexNet을 fine tune 할 때도 사용



[^3]: Projection
	- RoI의 공간 위치를 특성 맵에 맞게 매핑하는 역할
	- 이 과정을 통해 RoI의 위치 정보를 특성 맵과 일치시키고, 이후 RoI pooling 과정에서 해당 위치의 특성을 추출할 수 있게 함