# Network

## 1. Network Layer

1. AS : autonomous system (라우터들의 집합을 하나의 자치 구역처럼)
   1. AS는 식별하기 위한 번호 존재
   2. intra-AS protocol : link-state, distance-vector routing 
   3. inter-AS protocol : hierarchical routing (BGP: Border Gateway Protocol)
2. 네트워크 간 관계
   1. Customer-Provider
   2. Peer



## 2. Link Layer

### 1. Link Layer

1. link의 다음 node까지 안전하게 datagram을 전송: frame 단위
2. (핵심) collision detection, multiple access (MA; 여러 node가 한꺼번에 datagram 전송), broadcast
3. NIC (network interface card)에 구현
4. MAC protocol



### 2. MAC (Medium Access Protocol)

1. 충돌이 나지 않으면서 매체에 접근하는 프로토콜
2. channel partitioning
   1. 사용자가 많을 때 유리 + 충돌 X
   2. 사용되지 않는 채널이 있으면 낭비
   3. TDMA (Time Division Multiple Access)
   4. FDMA (Frequency Division Multiple Access)
3. random access
   1. 사용자가 적을 때 유리 
   2. 충돌 발생 + 원하는 때 바로 접근 가능
   3. CSMA (Carrier Sense Multiple Access) : 전송 전에 다른 channel에서 전송 중인지 listen, 이후 없으면 전송
      1. 듣더라도 propogation delay로 인해 충돌 발생 가능
      2. 충돌이 발생하더라도 계속 진행
   4. CSMA/CD (+ Collision Detection)
      1. 그냥 CSMA와는 달리 충돌 발생하면 바로 전송 중지
      2. 이후 random timer가 timeover되면 재전송
         1. 이때 binary (exponential) backoff: timer 시간이 적은 수 부터 충돌 시마다 두 배로 증가
4. taking turns
   1. Polling: master node가 존재해 access 중재 (decentralized x) + master에 문제 생기면 연관 노드에 모두 문제가 생길 가능성 존재
   2. token passing: 토큰이 있으면 access 가능, 채널을 사용할 일이 없으면 다른 채널에 토큰을 넘긴다.
      
5. Ethernet : MAC 프토토콜 중 지배적인 유선 프로토콜
   1. header
      1. MAC source Address
      2. MAC dest Address
      3. type
   2. ack가 없기 때문에 collision detection이 중요
   3. switch가 broadcast가 기본인 link layer cable을 충돌이 나지 않도록 분리하고, 양방향으로 오갈 수 있도록 돕는다.
   4. IP 주소로 목적지의 MAC dest address를 알아내는 ARP (Address Resolution Protocol) 
      1. switch에 forwarding table 존재
      2. 이 forwarding table은 **self-learning**
      3. forwarding table에 MAC dest address가 없다면 **flood**
         1. IP 패킷이 아닌 ARP query를 담아 전송
         2. flood로 정보가 쌓이면 selective하게 전송
   5.  switch를 계층적으로 연결해 host 확장 가능
   6. switch와 router 차이점
      1. router : network layer
         1. IP 주소 + 라우팅 알고리즘
      2. switch : link layer
         1. MAC 주소 + flood + self-learning



### 3. Wireless & Mobile network

1. wireless : 무선
   mobility : AP나 네트워크가 바뀌는 것
2. 802.11 Wifi
   1. AP : access point; 유선과 무선 연결지점
      BSS (basic service set) : 하나의 AP가 하나의 BSS 구성
   2. AP는 지속적으로 beacon frame을 보내 자신의 이름과 mac address, 신호 세기를 알린다. host에서는 이 정보를 보고 연결
   3. 유선과 달리 외부로부터 보호 받지 못하고 간섭이 심하다 → Hidden Terminal Problem (서로 carrier sense를 제대로 하지 못하는 구간 발생)
   4. collision detection이 불가능 → Ack 도입: **CSMA/CA** (collision avoidance)
   5. 일정 시간 동안 carrier sense를 해도 다른 채널에서 들려오지 않으면 전송 → 받은 쪽에서도 random한 시간이 지나면(충돌 방지) ack 보낸다. → sender는 ack가 와야만 다음 frame 전송
   6. CSMA/CA + **RTS/CTS**
      1. CSMA에서 carrier sense 이후 바로 보내는 것이 아니라, 전송을 시작한다는 정보를 담은 작은 데이터 조각인 RTS를 보낸다. (충돌시 발생하는 피해 최소화가 목적)
      2. 이를 AP에서 제대로 받으면 응답으로 CTS를 보낸다.
      3. RTS와 CTS는 (얼마나 채널을 쓸지 정보와 함께) broadcast되기 때문에, 다른 host에서는 그동안 전송을 중단한다.
3. wifi는 header에 4개의 주소가 존재
   1. AP로 가기 위한 **MAC dest. address, MAC source address**
   2. AP에서 router로 가기 위한 **MAC Addr.**
   3. ad hoc mode에서만 사용되는 주소