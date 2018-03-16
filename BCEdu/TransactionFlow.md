원문 : https://hyperledger-fabric.readthedocs.io/en/latest/txflow.html

## Transaction Flow
이 문서는 표준자산 교환동안 벌어지는 거래방법의 요약이다. 무를 사고 파는 두 클라이언트(A, B)에 대한 이야기다. 그들은 각각 거래내용을 보낼 수 있는 네트웍상의 peer 와 원장을 가진다.
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf1.png)

### 전제
이 흐름은 채널이 설정되어 있고, 작동중이라고 전제한다. 어플리케이션 사용자는 조직의 CA(인증기관)에 등록 및 권한 부여를 받았으며, 네트웍인증을 위한 필수 암호화 자료를 받았다.

chaincode(무 시장의 초기 상태를 나타내는 키/값 쌍을 포함)는 peer에 설치되고 채널에 인스턴스화된다. chaincode는 거래방식과 규칙과 합의된 무 가격을 포함한다. endorsement policy은 이 chaincode 에 대해, ```peer A```와 ```peer B``` 둘다 어떤 거래도 보증해야 한다고 설정되어 있다.
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf2.png)

### 1. Client A 가 트랙젝션 초기화하기
무슨일이 이루어 지는가? - Client A 는 무의 구입요청을 한다. 요청은 ClentA와 ClientB를 각각 대표하는 peerA 와 peerB 를 대상으로 한다. endorsement policy는 두 peer 의 어떤 거래도 보증해야 하기 때문에, 요청은 ```peerA``` 와 ```peerB```로 간다.

다음으로, 거래제안이 구성된다. 지원되는 SDK(Node, Java, Python)을 사용하는 어플리케이션은 사용가능한 API 중 하나는 활용하여 트랜잭션 제안을 생성한다. 제안은 자료가 원장을 읽고 쓸 수 있는 체인 함수를 호출하는 요청이다.(예를 들면, 자산에 새로운 key-value 쓰기). SDK 는 심을 사용하여 거래제안을 제대로 설계된 형식(gRPC의 프로토콜 버퍼)으로 패키징하고 사용자의 암호화 인증정보를 가져와 이 거래제안에 고유한 서명을 생성한다. 
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf3.png)

### 2. 승인받은 peer 의 서명확인 & 거래 이행
승인받은 peer의 (1)거래제안이 제대로 되었는지 확인하고, (2) 과거에 이미 제출되지 않았음(재생 공격 방지), (3)서명은 유효함(MSP 사용), 그리고 (4)제출자(예를 들면 Client A)는 채널에서 요청받은 동작을 수행하도록 적절하게 권한을 받았다.(즉, 각 승인받은 peer 는 제출자가 체널의 기록정책을 만족하도록 보장한다.) 승인받은 peer는 거래제안의 입력값을 체인코드 함수 호출의 변수로 받아들인다. 그러면 체인코드는 응답값, read set, write set을 포함한 거래결과를 포함한 현재 상태 데이터베이스에 대해 실행한다. 이 지점에서는 원장에 대한 업데이트가 없다. 이런 값 세트는 승인받은 peer의 서명과 함께 "제안의 응답" 으로 어플리케이션의 소모를 위한 페이로드를 분석하는 SDK에 반환된다.

{
MSP는 그들이 client에게 도달하는 거래요청을 확인하고 거래결과(승인)에 서명한느 것을 허락하는 peer 요소이다. 쓰기 정책은 채널생성시점에 정의되고, 그 채널에 트랜잭션을 제출할 권한이 있는 사용자를 결정한다.
}
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf4.png)

### 3. 제안의 결과는 검증된다.
어플리케이션은 승인된 peer 서명을 검증하고 제안응답이 동일한지를 결정하기 위해 제안응답을 비교한다. 체인코드가 원장만을 조회한다면, 어플리케이션은 조회결과를 검증하고 전형적으로 Ordering Service에 거래를 제출하지 않을 것이다. Client 어플리케이션이 원장을 갱신하기 위해 Ordering Service에 거래를 제출하려고 한다면, 어플리케이션은 제출전에 특정 승인정책이 만족스러운지를 결정한다.(즉, peerA와 peerB 둘다 승인한것이다.) 구조는 어플리케이션이 응답을 검증하지 않기로 선택하거나 미검증 거래를 추진하더라도, 승인정책이 여전히 peer들에 의해 시행되고 제출검증단계에서 유지되도록 한다.
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf5.png)

### 4. Client 는 거래로 승인들을 모은다.
어플리케이션은 거래 제안을 "broadcast" 하고 "transaction message" 안에서 Ordering Service에 대해 응답한다. 거래는 읽기/쓰기 집합과, 승인된 peer 서명과 Channel ID를 포함할 것이다. Ordering Service는 그 거래를 수행하기 위해 거래의 모든 내용을 검사할 필요는 없고, 네트워크상의 모든 체널에서 거래를 수신하고, 시간순서대로 주문하고, 채널당 거래 블록을 생성하기만 하면 된다.
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf6.png)

### 5. 거래는 검증되고 커밋됨
거래블록은 채널상에서 모든 peer로 "전달"된다. 그 블록안의 거래는 승인정책이 수행되고 있음을 보장하기 위해 검증되고, 거래 실행에 의해 읽기 세트가 생성된 이후 읽기 센트 변수에 대한 원장 상태가 변하지 않도록 한다. 블랙안의 거래는 유효하거나 유효하지 않은 것으로 태그가 지정된다.
![](https://github.com/aimmvp/BlockChain/blob/master/BCEdu/img/tf7.png)

### 6. 장부 갱신
각 peer는 블록을 채널의 체인에 추가하고, 각 유효 거래의 쓰기세트가 현재 상태 데이터베이스에 커밋된다. 거래(호출)가 체인에 변경없이 추가되었음을 client어플리케이션에 알리고 거래가 검증되었거나 무효화되었는지를 알리는 이벤트가 발생한다.









