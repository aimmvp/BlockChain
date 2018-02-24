원문 : https://hyperledger-fabric.readthedocs.io/en/release/txflow.html

## Transaction Flow
이 문서는 표준자산 교환동안 벌어지는 거래방법의 요약이다. 시나리오는 무를 사고 파는 두 클라이언트(A, B)에 대한 이야기다. 그들은 각각 그들의 거래와 원장과의 상호작용을 통한 네트웍에 peer 를 가진다.
![](https://github.com/aimmvp/BlockChain/blob/master/tf1.png)

### 전제
이 흐름은 채널이 설정되어 있고, 작동중이라고 전제한다. 어플리케이션 사용자는 조직의 CA에 등록하고 enroll했으며, 네트웍에 인증하는데 사용되기 위해 필요한 암호화 자료를 받환 받았다. 

체인코드(무 시장의 초기 상태를 나타내는 키/값 쌍을 포함)는 피어에 설치되고 채널에 인스턴스화된다. 체인코드는 거래지침세트와 무의 합의가격을 정의하는 논리를 포함한다. 이 체인코드에 대한 승인정책도 있고, ```peerA``` 와 ```peerB``` 모두 어떤 거래든지 보증해야 함을 의미한다.
![](https://github.com/aimmvp/BlockChain/blob/master/tf2.png)

### 1. Client A 가 트랙젝션 초기화하기
무슨일이 이루어 지는가? - Client A 는 무의 구입요청을 보내고 있다. 요청은 ClentA와 ClientB를 각각 대표하는 peerA 와 peerB 를 대상으로 한다. 승인정책은 peer 둘 다 어떤 거래를 보증해야함을 말하기 때문에, 요청은 ```peerA``` 와 ```peerB```로 간다.
![](https://github.com/aimmvp/BlockChain/blob/master/tf3.png)

### 2. 승인받은 peer 의 서명확인 & 거래 이행
승인받은 peer의 (1)거래제안이 제대로 되었는지 확인하고, (2) 과거에 이미 제출되지 않았음(재생 공격 방지), (3)서명은 유효함(MSP 사용), 그리고 (4)제출자(예를 들면 Client A)는 채널에서 요청받은 동작을 수행하도록 적절하게 권한을 받았다.(즉, 각 승인받은 peer 는 제출자가 체널의 기록정책을 만족하도록 보장한다.) 승인받은 peer는 거래제안의 입력값을 체인코드 함수 호출의 변수로 받아들인다. 그러면 체인코드는 응답값, read set, write set을 포함한 거래결과를 포함한 현재 상태 데이터베이스에 대해 실행한다. 이 지점에서는 원장에 대한 업데이트가 없다. 이런 값 세트는 승인받은 peer의 서명과 함께 "제안의 응답" 으로 어플리케이션의 소모를 위한 페이로드를 분석하는 SDK에 반환된다.
{
MSP는 그들이 client에게 도달하는 거래요청을 확인하고 거래결과(승인)에 서명한느 것을 허락하는 peer 요소이다. 쓰기 정책은 채널생성시점에 정의되고, 그 채널에 트랜잭션을 제출할 권한이 있는 사용자를 결정한다.
}
![](https://github.com/aimmvp/BlockChain/blob/master/tf4.png)








