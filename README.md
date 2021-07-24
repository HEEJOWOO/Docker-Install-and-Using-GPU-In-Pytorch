[Docker 설치과정, 컨테이너 내 Pytorch에서 GPU 사용,Ubuntu 18.04 LTS에서 진행](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package)
--------------------------------------------------------------------------
# Docker 설치

* 준비 단계 : 이미 설치된 Docker 삭제
<pre>
<code>
$ sudo apt-get remove docker docker-engine docker.io containerd runc
</code>
</pre>

* 1 단계 : Docker 레포지토리 추가, apt 커맨드로 필요한 패키지 설치, GPG 키 추가
<pre>
<code>
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
</code>
</pre>

* 2-1 단계 : 키가 제대로 설정되었는지 확인-> fingerpring를 검색 / Docker 레포지토리 추가(Ubuntu 환경이 amd64(x86_64)구조에서 실행되는 경우 다음 명령어 실행
<pre>
<code>
$ sudo apt-key fingerprint 0EBFCD88 
</code>
</pre>

* 2-2 단계 : Docker 레포지토리 추가(Ubuntu 환경이 amd64(x86_64)구조에서 실행되는 경우 다음 명령어 실행
<pre>
<code>
$ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
</code>
</pre>

* 3 단계 : Docker 설치, 패키지 갱신, Docker CE 설치
<pre>
<code>
$ sudo apt-get update | sudo apt-get install -y docker-ce docker-ce-cli containerd.io
</code>
</pre>

* 4 단계 : Docker 설치 확인, hello-world 컨테이너를 실행하여 이미지 인식 및 컨테이너 실행 여부 확인
<pre>
<code>
$ sudo docker version
$ sudo docker run hello-world
</code>
</pre>

* 5 단계 : Docker Compose 설치, 실행 권한 부여하여 docker-compose가 제대로 설치되었는지 확인
<pre>
<code>
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
</code>
</pre>

# Pytorch에서 GPU 사용 

* 1 단계 : nvidia-docker 설치
<pre>
<code>
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
  curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
  curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
</code>
</pre>

<pre>
<code>
$ sudo apt-get update & sudo apt-get install -y nvidia-container-toolkit
</code>
</pre>

<pre>
<code>
$ sudo systemctl restart docker
</code>
</pre>

* 2 단계 : pytorch가 사용가능한 이미지 다운
<pre>
<code>
$ sudo docker pull pytorch/pytorch
</code>
</pre>

* 3 단계 : 다운받은 docker image 확인, pytorch/pytorch가 존재하면 성공
<pre>
<code>
$ sudo docker images
</code>
</pre>

* 4-1 단계 : 컨테이너와 host간 공유 폴더생성 
<pre>
<code>
$ sudo mkdir /home/<user_name>/share_folder
</code>
</pre>

* 4-2 단계 : pytorch 컨테이너와 nvidia-docker 연동, user_name에 자신의 사용자 이름을 적어주어야함
<pre>
<code>
$ sudo docker run -itd --name docker_pytorch_nvidia -v /home/<user_name>/share:/root/share -p 8888:8888 --gpus all --restart=always pytorch/pytorch
</code>
</pre>

* 5-1 단계 : 생성된 pytorch 컨테이너 생성
<pre>
<code>
$ sudo docker run -itd --name docker_pytorch_nvidia -v /home/<user_name>/share:/root/share -p 8888:8888 --gpus all --restart=always pytorch/pytorch
</code>
</pre>

* 5-2 단계 : 생성된 pytorch 컨테이너 실행
<pre>
<code>
$ sudo docker exec -it docker_pytorch_nvidia bash
</code>
</pre>

* 6 단계 : GPU 동작 확인
<pre>
<code>
root@_______:/workspace# nvidia-smi
</code>
</pre>

간단한 pytorch 분류 모델이 들어가 있는 이미지 다운로드 후 확인
----------------------------------------------------------

* 1 단계 : 이미지 다운로드
<pre>
<code>
$ sudo docker pull heejowoo/python_torch_gpu_docker:v0.3
</code>
</pre>

* 2 단계 : 다운로드된 이미지 확인
<pre>
<code>
$ sudo docker imasges <-다운로드된 이미지 확인
</code>
</pre>

* 3 단계 : 컨테이너 생성
<pre>
<code>
$ sudo docker run -itd --name pytorch_classification -v /home/<username>/share:/root/share -p 8888:8888 --gpus all --restart=always heejowoo/python_torch_gpu_docker:v0.3
</code>
</pre>


* 4 단계 : 생성된 컨테이너 확인
<pre>
<code>
$ sudo docker ps
</code>
</pre>


* 5 단계 : 컨테이너 실행
<pre>
<code>
$ sudo docker exec -it pytorch_classification bash
</code>
</pre>


* 컨테이너 접속 -> workspace에 docker_mount_test.py, gpu_test_pytorch.ipynb 두 개의 파일이 존재
* docker_mount_test.py : torch 사용여부 버전 확인
* gpu_test_pytorch.ipynb : jupyter notebook 실행하여 확인

# 이미지 도커허브에 배포

* 1 단계 : docker commit 명령을 통한 컨테이너의 현재 상태 이미지 파일로 생성
<pre>
<code>
$ sudo docker commit dockerhub_test heejowoo/python_torch_gpu_docker:v0.3
</code>
</pre>

* 2 단계 : tag달기
<pre>
<code>
$ sudo docker tag dockerhub_test:v0.5 heejowoo/dockerhub_test:v0.5
</code>
</pre>

* 3-1 단계 : DockerHub Login
<pre>
<code>
$ sudo docker login
</code>
</pre>

* 3-2 단계 : DockerHub push
<pre>
<code>
$ sudo docker push heejowoo/dockerhub_test:v0.5
</code>
</pre>


