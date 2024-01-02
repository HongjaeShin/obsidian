1. debug 용 pkl파일에서 batch_size 1 267iter에서 계속 len(bbox)=0로 에러발생
	1. prepare_train_data에서 gt 없는 경우 제외해주니까 해결
2. 평균적으로 batch_size 1 4iter마다 data 시간이 다른 iter때보다 많이 걸림
	1. SparseBEV는 안그럼
	2. GT가 없는 경우에 이런 문제가 발생했던거 같음
	3. prepare_train_data에서 gt 없는 경우 제외해주니까 제대로 돌아옴
3. Random sample로 overfitting하면 validation할 때 이전 timestamp sample이 존재하지 않아서 에러발생함
4. GT를 split해서 사용하는 경우 prev나 next sweep이 split에 포함되지 않아 에러가 발생할 수 있음



