## Hyperledger 간단 통신 테스트
http://hyperledger-fabric.readthedocs.io/en/release-1.1/write_first_app.html

### 1. Hyperledger SDK 설치

### Setting up your Dev Environment
```
cd ~/workspace/fabric-samples
cd fabcar
ls
```
```
docker rm -f $(docker ps -aq)
docker network prune
```
### Install the clients & launch the network
```
unset HTTP_PROXY HTTPS_PROXY
npm install
./startFabric.sh
docker ps
```
### Enrolling the Admin User
```
node enrollAdmin.js
```
### Register and Enroll user1
```
node registerUser.js
```

### 2. 조회

### Querying the Ledger
```
node query.js
```
* query.js 수정
```
const request = {
  //targets : --- letting this default to the peers assigned to the channel
  chaincodeId: 'fabcar',
  fcn: 'queryCar',
  args: ['CAR4']
};
```
```
node query.js
```

### 3. 추가 및 수정

### Updating the Ledger
* invoke.js 수정
```
var request = {
  //targets: let default to the peer assigned to the client
  chaincodeId: 'fabcar',
  fcn: 'createCar',
  args: ['CAR10', 'Chevy', 'Volt', 'Red', 'Nick'],
  chainId: 'mychannel',
  txId: tx_id
};
```
```
node invoke.js
```

* query.js 수정
```
const request = {
  //targets : --- letting this default to the peers assigned to the channel
  chaincodeId: 'fabcar',
  fcn: 'queryCar',
  args: ['CAR10']
};
```
```
node query.js
```

* invoke.js 수정
```
var request = {
  //targets: let default to the peer assigned to the client
  chaincodeId: 'fabcar',
  fcn: 'changeCarOwner',
  args: ['CAR10', 'Dave'],
  chainId: 'mychannel',
  txId: tx_id
};
```
```
node invoke.js
```
