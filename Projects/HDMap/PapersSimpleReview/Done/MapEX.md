#### Problem statement
> While recent methods like MapTRv2 have become proficient at generating online HDMaps from raw sensors, we feel they overlook very useful and nearly always available data: existing maps.

#### Method
- MapModEX: 몇 가지 scenario를 기준으로 HD map GT에 noise를 줌
	1. Boundary만 남김
		- Divider와 ped crossing은 가려지기 쉽지만 boundary는 연석같이 높이가 있는 경우가 많으므로 비교적 찾기 쉽다고 판단하여 divider만 남기고 나머지 cls는 제거한 scenario
	2. Noisy label
		- Instance shift noise나 Point Gaussian noise를 통해 잘못 labeling되어 있는 경우를 표현. 대충 레이블링이 되어 있어도 모델이 작동한다는 것을 보여 labeling cost를 줄일 수 있다고 표현
	3. 전체적인 context는 비슷한데 도로가 좀 더 휘어 있는 등의 noise를 추가

