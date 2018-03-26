## chaincode 배포 & SDK 를 활용한 Client 모듈 개발 실습

### 1. express 설치  
```
 $ cd ~/workplace/fabric-samples/fabcar
 $ sudo npm install express --save
```

### 2. main.js 파일 생성
 * 참조 : expressjs.com/ko/starter/hello-world.html
 ```
 $ vi main.js
 ```
 ```
 var express = require('express');
 var app = express();
 app.get('/', function (req, res) {
    res.send('Hello World!');
 });
 app.listen(3000, function () {
    console.log('Example app listening on port 3000!');
 });
 ```
 * 정상 수행 여부 확인
 ```
 $ node main.js
 Example app listening on port 3000!
 ```

### 3. Local에서 VM 접근을 위한 VM 설정 추가 및 테스트
  * VirtualBox 네트워크 포트포워딩 Rule 추가 : 3000 port
  ![VM 3000 Port 추가 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_1.png)
  * 설정 테스트
  ```
  $ cd fabcar
  $ ./startFabric.sh
  $ node main.js
  ```
  * Local PC 에서 browser를 통하여 http://127.0.0.1:3000 접속 테스트
  ![explorer 결과 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_2.png)

### 4. query 결과 출력
  * main.js 파일 수정( query.js 파일 참조하여 초기 network 설정과 /query 추가)
   - query.js 파일을 main.js 로 복사 한 후 express와 /query 부분 추가
  ```
  $ cp query.js main.js
  $ vi main.js
  ```
  * https://github.com/aimmvp/BlockChain/blob/master/BCEdu/main.js 에서 ```// 추가``` 로 검색하여 수정

* 변경된 내용 테스트
 ```
 $ node main.js
 ```
   - Local Browser에서 http://127.0.0.1:3000/query 로 접속
   ![query 결과 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_3.png)

### 5. Local에서 개발을 위한 세팅
 * Local PC 에서 수행
 * VirtualMachine 과의 연동을 위한 포트포워딩 추가
  - 7051, 7053,  7054, 4369, 9100, 5984, 7050 Port 추가
 ![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_4.png)
 * Node.js 9.x 버전 설치([Node.js 다운로드](https://nodejs.org/en/)]
 * 설치된 Node.js 버전 확인
  ```
  $ node -v
  ```
 ![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_5.png)
 * myapp 폴더 생성 
 ```
 $ mkdir c:\myapp
 ```
 * npm 설치(관리자 권한으로 cmd  실행)
 ```
 $ cd c:\myapp
 $ npm init
 $ npm install
 ```
 * package.json 에 dependencies 추가
  ```
  {
  "name": "myapp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
 "windows-build-tools": "^2.2.1",
 "crypto": "^1.0.1",
 "express": "^4.16.2",
 "fabric-ca-client": "^1.0.2",
 "fabric-client": "^1.0.2",
 "grpc": "^1.6.0"
  }
}
  ```
 * 관리자 권한 cmd에서 npm install
 ```
 $ npm install windows-build-tools
 $ npm install
 ```
 - dependencies 추가 후, 관리자권한 cmd에서 npm install 재수행
  (npm install 오류 발생시 npm install windows-build-tools 수행 후, npm install 하면 됩니다.)
  - 오류 발생시 ```$ npm rebuild``` 실행 후 ```$ npm install``` 재실행
 * fabcar 에 있는 enrollAdmin.js, registerUser.js, query.js 복사
  - https://github.com/hyperledger/fabric-samples/blob/release-1.1/fabcar/enrollAdmin.js
  - https://github.com/hyperledger/fabric-samples/blob/release-1.1/fabcar/registerUser.js
  - https://github.com/hyperledger/fabric-samples/blob/release-1.1/fabcar/query.js
 - cmd에서 node enrollAdmin.js 실행
 (오류 발생시 npm rebuild 한뒤, npm install 재실행 하시면 됩니다.)
 ```
  C:\myapp>node enrollAdmin.js
 Store path:C:\myapp\hfc-key-store
Successfully enrolled admin user "admin"
Assigned the admin user to the fabric client ::{"name":"admin","mspid":"Org1MSP","roles":null,"affiliation":"","enrollmentSecret":"","enrollment":{"signingIdentity":"5d77400bd89e2d76958bad2a2f77b1166d12cb2fb9b8c7070f92bf58f8cea4f2","identity":{"certificate":"-----BEGIN CERTIFICATE-----\nMIICAjCCAaigAwIBAgIUWYZM6CF1ebHl96fy0kHTcJ5fWrQwCgYIKoZIzj0EAwIw\nczELMAkGA1UEBhMCVVMxEzARBgNVBAgTCkNhbGlmb3JuaWExFjAUBgNVBAcTDVNh\nbiBGcmFuY2lzY28xGTAXBgNVBAoTEG9yZzEuZXhhbXBsZS5jb20xHDAaBgNVBAMT\nE2NhLm9yZzEuZXhhbXBsZS5jb20wHhcNMTgwMzIwMTU0NTAwWhcNMTkwMzIwMTU1\nMDAwWjAhMQ8wDQYDVQQLEwZjbGllbnQxDjAMBgNVBAMTBWFkbWluMFkwEwYHKoZI\nzj0CAQYIKoZIzj0DAQcDQgAEi4DYcNcD4XxtW0s7ReP7aditmyFAxpDnzvGjquG5\nAQYqGzK/dXxTXwRQfhywvoGaJBK+H+eNvCfOQPiobBJi5KNsMGowDgYDVR0PAQH/\nBAQDAgeAMAwGA1UdEwEB/wQCMAAwHQYDVR0OBBYEFEPqXDElkJNCcf+UlT9D8jgK\nq5lPMCsGA1UdIwQkMCKAIEI5qg3NdtruuLoM2nAYUdFFBNMarRst3dusalc2Xkl8\nMAoGCCqGSM49BAMCA0gAMEUCIQCxUr6z20YcMgZBq65nLc9YnYVmTzFPOELexqO7\n9qD71QIgJkyEaH7vUGL/d2RBwKX6eTkR5lYxjNUqFo+p5QeZf7U=\n-----END CERTIFICATE-----\n"}}}
 ```
 - cmd 에서 resisterUser.js 실행
 ```
C:\myapp>node registerUser.js
Store path:C:\myapp\hfc-key-store
Successfully loaded admin from persistence
Successfully registered user1 - secret:MDcuGaFswsph
Successfully enrolled member user "user1"
User1 was successfully registered and enrolled and is ready to intreact with the fabric network
 ```
 - cmd에서 query.js 실행
 ```
C:\myapp>node query.js
Store path:C:\myapp\hfc-key-store
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  [{"Key":"CAR0", "Record":{"colour":"blue","make":"Toyota","model":"Prius","owner":"Tomoko"}},{"Key":"CAR1", "Record":{"colour":"red","make":"Ford","model":"Mustang","owner":"Brad"}},{"Key":"CAR2", "Record":{"colour":"green","make":"Hyundai","model":"Tucson","owner":"Jin Soo"}},{"Key":"CAR3", "Record":{"colour":"yellow","make":"Volkswagen","model":"Passat","owner":"Max"}},{"Key":"CAR4", "Record":{"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}},{"Key":"CAR5", "Record":{"colour":"purple","make":"Peugeot","model":"205","owner":"Michel"}},{"Key":"CAR6", "Record":{"colour":"white","make":"Chery","model":"S22L","owner":"Aarav"}},{"Key":"CAR7", "Record":{"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}},{"Key":"CAR8", "Record":{"colour":"indigo","make":"Tata","model":"Nano","owner":"Valeria"}},{"Key":"CAR9", "Record":{"colour":"brown","make":"Holden","model":"Barina","owner":"Shotaro"}}]
 ```
