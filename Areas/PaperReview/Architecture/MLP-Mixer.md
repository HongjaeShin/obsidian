# Abstract
- MLP Mixer는 두 종류의 layer로 이루어짐.
	- Image patch와 독립적인 MLP : Per-location feature를 혼합
	- Patch 단위로 적용되는 MLP : Spatial information을 혼합

# Introduction
- Convolution과 self-attention을 사용하지 않는 새로운 구조를 제안
- Mixer는 data layout을 바꾸면서(reshape, transposition) 계속 basic matrix multiplication하는 구조로 이루어져 있음
- Mixer는 2 종류의 layer로 이루어져 있음
	- Channel mixing MLP: Token에 독립적으로 서로 다른 채널 간의 정보를 공유
	- Token mixing MLP : 다른 token간의 정보를 공유
- Mixer는 어떻게 보면 특이한 형태의 CNN처럼 볼 수 있음. Channel mixing은 1x1 CNN처럼, token mixing은 full receptive field를 갖는 CNN처럼.

# Mixer architecture
- 