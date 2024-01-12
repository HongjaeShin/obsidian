#HDMap
###### Problem statement
1. Limitation of rasterized map
	- Dense semantic pixels로 많은 memory 사용
	- 각 pixel이 독립적임을 가정하여 기하학적 관계를 무시
	- Downstream task를 위해 vectorize하는 후처리가 필요
2. Limitation of fixed num(MapTR)
	- Geometry에 영향을 주지 못하는 중복 node가 존재
	- 둥근 모서리같은 세부 정보를 놓칠 수 있음
3. Limitation of pivot-based representation
	- Dynamic number of pivot points with different map elements
	- [[VectorMapNet]]은 이를 box->point & autoregressive한 구조로 해결하였지만 long inference time, accumulated errors라는 새로운 문제 발생
###### Problem formulation
- Pivot & collinear point
	- Line을 fine하게 나눴을 때,  geometry에 큰 영향을 주지 못하는 점들을 collinear, 중요한 점을 pivot으로 구분
###### Overall Method
1. Camera feature extractor
	- R50, EfficientNetB0, SwinTR
2. BEV feature decoder
	- Figure에 semantic BEV query라고 있는거 보면 transformer 기반 BEV encoder사용한 것 같음 #TODO (뭔진 코드 나와봐야 알듯)
3. Line-aware point decoder
	- [[Projects/HDMap/PapersSimpleReview/TODO/InsightMapper]]의 query에 각 instance들을 MLP해서 Line feature(instance feature)를 만들고 이를 이용하여 해당 line을 instance segmentation하여 얻은 mask로 mask cross attention
4. Pivotal point predictor

#TODO pivotal point predictor와 제안된 문제를 어떻게 해결했는지 설명 추가할 것