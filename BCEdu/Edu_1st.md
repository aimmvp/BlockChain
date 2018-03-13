## 개발 환경 세팅(part 1)
### 1. VirtualBox 설치
  - OS에 따른 VirtualBox설치(https://www.virtualbox.org/wiki/Downloads)
  - Version 5.2.8 설치(2018.3.13 기준)
    ![Version 5.2.8 설치](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_1.png)
  
### 2. 가상머신 만들기 
  - "새로만들기" 클릭 --> 이름에 "ubuntu" 입력 --> "다음" 클릭
    ![가상머신 만들기](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_2.png)

### 3. Ubuntu 설치
  - Ubuntu 16.04.3 LTS Download(https://www.ubuntu.com/download/server)
    ![Ubuntu Image File Download](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_3.png)
  - 가상머신에 다운받은 iso 파일 추가
    ![iso 파일 추가](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_4.png)
  - Ubuntu Server 16.04설치
    * Language(한국어) --> 우분투 서버 설치(I) --> Select a language(예) --> 위치를 선택하십시오(대한민국) --> 키보드 설정(아니요) --> 키보드 설정(Korean) --> 키보드 설정:키보드 배치(Korean) --> 네트워크 설정 --> 사용자 및 암호 설정(시작폴더 암호화 설정:아니요) --> 시계설정(Asia/Seoul 확인 후 예) --> 디스크 파티션 하기(자동-디스크 전체 사용하고 LVM 설정, 디스크 선택, 볼륨 그룹의 크기 입력, 바뀐 점을 디스크에 쓰기) --> 패키지 관리자 설정(HTTP 프록시 정보 : 빈칸) --> tasksel 설정(자동 업데이트 하지 않음) --> 소프트웨어 선택(OpenSSH Server 추가 선택) --> 하드 디스크에 GRUB 부트로더 설치(예)
  - 가상머신 IP 확인 
    ```
    $ ifconfig
    ```
    ![IP확인하기](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_5.png)
  - ssh 접속을 위한 네트워크 설정
    ![네트워크 어댑터 : NAT, 포트포워딩 설정](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_6.png)


    
### 3. Terminal 설치
  - Windows를 사용 할 경우 putty 등 Terminal 설치 필요
    ```
	$ sudo apt install openssh-client
	$ sudo apt install openssh-server
	```
## 개발 환경 세팅(part 2)
### 1. GO 설치
  - https://golang.org/doc/install 참고
  - 설치 파일 다운로드 및 압축풀기
  ```
  $ sudo apt-get install build-essential
  $ wget https://storage.googleapis.com/golang/go1.9.linux-amd64.tar.gz # Download Archive
  $ tar xvfz go1.9.linux-amd64.tar.gz # Unzip Archive File
  ```
  - GOPATH, PATH 설정
  ```
  $ mkdir $HOME/workplace
  $ export GOPATH=$HOME/workplace
  $ export PATH=$PATH:$HOME/go/bin
  ```
  
### 2. Docker 설치
  - https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository 참고
  - Install using the repository
  ```
  # 1. Update the apt package index
  $ sudo apt-get update     
  # 2. Install Packages
  $ sudo apt-get install apt-transport-https  ca-certificates  curl  software-properties-common
  # 3. Add Docker's official GPG Key
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  $ sudo apt-key fingerprint 0EBFCD88
  # 4. set up the stable repository
  $ sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable"
  ```
  - Install Docker Compose(https://docs.docker.com/compose/install/#install-compose)
  ```
  $ sudo apt-get update
  $ sudo apt install docker-compose
  ``` 
  ```
  # Manage Docker as a non-root user
  # add user to docker group
  $ sudo usermod -aG docker $USER
  ```
  ```
  # Reboot System
  $ sudo reboot
  ```
  
### 3. nodejs 설치
 - https://nodejs.org/ko/download/package-manager/
```
$ sudo apt install npm
$ sudo npm install npm@3.10.10 -g
$ sudo apt install nodejs-legacy
```
 - Node.js 9 사용을 위한 version update
 ```
 $ sudo apt-get update && sudo apt-get -y upgrade
 $ curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
 $ sudo apt-get install nodejs
 ```
