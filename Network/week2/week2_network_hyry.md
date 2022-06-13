# Network 2

## 1. TCP

### 1. Flow Control

1. receiver가 받아들일 수 있는 만큼만 전송
2. receive buffer에 빈 공간이 없는 경우
   1. 주기적으로 데이터 부분은 빈 segment를 보낸다
   2. 돌아오는 ack를 통해 receive 공간이 비었는지 체크



### 2. Connection Management

1. 3-way handshake
   1. TCP의 sender와 receiver는 데이터를 주고 받기 전에 connection을 형성한다
   1. 이 작업을 통해 server와 client host는 sequence number를 초기화하고, send buffer와 receive buffer를 할당한다.
   1. SYN : client가 server에 connection 요청 + sequence number 결정
   1. SYNACK : server가 clinet에 connection 수락 알림 + buffer 할당
   1. ACK : SYNACK에 대한 응답 + 본격적 request


2. 연결 끊기
   1. client가 server에 FIN 필드를 1로 전송
   2. server는 ACK와 FIN을 전송
   3. client가 다시 받은 FIN에 ACK 보내고 일정시간 대기(ACK가 제대로 전달되었는지 대기) 후에 종료



### 3. Congestion Control

1. 네트워크 수용량에 따라 보내는 양 조절
2. Network-assisted congestion control
   1. router가 네트워크 상황을 알려준다
   2. 구현되지 않은 상태 (router의 역할에 부담)
3. End-end congestion control
   1. 각 끝에서 내부 네트워크 상황을 알아서 유추해서 동작
   2. TCP segment와 Ack가 오가는 것으로 추정
   3. 3 phase
      1. slow start: 천천히 0부터 시작 + exponential 증가
      2. additive increase : 한계점에 다다른 거 같으면 linear 증가
      3. Multiplicative decrease : packet loss가 발생하면 1/2 만큼 줄인 후 다시 linear 증가
      4. 이때 단위는 MMS (maximum segment size)
   4. loss 판단 여부
      1. timeout
      2. 3 duplicate acks
   5. Tahoe
      1. loss가 발생하면 처음부터 시작
      2. threshold도 loss 발생 지점의 1/2로 낮춤
   6. Reno
      1. timeout과 3 duplicate acks 구별
      2. timeout인 경우 tahoe와 동일
      3. 3 duplicate acks 인 경우 네트워크 자체에는 문제가 없다고 판단, 1/2 지점부터 linear하게 시작







## 2. Network Layer

### 1. Network Layer

1. 전송 단위 : packet = datagram
2. 전송이 어떤 경로를 통해 이뤄지는지
3. IP, router, routing algorithm



### 2. Network Layer 두 가지 기능

1. forwarding
   1. forwarding table을 보고 packet을 보낼 다음 router 결정
   2. forwarding table은 address range 단위로 작성
   3. Longest prefix matching : 가장 길게 일치하는 주소로 보낸다
2. routing
   1. source부터 destination까지 전체 경로 설정
   2. routing algorithm
   3. 이를 위해 router는 data plane(사용자 정보)만이 아니라 control plane (router끼리 교환하는 정보)도 존재



### 3. IP Datagram Header

1. ver : Ip 프로토콜 버전
2. fragment flags, fragment offset : 패킷 fragment 여부와 offset
   1. network link가 받아들일 수 있는 패킷 최대 크기보다(MTU), 보내는 패킷 크기가 큰 경우 패킷을 분리하게 되는 것 = fragment
3. TTL : time to live : 데이터그램이 몇 홉까지 살아남을 수 있는지 + 알고리즘에서 cycle 방지
4. checksum : 에러 방지
5. 32 bit source IP address
6. 32 bit destination IP address



### 4. IPv4

1. 31 bits 주소 체계
2. 8bit로 4개씩 구분, 10진수로 표현
3. interface를 구별
4. 앞 부분은 네트워크 subnet을 나타내는 prefix, 뒷 부분은 host
   1. subnet : 중간에 router를 거치지 않고 갈 수 있는 주소 집합 (physically)
5. subnet mask로 어디까지가 subnet 주소인지 표현
6. 계층화된 주소 체계
   1. 새로운 host 추가 용이
   2. 과거 classful address  : 주소 분배의 형평성 문제. 선점한 곳에서는 너무 많은 host 주소 보유, 후발 주자는 host 주소 부족
   3. 현재 CIDR (Classless Inter-Domain Routing)
      1. network prefix로 어떤 bit 수도 가능
      2. 유연한 네트워크 크기 
      3. a.b.c.d/x 로 표현 x는 몇 bit까지가 subnet 부분인지 알려준다.



### 5. NAT (Network Address Translation)

1. IPv4의 주소 고갈문제를 임시로 해결하기 위해 나온 방안
   1. 주소 공간이 외부 네트워크 체계와 분리된 독립적인 공간 형성
   2. 속한 host에게 내부적으로는 고유한 주소 체계를 부여하지만, 이는 이 공간 내부에서만 해당되는 얘기로 외부로 나갈 때는 gateway의 IP로 변경 + 각 host는 port 번호로 구별
2. IPv6로 쉽게 바꾸지 못하는 이유
   1. router를 보유하고 있는 소유주가 모두 다르고 비용의 문제
   2. NAT의 존재 자체
3. NAT의 문제점
   1. gateway에서 주소를 변환하는 과정에서 IP는 그렇다쳐도 port 번호도 변경되고, port 번호로 각 host 구별 
   2. 원래는 network layer인 IP 주소로 구별해야 하는데, 상위 layer인 Transport layer를 간섭
   3. host가 동일한 gateway IP로 변환되기 때문에, server를 열기 어려움



### 6. DHCP (Dynamic Host Configuration Protocol)

1. Application layer protocol
2. client(port 68)와 server(port 67)간 이루어지는 교신. 이를 통해 client는 server에 join할 때 server에게서 IP 주소를 배정 받는다.
3. DHCP 과정
   1. discover : 주변 server들에게 IP 주소를 달라고 broadcast 요청
   2. offer : server에서 discover 때 온 id로 누가 보낸 건지 구별하면서, discover에 broadcast 응답(client에게 IP 주소가 배정되지 않았기 때문). 여러 server에서 제안
   3. request : server 중 원하는 server 선택해서 다시 broadcast 요청(+ 다른 server에게도 수락한 제안이 있음을 알림)
   4. ack : server에서 IP 주소, subnet mask, gateway IP 주소, DNS server IP 주소 제공



### 7. ICMP (Internet Control Message Protocol)

1. 네트워크 내부에서 router나 host 간 교신을 위한 프로토콜
2. 사용자 정보가 아닌, router의 고장, unreachable, unknown인 router, TTL의 expire 등 router의 정보가 IP 패킷에 담긴다
3. Traceroute : 각 router까지 hop 단위로 TTL을 설정해 어느 hop에 어떤 router가 위치해있는지, 어떤 경로로 패킷이 가는지 알아내는 utility



### 8. IPv6 & Tunneling

1. 128 bits 주소 체계
2. IPv4에서 IPv6로 옮기는 과도기에는 router에 두 프로토콜 모두 해석 가능한 router의 존재가 필요 (Tunneling)



### 9. Routing Algorithm

1. link-state
   1. 모든 link와 router의 상태를 알면서 짜는 알고리즘
   2. Dijkstra
2. distance vector
   1. Bellman Ford