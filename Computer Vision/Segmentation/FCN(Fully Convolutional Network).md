---
tags:
  - FCN
  - semantic-segmentation
  - FC-layer
  - CNN
---
**cf**[^0]

: [[Semantic Segmentation]] 모델을 위해 기존 image classification에서 우수한 성능을 보인 CNN 기반 모델(AlexNet, VGG16, GoogLeNet)을 변형시킨 구조

- [[FC layer]]와 다름
- VGG모델은 이미지의 feature을 추출하기 위한 네트워크 뒷단에 fully connected layer을 붙여, 계산한 class 별 확률을 바탕으로 image classification을 수행
- FCN에서는 Segmentation을 하기 위해 네트워크 뒷단에 fully connected layer 대신 ==CNN을 붙임==
	![[Pasted image 20231001152837.png]]
	-  VGG16으로부터 Transfer Learning을 사용
	-  VGG16 마지막 layer인 fully connected layer을 _1 x 1_ 의 convolution layer로 바꿈으로써 위치 정보를 유지하면서 class 단위의 feature map을 얻어 Segmentation할 수 있음
		⮚   ==fully connected layer을 없앤 이유==는 이 layer을 거치면 **이미지의 위치 정보가 사라지고 input image의 크기가 고정된다는 단점**이 있기 때문
	- 바뀐 convolution layer은 _1 x 1_ 의 kernel size와 class 개수 만큼의 channel을 가짐
		- 하지만 feature map의 크기는 일반적으로 원본 이미지보다 작기 때문에 **feature coarse**[^1]의 특성을 가짐
		- Transposed Convolution을 통해서 이 낮은 해상도의 heap map을 Upsampling해서 input과 같은 크기의 맵을 만듬
		- Upsampling 할 때 VGG16의 낮은 layer의 특징맵을 더함








[^0]:
	- https://medium.com/hyunjulie/1%ED%8E%B8-semantic-segmentation-%EC%B2%AB%EA%B1%B8%EC%9D%8C-4180367ec9cb
	- https://velog.io/@cha-suyeon/%EB%94%A5%EB%9F%AC%EB%8B%9D-Segmentation3-FCNFully-Convolution-Network
	- https://medium.com/@msmapark2/fcn-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-fully-convolutional-networks-for-semantic-segmentation-81f016d76204


[^1]: feature coarse
	- 거칠다, 조잡한
	- 출력이 입력 데이터보다 해상도가 낮다는 것을 의미
		- 이는 출력이 더 큰 공간적인 컨텍스트를 포착하고, 작은 세부 사항을 무시하는 경향