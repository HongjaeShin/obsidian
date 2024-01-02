### Problem statement
-  기존의 방법들은 line에 대한 specific knowledge를 사용하는 경우가 있음(line이 직선이다, parametric line)
	- 해당하면 잘 찾지만 complex topology를 가지는 경우 오히려 잘 못 찾음
- Dynamic convolution을 사용하는 방법들은 이러한 specific knowledge를 사용하진 않지만 얇고 긴 line의 특성상 잘 못찾음(+ occlusion)

### Solution
- Dynamic convolution을 이용한 dynamic kernel
![[Pasted image 20231203224041.png]]

### To ours?
1. 라인 별로 예측
	- 그에 따른 장점?
		- bev heatmap에서 다른 직선으로 projection될 수 있다는 문제가 해결
			- 이게 정말 문제인가?
			- 클래스 별로 구분하여 segmentation하기 때문에 차선이 줄어드는 상황처럼 같은 클래스의 line 여러 개가 겹치는 상황이 아니라면 상관 없을듯 
		- 중간에 끊기게 segmentation되도 채워줄 수 있나?
	- 그에 따른 단점?
		- 라인 수 만큼의 segmentation map이 필요해져 좀 더 무거워질 수 있음
			- 그런데 해봐야 50 * 200 * 100인데 큰 영향이 있나?
			- 400 * 200으로 shape을 바꾼다면?
2. Offset map
	- [[InstaGraM]]과 다른 점은 heatmap과 함께 line 별로 진행하기 때문에 graph를 만들어 연결성을 생성해줄 필요가 없음
	- Heatmap이 범위로 잡히니까 정확한 위치를 위해 offset map과 후처리를 사용
		- 이러한 접근은 괜찮은거 같음
		- 그런데 구현이 가능한가?
3. Dynamic convolution
	- 이건 본 논문처럼 kernel by line할 거 아니면 별로인거 아닌가?
		- kernel by class?
		- 지금 진행 중인 방법의 변형 뿐?