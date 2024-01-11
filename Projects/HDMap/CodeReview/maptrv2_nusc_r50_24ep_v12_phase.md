- phase 1, 2 차이는 transformer에서 pretrain/combi_use
## MapTRv2Head_v12_phase
- self.transformer output이 바뀌었음
	~~- transformer도 확인해봐야할 듯~~
- 


## MapTRPerceptionTransformer_v12_phase
- Phase 1에서는 Base와 비슷하게 학습
- Phase 2에서는 학습된 BEV feature를 이용하여 segmentation하고 그 결과를 feat에 붙여줄 수 있음
	1. embed_dim + 3으로 concat하고 conv 두 번
	2. BEV feat에 더해줌
	3. BEV feat에 더하고 linear
