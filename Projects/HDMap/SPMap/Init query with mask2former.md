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

| version | small segm loss weight | BEV encoder |     Assign     | Performance    |
|:-------:|:----------------------:|:-----------:|:--------------:| --- |
|    1    |          1/5           | SimpleConv  | same pts layer |     |
|    2    |           1            | SimpleConv  | same pts layer |     |
|    3    |           1            |  ResNet18   | same pts layer |     |
|    4    | 1                       | SimpleConv            | pts, segm assign dependently               |     |
|    5    | 1                       | CustomResNet            | same pts layer               |     |
|    6    | 1                       | CustomResNet            | pts, segm assign dependentlyy               |     |
|    7    | 1/2                       | CustomResNet            | same pts layer               |     |
|    8    | 1/2                       |             |                |     |
|    9    |                        |             |                |     |
|   10    |                        |             |                |     |
|   11    |                        |             |                |     |
|   12    |                        |             |                |     |
|   13    |                        |             |                |     |
|   14    |                        |             |                |     |
|   15    |                        |             |                |     |
|   16    |                        |             |                |     |
|   17    |                        |             |                |     |
|   18    |                        |             |                |     |
|   19    |                        |             |                |     |
|   20    |                        |             |                |     |
|   21    |                        |             |                |     |
|   22    |                        |             |                |     |
