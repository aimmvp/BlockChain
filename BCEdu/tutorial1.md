## Hyperledger Fabric sample download
http://hyperledger-fabric.readthedocs.io/en/latest/samples.html

```
cd ~/workspace
git clone https://github.com/hyperledger/fabric-samples.git
cd fabric-samples
ls
```
```
curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0
docker images
ls bin
mkdir ~/bin
ln -s $(pwd)/bin/* ~/bin/
ls -l ~/bin/
peer
```

## Building Your First Network
http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html

```
cd ~/workspace/fabric-samples
cd first-network
./byfn.sh --help
```
### Generate Network Artifacts
```
./byfn.sh -m generate
```

### Bring Up the Network
```
./byfn.sh -m up
docker ps
```
* 에러로 ./byfn.sh -m up 이 실행되지 않는 경우는 아래처럼 down, generate, up 다시 실행
```
./byfn.sh -m down
sudo rm -fr crypto-config channel-artifacts/genesis.block
./byfn.sh -m generate
./byfn.sh -m up
```

### Bring Down the Network
```
./byfn.sh -m down
docker ps
docker ps -a
```

* root 가 아닌 사용자로 실행시에는 추가로 root 사용자로 생성된 ```crypto-config``` 와 ```genesis.block``` 삭제 필요 ```sudo rm -fr crypto-config channel-artifacts/genesis.block```

### Crypto Generator
```
ls
../bin/cryptogen generate --config=./crypto-config.yaml
ls
export FABRIC_CFG_PATH=$PWD
ls channel-artifacts
../bin/configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
ls channel-artifacts
```

### Create a Channel Configuration Transaction
```
ls channel-artifacts
export CHANNEL_NAME=mychannel  && ../bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME
ls channel-artifacts
../bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID $CHANNEL_NAME -asOrg Org1MSP
ls channel-artifacts
../bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID $CHANNEL_NAME -asOrg Org2MSP
ls channel-artifacts
```
### Start the network
```
docker ps
docker-compose -f docker-compose-cli.yaml up -d
docker ps
```

#### Create & Join Channel
```
docker exec -it cli bash
```
```
export CHANNEL_NAME=mychannel
```
```
peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
```
```
ls -l
```
```
peer channel join -b mychannel.block
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp CORE_PEER_ADDRESS=peer0.org2.example.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt peer channel join -b mychannel.block
```
#### Update the anchor peers
```
peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org1MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
```
```
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp CORE_PEER_ADDRESS=peer0.org2.example.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org2MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
```

#### Install & Instantiate Chaincode
```
peer chaincode install -n mycc -v 1.0 -p github.com/chaincode/chaincode_example02/go/
```
```
peer chaincode instantiate -o orderer.example.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C $CHANNEL_NAME -n mycc -v 1.0 -c '{"Args":["init","a", "100", "b","200"]}' -P "OR ('Org1MSP.peer','Org2MSP.peer')"
```
#### Query
```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```
#### Invoke
```
peer chaincode invoke -o orderer.example.com:7050  --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem  -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'
```
#### Query
```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

#### How do I see these transactions?
```
docker logs -f cli
```

#### How can I see the chaincode logs?
```
docker logs dev-peer0.org2.example.com-mycc-1.0
docker logs dev-peer0.org1.example.com-mycc-1.0
docker logs dev-peer1.org2.example.com-mycc-1.0
```
