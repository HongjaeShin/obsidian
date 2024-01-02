- 설치가 제대로 안됨
	- KeyError: 'mmdet3d is not in the Codebases registry. Please check whether the value of `mmdet3d` is correct or it was registered as expected. More details can be found at https://mmengine.readthedocs.io/en/latest/advanced_tutorials/config.html#import-the-custom-module' ![[Screenshot from 2023-11-23 15-10-50.png]]

- engine is not None
	- mmdeploy config, onnx아닌 engine 파일 사용![[Screenshot from 2023-11-23 15-48-00.png]]
- Input shape should be between~~
	- min, max shape 변경![[Screenshot from 2023-11-23 15-48-46.png]]
- Serialization assertion magicTagRead == kMAGIC_TAG failed.Magic tag does not match
	- end2end.onnx -> end2end.engine![[Screenshot from 2023-11-23 16-28-33.png]]
- onnx2trt 변환 과정에(진행 환경 jetpack 4.6.2)
	- python: /root/gpgpu/MachineLearning/myelin/src/compiler/./ir/operand.h:166: myelin::ir::tensor_t*& myelin::ir::operand_t::tensor(): Assertion `is_tensor()' failed.
	- https://github.com/open-mmlab/mmdeploy/issues/2303
	- TensorRT 8.2 버전에서는 poinpillars를 구성하는 연산 중 하나를 변경하지 못함.
	- 8.4 이상에서 해결되었다고 함(https://github.com/NVIDIA/TensorRT/issues/1541)
ds