---
sticker: lucide//network
tags:
  - FC-layer
  - dense-layer
---
**cf**[^0]

: Fully Connected Layer, ==한 층의 모든 뉴런이 그 다음 층의 모든 뉴런과 연결된 상태==

- (2차원 배열 형태 이미지를)1차원 배열의 형태로 평탄화된 행렬을 통해 이미지를 **분류**하는데 사용되는 계층
- Convolution/[[Pooling]] 의 결과를 취하여 **이미지를 정의된 label로 분류**하는 데 사용
- ==dense layer==이라고도 함
	![[Pasted image 20231006120849.png]]

ex)
```python
model = keras.Sequential()
model.add(layers.Flatten(input_shape = (28,28)))
model.add(layers.Dense(128, activation='relu'))
model.asdd(layers.dense(10, activation='softmax'))
```

1. 2차원 배열 형태의 이미지를 1차원 배열로 평탄화
2.  Activation function(ReLU, Tanh 등...)로 뉴런을 활성화
3. Softmax(분류기) 함수로 분류


### FC layer의 한계?

- 이미지의 위치 정보가 사라짐
	![[Pasted image 20231006135006.png]]

- 입력 이미지의 크기가 고정
	![[Pasted image 20231006135021.png]]

- 3차원 컬러 이미지의 정보 손실 
	- 흑백이미지는 흑과 백의 명암으로만 이미지가 구성되기 때문에 RGB를 사용하는 컬러이미지와 달리 벡터를 1차원 행렬로 변환시키는데 어려움이 없음
		- 위 예시 (60000, 28, 28): 60000은 이미지 개수, 그 뒤에 28은 각각 가로 세로 크기
		- 만약 RGB 필터의 픽셀값을 가진 컬러 이미지였다면 (60000, 3, 28, 28)으로 표현, 여기서 3은 RGB 채널의 수
		- 이 경우 단순히 3차원 이미지를 1차원으로 평탄화하면 공간 정보가 손실될 수 밖에 없음, 즉 정보 부족으로 인해 이미지를 분류하는 데 한계가 발생







[^0]: - https://velog.io/@grovy52/Fully-Connected-Layer-FCL-%EC%99%84%EC%A0%84-%EC%97%B0%EA%B2%B0-%EA%B3%84%EC%B8%B5
	- https://dsbook.tistory.com/59
	- https://blog.naver.com/intelliz/221709190464