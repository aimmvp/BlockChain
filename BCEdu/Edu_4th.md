## chaincode 배포 &SDK 를 활용한 Client 모듈 개발 실습
### VM 환경에서 수행
  - express 설치
  
 ```
 $sudo npm install express --save
 ```
    
 - fabcar 디렉토리에 main.js 파일 생성  
 
  ``` 
 var express = require('express');var app = express();
 app.get('/', function (req, res) {
    res.send('Hello World!');
 });
 app.listen(3000, function () {
    console.log('Example app listening on port 3000!');
 });      
 ```
    
  - node main.js 수행 후 'Example app listening on port 3000!' 출력되어지면 정상.

  - Local에서 VM 접근을 위한 VM 설정 추가
  (127.0.0.1 3000 / 10.0.2.15 3000 설정 추가한 이미지 첨부하기)
  
  - fabcar 디렉토리 



