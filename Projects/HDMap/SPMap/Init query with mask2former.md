### TODO
#### 급한거
- [x] BEV encoder 단순화
	- ResNet18 -> double conv 몇 개로 이루어진 간단한 encoder로 변형
	- [x] Mask 형태를 바꾸면서 수렴이 빨라진건지 encoder의 단순화로 빨라진건지 확인
		- mask shape이 작아서 단순화된 encoder로 하는게 나을 듯 함
- [x] Mask 형태 [50,25] -> [200, 100]
	- [ ] 둘의 성능 차이도 확인해보기
- [ ] GT assign
	- [ ] segm으로 assign
		- segm으로 assign하고 permutation으로 어떻게 할지 고민
	- [x] pts와 segm의 gt assign을 각각
- [ ] Multi scale을 channel-wise로 concat해서 사용
- [ ] denoising
#### 안급한거
- [ ] one2many 2 이상 가능하게
- [ ] with_cp
- [ ] 기존에 사용하던 aux bev seg가 필요할까?
	- 중요한 부분은 아닐듯 함
- [ ] Decoder layer 수 변경해보기

- Variation
	- v2: v1에서 segm loss weight을 mask2former만큼 올렸음. v1에서는 1/5해서 사용
		- 중간에 끊긴 했는데 추세가 별로 안좋아보임
	- v3: v2에 BEV encoder를 다시 resnet18로 사용
	- v4: v2에 pts, segm 따로 assign
		- segm_loss weight을 낮춰야할 것 같음
	- v5: v2에 Custom ResNet
	- v6: v4에 Csutom Re
		| small segm loss weight | BEV encoder | assign |
		| -------- |