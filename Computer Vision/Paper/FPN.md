---
sticker: lucide//book
tags:
  - FPN
  - bottom-up-pathway
  - top-down-pathway
  - lateral-connection
  - pyramid
  - high-resolution
  - low-level-feature
  - low-resolution
  - high-level-feature
  - featurized-image-pyramid
  - single-feature-map
  - pyramidal-feature-hierarchy
  - feature-pyramid-network
  - semantic-gap
  - ResNet
  - nearest-neighbor-upsampling
---
**link**: https://arxiv.org/abs/1612.03144v2
**cf**[^0] : 

: Feature Pyramid Networks for Object Detection


### Abstract

- Object Detection은 이미지 내 존재하는 다양한 크기의 객체를 인식함
- 모델의 크기에 상관없이 객체를 detect 할 수 있도록 다양한 방법을 사용해 왔지만 기존의 방식들은 모델의 추론 속도가 너무 느려지고 메모리를 지나치게 많이 사용한다는 문제가 발생

- **FPN**은 컴퓨팅 자원을 적게 차지하면서 다양한 크기의 객체를 인식하는 방법
	1. 원본 이미지를 convolutional network에 입력하여 forward pass를 수행하고, 각 stage 마다 서로 다른 scale를 가지는 4개의 feature map을 추출(**Bottom-up pathway**)
	2. **Top-down pathway**를 통해 각 feature map에 _1 x 1 conv_ 연산을 적용하여 모두 256 channel을 가지도록 조정하고 unsapmling을 수행
	3. **Lateral connection** 과을 통해 pyramid level 바로 아래 있는 feature map과 element-wise addition 연산을 수행
	⮚   이를 통해 얻은 4개의 서로 다른 feature map에 _3 x 3 conv_ 연산을 적용


### Main Idea

##### Pyramid
- convolutional network에서 얻을 수 있는 ==서로 다른 해상도의 feature map을 쌓아올린 형태==
- **level**은 피라미드의 각 층에 해당하는 feature map
	![[Pasted image 20231009153419.png]]
	
	⮚   convolutional network에서 ==입력층에 가까울수록== feature map은 높은 해상도(==high resolution==)를 가짐
	- 가장자리, 곡선 등과 같은 ==low-level feature==을 보유
	
	⮚   convolutional network에서 출력층에 가까울수록 feature map은 높은 해상도(low resolution)를 가짐
	- 질감과 물체의 일부분 등 class를 추론할 수 있는 high-level feature을 보유

#### 기존의 방식

![[Pasted image 20231009153956.png]]

- (a) Featurized image pyramid
	⮚   입력 이미지의 크기를 resize하여 다양한 scale의 이미지를 네트워크에 입력
	⮚   Overfeat 모델 학습 시 해당 방법 사용, 다양한 크기의 객체를 포작하는데 좋은 결과를 보여줌
	⮚   but, 이미지 한 장을 독립적으로 모델에 입력하여 feature map을 생성하기 때문에 추론 속도가 매우 느리고 메모리를 지나치게 많이 사용함
	

- (b)  Single feature map
	⮚   단일 scale의 입력 이미지를 네트워크에 입력하여 단일 scale의 feature map을 통해 object detection을 수행하는 방법
	⮚   [[YOLO v1]] 모델 학습시 사용
	⮚   학습 및 추론 속도가 매우 빠르지만 성능이 떨어진다는 단점

- (c)  Pyramidal feature hierarchy
	⮚   네트워크에서 미리 지정한 conv layer마다 feature map을 추출하여 detect 하는 방법
	⮚   SSD 모델 학습 시 해당 방법을 사용
	⮚   multi-scale feature map을 사용하여 성능이 높지만 feature map간 해상도 차이로 인해 학습하는 representation에서 차이가 **semantic gap**이 발생한다는 문제


##### Feature Pyramid Network
- 임의의 크기의 single-scale 이미지를 convolutional network에 입력하여 다양한 scale의 feature map을 출력하는 네트워크
- FPN은 기존의 convolutional network에서 지정한 layer별로 feature map을 추출하여 수정하는 네트워크

1. **Bottom-up pathway**
	- 이미지를 convolutional network에 입력하여 forward pass 하여 2배씩 작아지는 feature map을 추출하는 과정
	- 각 stage의 마지막 layer의 output feature map을 추출
	- 같은 크기의 feature map을 출력하는 layer를 모두 같은 stage에 속해있다고 정의
	- 더 깊은 layer일수록 더 강력한 feature를 보유하고 있기 때문에  각 stage 별로 마지막 layer를 pyramid level로 지정
	![[Pasted image 20231009155553.png]]
	
	⮚  [[ResNet]]의 경우 각 stage의 마지막 residual block의 output feature map을 활용하여 feature pyramid를 구성하며 각 output을 _{c2, c3, c4, c5}_ 라 지정
	⮚   이는 conv2, conv3, conv4, conv5의 output feature map임을 의미하며, 각각 _{4, 8, 16, 32} stride_ 를 가지고 있음( _{c2, c3, c4, c5}_ 은 각각 원본 이미지의 1/4, 1/8, 1/16, 1/32 크기를 가진 feature map, 참고로 c1은 너무 많은 메모리를 차지하기에 제외시킴)   


2. **Top-down pathway and Lateral connection**
	- top-down pathway는  **nearest neighbor upsampling** 방식을 사용해 각 pyramid level에 있는 feature map을 2배로 upsampling하고 channe 수를 동일하게 맞춤
	- 각 pyramid level의 feature map을 2배로 upsampling해주면 바로 아래 level의 feature map과 크기가 같아짐
	- 이후 모든 pyramid level의 feature map에 _1 x 1 conv_ 연산을 적용하여 channel을 256으로 맞춤
	![[Pasted image 20231009160634.png]]
	![[Pasted image 20231009160645.png]]
	- 그 다음 upsample된 feature map과 바로 아래 level의 feature map과 element-wise addition 연산을 하는 **Lateral connections** 과정을 수행
	- 이후 각각의 feature map에 _3 x 3 conv_ 연산을 적용하여 얻은 feature map을 각각 _{p2, p3, p4, p5}_ ,  이는 _{c2, c3, c4, c5}_ feature map의 크기와 같음
	- 가장 높은 level에 있는 feature map c2의 경우 _1 x 1 conv_ 연산 후 그대로 출력하여 _p2_ 를 얻음

- 위 과정을 통해 FPN은 single-scale 이미지를 입력하여 4개의 서로 다른 scale을 가진 feature map을 얻음
- 단일 크기의 이미지를 모델에 입력하기 때문에 기존 방식 (a) 방식에 비해 빠르고 메모리를 덜 차지
- multi-scale feature map을 출력하기 때문에 (b) 방식보다 더 높은 detection 성능을 보여줌
- Detection task 시 저해상도 feature map 비해 downsample된 수가 적기 때문에 고해상도 feature map은 low-level feature를 가지지만 객체의 위치에 대한 정보를 상대적으로 정확하게 보존
	-  이런 고해상도 feature map의 특징을 element-wise addition을 통해 저해상도 feature map에 전달하기 때문에 (c)에 비해 작은 객체를 더 잘 detect 함

















[^0]: https://herbwood.tistory.com/18
