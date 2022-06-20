# Chapter 7. 전송 계층 3

### TCP Flow Control

- sender가 receiver가 받을 수 있는 속도만큼 보냄
- A - send buf/recv buf, B - send buf/recv buf 



### TCP Connection Management

- Recall 

  - TCP sender, receiver establish "connection" before exchanging data segments

- Three way handshake

  - client host sends TCP SYN segment to server 클라이언트가 서버에게 세그먼트 전송

    - specifies initial seq #
    - no data 데이터 없음

  - server host receives SYN, replies with SYNACK segment 서버가 응답 세그먼트 전송

    - server allocates buffers
    - specifies server initial seq. #

  - client receives SYNACK, replies with ACK segment, which may contain data 클라이언트가 ACK 세그먼트 전송 (데이터를 포함)

    

### TCP 3-way handshake



### Closing TCP Connection

client closes socket: clientSocket.close()

- client end system sends TCP FIN control segment to server
- server receives FIN, replies with ACK. Closes connection, sends FIN
- client receives FIN, replies with ACK
  - Enters "timed wait" - will respond with ACK to received FINs
- server, receives ACK. Connection closed



### Approaches towards congestion control

네트워크 상황에 맞춰 전송 (네트워크 상황이 안좋아지면 보내는 속도를 줄이고, 괜찮아지면 속도를 높인다. 남을 위한 것 뿐만 아니라 나자신을 위한 것(안 그러면 상황이 더 악화됨))

- End-end congestion control

  - no explicit feedback from network
  - Congestion inferred from end-system observed loss, delay
  - Approach taken by TCP

- Network-assisted congestion control

  - routers provide feedback to end systems

    - single bit indicating congestion (SNA, DECbit, TCP/IP ECN, ATM)

    - explicit rate sender should send at

      

---

# Chapter 8. 전송 계층 4

### TCP Congestion Control

네트워크로부터 직접적인 피드백은 없지만 상대방 리시버로부터 반응이 있기 때문에 그에 따라 조절

- 3 main phases
  1. Slow Start: Do not know bottleneck bandwidth. So start from zero and quickly ramp up 처음에는 네트워크의 상황을 잘 모르기 때문에 아주 조금씩 보내다가 점점 크게 증가
  2. Additive increase: Hey, we are getting close to capacity. Let's be conservative and increase slow 일정 부분을 지나면 증가폭을 줄임
  3. Multiplicative decrease: Oops! Packet drop. Start over from slow start (from scratch) Hmm! Many ACKs coming, start midway 데이터 유실이 발생하면 데이터전송량을 반으로 줄이고 다시 1번부터 시작함

- MSS (Maximum Segment Size)



### TCP Congestion Control: details

- sender limits transmission(전송 속도)

  LastByteSent-LastByteAcked <= CongWin

- 대략적으로,  rate = CongWin/RTT Bytes/sec



### Refinement

- TCP Tahoe
  - 80s
  - 처음에 1개만 보내고 윈도우 사이즈를 점점 늘림 (1씩 증가)
  - 패킷 유실(network congestion)이 되면 다시 처음부터 시작

- TCP Reno

  - 두번째 버전
  - 패킷 유실 탐지 : 타임아웃(패킷을 안 받음) / 3 duplicate ACK (패킷을 잘 받는데 해당 패킷만 문제) => 다른 상황이므로 다르게 대응해야 함

  - 3 duplicate ACK는 잠깐 윈도우사이즈를 절반으로 줄이고 시작
  - 타임아웃이이라면 다시 처음부터 시작

   

### TCP Fairness

Fairness goal: if K TCP sessions share same bottleneck link of bandwidth R, each should have average rate of R/K



### Why is TCP fair? (공평 분배)

- Two competing sessions
  - Additive increase gives slope of 1, as throughout increases
  - Multiplicative decrease decreases throughput proportionally



---

# Chapter 9. 네트워크 계층 1

전송단위: 패킷

### Network layer

- transport segment from sending to receiving host 

- on sending side encapsulates segments into datagrams
- on receiving side, delivers segments to transport layer



### Two key network-layer functions (router)

- forwarding: move packets from router's input to appropriate router output

  특정 목적지로 향하기 위해 어디로 내보내라

  들어온 패킷의 목적지 주소와 포워딩 테이블의 엔트리를 매칭시켜서 그 엔트리에 해당하는 링크로 전달

- Routing: determine route taken by packets from source to dest.

  포워딩 테이블을 만듦



### Longest prefix matching

When looking for forwarding table entry for given destination address, user longest address prefix that matches destination address



---

# Chapter 10. 네트워크 계층 2

### IP datagram format

- ver: IP 프로토콜 버전 (IPv4)

- length: 패킷 총 길이

- source IP address, destination IP address: 메세지를 생성해서 보내는 사람의 IP주소, 받는 사람의 IP 주소

- time to live: TTL. 처음에 센더가 20이라고 주고 전송하면 -19, -18... 0이 되면 패킷이 사라짐 (무한루프를 없애기 위해 사용)

- upper layer: TCP인지 UDP인지 명시해주기 위함 (리시버 측에서 사용함)

- IP 패킷의 헤더를 합해보면 20바이트 나옴 (40바이트 자체는 오버헤드다)



### IP Address (IPv4)

- A unique 32-bit number
- Identifies an interface (on a host, on a router)
- Repersented in dotted-quad notation
- 내적 인터페이스를 지칭하는 주소
- 255가 최대



IP주소는 어떤 식으로 배정되었는가?



### Grouping Related Hosts

- The internet an "inter-network"



### Scalability Challenge

- Suppose hosts had arbitrary addresses



IP주소는 계층화되어있음

### Hierarchical Addressing: IP Prefixes

- Network and host portions (left and right)



### IP address and 24-bit Subnet Mask

어디까지가 네트워크 ID인지 비트맵으로 표현한 것

IP 주소와 서브넷 마스크는 항상 같이 다님



### Scalability Improved

- Number releted hosts from a common subnet



### Easy to Add New Hosts

- No need to update the routers

같은 네트워크에 속한 호스트들은 같은 prefixe를 가지는 ip주소를 갖고 있음



### Classless Inter-Domain Routing (CIDR) 

클래스 없는 라우팅

- Use two 32-bit numbers to represent a network. Network number = IP address + Mask



### Separate Forwarding Entry Per Prefix

- Prefix-based forwarding
  - Map the destination address to matching prefix
  - Forward to the outgoing interface



### Longest Prefix Match Forwarding

- Destination-based forwarding
  - Packet has a destination address
  - Router identifies longest-matching prefix
  - Cute alogrithmic problem: very fast lookups



### ID addressing: CIDR

Classless InterDomain Routing

- Subnet portion of address of arbitrary length
- address format: a.b.c.d/x, where x is # bits in subnet portion of address



### Subnets

- 같은 prefix를 가진 디바이스의 집합
- 라우터를 거치지 않고 접근이 가능한 host들의 집합
- IP address
  - Subnet part - high order bits
  - host part - low order bits



### Network Address Translation (NAT)

- 주소공간 부족 문제를 해결함

- 내부에서는 유일한 ip주소를 사용 -> 외부로 나가면 안됨 (외부로 나갈 때는 NAT를 통해 ip주소를 바꿔줌)



### Principled Objections Against NAT

- Routers are not supposed to look at port #s
  - Network layer should care only about IP header
  - ... and not be looking at the port numbers at all
- NAT violates the end-to-end argument
  - Network nodes should not modify the packets
- IPv6 is a cleaner solution
  - Better to migrate than to limp along with a hack



---

# Chapter 11. 네트워크 계층 3

### Dynamic Host Configuration Protocol (DHCP)

- How does a host get IP address?
  - Hard-coded by system admin in a file
  - DHCP: Dynamic Host Configuration Protocol: dynamically get address from as server
    - "Plug-and-play"



### DHCP: Dynamic Host Configuration Protocol

고정 아이피가 아닌 유동적 아이피 부여

- 목표: allow host to dynamically obtain its IP address from network server when it joins network
  - Can renew its lease on address in use
  - allows reuses of addresses (only hold address while connected/"on")
  - support for mobile users who want to join network (more shortly)
- DHCP overview:
  - host broadcasts "DHCP discover" msg
  - DHCP server responds with "DHCP offer" msg
  - host request IP address: "DHCP request" msg
  - DHCP server sends address: "DHCP ack" msg



### DHCP client-server scenario



### IP fragmentation, reassembly

- Network links have MTU (max transfer size) - largest possible link-level frame
  - different link types, different MTUs
- large IP datagram divided ("fragmented") within net
  - One datagram becomes several datagrams
  - "reassembled" only at final destination
  - IP header bits used to identify, order related fragments



---

# Chapter 12. 네트워크 계층 4

### ICMP: internet control message protocol

- used by hosts & routers to communicate network-level information
  - error reporting: unreachable host, network, port, protocol
  - Echo request/reply (used by ping)
- Network-layer "above" IP:
  - ICMP msgs carried in IP datagrams
- ICMP message: type, code plus first 8 bytes of IP datagram causing error



### Traceroute and ICMP

- source sends series of UDP segments to dest
- When nth set of datagrams arrives to nth router
- When ICMP messages arrives, source records RTTs
- stopping criteria:
  - UDP segment eventually arrives at destination host
  - destination returns ICMP "port unreachable" message
  - source stops



### IPv6: motivation

- initial motivation: 32-bit address space soon to be completely allocated.



### Transition from IPv4 to IPv6

바로 변경되는 것이 아니라 과도기가 존재할 것.



#### Tunneling

두 가지 프로토콜을 동시에 이해해서 변환시켜주는 tunneling 작업이 필요할 것



### Graph abstraction

- routing algorithm: 목적지까지 최소 cost의 경로를 찾는 것이 목적



### Routing algorithm classfication

- link state 알고리즘
  - 브로드캐스트를 통해 모든 노드의 정보를 알고 있음
  - 다익스트라 알고리즘
- Distance vector 알고리즘
  - 나와 직접 연결된 노드와 메세지를 공유하고 최소 경로를 구함
  - 직관적이지 않음 (분산처리 시스템)