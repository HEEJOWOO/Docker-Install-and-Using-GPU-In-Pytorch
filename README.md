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
