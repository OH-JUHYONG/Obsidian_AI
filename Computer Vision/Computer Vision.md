---
tags:
  - neural-network
  - ResNet
  - CNN
  - FC-layer
  - pooling
  - classification
  - translation-invariance
  - localization
  - object-detection
  - NMS
  - segmentation
  - semantic-segmentation
  - instance-segmentation
  - FCN
  - R-CNN
  - Fast-R-CNN
  - Faster-R-CNN
  - FPN
  - Mask-R-CNN
  - YOLOv1
  - DETR
  - data-annotation
  - JSON
---
**cf**[^0]


### [[Neural Network]]

- [[ResNet]]

##### CNN
-  [[FC layer]]
-  [[Pooling]]



---


![[Pasted image 20231001112246.png]]

### Classification(분류)

- 해당 이미지가 어떤 객체(물체)를 나타내는지 분류하는 작업  
- [[Translation Invariance]]를 만족해야 함 
	![[Pasted image 20231001112755.png]]


### Localization(발견)

- 이미지 안에 있는 하나의 객체 위치를 찾아주는 작업
- Bounding box
	⮚   객체 위치를 표시하는 네모 박스 경계 박스
	⮚   위치 x, y좌표는 연속된 값이기 때문에 회귀에 해당 
	![[Pasted image 20231001112950.png]]

### Object Detection(발견)

- 이미지 안에 있는 여러 객체들의 위치를 찾아내고, 각각 분류까지 하는 작업
- Classification과  Localization에 비해 어려운 이유?
	- Classification과 Localization 모두 적용해야 함
	- 다양한 크기와 모양을 갖는 객체가 섞여 있음
	- 객체 탐지를 실시간으로 해야 하는 경우가 많음
	- 전체 이미지에서 배경이 차지하는 비중이 크기 때문
	- Classifiaction과 다르게 위치를 찾아야 하므로 [[Translation Invariance]]이면 안되고 translation variance를 만족해야 함

- [[NMS(Non-Maximum Suppression)]]
-   


### Segmentation(분할)

- object detection보다 더 발전된 단계, 픽셀 단위로 객체를 탐지하는 작업
- Image segmentation은 이미지 영역을 분할해서 각 object에 맞게 합쳐주는 것을 말함

- [[Semantic Segmentation]]
- [[Instance Segmentation]]
- [[FCN(Fully Convolutional Network)]]


### Computer Graphics

##### [[3D Reconstruction]]





### Paper

- [[R-CNN]]
- [[Fast R-CNN]]
- [[Faster R-CNN]]
- [[FPN]]
- [[Mask R-CNN]]
- [[YOLO v1]]
- [[DETR]]
- [[NeRF]]



### Dataset

- ImageNet


### Preprocessing

- [[Data annotation]]
- [[JSON]]





[^0]:
	- https://medium.com/hyunjulie/1%ED%8E%B8-semantic-segmentation-%EC%B2%AB%EA%B1%B8%EC%9D%8C-4180367ec9cb