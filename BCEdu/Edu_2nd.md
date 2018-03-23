## 개발 환경 세팅(part 2)
* http://hyperledger-fabric.readthedocs.io/en/release-1.1/prereqs.html *

### 0. 사전준비
* Update the apt package update
```
$ sudo apt update
$ sudo apt dist-upgrade
```

### 1. Docker 설치
 * https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository *
  
* Install Packages to allow ```apt``` to use a repository over HTTPS
  ```
  $ sudo apt install apt-transport-https  ca-certificates  curl  software-properties-common python unzip
  ```
* Add Docker's official GPG Key & virify fingerprint
  ```
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  $ sudo apt-key fingerprint 0EBFCD88
  ```
* set up the stable repository
  ```
  $ sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable"
  $ sudo apt update
  ```
* Install docker-ce(Community Edition), docker-compose & add user to docker group
  ```
  $ sudo apt-get install -y docker-ce docker-compose
  $ sudo usermod -aG docker $USER
  ```
* Reboot System
  ```
  $ sudo reboot
  ```

### 2. GO 설치
  - https://golang.org/doc/install 참고
  - 설치 파일 다운로드 및 압축풀기
   
* move to $HOME root
 ```
 $ cd $HOME
 ```
* install Go language
  ```
  $ sudo add-apt-repository -y ppa:gophers/archive
  $ sudo apt-get update
  $ sudo apt-get install -y golang-1.9-go
  ```
* GOPATH, PATH 설정
  ```
  $ mkdir $HOME/workplace
  $ export GOPATH=$HOME/go
  $ export PATH=$PATH:$HOME/go/bin
  ```

### 3. Node.js 설치
* https://nodejs.org/ko/download/package-manager/ *
* install node.js version 9.x
```
$ sudo apt-get update && sudo apt-get -y upgrade
$ curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
$ sudo apt-get install nodejs   # install Node.js v9.x
```
* install nodejs-leday and npm
```
$ sudo apt install nodejs-legacy
$ sudo npm install npm@3.10.10 -g
```

### 4. Hyperledger Fabric 환경구축
  -  참고 : http://hyperledger-fabric.readthedocs.io/en/stable/write_first_app.html

  1. 테스트 시작 전에 기존에 생성되어 있는 docker 컨테이너를 내리기
  ```
  $ docker rm -f $(docker ps -qa) 
  ```
  2. 기존 사용하던 이미지 파일이 있다면 삭제
  ```
  $ docker rmi  dev-peer0.org1.example.com-fabcar-1.0-5c906e402ed29f20260ae42283216aa75549c571e2e380f3615826365d8269ba
  ```
  3-1. Hyperledger Fabric sample 소스 가져오기(Git Clone)
  ```
  $ git clone https://github.com/hyperledger/fabric-samples.git
  ```
  3-2. Hyperledger Fabric sample 소스 가져오기(Git이 설치되어 있지 않은 경우)
  ```
  $ sudo apt-get install unzip
  $ wget https://github.com/hyperledger/fabric-samples/archive/release.zip
  $ unzip release.zip
  $ mv fabric-samples-release fabric-samples
  ```
  4. fabric 관련 platform binary, fabric Image 다운로드 및 설치 
  ```
  $ curl -sSL https://goo.gl/6wtTN5 | sudo bash -s 1.1.0
  ```
  5. 샘플 폴더로 이동
  ```
  $ cd fabric-samples
  ```
  6. 하이퍼레저 네트웍 환경 구성을 위한 쉘 실행
  ```
  $ ./startFabric.sh
  ```
  7. 현재 실행중인 docker container 목록 확인 
  ```
  $ docker ps
  ```
  ![Container List](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu2_1.png)
  총 6개의 docker container 가 실행되고 있음.
  
    - dev-peer0.org1.example.com-fabcar-1.0.*
    - hyperledger/fabric-tools
    - hyperledger/fabric-peer
    - hyperledger/fabric-ca
    - hyperledger/fabric-couchdb
    - hyperledger/fabric-orderer
