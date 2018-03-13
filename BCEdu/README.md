## 개발 환경 세팅(part 1)
### 1. VirtualBox 설치
  - OS에 따른 VirtualBox설치(https://www.virtualbox.org/wiki/Downloads)
  - Version 5.2.6 설치
    ![Version 5.2.6 설치](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/bc1_1.png)
  - ssh 접속을 위한 네트워크 설정
    ![네트워크 어댑터 : NAT, 포트포워딩 설정](https://github.com/aimmvp/BlockChain/blob/master/https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/bc1_2.png)
	
### 2. Ubuntu 설치
  - Ubuntu 16.04.3 LTS Download(https://www.ubuntu.com/download/server)
    ![Ubuntu Image File Download](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/bc2_1.png)
  - Ubuntu Server 16.04설치
    ![가상머신 생성](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/bc2_2.png)
    * 메모리 크기 설정(1024MB) --> 하드디스크(10GB) --> 하드디스크 파일 종류(VDI) --> 물리적 하드 드라이브에 저장(동적할당) --> 파일 위치 및 크기 --> 만들기
  - Ubuntu 설정
    * Language(한국어) --> 우분투 서버 설치(I) --> Select a language(예) --> 위치를 선택하십시오(대한민국) --> 키보드 설정(아니요) --> 키보드 설정(Korean) --> 키보드 설정:키보드 배치(Korea) --> 네트워크 설정 --> 사용자 및 암호 설정(시작폴더 암호화 설정:아니요) --> 시계설정(Asia/Seoul 확인 후 예) --> 디스크 파티션 하기(자동-디스크 전체 사용하고 LVM 설정, 디스크 선택, 볼륨 그룹의 크기 입력, 바뀐 점을 디스크에 쓰기) --> 패키지 관리자 설정(HTTP 프록시 정보 : 빈칸) --> tasksel 설정(자동 업데이트 하지 않음) --> 소프트웨어 선택(OpenSSH Server 추가 선택) --> 하드 디스크에 GRUB 부트로더 설치(예)
    * OpenSSH Server 설치 안했을 경우 재설치하기
    ```
	sudo apt install openssh-client
	sudo apt install openssh-server
	```
	
### 3. Terminal 설치
  - Windows를 사용 할 경우 putty 등 Terminal 설치 필요

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


### 4. Hyperledger Fabric Samples
 - Copy fabric-samples (https://hyperledger-fabric.readthedocs.io/en/release/samples.html)
 ```
 $ git clone https://github.com/hyperledger/fabric-samples.git
 $ cd fabric-samples
 ```
 - Download Platform-specific Binaries
 ```
 $ curl -sSL https://goo.gl/byy2Qj | bash -s 1.0.5
 ```

### 5. first-network
 - https://hyperledger-fabric.readthedocs.io/en/release/build_network.html
 ```
 $ cd first_network
 ```
 - Generate Network Artifacts(http://bit.ly/2sBCGjA)
 ```
 $ sudo ./byfn.sh -m generate
 ```
 - Bring Up the Network(http://bit.ly/2syIdaG)
 ```
 $ sudo ./byfn.sh -m up
 ```
 - Bring Down the Network(http://bit.ly/2EQM1cc)
 ```
 $ sudo ./byfn.sh -m down
 ```
 
## First Application(fabcar)
### 1. Setting up dev environment
 - https://hyperledger-fabric.readthedocs.io/en/release/write_first_app.html
 - kill any stale or active containers and clear cached networks
 ```
 $ docker rm -f $(docker ps -aq)
 $ docker network prune   # Press 'y' when prompted by the command
 ```
 - install and rebuild the Fabric dependencies
 ```
 $ cd fabric-samples/fabcar
 $ sudo npm install
 $ sudo npm rebuild
 ```
 - launch a smart contract container for chaincode
 ```
 $ ./startFabric.sh
 ```
### 2. Register and Enroll admin / user1
 - enrolling the admin user
  ```
  $ node enrollAdmin.js
  ```
 - register and enroll new user(user1)
  ```
  $ node registerUser.js
  Store path:/home/s0wnd/fabric-samples/fabcar/hfc-key-store
  Successfully loaded admin from persistence
  Successfully registered user1 - secret:BEeFoPdbMtVJ
  Successfully enrolled member user "user1"
  User1 was successfully registered and enrolled and is ready to intreact with the fabric network
  ```
### Querying the Ledger
 - https://hyperledger-fabric.readthedocs.io/en/release/write_first_app.html#querying-the-ledger
 ```
 $ node query.js
 Store path:/home/s0wnd/fabric-samples/fabcar/hfc-key-store
 Successfully loaded user1 from persistence
 Query has completed, checking results
 Response is  [{"Key":"CAR0", "Record":{"colour":"blue","make":"Toyota","model":"Prius","owner":"Tomoko"}},{"Key":"CAR1", "Record":{"colour":"red","make":"Ford","model":"Mustang","owner":"Brad"}},{"Key":"CAR2", "Record":{"colour":"green","make":"Hyundai","model":"Tucson","owner":"Jin Soo"}},{"Key":"CAR3", "Record":{"colour":"yellow","make":"Volkswagen","model":"Passat","owner":"Max"}},{"Key":"CAR4", "Record":{"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}},{"Key":"CAR5", "Record":{"colour":"purple","make":"Peugeot","model":"205","owner":"Michel"}},{"Key":"CAR6", "Record":{"colour":"white","make":"Chery","model":"S22L","owner":"Aarav"}},{"Key":"CAR7", "Record":{"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}},{"Key":"CAR8", "Record":{"colour":"indigo","make":"Tata","model":"Nano","owner":"Valeria"}},{"Key":"CAR9", "Record":{"colour":"brown","make":"Holden","model":"Barina","owner":"Shotaro"}}]
 ```
 - 2번째 client로 user1의 context 사용
 ```
 return fabric_client.getUserContext('user1', true);
 ```
 - request로 변수없이 queryAllCars 를 호출
 ```
 // queryCar chaincode function - requires 1 argument, ex: args: ['CAR4'],
 // queryAllCars chaincode function - requires no arguments , ex: args: [''],
 const request = {
 	//targets : --- letting this default to the peers assigned to the channel
   chaincodeId: 'fabcar',
   fcn: 'queryAllCars',
   args: ['']
 };
 ```
 - chaincode 확인
 ```
 $ cd ~/fabric-samples/chaincode/fabcar
 ```
  * CAR0 ~ CAR999까지 GetStateByRange 를 이용하여 저장되어 있는 값을 반환
  * GetStateByRange는 http://bit.ly/2CFi5uv 참고
  * 수행가능한 function 확인 가능 : queryCar, initLedger, createCar, queryAllcars, changeCarOwner
 ```
 $ vi chaincode.go
  func (s *SmartContract) queryAllCars(APIstub shim.ChaincodeStubInterface) sc.Response {
        startKey := "CAR0"
        endKey := "CAR999"

        resultsIterator, err := APIstub.GetStateByRange(startKey, endKey)
        if err != nil {
                return shim.Error(err.Error())
        }
        defer resultsIterator.Close()

        // buffer is a JSON array containing QueryResults
        var buffer bytes.Buffer
        buffer.WriteString("[")
 ```
 - query.js 수정해보기(CAR4의 정보 가지고 오기)
 ```
 const query={
 	chaincodeId: 'fabcar',
	fcn: 'queryCar',
	args: ['CAR4']
 };
 ```
 ```
 $ node query.js
 Store path:/home/s0wnd/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  {"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}
 ```
### Updating the Ledger
 - update peocess : proposed --> endorsed --> returned to the application
 - ```invoke.js``` 수정
 ```
 var request = {
   //targets: let default to the peer assigned to the client
   chaincodeId: 'fabcar',
   fcn: 'createCar',
   args: ['CAR10', 'Chevy', 'Volt', 'Red','Nick'],
   chainId: 'mychannel',
   txId: tx_id
 };
 ```
 ```
 $ node invoke.js
Store path:/home/s0wnd/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Assigning transaction_id:  71f9120c440f45ab101895b447f5aebdc6c81fab8a3a97f9b141afac0143952c
Transaction proposal was good
Successfully sent Proposal and received ProposalResponse: Status - 200, message - "OK"
info: [EventHub.js]: _connect - options {}
The transaction has been committed on peer localhost:7053
Send transaction promise and event listener promise have completed
Successfully sent transaction to the orderer.
Successfully committed the change to the ledger by the peer
 ```
 - update  결과 확인
  * ```query.js``` 수정
  ```
  const request = {
    //targets : --- letting this default to the peers assigned to the channel
   chaincodeId: 'fabcar',
   fcn: 'queryCar',
   args: ['CAR10']
  ```
  ```
  $ node query.js
  ...
  Response is  {"colour":"Red","make":"Chevy","model":"Volt","owner":"Nick"}
  ```


-----
* refernce
 - https://www.virtualbox.org/wiki/Downloads
 - https://www.ubuntu.com/download/server
 - https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository
 - https://nodejs.org/ko/download/package-manager/
 - https://hyperledger-fabric.readthedocs.io/en/release/samples.html
 - https://hyperledger-fabric.readthedocs.io/en/release/build_network.html
 - https://hyperledger-fabric.readthedocs.io/en/release/write_first_app.html
 - http://bit.ly/2CFi5uv 