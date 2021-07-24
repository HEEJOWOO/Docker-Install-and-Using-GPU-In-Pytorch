(Docker 설치과정, 컨테이너 내 Pytorch에서 GPU 사용,Ubuntu 18.04 LTS에서 진행)[https://docs.docker.com/engine/install/ubuntu/#install-from-a-package]
--------------------------------------------------------------------------

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

* 2 단계 : Docker 레포지토리 추가, apt 커맨드로 필요한 패키지 설치, GPG 키 추가
<pre>
<code>
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
</code>
</pre>
