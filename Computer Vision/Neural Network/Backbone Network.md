---
sticker: lucide//network
tags:
  - backbone-network
  - object-detection
  - segmentation
  - 1-stage-detector
  - 2-stage-detector
---
cf[^0]

: 주로 [[Computer Vision|Object Detection]]과 [[Computer Vision|Segmentation]]과 같은 복잡한 작업을 수행하는 더 큰 신경망의 기본 구조 또는 **Spine**(척추, 핵심적인 기능을 수행) 역할

- input image를 feature map으로 변형시켜주는 부분, input image에서 고수준의 특징을 추출하는 역할
- 대량의 데이터로 사전 훈련되었기 때문에, 효율적인 특징 추출이 가능
	ex) VGG16, ResNet-50 ...

### Detector 구조
- Backbone과 Head라는 두 부분으로 구성
- **Head**: Backbone에서 추출한 feature map의 location 작업 수행, predict classes, bounding boxes 작업이 수행
	- **Dense prediction** 헤드를 사용하는 [[YOLO v1|1-stage detector]]
	- **Sparse prediction** 헤드를 사용하는 [[YOLO v1|2-stage detector]]








[^0]: https://ddangjiwon.tistory.com/103

