---
tags:
  - ResNet
  - skipconnection
  - identitiy-mapping
  - plain-network
  - skip-connection
---

**link**: https://arxiv.org/abs/1512.03385 
**cf**[^0] : CVPR 2016

: Deep Residual Learning for Image Recognition


### Abstract

- Plain netwrok[^1] 가 점점 깊어질 수록 gradient 소실과 폭팔의 문제가 발생 
	⮚   layer가 뒷단으로 갈수록 활성화 함수의 미분값이 점점 작아지거나 커지는 효과
	![[Pasted image 20231009132636.png]]
	- 위 그림을 통해 20-layer가 56-layer보다 더 낮은 train error와 test error를 얻은 것을 보여줌

-  기울기 소실/폭팔 문제를 해결하기 위해, 입력 x를 몇 layer 이후의 출력값을 더해주는 **skip/shortcut connection**을 더해줌


### Main Idea

##### ResNet
- residual neural network의 줄임말
- input 값들이 중간의 특정 layer들을 모두 거치지 않고 **한번에 건너뜀**
	![[Pasted image 20231009133705.png]]
	 
	-  **identity mapping**: 입력갑과 출력값이 같은 매핑, skip connection 또는 identity shortcut connection이라고 부름  
	- 기존의 신경망은 _H(x) = x_ 가 되도록 학습
	- **skip connection**에 의해 출력값에 x를 더하고 _H(x) = x + F(x)_ 로 정의
	- _F(x) = 0_ 이 되도록 학습하여 _H(x) = 0 + x_ 가 되도록 함
	- 이는 미분을 했을 때 더해진 x가 1이 되어 기울기 소실 문제가 해결


![[Pasted image 20231009134451.png]]
![[Pasted image 20231009134507.png]]





[^0]: https://deep-learning-study.tistory.com/473
	- https://losskatsu.github.io/machine-learning/resnet/#%EC%B0%B8%EA%B3%A0%EB%A7%81%ED%81%AC
	- https://wjunsea.tistory.com/99

[^1]: Plain network
	- skip/shortcut connection을 사용하지 않은 일반적인 CNN 신경망(AlexNet, VGGNet)을 의미