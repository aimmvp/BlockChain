1. VirtualBox 설치
  - OS에 따른 VirtualBox설치(https://www.virtualbox.org/wiki/Downloads)
  - Version 5.2.6 설치
    ![Version 5.2.6 설치](https://github.com/aimmvp/BlockChain/blob/master/bc1_1.png)
  - ssh 접속을 위한 네트워크 설정
    ![네트워크 어댑터 : NAT, 포트포워딩 설정](https://github.com/aimmvp/BlockChain/blob/master/bc1_2.png)
2. Ubuntu 설치
  - Ubuntu 16.04.3 LTS Download(https://www.ubuntu.com/download/server)
    ![Ubuntu Image File Download](https://github.com/aimmvp/BlockChain/blob/master/bc2_1.png)
  - Ubuntu Server 16.04설치
    ![가상머신 생성](https://github.com/aimmvp/BlockChain/blob/master/bc2_2.png)
    * 메모리 크기 설정(1024MB) --> 하드디스크(10GB) --> 하드디스크 파일 종류(VDI) --> 물리적 하드 드라이브에 저장(동적할당) --> 파일 위치 및 크기 --> 만들기
  - Ubuntu 설정
    * Language(한국어) --> 우분투 서버 설치(I) --> Select a language(예) --> 위치를 선택하십시오(대한민국) --> 키보드 설정(아니요) --> 키보드 설정(Korean) --> 키보드 설정:키보드 배치(Korea) --> 네트워크 설정 --> 사용자 및 암호 설정(시작폴더 암호화 설정:아니요) --> 시계설정(Asia/Seoul 확인 후 예) --> 디스크 파티션 하기(자동-디스크 전체 사용하고 LVM 설정, 디스크 선택, 볼륨 그룹의 크기 입력, 바뀐 점을 디스크에 쓰기) --> 패키지 관리자 설정(HTTP 프록시 정보 : 빈칸) --> tasksel 설정(자동 업데이트 하지 않음) --> 소프트웨어 선택(OpenSSH Server 추가 선택)
3. Terminal 설치


### nodejs update
sudo apt-get update && sudo apt-get -y upgrade
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install nodejs




http://hyperledger-fabric.readthedocs.io/en/release/write_first_app.html?highlight=enroll


docker ps 로 확인
docker rm -f $(docker ps -aq)
docker network prune    # 도커 설정까지 다 내리기

cd ~/fabric-samples/fabcar
sudo npm install
sudo npm rebuild
node enrollAdmin.js



docker exec -it cli bash    :  cli container 에 들어가기


