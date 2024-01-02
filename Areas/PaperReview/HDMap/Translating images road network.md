# Abstract
- Road network를 구성할 때 Euclidean(landmark location)과 non-Euclidean(road topological connectivity)가 충돌하는 경우가 있음
- 이 둘을 RoadNet Sequence라는 integer series로 projection하여 이를 해결
- Autoregressive한 부분과 non-autoregressive한 부분을 나눴음
- Non-autoregressive sequence-to-sequence 방법이 autoregressive에 대한 의존성을 줄이면서 성능을 향상시킴

# Introduction
- Euclidean data는 $\mathbb{R}^2$에서 선언되어 있지만 non-Euclidean data를 $\mathbb{R}^n$에 projection하면 중요한 정보들을 잃게 됨
- 기존의 방법들은 Euclidean과 non-Euclidean을 조화롭게 사용하는 방법이 없었음
	- STSU같은 centerline detection과 centerline connectivity reasoning 이라는 two-stage로 이루어져 있었음
- 이러한 부분들을 해결하기 위해 Euclidean과 non-Euclidean 모두를 projection하는 integer series domain을 제안
- Efficiency는 RoadNet Sequence length를 모든 centerline 중 가장 짧은 centerline으로 제한함으로써 얻음
- Interaction은 단일 시퀀스 내에서 Euclidean과 non-Euclidean 사이의 상호 의존성?
- Auto-regressive dependency가 inference 속도를 너무 낮춤
- 그래서 이를 Semi-autoregressive하게 변형함 (SARRNTR)
	- 이것만으로도 6배의 inference 속도 향상을 얻을 수 있었음
- Masked training technique을 추가하여 반복 예측을 이용하여 auto-regressive를 모방하였음
	- 이를 통해 47배의 inference 속도 향상을 얻을 수 있었음
# Method
### Mathematics modeling of road network
- Directed Acycle Graph(DAG)라는 걸로 모든 road landmark(vertex set)와 centerline(edge set)
- Vertex는 location과 category를 포함
- Edge는 edge의 source와 target, bezier middle control point

### RoadNet Sequence
- Projection from road network to RoadNet
	- from DAG to Directed Forest
	- Topological sorting of Directed Forest
	- Sequence construction
#### From DAG to Directed Forest
- 