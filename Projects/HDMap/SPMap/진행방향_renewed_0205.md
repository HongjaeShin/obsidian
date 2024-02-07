#### Motivation
Coarse to fine
Online hd map task는 정확하게 예측하는 것도 중요하지만 잘못 예측하지 않는 것도 중요하다.
이를 위해 우리는 Instance segmentation을 이용하여 coarse하게 map 영역을 찾고 이를 바탕으로 hd map construction을 진행하는 새로운 2 stage 모델을 제안한다

#### Method
1. Instance segmentation module
		가벼우면서도 높은 성능을 보인 query based instance segmentation model인 Mask2Former를 Instance segmentation decoder로써 사용 

3. Key point sampling
		예측된 instance mask를 이용하여 Instance의 reference point를 예측하고 기존의 instance query


3. GT denoising for 2 stage model