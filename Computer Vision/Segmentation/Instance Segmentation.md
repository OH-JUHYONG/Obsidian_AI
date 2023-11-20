---
tags:
  - instance-segmentation
  - masking
  - bottom-up
  - top-down
---
: 이미지 내에 존재하는 모든 객체를 탐지하는 동시에 ==각각의 경우(instance)를 정확하게 pixel 단위로 분류==하는 task

- 동일한 객체들이여도 개별로(object 별로) Masking을 수행
- 표적 탐지와 시멘틱 분할이 함께 이루어짐
	![[Pasted image 20231001214129.png]]
	- **Bottom-up**: 이미지의 개별 픽셀을 감지하는 것부터 시작해, 이러한 픽셀을 함께 그룹화하여 객체를 형성
	- **Top-down**: 이미지의 전체 장면을 감지하고 개별 개체를 식별한 뒤 segmentation 





