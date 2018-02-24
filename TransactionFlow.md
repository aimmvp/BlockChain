원문 : https://hyperledger-fabric.readthedocs.io/en/release/txflow.html

## Transaction Flow

This document outlines the transactional mechanics that take place during a standard asset exchange. The scenario includes two clients, A and B, who are buying and selling radishes. They each have a peer on the network through which they send their transactions and interact with the ledger.

이 문서는 표준자산 교환동안 벌어지는 거래방법의 요약이다. 시나리오는 무를 사고 파는 두 클라이언트(A, B)에 대한 이야기다. 그들은 각각 그들의 거래와 원장과의 상호작용을 통한 네트웍에 peer 를 가진다.
![](https://github.com/aimmvp/BlockChain/blob/master/tf1.png)

### 전제
The application user has registered and enrolled with the organization’s certificate authority (CA) and received back necessary cryptographic material, which is used to authenticate to the network.

이 흐름은 채널이 설정되어 있고, 작동중이라고 전제한다. 어플리케이션 사용자는 조직의 CA에 등록하고 enroll했으며, 네트웍에 인증하는데 사용되기 위해 필요한 암호화 자료를 받환 받았다. 

The chaincode (containing a set of key value pairs representing the initial state of the radish market) is installed on the peers and instantiated on the channel. The chaincode contains logic defining a set of transaction instructions and the agreed upon price for a radish. An endorsement policy has also been set for this chaincode, stating that both peerA and peerB must endorse any transaction.

체인코드(무 시장의 초기 상태를 나타내는 키/값 쌍을 포함)는 피어에 설치되고 채널에 인스턴스화된다. 체인코드는 거래지침세트와 무의 합의가격을 정의하는 논리를 포함한다. 이 체인코드에 대한 승인정책도 있고, peerA 와 peerB 모두 어떤 거래든지 보증해야 함을 의미한다.
![](https://github.com/aimmvp/BlockChain/blob/master/tf2.png)
