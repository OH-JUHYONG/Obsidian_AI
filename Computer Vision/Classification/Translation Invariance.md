---
tags:
  - translation-invariance
  - invariance
  - CNN
  - translation-equivariance
  - softmax
  - max-pooling
  - weight-sharing
  - learn-local-feature
---
- **invariance**(불변성): 함수의 입력이 바뀌어도 출력은 그대로 유지되어 바뀌지 않는다는 뜻

- CNN에서 translation invariance는 ==입력의 위치가 변해도 출력은 변하지 않는다는 의미==
	⮚   이미지 내에서 객체나 패턴이 이동하더라도 CNN이 그것을 인식하고 동일한 판단을 내릴 수 있음
	⮚   ==하지만 CNN 자체는 translation equivariance(variance)한 architecture==  
	- convolution filter 연산을 할 때 특정 feature의 위치가 바뀌면 output에 해당 feature에 대한 연산 결과의 위치도 바뀌기 때문
	- 그러나 **max pooling**, **Weigth sharing & Learn local feature**, **Softmax를 통한 확률값 계산** 이렇게 3가지 과정을 통해 ==CNN으로 이루어진 Classification==은 translation invariance한 성질을 가짐

#### 일반적인 CNN architecture은 translation equivariance한데 CNN으로 이루어진 classification이 translation invariance한 이유?

##### Max pooling  
- 대표적인 small translation invariance 함수
- _k x k filter_ 사이즈만큼의 값들을 1개의 max값을 치환시킴
	⮚   _k x k_ 내에서 값들의 위치가 달라져도 모두 동일한 값을 output으로 갖게 됨
	⮚   따라서 _k x k_ 범위내에서 translation에 대해서는 invariance 하다

ex) 
- orginal image의 pixel 값 [1, 0, 0, 0]인 경우에 original image를 각각 translate 시킨 A, B는 [0, 0, 0, 1], [0, 1, 0, 0]의 값을 갖고 있음
- 이들을 2 x 2 max pooling 시키면 모두 동일하게 output이 1을 가짐


##### Weight sharing & Learn local feature  
- **weight sharing**: 동일한 weight를 가진 filter를 sliding window로 연산
	⮚   parameter를 공유하지 않으면 위치마다 다른 필터를 사용해
- **learn local feature**: global이 아닌 local feature들과 연산함으로써 local feature를 학습
- CNN은 _k x k_ 사이즈의 필터를 모든 픽셀에 대하여 동일 값으로 sliding window로 연산을 진행
	⮚   각 필터는 image내 어떤 object의 위치와 상관없이 특정 패턴을 학습
	![[Pasted image 20231001141610.png]]


##### Softmax를 통한 확률값 계산  
- 위 과정까지는 아직 translation equivariance하지만 이를 invariance하게 만들어주는 과정이 Softmax 과정
- classification에서는 feature map들을 FC layer와 연결하고 마지막 label 개수만큼 output node를 설정하여 최종적으로 이들을 softmax를 통해 classification 결과를 결정
- conv 연산은 equivariant해서 서로 다른 위치에 있는 특징을 입력으로 넣으면 feature map에서도 각 특징을 서로 다른 위치에 배치 
- 그러나 conv 레이어를 지나 FC layer와 softmax를 거친 결과는 특징의 위치와 상관없이 무조건 특징이 포함된 라벨의 확률 값을 높게 출력
	![[Pasted image 20231001140543.png]]
	- output label k개와 FC layer를 연결하였고 마지막 sotfmax로 output값을 결정
	- 이때 각 output node가 channel 개수만큼의 노드들과만 대응되도록 weight를 channel 개수만큼 1로 하고 나머지는 0으로 한다고 가정
	- 위 예시에서 output node 1은 1~25 노드와 weight 1로 연결되어있고 나머지는 weight 0으로 연결됨
	- 따라서 위의 예시와 같이 feature map k에 높은 값들이 많이 있으면 output node k는 높은 값을 가짐
	- 이는 feature map k에 높은 값이 많으려면 channel k와 original image가 비슷한 패턴을 갖고 있어야 함

