### Idea for us
- Geometry decoupled attention처럼 새로운 attention을 생각해볼까?
	- GDA로 성능이 많이 올랐는데 이게 G-representation이랑 같이 써서 오른건지 이 attention이 효과적인건지 판단이 어려움
- 
### Problem statement
> Recent efforts have built strong baselines for this task, however, shapes and relations of instances in urban road systems are still under-explored, such as parallelism, perpendicular, or rectangle-shape.

### Proposed method
- Overall architecture![[Screenshot from 2023-12-28 15-55-23.png]]
- Geometric loss
	- Geometric Representation
		- Euclidean shape clues: local geometry를 변위 벡터의 길이와 벡터 간의 각도로 geometry를 표현
		- Euclidean relation clues: 두 geometry 간의 관계를 표현
		![[Screenshot from 2023-12-28 15-32-19.png]]
		![[Screenshot from 2023-12-28 15-41-22.png]]
- Geometry decoupled attention
	- Precise shape geometry capture requires the model to discern token correlations specific to an instance while avoiding interference from tokens of unrelated instances.