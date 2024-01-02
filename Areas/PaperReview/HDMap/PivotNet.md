#limitation_of_rasterized_map #limitation_of_MapTR
# Introduction
## Limitation of rasterized representation
1. Rasterized maps are composed of dense semantic pixels that contain redundant information, requiring large amounts of memory and transmission bandwidth, especially if the map extent is large
2. Rasterized representation assumes the independence of map grids, which ignores the geometric relationship between and within map elements
3. Complex post-processing is required to obtain vectorized maps for downstream tasks, which brings additional computation, time consumption, and accumulated errors

## Limitation of MapTR
1. Evenly-based representation contains redundant points that have little effect on the geometry
2. Representing a dynamically shaped line with a fixed number of points may miss essential details in map elements, resulting in information loss, particularly for rounded corners and right angles

# Method
## Problem formulation

### Pivot and collinear point
Vectorized sequence 를 pivotal point sequence로 쪼갬. 이게 뭔 소리냐면 polyline을 충분히 densify하고 약한 simplify를 해줘서 중요한 포인트만 뽑아내겠다는거임. 중요 포인트를 pivot, 나머지 점을 collinear point
### Challenges
1. Point-line relation modeling
	- Point와 line을 묶어주기 위한 Point-to-Line Mask
2. Intra-instance topology modeling
	- 같은 instance를 이루는 점들간의 topology를 위해 Pivot dynamic matching을 제안
	- Point-line relation modeling으로는 부족한 이유가 뭘까?
3. Intra-instance dynamic point number modeling
	- 같은 class의 instance더라도 이루는 점의 개수가 다른 것을 해결하기 위해 Pivot dynamic matching

## PivotNet overview
### BEV feature decoder
- Camera와 BEV feature 사이의 geometry prior를 사용 <- GKT? BEVPool?

### Line-aware point decoder
- Node detection with transformer
	- Input: BEV features $F^b\in\mathbb{R}^{C\times{H^b}\times{W^b}}$, learnable point query $Q_{m,n}\in\mathbb{R}^{C}$ M: max # of instance, N: max # of points in an instance
	- output: Point descriptors
- Mask attention을 어떻게 한다는거지?

### Pivot point head
- Pivotal point predictor와 pivot dynamic matching module로 이루어짐
- Point descriptor로 point regression하고 Pivot dynamic matching
	- 매칭 결과 Pivot과 collinear가 분리됨
## Point-line relation modeling
- Subordinate prior: 어떤 점이 어떤 직선에 포함될지
- Geometrical prior: 점들이 어떤 순서대로 배열될지
### Point-to-line mask module
같은 instance에 포함되어 있는 point를 묶어주기 위한 mask
하나의 line을 이루는 point query를 MLP해서 만든 line feature와 BEV feature를 matrix multiplication해서 Line-aware mask를 만든다. <- Segmentation해서 mask로 사용
대신 line feature를 segmentation GT를 이용하여 supervision(BCE, Dice)
이렇게 만들어진 line-aware mask, BEV feature, point query로 cross attention


## Vectorized pivot learning
### Pivot dynamic matching
N개의 고정된 line 당 최대 point수와 T개의 동적인 GT point 수
N개의 점들로 T개의 점을 갖는 모든 조합 중 가장 loss가 작은 값을 정하는 방법도 가능하지만 너무 무거움.
그래서 dp라는 동적 리스트를 만드는데 이 부분이 잘 이해 안감

### Dynamic vectorized sequence loss
1. Pivotal point supervision
	- Matching을 통해 얻은 GT와 predicted pivot의 L1 loss
2. Collinear point supervision
	- GT에는 pivot밖에 없으니 pivot들 사이를 등분하도록 collinear point를 supervision
	- 등분이 아닌거 같음 
1. Pivot classification loss
	- 예측된 점이 pivot인지 collinear point인지 classification하기 위해 bce loss를 사용

## Training loss
### Auxiliary BEV supervision
BEV segmentation loss를 이용하여 bev feature를 supervision


# Discussion
- Node detection을 heatmap base로 고집해야할 필요가 있을까?
	- 우리가 heatmap과 box detection을 고집해야할 이유가 있나?
	- 

