# Abstract
- Dense detector는 일반적으로 2stage 구조를 사용
	- 먼저 dense BEV feature를 만들고 BEV space에서 detection
	- Complex view transformation, high computation cost
- Sparse detector는 query기반의 sparse한 구조를 사용
	- Dense detector에 비해 낮은 성능
- 이 두 모델의 gap을 줄이기 위해 SparseBEV를 제안함
	1. Scale-adaptive self attention to aggregate features with adaptive receptive field in BEV space
	2. Adaptive spatio-temporal sampling to generate sampling locations under the guidance of queries
	3. Adaptive mixing to decode the sampled features with dynamic weights from the queries

# Introduction
- Dense BEV-based detectors multi-scale을 사용하기 위해 FPN이나 multi-scale deformable attention을 사용함
- Sparse detector는 multi head self attention을 사용하는데 이는 global receptive field를 갖기 때문에 multi scale에 부적합함. image space에서는 비슷한 크기를 갖는 bev space에서와 다르게 객체마다 다른 크기를 가지고 있음. 이러한 문제로 single point sampling을 사용하는 DETR3D에서 성능 하락 요인이 됨

# SparseBEV
- Query-based 1 stage detector
- sparse pillar query를 사용하며 이는 scale-adaptive self attention을 통해 aggregation
## Query formulation
(x, y, z, w, l, h, sin$\theta$, cos$\theta$, vx, vy)
각 쿼리는 pillar가 되도록 학습됨
각 query에 D dim feature를 달아서 query box를 encoding
## Scale-adaptive self attention
- Multi-scale  feature를 BEV space에서 aggregate하는 self attention으로 BEV encoder를 대체
- 기존의 self attention은 global receptive field를 가져 local multi-scale aggregation을 잘 못했음
	- 이를 위해 query간의 거리를 이용하여 receptive field를 제한하는 모듈을 제안
1. 모든 쿼리 center 간의 거리를 계산하여 Attention$$Attn(Q, K, V)=Softmax(\frac{QK^{T}}{\sqrt d}-\tau D)V$$
이 때, $\tau$는 receptive field를 결정하는 scaler factor로 **각 헤드 간의 공유하는 쿼리**를 linear projection하여 각 헤드 간의 고유한 $\tau$를 얻게 됨.

## Adaptive spatio-temporal sampling
- Query feature를 이용해서 sampling offset을 예측
	- 범위를 제한하지 않기 때문에 포인트 쿼리에 제한되지 않는다는데 무슨 의민지 잘 모르겠음
- Motion(Object motion, Ego motion)에 따라 sampling point를 왜곡하여 temporal alignment
	- 예측된 객체의 속도를 기준으로 이전 timestamp의 위치로 align
	- 현재 시점의 sampling point를 global coordinate으로 옮긴 후 이전 timestamp의 coordinate으로 align
$$\begin{align*} f_{t,v} &= \frac{1}{|\mathscr{V}|}\sum_{k\in\mathscr{V}}{\sum\limits_{j=1}^{N_{feat}}w_{i,j}\mathbb{B}(F_{k,j}, \mathbb{P}_k(x'_{t,i}, y'_{t,i}, z'_{t, i}))} \\ \mathbb{B} & : \text{bilinear interpolation},\quad \mathbb{P} : \text{projection function} \end{align*}$$
## Adaptive mixing
- 다른 timestamp와 location으로부터 추출된 sampled feature를 dynamic conv와 MLP Mixer를 이용하여 decode, aggregate하는 방법이라는데 잘 모르겠음
- Channel mixing: 
$$\begin{align*} W_C&=Linear(q)\in\mathbb{R}^{C\times C} \\ M_C(f)&=ReLU(LayerNorm(fW_C)) \end{align*}$$
- Point mixing:
$$\begin{align*} W_P&=Linear(q^T)\in\mathbb{R}^{P\times P} \\ M_P(f)&=ReLU(LayerNorm(fW_P)) \end{align*}$$
- 두 mixing을 마친 spatio-temporal feature는 linear layer에 의해 flattened, aggregated

## Dual-branch SparseBEV(inspired  by SlowFast)
- Input video를 slow stream과 fast stream으로 분리
	- Slow stream : capture fine-grained appearance with low frame and high resolution
	- Fast stream : capture long-term temporal stereo with high frame and low resolution
- Sample point는 두 stream에 모두 projection되고 sampled feature는 mixing 전에 stack됨



#특이사항
- Memory bank를 사용하지 않고 매번 $N_{frame}\times N_{cams}$ 개의 이미지의 feature를 추출함
	- 그런데도 속도는 빠름
	- 그러면 이 구조를 기반으로 post processing을 추가해주면 더 좋지 않을까?
	- 