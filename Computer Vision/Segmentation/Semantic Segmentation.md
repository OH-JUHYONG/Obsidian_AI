---
tags:
  - semantic-segmentation
  - dense-prediction
  - FCN
  - upsampling
  - downsampling
  - pooling
  - FC-layer
---
: 이미지에 있는 모든 pixel을 해당하는 class로 분류

- 이미지에 있는 모든 pixel에 대한 예측을 하는 것이기 때문에 dense prediction이라고 불림
- 먼저 대상 감지가 발생한 다음, 각 pixel에 label이 지정
- ==같은 class의 instance를 구별하지 않음==
	- 같은 class의 object들에 대해 서로 구분지을 수 없다는 단점

#### Process
- **input**: RGB color 이미지(_height_ x _width_ x _3_ ) 또는 흑백 이미지(_height_ x _width_ x _1_ )
- **output**: 각 pixel 별로 어느 class에 속하는지 나타내는 label을 나타낸 Segmentation Map
	![[Pasted image 20231001150112.png]]
	![[Pasted image 20231001214326.png]]
	1. One-Hot encoding으로 각 class에 대해 출력채널을 만들어서 segmentation map을 만듬
	2. class의 개수 만큼 만들어진 채널을 argmax를 통해 위에 있는 이미지 처럼 하나의 출력물을 내놓음
	![[Pasted image 20231001150355.png]]
	
	- semantic segmentation은 pixel들이 각 class에 대해 binary하게 포함되는지 안되는지 여부만 따짐
	


#### When use Semantic Segmentaton?
- AlexNet, VGV 등 ==분류에 자주 쓰이는 신경망은 적합하지 않음==
	⮚  주로 parameter의 개수와 차원을 줄이는 layer를 가지고 있어서 자세한 위치정보를 잃게 됨
	⮚   보통 마지막에 쓰이는 FC layer에 의해서 위치에 대한 정보를 잃음

- ==공간/위치에 대한 정보를 잃지 않기 위해==서 ==Pooling과 FC layer을 없애고== stride 1이고 padding이 일정한 convolution을 진행할 수 있지만 이는 parameter의 개수가 많아지고 메모리 문제나 계산하는데 비용이 너무 많이 든다는 문제
- 이를 해결하기 위해 Semantic Segmentation은 Downsampling, Upsampling 형태를 사용
	**Downsampling**(encoder)
	- 차원을 줄여 적은 메모리로 깊은 convolution을 할 수 있게 함
	- encoder 통해 입력받은 이미지의 정보를 압축된 vector로 표현
	- 보통 stride를 2로 하거나 pooling 사용, 이 과정은 feature 정보를 잃게 됨
	- 마지막에 Fully-Connected Layer를 넣지 않고, Fully Connected Network를 주로 사용
	 
	**Upsampling**(decoder)
	- Downsampling을 통해 받은 결과의 차원을 늘려서 input과 같은 차원으로 만들어 주는 과정
	- decoder를 통해 원하는 결과물의 크기로 만들어
	- 주로 Strided Transpose Convolution을 사용
	 
	⮚  encoder-decoder 형태를 가진 모델로  [[FCN(Fully Convolutional Network)]], SegNet, UNet이 있음
	 




