# ✨ week2_network_questions ✨

**✏ Flow control과 congestion control의 차이점에 대해서 설명해주세요.** 

- Flow control의 경우에는 데이터를 전송하는 sender가 receiver의 buffer 사이즈를 고려하여 전송하여 overflow를 방지하는 것을 의미합니다. congestion control은 네트워크에서 발생하는 overflow를 방지하는 것을 의미합니다.



**✏ IPv4는 몇 bit의 주소체계인가**

- 32bit



**✏ 라우팅 알고리즘 중에서 이웃 라우터의 정보만 아는 경우 사용하는 알고리즘은?**

- distance vector algorithm



**✏ Three way handshake 과정에 대해서 설명해주세요.**

- 1. 클라이언트가 서버에게 요청을 보내기 전에 TCP SYN 세그먼트를 보냅니다. 이 때, 보내는 데이터의 seq #를 설정합니다. 
  2. 이를 받으면 서버는 SYNACK 세그먼트를 전송합니다. 이 때, 서버는 buffer를 할당하고 seq #를 설정합니다.
  3. 그리고 클라이언트가 SYNACK를 받으면 ACK를 응답하며 데이터를 포함하여 전송합니다.



**✏ 3way handshake를 하는 이유는?**

- 2way를 하면 서버가 클라이언트의 응답을 모르기 때문



**✏ DHCP (Dynamic Host Configuration Protocol)에 대해 간략하게 말해주세요**

- 인터넷에 접속할 때마다 자동으로 IP 주소를 할당해 줌



**✏  Longest Prefix Match Forwarding에 대해서 설명해주세요.**

- forwarding table에서 일치하는 IP 주소 중 가장 prefix가 긴 네트워크를 선택한다는 것이다. 예를 들어 123.123.123의 아이피 주소가 destination일 때, 123.0.0/8과 123.123.0/16이 있다면 둘중 후자의 링크를 선택하여 packet을 전송한다.



**✏ NAT의 문제점 **

- 1. gateway에서 주소를 변환하는 과정에서 IP는 그렇다쳐도 port 번호도 변경되고, port 번호로 각 host 구별
  2. 원래는 network layer인 IP 주소로 구별해야 하는데, 상위 layer인 Transport layer를 간섭
  3. host가 동일한 gateway IP로 변환되기 때문에, server를 열기 어려움
  4. IPv4의 주소 고갈 문제를 근본적으로 해결 못한다



**✏ Routing Algorithms에서 link-state는 어떤 알고리즘과 유사한가요?**

- dijkstra's algorithm



**✏ TCP연결을 해제할때 요청되는 플래그 2개를 말해주세요.**

- FIN, ACK



**✏ ICMP는 무엇을 하기 위해 있는 프로토콜인가?**

- 네트워크 내부에서 router와 host 간 network level의 정보(unreachable host, network 등)을 교환하기 위한 프로토콜



**✏ subnets을 한마디로 정리하면?**

- 같은 subnet ID(prefix)를 가진 디바이스의 집합



**✏  Classful Addressing에 비해서 Classless Inter-Domain Routing(CIDR)의 장점에 대해서 설명해주세요.**

- Classful Addressing의 경우에는 prefix로 할당되는 사이즈가 8비트 단위로 정해져있기 때문에 IP 주소를 분배하는 것이 쉽지 않다. 반면 CIDR의 경우에는 8비트 단위로 정해져있지 않기 때문에 IP 분배에 있어서 보다 효율적이다.



**✏ IP fragmentation에 대해서 설명해주세요 **

- 패킷을 전송하고자 하는데 패킷의 크기가 MTU(Maximum Transmission Unit)을 초과하면 한번에 전송할 수 없기 때문에 패킷을 작게 Fragmentation하는 것을 의미한다.



**✏  IP tunneling에 대해서 설명해주세요.** 

- IP tunneling 은 데이터그램 안에 데이터그램을 넣는 기술로서, 어떤 IP 주소를 향하는 데이터그램을 감싸 다른 IP 주소로 재지향할 수 있다.



**✏ IP datagram header에서 TTL이란 ? **

- Time to live: 패킷이 몇 hop을 거쳐서 살아남을 수 있는가 + 잘못된 routing algorithm으로 cycle 발생 방지

