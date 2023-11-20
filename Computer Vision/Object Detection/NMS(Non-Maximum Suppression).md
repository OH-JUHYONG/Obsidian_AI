---
tags:
  - NMS
  - bounding-box
  - loU
---
:  bounding box 가운데 ==가장 확실한 bounding box만 남기고 나머지 bounding box는 제거==하는 기법


![[Pasted image 20231001001228.png]]
1. bounding box별로 지정한 confidence score threhold 이하의 box를 제거
	- confidence score threshold = 0.5로 지정
	- 위 그림에서는 confidence score가 0.3인 box를 제거
2. 남은 bounding box를 confidence score에 따라 내림차순으로 정렬
3. 정렬된 confidence score가 높은 순의 bounding box부터 다른 box와의 [[IoU]] 값을 조사하여 IoU threshold 이상인 box를 모두 제거
	- IoU threshold = 0.4로 지정
	- confidence score에 따라 내림차순으로 box를 정렬 [0.9, 0.85, 0.81, 0.73, 0.7]
	- confidence score가 0.9인 box와 나머지 box와의 IoU 값 조사 {0.85 : 0, 0.81 : 0.44, 0.73 : 0, 0.7 : 0.67}
	- IoU threshold 이상인 confidence score가 0.81, 0.7인 box 제거 [0.9, 0.85, 0.73]
	- 남은 box에 대하여 위의 과정을 반복
4. 남아있는 box만 선택
	- 남은 box는 confidence score가 0.9, 0.85인 box


```python
import torch

from IoU import intersection_over_union

def nms(bboxes, iou_threshold, threshold, box_format='corners'):

    # bboxes가 list인지 확인합니다.
    assert type(bboxes) == list

    # box 점수가 threshold보다 높은 것을 선별합니다.
    # box shape는 [class, score, x1, y1, x2, y2] 입니다.
    bboxes = [box for box in bboxes if box[1] > threshold]
    # 점수 오름차순으로 정렬합니다.
    bboxes = sorted(bboxes, key=lambda x: x[1], reverse=True)
    bboxes_after_nmn = []

    # bboxes가 모두 제거될때 까지 반복합니다.
    while bboxes:
        # 0번째 index가 가장 높은 점수를 갖고있는 box입니다. 이것을 선택하고 bboxes에서 제거합니다.
        chosen_box = bboxes.pop(0)

        # box가 선택된 box와의 iou가 임계치보다 낮거나
        # class가 다르다면 bboxes에 남기고, 그 이외는 다 없앱니다.
        bboxes = [box for box in bboxes if box[0] != chosen_box[0] \
               or intersection_over_union(torch.tensor(chosen_box[2:]),
                                          torch.tensor(box[2:]),
                                          box_format=box_format)
                    < iou_threshold]

        # 선택된 박스를 추가합니다.
        bboxes_after_nmn.append(chosen_box)

    return bboxes_after_nmn
```



