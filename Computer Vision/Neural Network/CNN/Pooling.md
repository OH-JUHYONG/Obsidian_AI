---
sticker: lucide//minimize-2
tags: pooling, sub-sampling
---
**cf**[^0]

: to progressively ==reduce the spatial size of the representation to reduce the amount of parameters and computation in the network==, and hence to also ==control overfitting==.

### Function

- Pooling은 **sub sampling**이라고도 함  
	⮚   ==To reduce the reliance of precise positioning within feature map== that are produced by convolutional layer within a CNN
	- 이는 sub-sampling 통해 pixel 수를 줄이는 the reduction of spatial resolution(공간 해상도 감소)가 수행
	- specific feature positioning reliance is a disadvantage to building and developing a network that can perform relatively well in input data
	
	
	⮚   sub sampling은 해당하는 image data를 작은 size로 image로 줄이는 과정 
	⮚   parameter 개수, 연산량을 줄여 계산 속도 향상, 해당 network의 표현력이 줄어들어 **overfitting**을 억제
	- Convolutional layer의 깊이가 깊어지면 많은 계산 양을 요구함

- 중간중간마다 pooling layer를 넣는 것이 일반적
- training을 통해 train되어야 할 parameter가 없음
- Pooling의 결과는 channel 수에는 영향이 없으므로 channel 수는 유지(independent)
- input feature map에 변화(shift)가 있어도 pooling의 결과는 변화가 적음(robustness)

### Pooling type

- Max pooling
- Average pooling
- L2 norm pooling














[^0]: - https://cs231n.github.io/convolutional-networks/ 
	- https://towardsdatascience.com/you-should-understand-sub-sampling-layers-within-deep-learning-b51016acd551