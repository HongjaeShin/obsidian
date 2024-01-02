[[진행계획]]
## 세팅
	- https://developer.nvidia.com/embedded/jetpack 가서 sdkmanager  설치
	- 필요한 버전 선택 20.04는 5.0.1부터 5.1.0까지 설치 가능
	- 선택 후 설치 시작하면 설치 진행되다가 중간에 ID, Password 입력을 위해 잠시 정지됨
		- Automatic Setup을 Manual Setup으로 변경 후 진행
	- 다 끝나면 키보드, 마우스, 모니터 연결하고 로그인
		- 현재 ID/Password
			- spalab
			- 병재현진
	- 터미널 열고 sudo fdisk로 ssd 이름 확인
		- 아마 /dev/nvme0n1
	- ```
	sudo mount /dev/nvme0n1p1 /mnt
	cd /mnt
	ls
	sudo rm -rf lost+found
	sudo cp -rp /home/* /mnt  # 용량이 부족해서 home 디렉토리를 옮김
	sudo mv /home /home.orig  # 혹시 모르니 home을 백업
	sudo mkdir /home
	sudo umount /dev/nvme0n1p1
	sudo mount /dev/nvme0n1p1 /home/
	cd /home                  # 잘 됐는지 확인
	sudo cp /etc/fstab /etc/fstab.orig # 백업해두고
	sudo gedit /etc/fstab              # 부팅될 때마다 자동으로 마운트해주기 위함
	/dev/nvme0n1p1 /home ext4 defaults 0 0 # 맨 아랫줄에 추가
	재부팅해서
	df -h로 /dev/nvme0n1p1에 /home이 할당되고 용량 SSD것으로 바뀌어 있으면 백업한 home지우기

- Docker setting
	- Docker image를 받기 전에 OS가 깔린 SSD에 용량이 없어서 그대로 사용이 불가능함
	- 이를 위해 docker image 설치 장소를 SSD로 변경하고 docker에서 환경을 구축
```sudo docker info | grep 'Docker Root dir' # 기존 도커 저장 경로
	sudo systemctl stop docker.service
	sudo systemctl stop docker.socket
	sudo rsync -aP {docker 기존 저장 경로} {새로운 경로} # 저장한 이미지가 있다면
	sudo vi /etc/docker/daemon.json
	# 값이 하나 저장되어 있는데 그 위에 다음을 추가
	{
		"data-root":"{새로운경로}",
		"runtimes"...
	}
	sudo systemctl start docker
```
- docker run command
```
docker run -it --privileged -w /home -v /home/spalab:/home --name mmdeploy -e DISPLAY=:0 -v /tmp/.X11-unix:/tmp/.X11-unix nvcr.io/nvidia/l4t-pytorch:r35.1.0-pth1.11-py3 /bin/bash
```
- mmdeploy 설치
	- conda 세팅 안하면 이유는 모르겠으나 mmdeploy가 설치되지 않음
```
# 환경변수 설정
export TENSORRT_DIR=/usr/include/aarch64-linux-gnu
export PATH=$PATH:/usr/local/cuda/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
echo -e '\n# set environment variable for TensorRT' >> ~/.bashrc
echo 'export TENSORRT_DIR=/usr/include/aarch64-linux-gnu' >> ~/.bashrc
echo -e '\n# set environment variable for CUDA' >> ~/.bashrc
echo 'export PATH=$PATH:/usr/local/cuda/bin' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64' >> ~/.bashrc
source ~/.bashrc


apt-get install -y libssl-dev
apt-get install protobuf-compiler libprotoc-dev
apt-get install -y pkg-config libhdf5-dev
pip3 install versioned-hdf5 pycuda
# git clone --branch 2.x https://github.com/open-mmlab/mmcv.git
pip3 install opencv-python==4.5.5.64
cd mmcv
MMCV_WITH_OPS=1 pip install -e .
pip3 install onnx==1.10.0
cd ..
git clone https://github.com/openppl-public/ppl.cv.git
cd ppl.cv
export PPLCV_DIR=$(pwd)
echo -e '\n# set environment variable for ppl.cv' >> ~/.bashrc
echo "export PPLCV_DIR=$(pwd)" >> ~/.bashrc
./build.sh cuda
cd ..
git clone -b main --recursive https://github.com/open-mmlab/mmdeploy.git
cd mmdeploy
pip3 install -U setuptools
export MMDEPLOY_DIR=$(pwd)
mkdir -p build && cd build
cmake .. \
    -DMMDEPLOY_BUILD_SDK=ON \
    -DMMDEPLOY_BUILD_SDK_PYTHON_API=ON \
    -DMMDEPLOY_BUILD_EXAMPLES=ON \
    -DMMDEPLOY_TARGET_DEVICES="cuda;cpu" \
    -DMMDEPLOY_TARGET_BACKENDS="trt" \
    -DMMDEPLOY_CODEBASES=all \
    -Dpplcv_DIR=${PPLCV_DIR}/cuda-build/install/lib/cmake/ppl
make -j$(nproc) && make install
cd ${MMDEPLOY_DIR}
pip3 install -v -e .

```

- 버전 세팅
	- mmdeploy 0.10.0
	- mmdet3d 1.0.0rc6 (SPA mmdet3d 사용하기 위해)
	- mmcv-full 1.5.2
	- mmdet 2.24.0
	- mmsegmentation 0.24.0
	- mmengine 0.10.1

> python ../mmdeploy_0_10_0/tools/test.py ../mmdeploy_0_10_0/configs/mmdet3d/voxel-detection/voxel-detection_tensorrt_dynamic-nus-64x4-fp16.py configs/pointpillars/hv_pointpillars_secfpn_sbn-all_fp16_2x8_2x_nus-3d.py --model ../mmdeploy_0_10_0/end2end_nus_fp16.engine --device cuda:0

