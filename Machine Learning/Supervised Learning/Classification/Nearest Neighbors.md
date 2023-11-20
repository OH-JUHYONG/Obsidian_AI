---
tags:
  - NN
  - nearest-neighbor-upsampling
  - euclidean-distance
  - decision-boundaries
  - k-nearest-neighbors
---
: find the nearest input vector to _x_ in the training set and copy its labels

- "nearest" in term of ==Euclidean distance==

![[Pasted image 20231001094850.png]] 
- arg(argument)ㅁ: ㅁ에 해당하는 요소를 가져옴

#### Decision Boundaries
- the boundary between region of input space assigned to different categories
- 입력이 어떤 _class_ 인지 결정

![[Pasted image 20231001095139.png]]

#### K-Nearest Neighbors
- Instead of finding the single closet image in the training set, we will find the top k closet images, and have them vote on the label of the test image  
	⮚   비교 대상이 되는 데이터 포인트 주변에 가장 가까이 존재하는 _k_ 개의 데이터와 비교해 가장 가까운 데이터 종류로 판별
	![[Pasted image 20231024134143.png]]

- 적절한 _k_ 값 중요  
	- Small _k_: overfit의 주요 원인, 학습데이터에만 최적화, too expressive
	- Large _k_: underfit의 주요 원인, fail to capture important regularities, classifier가 너무 단순
	- _k_ 는 [[Machine Learning|hyperparameter]] 
	- _k_ 가 even인 경우 동률이 될 수 있는 문제가 발생