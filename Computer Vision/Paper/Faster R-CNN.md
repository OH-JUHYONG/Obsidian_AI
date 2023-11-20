---
tags:
  - Fast-R-CNN
  - anchor-box
  - dense-sampling
  - aspect-ratio
  - RPN
  - feature-map
---

**link**: https://arxiv.org/abs/1506.01497
**cf**[^0] : ICCV 2015


![[Pasted image 20231003013012.png]]


### Main Idea

##### Anchor box
- Selective search를 통해 region proposal을 추출하지 않을 경우, ==원본 이미지를 일정 간격의 grid로 나눠 각 grid cell을 bounding box로 간주하여 featur map에 encode==하는 **Dense Sampling** 방식을 사용 
	![[Pasted image 20231005165243.png]]
	⮚   sub-sampling ratio 기준으로 grid를 나누게 됨
	 ex) 원본 이미지 크기 _800 x 800_, sub-sampling ratio _1/100_ 인 경우 CNN 모델에 입력시켜 얻은 최종 feature map의 크기는 _8 x 8(800 x 1/100)_ 
	- feature map의 각 cell은 원본 이미지의 _100 x 100_ 만큼의 영역에 대한 정보를 함축하고 있다고 할 수 있음, 원본 이미지에서는 _8 x 8_ 개만큼의 bounding box가 생성
	
	⮚   고정된 크기의 bounding box를 사용할 경우, 다양한 크기의 객체를 포착하지 못할 수 있다는 문제가 발생

- ==지정한 위치에 사전에 정의한 서로 다른 크기(scale)와 가로세로비(aspect ratio)를 가지는 bounding box==인 **Anchor box**를 생성하여 다양한 크기의 객체를 포착하는 방법 제시
	![[Pasted image 20231005165654.png]]
	![[Pasted image 20231005165703.png]]
	- _scale_: anchor box의 width(=_w_), height(=_h_)의 길이
	- _aspect ratio_: width, height의 길이의 비율
		⮚   aspect ratio에 따른 width, height의 길이는 aspect ratio가 1:1일 때의 anchor box의 넓이를 유지한 채 구함
	

- anchor box는 원본 이미지의 각 grid cell의 중심을 기준으로 생성
	⮚   원본 이미지에서 sub-samping ration를 기준으로 anchor box를 생성하는 기준점인 anchor를 고정
	⮚   이 anchor를 기준으로 사전에 정의한 anchor box 9개를 생성  
	![[Pasted image 20231005170710.png]]
	- 위 그림에서 원본 이미지의 크기는 _600 x 800_, sub-sampling ratio은 _1/16_ 
	- anchor가 생성되는 수는 _1900(=600/16 x 800/16)_, anchor box는 총 _17100(=1900 x 9)_ 개 
	
	⮚   기존에 고정된 boundign box를 사용할 때보다 9배 많은 bounding box를 생성, 보다 다양한 크기의 객체를 포착하는 것이 가능  


##### RPN(Region Proposal Network)
- RPN은 ==원본 이미지에서 region proposals를 추출하는 네트워크==
- 원본 이미지에서 **anchor box**를 생성하면 수많은 region proposals가 만들어짐
- RPN은 region proposals에 대하여 class score을 매기고, bounding box coefficient를 출력하는 기능
	![[Pasted image 20231003013328.png]]
	
	1. 원본 이미지를 pre-trained된 VGG 모델에 입력하여 feature map을 얻음
		⮚   원본 이미지의 크기가 _800 x 800_ 이며 sub-sampling ratio가 1/100라고 했을 때 _8 x 8_ 크기의 feature map이 생성, channel 수는 512개
		
	2. 위에서 얻은 feature map에 대하여 _3 x 3 conv_ 연산을 적용, ==feature map의 크기가 유지될 수 있도록 padding을 추가==
		⮚   _8 x 8 x 512_ feature map에 대하여 _3 x 3_ 연산을 적용하여 _8 x 8 x 512_ 개의 feature map이 출력
		
	3. class score을 매기기 위해 feature map에 대하여 _1 x 1 conv_ 연산을 적용
		⮚   이때 출력하는 feature map의 channel 수가 _2 x 9_ 가 되도록 설정
		⮚   RPN에서는 후보 영역이 어떤 class에 해당하는지까지 구체적인 분류를 하지 않고 객체가 포함되어 있는지 여부만을 분류
		⮚   anchor box가 각 grid cell마다 9개가 되도록 설정 --> channel 수는 _2(object 여부) x 9(anchor box 9개)_ 가 됨
		
		⮚   _8 x 8 x 512_ 크기의 feature map을 입력받아 _8 x 8 x 2 x 9_ 크기의 feature map을 출력
		
	4. bounding box regressor를 얻기 위해 feature map에 대하여 _1 x 1 conv_ 연산을 적용
		⮚   _8 x 8 x 512_ 크기의 feature map을 입력받아 _8 x 8 x 4 x 9_ 크기의 feature map을 출력
		
	![[Pasted image 20231003014308.png]]
	- 좌측 표는 anchor box의 종류에 따라 객체 포함 여부를 나타낸 feature map
	- 우측 표는 anchor box의 종류에 따라 bounding box regressor를 나타낸 feature map
	- 이를 통해 _8 x 8 grid cell_ 마다 9개의 anchor box가 생성되어 총 576(= _8 x 8 x 9_)개의 region proposals가 추출, feature map을 통해 각각에 대한 객체 포함 여부와 bounding box regressor를 파악할 수 있음
	- 이후 class score에 따라 상위 N개의 region proposals만을 추출하고, Non maximum supression을 적용하여 최적의 region proposals만을 Fast R-CNN에 전달
	 












[^0]:
	- https://herbwood.tistory.com/10 
	-  