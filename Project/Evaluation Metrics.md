cf[^0]

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

