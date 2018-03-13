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
