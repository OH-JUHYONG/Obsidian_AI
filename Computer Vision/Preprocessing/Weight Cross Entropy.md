- Image 마다 각 클래스별 존재하는 이미지 개수 차이가 크면 문제가 발생할 수 있음
- 이미지 개수가 많은 클래스에 과적합이 발생할 수 있음

```python
numSample_list = [4959, 8589]
weights = [1 - (x / sum(numSample_list)) for x in numSample_list]
# [0.6339681133746679, 0.36603188662533215]

weights = torch.FloatTensor(weights).to(device)

loss = nn.CrossEntropyLoss(weights)
```

