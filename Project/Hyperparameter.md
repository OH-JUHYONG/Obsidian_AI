: 모델링할 때 사용자가 직접 세팅해 주는 값

### Epoch
- 전체 데이터 셋에 대해 한 번 학습을 완료한 상태
	- epoch 값이 너무 작으면 underfitting이 발생할 확률이 높음
	- epoch 값이 너무 크다면 overfitting이 발생할 확률이 높음

### Batch Size
- 모델이 한 번에 학습하는 데이터의 수
	- batch size가 크면 메모리 사용량이 증가하고 계산 속도가 빨라짐, 너무 큰 크기는 모델의 generalization을 저하시킴
	- batch size가 작으면 학습시간이 길어질 수 있음
### Iteration
- epoch와 batch size에 따라 결정
	ex) 2000개의 데이터, epoch = 20, batch size = 500인 경우 1 epoch는 각 데이터의 크기가 500인 batch가 들어간 4번의 iteration으로 나누어짐
	- 즉, 전체 데이터셋에 대해서 20번의 학습이 이루어지면, iteration 기준으로 총 80번의 학습이 이루어짐

