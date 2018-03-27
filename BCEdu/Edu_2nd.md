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
$ sudo apt-get install nodejs
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
  3. Hyperledger Fabric 소스 가져오기
  ```
  $ cd ~/workplace
  ```
  3-1. Hyperledger Fabric sample 소스 가져오기(Git Clone)
  ```
  $ git clone https://github.com/hyperledger/fabric-samples.git
  ```
  3-2. Hyperledger Fabric sample 소스 가져오기(Git이 설치되어 있지 않은 경우)
  ```
  $ wget https://github.com/hyperledger/fabric-samples/archive/release.zip
  $ unzip release.zip
  $ mv fabric-samples-release fabric-samples
  ```
  4. fabric 관련 platform binary, fabric Image 다운로드 및 설치 
  ```
  $ cd fabric-samples
  $ curl -sSL https://goo.gl/6wtTN5 | sudo bash -s 1.1.0
  ```
  5. ```peer``` 명령어를 편하게 사용하기 위한 link 생성
  ```
  $ mkdir ~/bin
  $ ln -s $(pwd)/bin/* ~/bin/
  $ ls -l ~/bin/
  $ peer
  ```

### 5. Building Your First Network
http://hyperledger-fabric.readthedocs.io/en/release-1.1/build_network.html
 ```
 $ cd ~/workplace/fabric-samples/first-network
 $ ./byfn.sh --help
 ```

* Generate Network Artifacts
 ```
 $ ./byfn.sh -m generate
 ```
 ```
 s0wnd@ubuntu:~/workplace/fabric-samples/first-network$ ./byfn.sh -m generate
Generating certs and genesis block for with channel 'mychannel' and CLI timeout of '10' seconds and CLI delay of '3' seconds
Continue? [Y/n] y
proceeding ...
/home/s0wnd/workplace/fabric-samples/first-network/../bin/cryptogen

##########################################################
##### Generate certificates using cryptogen tool #########
##########################################################
+ cryptogen generate --config=./crypto-config.yaml
org1.example.com
org2.example.com
+ res=0
+ set +x

/home/s0wnd/workplace/fabric-samples/first-network/../bin/configtxgen
##########################################################
#########  Generating Orderer Genesis block ##############
##########################################################
+ configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
2018-03-28 08:17:18.503 KST [common/tools/configtxgen] main -> INFO 001 Loading configuration
2018-03-28 08:17:18.513 KST [msp] getMspConfig -> INFO 002 Loading NodeOUs
2018-03-28 08:17:18.514 KST [msp] getMspConfig -> INFO 003 Loading NodeOUs
2018-03-28 08:17:18.514 KST [common/tools/configtxgen] doOutputBlock -> INFO 004 Generating genesis block
2018-03-28 08:17:18.515 KST [common/tools/configtxgen] doOutputBlock -> INFO 005 Writing genesis block
+ res=0
+ set +x

#################################################################
### Generating channel configuration transaction 'channel.tx' ###
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID mychannel
2018-03-28 08:17:18.528 KST [common/tools/configtxgen] main -> INFO 001 Loading configuration
2018-03-28 08:17:18.538 KST [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2018-03-28 08:17:18.539 KST [msp] getMspConfig -> INFO 003 Loading NodeOUs
2018-03-28 08:17:18.540 KST [msp] getMspConfig -> INFO 004 Loading NodeOUs
2018-03-28 08:17:18.566 KST [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 005 Writing new channel tx
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org1MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID mychannel -asOrg Org1MSP
2018-03-28 08:17:18.579 KST [common/tools/configtxgen] main -> INFO 001 Loading configuration
2018-03-28 08:17:18.590 KST [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2018-03-28 08:17:18.591 KST [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x
```
* Bring Up the Network
 ```
 $ ./byfn.sh -m up
 ```
 ```
 s0wnd@ubuntu:~/workplace/fabric-samples/first-network$ ./byfn.sh -m up
Starting with channel 'mychannel' and CLI timeout of '10' seconds and CLI delay of '3' seconds
Continue? [Y/n] y
proceeding ...
2018-03-27 23:21:30.872 UTC [main] main -> INFO 001 Exiting.....
LOCAL_VERSION=1.1.0
DOCKER_IMAGE_VERSION=1.1.0
Creating network "net_byfn" with the default driver
Creating volume "net_peer0.org2.example.com" with default driver
Creating volume "net_peer1.org2.example.com" with default driver
Creating volume "net_peer1.org1.example.com" with default driver
Creating volume "net_peer0.org1.example.com" with default driver
Creating volume "net_orderer.example.com" with default driver
Creating peer0.org2.example.com
Creating peer1.org1.example.com
Creating peer0.org1.example.com
Creating peer1.org2.example.com
Creating orderer.example.com
Creating cli

 ____    _____      _      ____    _____
/ ___|  |_   _|    / \    |  _ \  |_   _|
\___ \    | |     / _ \   | |_) |   | |
 ___) |   | |    / ___ \  |  _ <    | |
|____/    |_|   /_/   \_\ |_| \_\   |_|

Build your first network (BYFN) end-to-end test

Channel name : mychannel
Creating channel...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer channel create -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/channel.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2018-03-27 23:21:35.144 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:35.185 UTC [channelCmd] InitCmdFactory -> INFO 002 Endorser and orderer connections initialized
2018-03-27 23:21:35.389 UTC [main] main -> INFO 003 Exiting.....
===================== Channel "mychannel" is created successfully =====================

Having all peers join the channel...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-03-27 23:21:35.448 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:35.487 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2018-03-27 23:21:35.487 UTC [main] main -> INFO 003 Exiting.....
===================== peer0.org1 joined on the channel "mychannel" =====================

CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org1.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-03-27 23:21:38.550 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:38.593 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2018-03-27 23:21:38.594 UTC [main] main -> INFO 003 Exiting.....
===================== peer1.org1 joined on the channel "mychannel" =====================

CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-03-27 23:21:41.655 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:41.710 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2018-03-27 23:21:41.710 UTC [main] main -> INFO 003 Exiting.....
===================== peer0.org2 joined on the channel "mychannel" =====================

CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org2.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-03-27 23:21:44.769 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:44.806 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2018-03-27 23:21:44.806 UTC [main] main -> INFO 003 Exiting.....
===================== peer1.org2 joined on the channel "mychannel" =====================

Updating anchor peers for org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer channel update -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/Org1MSPanchors.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2018-03-27 23:21:47.879 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:47.893 UTC [channelCmd] update -> INFO 002 Successfully submitted channel update
2018-03-27 23:21:47.893 UTC [main] main -> INFO 003 Exiting.....
===================== Anchor peers for org "Org1MSP" on "mychannel" is updated successfully =====================

Updating anchor peers for org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer channel update -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/Org2MSPanchors.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2018-03-27 23:21:50.961 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-03-27 23:21:50.977 UTC [channelCmd] update -> INFO 002 Successfully submitted channel update
2018-03-27 23:21:50.977 UTC [main] main -> INFO 003 Exiting.....
===================== Anchor peers for org "Org2MSP" on "mychannel" is updated successfully =====================

Installing chaincode on peer0.org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2018-03-27 23:21:54.164 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:21:54.164 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-03-27 23:21:54.624 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode is installed on peer0.org1 =====================

Install chaincode on peer0.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2018-03-27 23:21:54.704 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:21:54.704 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-03-27 23:21:54.896 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode is installed on peer0.org2 =====================

Instantiating chaincode on peer0.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer chaincode instantiate -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc -l golang -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P 'OR     ('\''Org1MSP.peer'\'','\''Org2MSP.peer'\'')'
+ res=0
+ set +x
2018-03-27 23:21:54.959 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:21:54.959 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-03-27 23:22:22.891 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode Instantiation on peer0.org2 on channel 'mychannel' is successful =====================

Querying chaincode on peer0.org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
===================== Querying on peer0.org1 on channel 'mychannel'... =====================
Attempting to Query peer0.org1 ...3 secs
+ peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
+ res=0
+ set +x

2018-03-27 23:22:25.976 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:22:25.976 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
Query Result: 100
2018-03-27 23:22:54.467 UTC [main] main -> INFO 003 Exiting.....
===================== Query on peer0.org1 on channel 'mychannel' is successful =====================
Sending invoke transaction on peer0.org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer chaincode invoke -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc -c '{"Args":["invoke","a","b","10"]}'
+ res=0
+ set +x
2018-03-27 23:22:54.696 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:22:54.697 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-03-27 23:22:54.732 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 003 Chaincode invoke successful. result: status:200
2018-03-27 23:22:54.733 UTC [main] main -> INFO 004 Exiting.....
===================== Invoke transaction on peer0.org1 on channel 'mychannel' is successful =====================

Installing chaincode on peer1.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org2.example.com:7051
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2018-03-27 23:22:54.801 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:22:54.801 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-03-27 23:22:55.118 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode is installed on peer1.org2 =====================

Querying chaincode on peer1.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org2.example.com:7051
===================== Querying on peer1.org2 on channel 'mychannel'... =====================
Attempting to Query peer1.org2 ...3 secs
+ peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
+ res=0
+ set +x

2018-03-27 23:22:58.214 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-03-27 23:22:58.214 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
Query Result: 90
2018-03-27 23:23:27.356 UTC [main] main -> INFO 003 Exiting.....
===================== Query on peer1.org2 on channel 'mychannel' is successful =====================

========= All GOOD, BYFN execution completed ===========


 _____   _   _   ____
| ____| | \ | | |  _ \
|  _|   |  \| | | | | |
| |___  | |\  | | |_| |
|_____| |_| \_| |____/

```
```
 docker ps
 ```
 
* ```./byfn.sh -m up``` 실행 시 에러가 발생하면 다음을 다시 실행한다.
 ```
 $ ./byfn.sh -m down
 $ sudo rm -rf crypto-config channel-artifacts/genesis.block
 $ ./byfn.sh -m generate
 $ ./byfn.sh -m up
 ```
* Bring Down the Network
 ```
 $ ./byfn.sh -m down
 $ docker ps -a
 ```
