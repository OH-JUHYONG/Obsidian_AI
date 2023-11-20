: Intersection over Union

- 두 영역이 겹치는 영역을 두 영역의 합으로 나눈 값
- object detector의 정확도를 측정하는데 이용되는 평가 지표
- IoU을 적용하기 위해 필요한 것
	- **ground-truth bounding boxes**: testing set에서 object 위치를 labeling 한 것
	- **prediceted bounding boxes**: model이 출력한 object 위치 예측값
	![[Pasted image 20231114140608.png]]
	- 두 개의 box가 적어도 2/3가 겹쳐줘야 0.5의 값이 나옴
	- 만약 IoU 임계값을 0.5로 설정하면 예측된 바운딩 박스와 실제 바운딩 박스의 겹치는 부분이 전체 바운딩 박스 크기의 50% 이상이어야 해당 탐지를 성공으로 간주
	![[Pasted image 20231114140838.png]]

``` python
import torch

def intersection_over_union(boxes_preds, boxes_labels, box_format='midpoint'):

    # box 좌표는 midpoint, corners 두 가지 형식이 있습니다.

    # boxes_preds shape : (N,4) N은 bboxes의 개수 입니다.
    # boxes_labels shape : (N,4)

    if box_format == 'midpoint':
    	box1_x1 = boxes_preds[..., 0:1] - boxes_preds[..., 2:3] / 2
    	box1_y1 = boxes_preds[..., 1:2] - boxes_preds[..., 3:4] / 2
    	box1_x2 = boxes_preds[..., 0:1] + boxes_preds[..., 2:3] / 2
    	box1_y2 = boxes_preds[..., 1:2] + boxes_preds[..., 3:4] / 2
    	box2_x1 = boxes_labels[..., 0:1] - boxes_labels[..., 2:3] / 2
    	box2_y1 = boxes_labels[..., 1:2] - boxes_labels[..., 3:4] / 2
    	box2_x2 = boxes_labels[..., 0:1] + boxes_labels[..., 2:3] / 2
    	box2_y2 = boxes_labels[..., 1:2] + boxes_labels[..., 3:4] / 2

    if box_format == 'corners':
        box1_x1 = boxes_preds[..., 0:1] # (N,1)
        box1_y1 = boxes_preds[..., 1:2]
        box1_x2 = boxes_preds[..., 2:3]
        box1_y2 = boxes_preds[..., 3:4]
        box2_x1 = boxes_labels[..., 0:1]
        box2_y1 = boxes_labels[..., 1:2]
        box2_x2 = boxes_labels[..., 2:3]
        box2_y2 = boxes_labels[..., 3:4]

    x1 = torch.max(box1_x1, box2_x1)
    y1 = torch.max(box1_y1, box2_y1)
    x2 = torch.max(box1_x2, box2_x2)
    y2 = torch.max(box1_y2, box2_y2)

    # .clamp(0)은 intersect하지 않는 경우입니다.
    # 최소값을 0으로 설정합니다.
    intersection = (x2 - x1).clamp(0) * (y2 - y1).clamp(0)

    box1_area = abs((box1_x2 - box1_x1) * (box1_y2 - box1_y1))
    box2_area = abs((box2_x2 - box2_x1) * (box2_y2 - box2_y1))

    return intersection / (box1_area + box2_area - intersection + 1e-6)
```



### Tiny Object Detection
- COCO dataset에서 정의한 Object의 크기
	- Large: _96 x 96 pixel_ 이상
	- Medium: _32 x 32 pixel ~ 96 x 96 pixel_
	- Small: _32 x 32 pixel_ 이하

##### Why AP of Small Object is Bad?
- IoU가 Small Object를 탐지하는데 문제가 되는 이유[^0]
	![[Pasted image 20231114143033.png]]
	- 물체의 크기에 따라 박스들이 같은 크기의 translation이 되었음에도 IoU 값이 차이가 발생, 이는 설정한 IoU 임계값 이하인 경우 모두 Negative sample(background)로 할당되어 악영향을 끼칠 수 있음
		- Tiny Object에서 IoU를 사용했을때 빈번하게 발생할 수 있는 문제
	- 박스 B와 C는 각각 A 박스 기준으로 _1 pixel_, C는 _4 pixel_ 이 translation
	- 왼쪽 A와 B의 IoU는 _0.53_, 오른쪽 A와 B는 _0.9_
		- 같은 크기만큼 translation이 되었음에도 IoU 값이 차이가 남
	![[Pasted image 20231114143359.png]]
	- IoU의 metric은 박스 크기가 같은 경우에 박스 크기에 따라서 IoU가 급격하게 증가하거나 감소하고 박스가 커지면 커질수록 그래프의 기울기와 완만해짐을 확인
	- 오른쪽 그림에서 제안한 방법은 **NWD**
		- 박스의 크기와 상관없이 동일한 기울기를 가진 metric을 확인

- Label assignment은 수많은 박스들 중 ground truth를 선정하는 방법
- 보통 ground truth와 가장 IoU가 높은 예측 박스 중에서 선정을 해서 ground truth를 계산
	- IoU가 작은 물체는 **Negative Sample**(background)로 간주
	- ground truth가 가장 높은 것은 Object라고 생각하고 Loss를 계산
	![[Pasted image 20231114144010.png]]









[^0]: https://cobslab.tistory.com/11
	- https://arxiv.org/pdf/2110.13389.pdf

