## Writing Your First Application
http://hyperledger-fabric.readthedocs.io/en/latest/writefirstapp.html

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

## 네트워크 삭제
### Chaincode for Developers
http://hyperledger-fabric.readthedocs.io/en/latest/chaincode4ade.html

### Clear previous network
```
docker rm -f $(docker ps -aq)
docker network prune
```
### Simple Asset Chaincode
#### Choosing a Location for the Code
```
mkdir -p $GOPATH/src/sacc
cd $GOPATH/src/sacc
touch sacc.go
```
#### Pulling it All Together
```
package main

import (
    "fmt"

    "github.com/hyperledger/fabric/core/chaincode/shim"
    "github.com/hyperledger/fabric/protos/peer"
)

// SimpleAsset implements a simple chaincode to manage an asset
type SimpleAsset struct {
}

// Init is called during chaincode instantiation to initialize any
// data. Note that chaincode upgrade also calls this function to reset
// or to migrate data.
func (t *SimpleAsset) Init(stub shim.ChaincodeStubInterface) peer.Response {
    // Get the args from the transaction proposal
    args := stub.GetStringArgs()
    if len(args) != 2 {
            return shim.Error("Incorrect arguments. Expecting a key and a value")
    }

    // Set up any variables or assets here by calling stub.PutState()

    // We store the key and the value on the ledger
    err := stub.PutState(args[0], []byte(args[1]))
    if err != nil {
            return shim.Error(fmt.Sprintf("Failed to create asset: %s", args[0]))
    }
    return shim.Success(nil)
}

// Invoke is called per transaction on the chaincode. Each transaction is
// either a 'get' or a 'set' on the asset created by Init function. The Set
// method may create a new asset by specifying a new key-value pair.
func (t *SimpleAsset) Invoke(stub shim.ChaincodeStubInterface) peer.Response {
    // Extract the function and args from the transaction proposal
    fn, args := stub.GetFunctionAndParameters()

    var result string
    var err error
    if fn == "set" {
            result, err = set(stub, args)
    } else { // assume 'get' even if fn is nil
            result, err = get(stub, args)
    }
    if err != nil {
            return shim.Error(err.Error())
    }

    // Return the result as success payload
    return shim.Success([]byte(result))
}

// Set stores the asset (both key and value) on the ledger. If the key exists,
// it will override the value with the new one
func set(stub shim.ChaincodeStubInterface, args []string) (string, error) {
    if len(args) != 2 {
            return "", fmt.Errorf("Incorrect arguments. Expecting a key and a value")
    }

    err := stub.PutState(args[0], []byte(args[1]))
    if err != nil {
            return "", fmt.Errorf("Failed to set asset: %s", args[0])
    }
    return args[1], nil
}

// Get returns the value of the specified asset key
func get(stub shim.ChaincodeStubInterface, args []string) (string, error) {
    if len(args) != 1 {
            return "", fmt.Errorf("Incorrect arguments. Expecting a key")
    }

    value, err := stub.GetState(args[0])
    if err != nil {
            return "", fmt.Errorf("Failed to get asset: %s with error: %s", args[0], err)
    }
    if value == nil {
            return "", fmt.Errorf("Asset not found: %s", args[0])
    }
    return string(value), nil
}

// main function starts up the chaincode in the container during instantiate
func main() {
    if err := shim.Start(new(SimpleAsset)); err != nil {
            fmt.Printf("Error starting SimpleAsset chaincode: %s", err)
    }
}
```
#### Building Chaincode
```
go get -u --tags nopkcs11 github.com/hyperledger/fabric/core/chaincode/shim
go build --tags nopkcs11
ls -l
```

### Testing Using dev mode
```
cd ~/workspace/fabric-samples
cd chaincode-docker-devmode
```
### Terminal 1 - Start the network
```
docker-compose -f docker-compose-simple.yaml up
```

### Terminal 2 - Build & start the chaincode
```
docker exec -it chaincode bash
```
```
cd sacc
go build
CORE_PEER_ADDRESS=peer:7052 CORE_CHAINCODE_ID_NAME=mycc:0 ./sacc
```

### Terminal 3 - Use the chaincode
```
docker exec -it cli bash
```
```
peer chaincode install -p chaincodedev/chaincode/sacc -n mycc -v 0
```
```
peer chaincode instantiate -n mycc -v 0 -c '{"Args":["a","10"]}' -C myc
```
```
peer chaincode invoke -n mycc -c '{"Args":["set", "a", "20"]}' -C myc
```
```
peer chaincode query -n mycc -c '{"Args":["query","a"]}' -C myc
```
