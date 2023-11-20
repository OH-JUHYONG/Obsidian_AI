---
tags:
  - Mask-R-CNN
  - semantic-segmentation
  - instance-segmentation
  - Faster-R-CNN
  - mask-branch
  - RoI
  - RPN
  - Fast-R-CNN
  - FCN
  - object-detection
  - pixel
  - RoI-pooling
  - binary-mask
  - RoI-Align
  - misalignment
  - quantization
  - translation-invariance
  - bilinear-interpolation
  - max-pooling
  - loss-function
  - ResNet
  - FPN
---
**link**: https://arxiv.org/abs/1703.06870
**code**: https://github.com/facebookresearch/Detectron
**cf**[^0]
[[Custom_Mask R-CNN|Project]]

### Abstract

- Mask R-CNN은 이미지 내에서 각 instance[^1]에 대한 segmentation mask[^2]를 생성
	- object detection: 이미지 안에 있는 여러 객체들의 위치를 찾아내고, 각각 분류까지 하는 작업
	- [[Semantic Segmentation]]: 이미지에 있는 모든 pixel을 해당하는 class로 분류, 같은 class의 instance를 구별하지 않음, 이는 같은 class의 object들에 대해 서로 구분지을 수 없다는 단점
		

- Mask R-CNN = Faster R-CNN + **mask branch** 
	- Faster R-CNN의 RPN에서 얻은 [[Fast R-CNN|RoI]](Region of Interest)에 대해여 객체의 class를 예측하는 ==classification branch==
	- [[Faster R-CNN]] = RPN + [[Fast R-CNN]]
	-  ==mask brach==는 object의 mask를 예측하는 branch
	-    mask branch는 각각의 RoI에 작은 크기의 [[FCN(Fully Convolutional Network)]]가 추가된 형태 ![[Pasted image 20231001220337.png]]



### Introduction

- [[Instance segmentation]]은 individual objects를 분류하고 bounding box를 사용하여 각각의 위치를 파악하는 것이 목표인 **object detection**과 object instance를 구분하지 않고 각 pixel을 고정된 범주 집합으로 분류하는 것이 목표인 **semantic segmentation**를 결합한 방식

- Mask R-CNN은 Faster R-CNN을 확장한 방법
	⮚   각각의 **RoI**(Region of Interest, 관심 영역)에서 classification branch와 bounding box regression branch, 두 branch와 평행으로 segmentation mask를 예측하기 위한 branch를 추가
	⮚   mask branch는 각 RoI에 적용되는 작은 FCN으로, pixel 단위로 segmentation mask를 예측
	⮚   Faster R-CNN은  network input과 output간의 pixel 간 정렬을 위해 설계되지 않음

##### Mask branch
- **Faster R-CNN**은 [[Backbone Network|backbone network]]를 통해 얻은 feature map을 RPN(Region Proposal Network)에 입력하여 **RoI**를 얻음
	⮚   얻어진 **RoI**를 **RoI pooling**을 통해 **고정된 크기**의 feature map을 얻고 이를 [[FC layer]]에 입력한 후 ==classification branch==와 ==bounding box regression branch==에 입력하여 class label과 bounding box offset이라는 두 가지 결과를 예측 
	

- **Mask R-CNN**은 ==두 branch와 평행(두 작업이 동시에 진행)으로 segmentation mask를 예측==하는 **mask branch가 추가**된 구조
	⮚   RoI pooling을 통해 얻은 **고정된 크기의 feature map**을 mask branch에 입력하여 segmentation mask(class에 따라 분할된 이미지 조각)를 얻음
	![[Pasted image 20231002000011.png]]
	

- segmentation task는 pixel 단위로 class 분류하기 때문에 정교한 spatial layout[^3]를 필요
	⮚   이를 위해 ==mask branch는 여러 개의 conv layer로 구성된 작은 [[FCN(Fully Convolutional Network)|FCN]] 구조==를 가짐
	⮚   mask는 이미지 내 객체에 대한 공간 정보를 효과적으로 encode하는 것이 가능
	

- mask branch는 각각의 RoI에 대하여 class 별로 **binary mask**를 출력
	⮚   기존의 [[Instance segmentation]] 모델은 하나의 이미지에서 여러 class를 예측하는 반면, Mask R-CNN은 class별로 mask를 생성한 후 pixel이 해당 class에 해당하는지 여부를 표시
	

- mask branch는 최종적으로 $$K^2m$$크기의 feature map을 출력
	⮚   _m_: feature map의 크기
	⮚   _K_: class의 수
	

##### RoIAlign
- [[Fast R-CNN|RoI pooling]]을 사용하면 입력 이미지 크기와 상관없이 고정된 크기의 feature map을 얻을 수 있다는 이점
	⮚   그러나 이는 RoI pooling으로 인해 얻은 feature와 RoI 사이가 어긋나는 ==misalignment==가 발생, 이는 pixel mask를 예측하는데 매우 안 좋은 영향
	
	![[Pasted image 20231003113849.png]]
	⮚   RoI pooling에 앞서 RPN을 통해 얻은 RoI를 backbone network에서 추출한 feature map의 크기에 맞게 **projection**[^4]하는 과정을 거침
	⮚   위에 그림을 예시로 들면 RoI의 크기는 _145 x 200_ 이며, feature map은 _16 x 16_
	⮚   이를 sub sampling ratio(=_32_)에 맞게 나눠주면 project을 수행한 feature map은 _4.53 x 6.25_ 크기를 가지지만 pixel 미만의 크기로 분할하는 것은 불가능하기 때문에 크기에서 정수값을 사용하여 _4 x 6_ 크기의 feature map을 얻음
	⮚   _4 x 6_ 크기의 RoI에 대하여 고정된 크기에 맞추기 위해 stride는 _1 x 2_ 로 설정한 RoI pooling을 수행해 _3 x 3_ (위에 그림 맨 오른쪽) 크기의 고정된 feature map을 얻음
	![[Pasted image 20231003135956.png]]
	⮚   Mask R-CNN 논문에서는 ==RoI pooling 방식이 quantization 과정을 수반==하여 misalignment를 유도한다고 봄
	- quantization은 실수(floating) 입력값을 정수와 같은 이산 수치(discrete value)으로 제한하는 방법
	- 위에 왼쪽 그림을 통해 RoI projection을 수행하는 과정에서 소수점 부분이 반올림 되면서 초록색과 파란색 영역에 대한 정보가 손실됨을 알 수 있음
	- 위에 오른쪽 그림은 RoI pooling시 stride를 반올림하게 되면서 feature map의 마지막 row에 대한 정보가 소실됨을 보여주고 있음
	   
	⮚   ==quantization으로 인한 misalignment는== translation invariant한 classification task시에는 큰 영향이 없지만 ==pixel 단위로 mask를 예측하는 segmentation task 시에는 매우 안좋은 영향==을 끼침
	⮚   이 문제를 해결하기 위해 각각 RoI에 대하여 **RoIAlign** 방법을 적용
	

- RoIAlign이 이루어지는 과정
	![[Pasted image 20231003141306.png]]
	1. RoI projection을 통해 얻은 feature map을 ==quantization 과정 없이 그대로 사용==
	2. 출력하고자 하는 feature map의 크기에 맞게 projection된 feature map을 분할
		- 위의 그림에서는 _3 x 3_ 크기의 feature map으로 출력할 것이기 때문에 width, height를 각각 3등분 해줌 
		
	3. 분할된 하나의 cell에서 4개의 sampling point를 찾음, 이는 cell의 height, width를 각각 3등분하는 점에 해당
	4. **Bilinear interpolation**을 적용
		![[Pasted image 20231003142045.png]]
		- 2차원 좌표 상에서 두 좌표가 주어졌을 때 ==중간에 있는 값을 추정==하는 방법
		
		![[Pasted image 20231003142159.png]]
		- _x_, _x1_, _x2_, _y_, _y1_, _y2_: RoIAlign의 2번 과정에서 얻은 4개의 sampling point 좌표
		- _Q11_, _Q21_, _Q12_, _Q22_: sampling point에 인접한 cell의 값
		
		⮚ 위 공식에 따라 입력된 feature의 각각이 RoI bin의 4개의 sampled location의 값을 연산
		![[Pasted image 20231003142720.png]]
		
	5. RoIAlign의 2~4번 과정을 모든 cell에 대하여 반복
		![[Pasted image 20231003142824.png]]
		
	6. 하나의 cell에 있는 4개의 sampling point에 대하여 **max pooling**을 수행

- RoIAlign 방법을 통해 feature와 RoI 사이가 어긋나는 misalignment 문제를 해결하고 RoI의 정확한 spatial location을 보존하는 것이 가능해짐
- mask accuracy가 크게 향상

##### Loss function
- Mask R-CNN은 muli-task loss function을 통해 네트워크를 학습시킴
	![[Pasted image 20231003150224.png]]
	- _Lcls_: classification loss
	- _Lbox_: bounding box loss로 Faster R-CNN과 동일
	- _Lmask_: mask loss로 binary cross entropy loss
	 
	⮚   mask branch에서 출력한 _K(^2) * m_ 크기의 feature map의 각 cell에 sigmoid function을 적용한 후 loss를 구함
	⮚   기존의 segmentation 모델이 pixel별로 서로 다른 class를 softmax loss function을 사용
	⮚   Mask R-CNN은 class branch와 mask branch를 분리하여 class별로 mask 생성한 후 binary loss 구함


##### Backbone network
- Mask R-CNN은 backbone network로 [[ResNet]]-[[FPN]]을 사용








[^0]: 
	- https://herbwood.tistory.com/20
	- https://ropiens.tistory.com/76
	- https://velog.io/@sksmslhy/Paper-Review-Mask-R-CNN
	- https://ganghee-lee.tistory.com/40
	- https://blog.kubwa.co.kr/%EB%85%BC%EB%AC%B8%EB%A6%AC%EB%B7%B0-mask-r-cnn-2018-mask-r-cnn-%EC%8B%A4%EC%8A%B5-w-pytorch-cd525ea9e157
	- https://blahblahlab.tistory.com/139

[^1]: 이미지 내에서 식별할 수 있는 개별 객체나 물체


[^2]: class에 따라 분할된 이미지 조각(segment)


[^3]: spatial layout
	- 공간에 대한 배치 정보


[^4]: projection
	- RoI의 공간 위치를 특성 맵에 맞게 매핑하는 역할
	- 이 과정을 통해 RoI의 위치 정보를 특성 맵과 일치시키고, 이후 RoI pooling 과정에서 해당 위치의 특성을 추출할 수 있게 함
	