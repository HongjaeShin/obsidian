# Abstract
- 이전의 모델들은 Dynamic object detection 구조를 사용하면서 linear map element를 예측할 때 제약이 생김.
```
We extract more accurate bird's eyeview features guided by their linear structure, and then propose a hierarchical sparse map representation to further leverage the scalability of vectorized map elements, and design a progressive decoding mechanism and a supervision strategy based on this representation.
```

# Introduction
- 기존의 방법들은 차선을 vectex들의 집합으로 생각하여 객체 인식의 방식을 따라서 진행했음([[VectorMapNet]])
	- 차선은 동적 객체와 달리 선형적이거나 축에 평행한 경우가 많아 bounding box로 정의하기 어려움
- 도로에 차량이 많은 scenario에서는 차선의 vertices가 보이지 않을 수 있기 때문에 heatmap만으로 정의하기도 어려움([[InstaGraM]])
- Hierarchical query를 이용하여 각 점을 query로 모델링하여 임의의 모양을 더 잘 설명할 수 있는 구조를 제안하였지만, 차선의 모양을 보장하기 위해 dense한 vertex가 필요하기 때문에 추가적은 구조적 가이드 없이 많은 수의 점을 예측해야해서 낮은 수렴 속도와 성능을 갖음([[MapTR]])

# Methodology
## BEV feature extractor
- 2D to 3D transformation은 linear한 map element의 특성때문에 feature misalignment와 불연속성을 발생
- 이를 해결하기 위해 position-aware BEV feature와 instance aware BEV feature를 만들어 fusion함
### Perspective view converter
- BEVFormer를 사용하여 position aware BEV feature를 만들고 각 feature당 하나의 MLP를 사용하여 Instance aware BEV feature를 만듦

### Structure-guided feature fusion
- Perspective view converter를 통해 만들어진 두 종류의 BEV feature를 더하고 segmentation aux branch를 달았음
- 이 후  point aware BEV feature를 concat하고 conv해서 둘을 fusion

## Progressive decoder
- 