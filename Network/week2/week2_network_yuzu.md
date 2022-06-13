| Week 2 | Lecture 7~12 |
| :----: | :----------: |

<br/>

<br/>

# 전송계층

---

<br/>

### TCP Flow Control

- 받는 사람이 받을 수 있는 속도만큼 보내주는 것

<br/>

### TCP 3-way handshake

- 동작과정
  - Client에서 Server로 SYN과 seq num 보냄(SYN이 1이라면 tcp연결하겠다는 뜻)
  - Server에서 SYN(똑같이 1), seq num 보냄, ACK(1)와 ACK seq num(Client seq num + 1)도 함께 보냄
  - Client에서 ACK, ACK seq num(Server seq num + 1)를 보냄

<br/>

### Closing TCP Connection

- Client가 연결을 닫으려고 할 때 FIN segment 보냄
- Server는 Client로 ACK 승인 segment를 보냄
- Server는 ACK를 보내고 일정시간 이후에 Client에 FIN segment 보냄
- Client는 다시 ACK를 보내고 Connection closed가 됨

<br/>

### TCP congestion control

- Slow Start
- Additive increase
- Multiplicative decrease:

<br/>

### TCP Tahoe vs TCP Reno

- Tahoe는 무조건 1MSS로 돌아오므로 다시 원상태의 속도로 돌아올 때 까지 시간이 많이 걸림
  - 이를 개선한 것이 Reno

- Reno
  - 패킷유실 판별 조건에 따라 다름
  - 3 dup ACK: Congestion Window size를 현재의 절반으로 내림
  - Timeout: Congestion Window size를 1MSS로 내림
  - Threshold 값은 직전에 패킷 유실이 발생한 window size의 절반


<br/>

<br/>

# 네트워크계층

---

<br/>

### IP datagram format

<br/>

### IP Address (IPv4)

- 32비트를 8비트 단위로 점찍어 표기
- IP주소를 아무렇게나 배정하면 forwarding table이 너무 커지는 문제가 있음
- Subnet Mask IP주소 = Network Id + Host Id

<br/>

### NAT (network address translation)

- 여러대의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위해 사용

- IP address space depletion

- NAT is widely deployed, much more than IPv6

<br/>

### DHCP (Dynamic Host Configuration Protocol)

- 인터넷에 접속할 때마다 자동으로 IP 주소 할당

<br/>

### Routing Algorithms

목적지까지의 최소 cost 경로를 찾음

- link-state
  - 다익스트라 알고리즘
  - 전체 노드 연결정보를 토대로 하나의 노드에서 다른 노드까지의 최소 비용을 계산
- distance-vector
  - 벨만포드 알고리즘
  - 전체 노드 연결정보는 알지 못하지만 본인과 이웃 라우터의 갱신정보를 가지고 최소비용을 계산



<br/>

<br/>