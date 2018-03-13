## 개발 환경 세팅(part 2)
### 1. nodejs 설치
 - 참고 : https://nodejs.org/ko/download/package-manager/
 - Node.js 9 사용을 위한 version update
 ```
 $ sudo apt-get update && sudo apt-get -y upgrade
 $ curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
 $ sudo apt-get install nodejs   # install Node.js v9.x and npm
 $ node -v
 v9.8.0  # nodejs 버전 확인
 $ sudo apt install nodejs-legacy
 $ sudo npm install npm@3.10.10 -g
```
### 2. GO 설치
  - https://golang.org/doc/install 참고
  - 설치 파일 다운로드 및 압축풀기
  ```
  $ cd $HOME     # move to HOME root
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

### 3. Docker 설치
  - https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository 참고
  - Install using the repository
  1. Update the apt package index
  ```
  $ sudo apt-get update     
  ```
  2. Install Packages to allow ```apt``` to use a repository over HTTPS
  ```
  $ sudo apt-get install apt-transport-https  ca-certificates  curl  software-properties-common
  ```
  3. Add Docker's official GPG Key & virify fingerprint
  ```
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  $ sudo apt-key fingerprint 0EBFCD88
  ```
  4. set up the stable repository
  ```
  $ sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable"
  ```
  5. Install Docker Compose(https://docs.docker.com/compose/install/#install-compose)
  ```
  $ sudo apt-get update
  $ sudo apt install docker-compose
  ``` 
  6. Manage Docker as a non-root user(add user to docker group)
  ```
  $ sudo usermod -aG docker $USER
  ```
  7. Reboot System
  ```
  $ sudo reboot
  ```