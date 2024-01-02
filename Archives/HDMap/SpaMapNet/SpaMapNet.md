- 문제점
	- 중복된 객체를 찾는 경우가 많음
			- ~~이거 마지막 decoder의 결과만 출력하는게 아닌가? 확인해볼 것
		- Instance간의 self attention이 필요
			- Instance mixer?
	- one2many loss는 원래 point query는 20개로 그대로 두고 instance query를 늘려서 $50\times 20$을 $350\times20$같이 추가적인 query를 통해 학습을 빠르게 하는 것이 목적이라고 이해를 했는데 hybrid query에서는 이러한 과정이 어떻게 되는 것인지 이해가 잘 되지 않음
		- InsightMapper의 supplementary보면 MapTRv2와 비교하여 PV segmentation이나 hybrid matching을 사용하여 성능 향상을 얻었다는데 어떻게 한 건지 확인해볼 것
### TODO
- [x] Sampling point visualize (SparseBEV에 있음)
- [x] Instance mixer 생각해보기 가능하면 overfit 실험까지 해보기
- [ ] Sampling offset 쪽 파보기
- [ ] SparseBEV에서 dn 켰을 때 껐을 때 sampling points 차이 보기


### SpaMapNet의 novelty
- Online HD map 처음으로 sparse query를 도입
	- 왜 도입해야하는가?
		- Online HD Map의 핵심은 빠른 속도와 높은 정확도
		- 기존의 모델들은 BEV encoder때문에 많은 inference cost를 허비중
		- BEV encoder를 제거하면서 streaming을 통해 모델의 computational cost를 줄임
- 무언가 추가적인 모듈이 있어야 할 것 같은데
	1. Query design
		- Fixed num는 너무 제약이 큼
		- Flownet을 살릴 수 있나?
			- BEV feature가 없어서 힘들 것 같은데
		- Points per instance를 좀 늘리고 decoder에서 regression하면서 이 노드가 필요한지에 대한 binary classification 해서 Mask를 만들고 masking된 preds에 대해 loss를 계산
			- 간단한 polyline에 대해서는 좀 더 나을 수 있는데 긴 Polyline에 대해서는 이전 모델들과 차이가 없음
			- 한 번 False되면 True가 될 수 있게 좋은 position을 찾도록 학습되진 않될거 같기도
			- 긴 polyline에 대한 문제는 어떻게 해결?
				- Query 추가 생성?

### Variation
1. Denoising
	- [ ] 포인트 별로 박스 coord를 이용한 noise
	- [ ] Instnace 별로 박스 coord를 이용한 noise
		- 박스 크기가 너무 커서 박스 coord를 사용하면 범위 밖으로 나가 clamping되는 점이 너무 많아질 것 같음
		- 세환이형은 그래서 아예 0~1의 좌표 noise만 줌
	- [ ] Clamping을 하지 않고 normalize
		- outlier가 있으면 너무 왜곡될 거 같고 이래도 되는지 모르겠음
	- [ ] 어차피 noise를 준다는 것 자체가 중요한 것. random_prob에 scale만 좀 키워서 곱해준다? 5 이렇게. 
1. Mixer
	- [ ] 