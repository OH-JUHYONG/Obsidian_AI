cf[^0]


### 모델의 분류와 정답
- **TP**(True Positive): 실제 True인 정답을 True라고 예측(정답)
- **FP**(False Positive): 실제 False인 정답을 True라고 예측(오답)
- **FN**(False Negative): 실제 True인 정답을 False라고 예측(오답)
- **TN**(True Negative): 실제 False인 정답을 False라고 예측(정답)
	![](Pasted%20image%2020231124174847.png)
#### Precision
- **정밀도**: model이 True라고 분류한 것 중에서 실제 True인 것의 비율
	![](Pasted%20image%2020231124175240.png)
	- **PPV**(Positive 정답률)

#### Recall
- **재현율**: 실제 True인 것 중에서 모델이 True라고 예측한 것의 비율
	![](Pasted%20image%2020231124175410.png)







### Segmentation

#### Average Precision(AP)
- 정확하게 감지하는 능력, 값이 높을 수록 더 정확도가 높음
- bbox_mAP_50, 75: [IoU](IoU.md) 임계값에서 측정
	- mAP_50인 경우 IoU가 50% 이상인 경우

#### Pixel Accuracy(PA)
- pixel의 비율을 올바르게 예측한 비율을 나타냄
	![[Pasted image 20231116225736.png]]
	- _TP_: True Positive
	- _FN_: False Negative
- _K+1_ 개의 클래스에 대해 다음과 같이 나타낼 수 있음
	![[Pasted image 20231116230042.png]]
	- _K_: foreground의 수
	- _1_: background
	- _Pij_: class i의 pixel 수가 class j에 속하는 것으로 예측











[^0]: https://biology-statistics-programming.tistory.com/109
	- 

