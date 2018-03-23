## Hyperledger 간단 통신 테스트
### 1. Hyperledger SDK node module 설치

Blockchain Network와 통신하기 위해서는 ‘Network endpoint’를 통해 통신할 수 있음.
endpoint와 같은 API들에 접근하기 위해서 Hyperledger SDK를 먼저 설치해야 함.
SDK를 설치하고 Blockchain Network에 접근하는 방법 소개하고자 함.

 - 샘플폴더로 이동
  ```
 $ cd fabric-samples/fabcar
```
 - Hyperledger SDK 설치
 ```
 $ $ npm install
fabcar@1.0.0 /home/hirohi/fabric-samples/fabcar
├─┬ fabric-ca-client@1.0.5
│ ├── bn.js@4.11.8
│ ├─┬ elliptic@6.4.0
│ │ ├── brorand@1.1.0
│ │ ├── hash.js@1.1.3
│ │ ├── hmac-drbg@1.0.1
│ │ ├── inherits@2.0.3
│ │ ├── minimalistic-assert@1.0.0
│ │ └── minimalistic-crypto-utils@1.0.1
│ ├─┬ fs-extra@0.30.0
│ │ ├── graceful-fs@4.1.11
│ │ ├── jsonfile@2.4.0
│ │ ├── path-is-absolute@1.0.1
│ │ └── rimraf@2.6.2
│ ├── js-sha3@0.5.7
│ ├── jsrsasign@6.2.3
│ ├── jssha@2.3.1
│ ├─┬ nconf@0.8.5
│ │ ├── async@1.5.2
│ │ ├── ini@1.3.5
│ │ ├── secure-keys@1.0.0
│ │ └─┬ yargs@3.32.0
│ │   ├── camelcase@2.1.1
│ │   ├─┬ cliui@3.2.0
│ │   │ ├─┬ strip-ansi@3.0.1
│ │   │ │ └── ansi-regex@2.1.1
│ │   │ └── wrap-ansi@2.1.0
│ │   ├── decamelize@1.2.0
│ │   ├─┬ os-locale@1.4.0
│ │   │ └─┬ lcid@1.0.0
│ │   │   └── invert-kv@1.0.0
│ │   ├─┬ string-width@1.0.2
│ │   │ ├── code-point-at@1.1.0
│ │   │ └─┬ is-fullwidth-code-point@1.0.0
│ │   │   └── number-is-nan@1.0.1
│ │   ├── window-size@0.1.4
│ │   └── y18n@3.2.1
│ ├── sjcl@1.0.7
│ ├── sjcl-codec@0.1.1
│ ├─┬ url@0.11.0
│ │ ├── punycode@1.3.2
│ │ └── querystring@0.2.0
│ ├─┬ util@0.10.3
│ │ └── inherits@2.0.1
│ └─┬ winston@2.4.1
│   ├── async@1.0.0
│   ├── colors@1.0.3
│   ├── cycle@1.0.3
│   ├── eyes@0.1.8
│   ├── isstream@0.1.2
│   └── stack-trace@0.0.10
├─┬ fabric-client@1.0.5
│ ├── callsite@1.0.0
│ ├── crypto@0.0.3
│ ├── fs@0.0.2
│ ├── jsrsasign@6.2.2
│ ├── klaw@1.3.1
│ ├── long@3.2.0
│ ├─┬ nano@6.4.3
│ │ ├─┬ cloudant-follow@0.16.1
│ │ │ ├── browser-request@0.3.3
│ │ │ └── debug@3.1.0
│ │ ├─┬ debug@2.6.9
│ │ │ └── ms@2.0.0
│ │ ├── errs@0.3.2
│ │ ├─┬ request@2.83.0
│ │ │ ├── aws-sign2@0.7.0
│ │ │ ├── aws4@1.6.0
│ │ │ ├── caseless@0.12.0
│ │ │ ├─┬ combined-stream@1.0.6
<중략>
│ │ │ └── uuid@3.2.1
│ │ └── underscore@1.8.3
│ ├─┬ path@0.12.7
│ │ └── process@0.11.10
│ ├── pkcs11js@1.0.13
│ ├── promise-settle@0.3.0
│ ├── stream-buffers@3.0.1
│ └─┬ tar-stream@1.5.2
│   ├── bl@1.2.1
│   ├─┬ end-of-stream@1.4.1
│   │ └─┬ once@1.4.0
│   │   └── wrappy@1.0.2
│   ├─┬ readable-stream@2.3.5
│   │ ├── core-util-is@1.0.2
│   │ ├── isarray@1.0.0
│   │ ├── process-nextick-args@2.0.0
│   │ ├── string_decoder@1.0.3
│   │ └── util-deprecate@1.0.2
│   └── xtend@4.0.1
└─┬ grpc@1.10.0
  ├── lodash@4.17.5
  ├── nan@2.9.2
  ├─┬ node-pre-gyp@0.6.39
  │ ├── detect-libc@1.0.3
  │ ├─┬ hawk@3.1.3
  │ │ ├── boom@2.10.1
  │ │ ├── cryptiles@2.0.5
  │ │ ├── hoek@2.16.3
  │ │ └── sntp@1.0.9
  │ ├─┬ mkdirp@0.5.1
  │ │ └── minimist@0.0.8
  │ ├─┬ nopt@4.0.1
  │ │ ├── abbrev@1.1.1
  │ │ └─┬ osenv@0.1.4
  │ │   ├── os-homedir@1.0.2
  │ │   └── os-tmpdir@1.0.2
  │ ├─┬ npmlog@4.1.2
  │ │ ├─┬ are-we-there-yet@1.1.4
  │ │ │ └── delegates@1.0.0
<중략>
  │ │ ├── minimist@1.2.0
  │ │ └── strip-json-comments@2.0.1
  │ ├─┬ request@2.81.0
  │ │ ├── aws-sign2@0.6.0
  │ │ ├── aws4@1.6.0
  │ │ ├── caseless@0.12.0
  │ │ ├─┬ combined-stream@1.0.5
  │ │ │ └── delayed-stream@1.0.0
  │ │ ├── extend@3.0.1
  │ │ ├── forever-agent@0.6.1
  │ │ ├─┬ form-data@2.1.4
  │ │ │ └── asynckit@0.4.0
  │ │ ├─┬ har-validator@4.2.1
  │ │ │ ├─┬ ajv@4.11.8
  │ │ │ │ ├── co@4.6.0
  │ │ │ │ └─┬ json-stable-stringify@1.0.1
  │ │ │ │   └── jsonify@0.0.0
  │ │ │ └── har-schema@1.0.5
  │ │ ├─┬ http-signature@1.1.1
  │ │ │ ├── assert-plus@0.2.0
  │ │ │ ├─┬ jsprim@1.4.1
  │ │ │ │ ├── assert-plus@1.0.0
  │ │ │ │ ├── extsprintf@1.3.0
  │ │ │ │ ├── json-schema@0.2.3
  │ │ │ │ └─┬ verror@1.10.0
  │ │ │ │   └── assert-plus@1.0.0
  │ │ │ └─┬ sshpk@1.13.1
  │ │ │   ├── asn1@0.2.3
  │ │ │   ├── assert-plus@1.0.0
  │ │ │   ├── bcrypt-pbkdf@1.0.1
  │ │ │   ├─┬ dashdash@1.14.1
  │ │ │   │ └── assert-plus@1.0.0
  │ │ │   ├── ecc-jsbn@0.1.1
  │ │ │   ├─┬ getpass@0.1.7
  │ │ │   │ └── assert-plus@1.0.0
  │ │ │   ├── jsbn@0.1.1
  │ │ │   └── tweetnacl@0.14.5
  │ │ ├── is-typedarray@1.0.0
  │ │ ├── isstream@0.1.2
  │ │ ├── json-stringify-safe@5.0.1
  │ │ ├─┬ mime-types@2.1.17
  │ │ │ └── mime-db@1.30.0
  │ │ ├── oauth-sign@0.8.2
  │ │ ├── performance-now@0.2.0
  │ │ ├── qs@6.4.0
  │ │ ├── safe-buffer@5.1.1
  │ │ ├── stringstream@0.0.5
  │ │ ├─┬ tough-cookie@2.3.3
  │ │ │ └── punycode@1.4.1
  │ │ ├── tunnel-agent@0.6.0
  │ │ └── uuid@3.2.1
  │ ├─┬ rimraf@2.6.2
  │ │ └─┬ glob@7.1.2
  │ │   ├── fs.realpath@1.0.0
  │ │   ├── inflight@1.0.6
  │ │   ├─┬ minimatch@3.0.4
  │ │   │ └─┬ brace-expansion@1.1.8
  │ │   │   ├── balanced-match@1.0.0
  │ │   │   └── concat-map@0.0.1
  │ │   └── path-is-absolute@1.0.1
  │ ├── semver@5.5.0
  │ ├─┬ tar@2.2.1
  │ │ ├── block-stream@0.0.9
  │ │ ├─┬ fstream@1.0.11
  │ │ │ └── graceful-fs@4.1.11
  │ │ └── inherits@2.0.3
  │ └─┬ tar-pack@3.4.1
  │   ├─┬ debug@2.6.9
  │   │ └── ms@2.0.0
  │   ├── fstream-ignore@1.0.5
  │   ├─┬ once@1.4.0
  │   │ └── wrappy@1.0.2
  │   ├─┬ readable-stream@2.3.3
  │   │ ├── core-util-is@1.0.2
  │   │ ├── isarray@1.0.0
  │   │ ├── process-nextick-args@1.0.7
  │   │ ├── string_decoder@1.0.3
  │   │ └── util-deprecate@1.0.2
  │   └── uid-number@0.0.6
  └─┬ protobufjs@5.0.2
    ├─┬ ascli@1.0.1
    │ ├── colour@0.7.1
    │ └── optjs@3.2.2
    ├── bytebuffer@5.0.1
    └─┬ glob@7.1.2
      ├── fs.realpath@1.0.0
      ├── inflight@1.0.6
      └─┬ minimatch@3.0.4
        └─┬ brace-expansion@1.1.11
          ├── balanced-match@1.0.0
          └── concat-map@0.0.1

npm WARN fabcar@1.0.0 No repository field.
```
 - Node_modules 폴더 생성된 것 확인
 ```
hirohi@ubuntu:~/fabric-samples/fabcar$ ls -al
합계 40
drwxrwxr-x   3 hirohi hirohi 4096  3월 14 18:03 .
drwxrwxr-x  10 hirohi hirohi 4096  3월  1 23:25 ..
-rw-rw-r--   1 hirohi hirohi 2809  3월  1 23:25 enrollAdmin.js
-rw-rw-r--   1 hirohi hirohi 6354  3월  1 23:25 invoke.js
drwxrwxr-x 145 hirohi hirohi 4096  3월 14 18:04 node_modules
-rw-rw-r--   1 hirohi hirohi  533  3월  1 23:25 package.json
-rw-rw-r--   1 hirohi hirohi 2606  3월  1 23:25 query.js
-rw-rw-r--   1 hirohi hirohi 3132  3월  1 23:25 registerUser.js
-rwxr-xr-x   1 hirohi hirohi 1829  3월  1 23:25 startFabric.sh
hirohi@ubuntu:~/fabric-samples/fabcar$
 ```
$ node enrollAdmin.js
```
$ node registerUser.js
 ```

  - Blockchain network와 통신할 준비 완료

 - Admin 등록 과 User 생성/등록 
	새로운 User를 만들기 위해 CA서버에 먼저 Admin 등록 후, Ledger를 조회하고 업데이트할 사용자를 생성하고 등록함.

  - BC Network를 통해서 ledger들의 목록 가져오기(query.js 의 내용)
  - Smart contract 내용 파일 확인 (fabcar.go)

	

### 특정 항목 대한 정보 출력
  - 기존 query.js 파일을 queryCar.js 로 수정하여 request부분의 fnc부분과 args 부분을 수정
  ```
$ cp query.js queryCar.js
$ vim queryCar.js
아래 처럼 수정합니다.

        const request = {
                chaincodeId: 'fabcar',
                fcn: 'queryCar',
                args: ['CAR7']
        };
		
$ node queryCar.js
Store path:/home/hirohi/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  {"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}
  ```
  - args를 인자로 받도록 수정
  ```
아래 처럼 수정합니다.

상단에 아래 추가 및 request args 부분을 수정

var args = process.argv.slice(2);

        const request = {
                chaincodeId: 'fabcar',
                fcn: 'queryCar',
                args: [args[0]]
        };
$ node queryCar.js
Store path:/home/hirohi/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  {"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}	
  ```

### 3. 추가 및 수정
  - https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository 참고
  1. 추가
    - 새로운 ledger 추가를 위해서는 invke.js 수정. 
    - createCar 함수를 통해서 CAR10 추가
  ```
/fabric-samples/fabcar$ vim invoke.js
        const request = {
                chaincodeId: 'fabcar',
                fcn: createCar',
                args: ['CAR10', ‘KIA’, ‘K7’, ‘Silver’, ‘LEEE’]
        };
hirohi@ubuntu:~/fabric-samples/fabcar$ node invoke.js
Store path:/home/hirohi/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Assigning transaction_id:  26e545b5e598f7371afe2c19f1a4afe5a7d7204b37e4896e28bc4a68117b44b2
Transaction proposal was good
Successfully sent Proposal and received ProposalResponse: Status - 200, message - "OK"
info: [EventHub.js]: _connect - options {"grpc.max_receive_message_length":-1,"grpc.max_send_message_length":-1}
The transaction has been committed on peer localhost:7053
Send transaction promise and event listener promise have completed
Successfully sent transaction to the orderer.
Successfully committed the change to the ledger by the peer
hirohi@ubuntu:~/fabric-samples/fabcar$ node queryCar.js CAR10
Store path:/home/hirohi/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  {"colour":"Silver","make":"KIA","model":"K7","owner":"LEE"}
  ```

  2. 변경
  - 소유자를 변경
  ```
/fabric-samples/fabcar$ vim invoke.js
        var request = {
                chaincodeId: 'fabcar',
                fcn: ‘changeCarOwner’,
                args: ['CAR10', ‘Hiro Haha’]
<중략>
        };

hirohi@ubuntu:~/fabric-samples/fabcar$ node queryCar CAR10
Store path:/home/hirohi/fabric-samples/fabcar/hfc-key-store
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  {"colour":"Silver","make":"KIA","model":"K7","owner":"Hiro Haha"}
  ```
  3. App에서 endpoint로 변경요청 후 처리 알림
  ```
-	invoke.js 수정
hirohi@ubuntu:~/fabric-samples/fabcar$ vim invoke.js
var request = {
    targets: targets,
    chaincodeId: options.chaincode_id,
    fcn: 'createCar',
    args: ['CAR10', 'Chevy', 'Volt', 'Red', 'Nick'],
    chainId: options.channel_id,
    txId: tx_id
};
  ```

  ```
-	queryCar.js 수정
hirohi@ubuntu:~/fabric-samples/fabcar$ vim queryCar.js
var request = {
    targets: targets,
    chaincodeId: options.chaincode_id,
    fcn: 'createCar',
    args: ['CAR10', 'Chevy', 'Volt', 'Red', 'Nick'],
    chainId: options.channel_id,
    txId: tx_id
};
  ```
  총 6개의 docker container 가 실행되고 있음.
  
    - dev-peer0.org1.example.com-fabcar-1.0.*
    - hyperledger/fabric-tools
    - hyperledger/fabric-peer
    - hyperledger/fabric-ca
    - hyperledger/fabric-couchdb
    - hyperledger/fabric-orderer
