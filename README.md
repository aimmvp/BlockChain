1. VirtualBox 설치
  - OS에 따른 VirtualBox설치(https://www.virtualbox.org/wiki/Downloads)
  - Version 5.2.6 설치
  - ![Version 5.2.6 설치](https://github.com/aimmvp/BlockChain/blob/master/bc1_1.png)
2. Ubuntu 설치
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


