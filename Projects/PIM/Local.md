- docker pytorch/pytorch:1.9.1-cuda11.1-cudnn8-devel
	- name : mmdeploy
![[Screenshot from 2023-11-23 15-08-37.png]]
- Inference speed
	- Pytorch
		- 1.3fps for 2000samples
	- TensorRT
		- 27fps
	- CUDA PointPillars(FP16)
		- github 기준 64.2fps *Local 기준 FP16 nuscenes도 직접 진행해볼 필요 있을 듯*