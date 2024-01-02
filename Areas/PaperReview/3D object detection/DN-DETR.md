# Abstract
- DETR은 bipartite 매칭의 불안정성으로 인한 느린 수렴 속도를 갖음
- 이를 해결하기 위해 hungarian loss뿐만 아니라 GT box에 noise를 추가하여 모델을 학습.
	- 매칭 정확도를 올래 더 빠른 수렴 속도를 얻음

# Introduction
- DETR은 이전 detector들과 비교해서 느린 수렴 속도를 갖음. 
	- COCO로 학습할 때 500epoch이 필요. (Faster-RCNN은 12)
- 이러한 문제를 해결하려는 시도가 많았음
	- 모델의 구조를 변경해서 해결
		- 수렴이 느린 것이 cross attention으로 인한 것이라 생각하여 encoder only DETR을 제안
		- RoI-based dynamic decoder로 학습을 빠르게 하는 DETR을 제안
	- 좀 더 최근에는 DETR query를 특정 위치에 연결하여 효율적으로 feature probing
		- 각각의 query를 content와 positional로 나누어 쿼리가 특정 공간과 일치할 수 있도록 학습
			- [[Conditional DETR]]
		- 2D reference point를 query로 cross attention
			- [[Deformable DETR]], [[Anchor DETR]]
		- Query를 4D anchor box로 해석하고 layer by layer로 점진적으로 향상시킴
			- [[DAB DETR]]
	- Stochastic optimization의 특성상 학습 단계 초기에는 bipartite matching이 불안정할 수 밖에 없음
- 이러한 불안정한 bipartite matching을 안정화시키기 위한 학습 방법을 제안
	- Decoder에 noised GT box를 noised query로써 learnable anchor query와 함께 입력
- 