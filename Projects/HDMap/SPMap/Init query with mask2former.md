### TODO
- [x] BEV encoder 단순화
	- ResNet18 -> double conv 몇 개로 이루어진 간단한 encoder로 변형
	- [ ] Mask 형태를 바꾸면서 수렴이 빨라진건지 encoder의 단순화로 빨라진건지 확인
- [x] Mask 형태 [50,25] -> [200, 100]
	- [ ] 둘의 성능 차이도 확인해보기
- [ ] Decoder layer 수 변경해보기
- [ ] 기존에 사용하던 aux bev seg가 필요할까?
	- 중요한 부분은 아닐듯 함
- [ ] pts와 segm의 gt assign을 동일하게 해줘야할까?
- [ ] segm으로 assign
- [ ] one2many 2 이상 가능하게
- [ ] with_cp

- v2: v1에서 segm loss weight을 mask2former만큼 올렸음. v1에서는 1/5해서 사용
	- [ ] overfit에서는 v1의 pts loss가 더 작게 나왔는데 이를 1/7set에서 확인해볼 것
- v3: v2에 BEV encoder를 다시 resnet18로 사용
- v4: v2에 pts, segm 따로 assign