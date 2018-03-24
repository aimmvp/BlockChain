## 개발 환경 세팅(part 1)
### 1. VirtualBox 설치
  - OS에 따른 VirtualBox설치(https://www.virtualbox.org/wiki/Downloads)
  - Version 5.2.8 설치(2018.3.13 기준)
    ![Version 5.2.8 설치](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_1.png)
  
### 2. 가상머신 만들기 
  - "새로만들기" 클릭 --> 이름에 "ubuntu" 입력 --> "다음" 클릭
    ![가상머신 만들기](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_2.png)

### 3. Ubuntu 설치
  - Ubuntu 16.04.3 LTS Download(https://www.ubuntu.com/download/server)
    ![Ubuntu Image File Download](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_3.png)
  - 가상머신에 다운받은 iso 파일 추가
    ![iso 파일 추가](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_4.png)
  - Ubuntu Server 16.04설치
    * Language(한국어) --> 우분투 서버 설치(I) --> Select a language(예) --> 위치를 선택하십시오(대한민국) --> 키보드 설정(아니요) --> 키보드 설정(Korean) --> 키보드 설정:키보드 배치(Korean) --> 네트워크 설정 --> 사용자 및 암호 설정(시작폴더 암호화 설정:아니요) --> 시계설정(Asia/Seoul 확인 후 예) --> 디스크 파티션 하기(자동-디스크 전체 사용하고 LVM 설정, 디스크 선택, 볼륨 그룹의 크기 입력, 바뀐 점을 디스크에 쓰기) --> 패키지 관리자 설정(HTTP 프록시 정보 : 빈칸) --> tasksel 설정(자동 업데이트 하지 않음) --> 소프트웨어 선택(OpenSSH Server 추가 선택) --> 하드 디스크에 GRUB 부트로더 설치(예)
    ![OpenSSH Server](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_9.png)
  - 가상머신 IP 확인 
    ```
    $ ifconfig
    ```
    ![IP확인하기](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_5.png)
  - ssh 접속을 위한 네트워크 설정
    ![네트워크 어댑터 : NAT, 포트포워딩 설정](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_6.png)
    * ubuntu 의 10.0.2.15 포트 22를 2222 포트로 포워딩 설정
### 4. Terminal(PuTTY) 설치
  - Windows를 사용 할 경우 putty 등 Terminal 설치 필요
  - PuTTY Download(https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
    ![PuTTY Download](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_7.png)
  - PuTTY Session 정보 설정
    * IP : 127.0.0.1, PORT : 2222    
    ![PuTTY Configuration](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/edu1_8.png)
    * 접속이 되지 않을 경우 Ubuntu Server 설치 시 소프투웨어 설치가 안된 것으로 아래의 script 로 openssh server 설치
    ```
	$ sudo apt install openssh-client
	$ sudo apt install openssh-server
	```
### 5. (option) Mac 에서 터미널로 직접 ssh 접근하기
  ```
  $ ssh s0wnd@localhost -p 2222
  ```
