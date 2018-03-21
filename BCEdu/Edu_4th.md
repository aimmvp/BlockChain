## chaincode 배포 &SDK 를 활용한 Client 모듈 개발 실습

 - express 설치
  
 ```
 $sudo npm install express --save
 ```
 
 ```
 express 설치 결과
 
zing@zing:~/workplace/fabric-samples/fabcar$ sudo npm install express --save
[sudo] password for zing:
fabcar@1.0.0 /home/zing/workplace/fabric-samples/fabcar
└─┬ express@4.16.3
  ├── accepts@1.3.5
  ├── array-flatten@1.1.1
  ├─┬ body-parser@1.18.2
  │ ├── bytes@3.0.0
  │ ├─┬ http-errors@1.6.2
  │ │ ├── depd@1.1.1
  │ │ └── setprototypeof@1.0.3
  │ ├── iconv-lite@0.4.19
  │ └── raw-body@2.3.2
  ├── content-disposition@0.5.2
  ├── content-type@1.0.4
  ├── cookie@0.3.1
  ├── cookie-signature@1.0.6
  ├── depd@1.1.2
  ├── encodeurl@1.0.2
  ├── escape-html@1.0.3
  ├── etag@1.8.1
  ├─┬ finalhandler@1.1.1
  │ └── unpipe@1.0.0
  ├── fresh@0.5.2
  ├── merge-descriptors@1.0.1
  ├── methods@1.1.2
  ├─┬ on-finished@2.3.0
  │ └── ee-first@1.1.1
  ├── parseurl@1.3.2
  ├── path-to-regexp@0.1.7
  ├─┬ proxy-addr@2.0.3
  │ ├── forwarded@0.1.2
  │ └── ipaddr.js@1.6.0
  ├── range-parser@1.2.0
  ├─┬ send@0.16.2
  │ ├── destroy@1.0.4
  │ └── mime@1.4.1
  ├── serve-static@1.13.2
  ├── setprototypeof@1.1.0
  ├── statuses@1.4.0
  ├─┬ type-is@1.6.16
  │ └── media-typer@0.3.0
  ├── utils-merge@1.0.1
  └── vary@1.1.2

npm WARN fabcar@1.0.0 No repository field.
```
    
 - fabcar 디렉토리에 main.js 파일 생성
 (참조 : expressjs.com/ko/starter/hello-world.html)
 
  ``` 
 var express = require('express');var app = express();
 app.get('/', function (req, res) {
    res.send('Hello World!');
 });
 app.listen(3000, function () {
    console.log('Example app listening on port 3000!');
 });      
 ```
    
 - $ node main.js 수행 후 'Example app listening on port 3000!' 출력되어지면 정상.
```   
zing@zing:~/workplace/fabric-samples/fabcar$ node main.js
Example app listening on port 3000!
```
 - Local에서 VM 접근을 위한 VM 설정 추가
  ![VM 3000 Port 추가 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_1.png)
  
 - $ cd fabcar # fabcar 디렉토리로 이동
  
 - $ ./startFabric.sh  수행

 - $ node main.js 실행 후

 - Local PC 에서 browser를 통하여 http://127.0.0.1:3000 주소로 접속
  
 - "hello world!" 출력 되어지면 정상
  ![explorer 결과 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_2.png)
  
 - main.js 파일에서 query 결과를 출력할 수 있도록 소스 수정   
  
  ```
  수정된 main.js 파일
  (query.js 파일 참조하여 초기 network 설정과, /query 추가)
  
   'use strict';
   var express =  require('express');
   var app = express();
   var Fabric_Client =  require('fabric-client');
   var path = require('path');
   var util =  require('util');
   var os = require('os');
   //
   var fabric_client = new  Fabric_Client();
   // setup the fabric networkvar channel =  fabric_client.newChannel('mychannel');
   var peer =  fabric_client.newPeer('grpc://localhost:7051');
   channel.addPeer(peer);
   //
   var member_user = null;
   var store_path =  path.join(__dirname, 'hfc-key-store');
   console.log('Store  path:'+store_path);
   var tx_id = null;
   app.get('/', function (req, res) {
     res.send('Hello World!');
   });
   
   app.get('/query', function (req,res)  {
   // create the key value store as defined in the  fabric-client/config/default.json 'key-value-store' setting
   Fabric_Client.newDefaultKeyValueStore({ path:  store_path
   }).then((state_store) => {
     // assign the store to  the fabric client
     fabric_client.setStateStore(state_store);
     var crypto_suite =  Fabric_Client.newCryptoSuite();
     // use the same location for the  state store (where the users' certificate are kept)
     // and the crypto  store (where the users' keys are kept)
     var crypto_store =  Fabric_Client.newCryptoKeyStore({path: store_path});
     crypto_suite.setCryptoKeyStore(crypto_store);
     fabric_client.setCryptoSuite(crypto_suite);
     // get the enrolled user from  persistence, this user will sign all requests
     return  fabric_client.getUserContext('user1', true);
   }).then((user_from_store) =>  {
     if (user_from_store && user_from_store.isEnrolled())  {
       console.log('Successfully loaded user1 from  persistence');
       member_user = user_from_store;
     }  else {
       throw new Error('Failed to get user1.... run  registerUser.js');
     }
     // queryCar chaincode function - requires  1 argument, ex: args: ['CAR4'],
     // queryAllCars chaincode function -  requires no arguments , ex: args: [''],
     const request =  {
       //targets : --- letting this default to the peers assigned  to the channel
       chaincodeId: 'fabcar',
       fcn:  'queryAllCars',
       args: ['']
     };
     // send the query proposal to the  peer
     return  channel.queryByChaincode(request);  
   }).then((query_responses) => {
     console.log("Query has completed, checking results");
     //  query_responses could have more than one  results if there multiple peers were  used as targets
     if (query_responses && query_responses.length  == 1) {
       if (query_responses[0] instanceof Error)  {
         console.error("error from query = ",  query_responses[0]);
       } else {
         console.log("Response is ",  query_responses[0].toString());
         res.end(query_responses[0].toString());   // 이결과로 브라우져에서 조회시 쿼리 조회 결과가 보여집니다
       }
     } else {
       console.log("No  payloads were returned from query");
     }
   }).catch((err) =>  {
     console.error('Failed to query successfully :: ' +  err);
   });
 });
 app.listen(3000, function () {
   console.log('Example app listening on port 3000!');
 });
  ```
 - 변경된 main.js 재실행 한 뒤 Local PC Browser에서 127.0.0.1:3000/query로 접속
 
 - query 결과 출력 되어지면 정상
 ![query 결과 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_3.png)
 
 - Local에서 개발 가능하도록 VM서버와 연동을 위한 Network Port 추가
 - 7051, 7053,  7054, 4369, 9100, 5984, 7050 Port 추가
 ![Network Port추가 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_4.png)
 
 - Local에 nodejs 9.X 버전 설치 (다운로드 페이지 : https://nodejs.org/en/)
 
 - 설치된 nodejs 버전 확인 (nodejs 설치 후 cmd 환경에서 node -v 명령어 수행)
 ![Local nodejs 버전확인 ](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu4_5.png)
 
 - c:\myapp 폴더 생성
 
 - npm 설치(cmd에서 npm init 수행)
  ```
  C:\myapp>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (myapp)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to C:\myapp\package.json:

{
  "name": "myapp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes)
  
  ```
  
 - npm install 수행
  ```
C:\myapp>npm install
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN myapp@1.0.0 No description
npm WARN myapp@1.0.0 No repository field.

up to date in 0.085s
  
  ```
  
 - package.json에 dependencies 추가
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
 - dependencies 추가 후, 관리자권한 cmd에서 npm install 재수행
