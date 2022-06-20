### Distance vector algorithm

- z까지의 최솟값을 알고 싶을 때
- link state 알고리즘과 달리 모든 라우터 정보가 아니라 인접한 라우터의 정보만 앎
- 이웃된 라우터에게 distance의 array (vector)를 넘겨줌

- 각 노드는 distance vector 정보를 직접적으로 연결된 이웃에게 넘겨주고, 업데이트되면 다시 연결된 이웃에게 전달하는 것이 계속 반복됨

- 어느 순간 업데이트 되지 않는 상황이 됨

- iterative, asynchronous: each local interation caused by
  - local link cost change
  - DV update message from neighbor
- distributed
  - each node notifies neighbors only when its DV changes
    - neighbors then notify their neighbors if necessary

- each node: 

  - Wait for (change in local link cost or msg from neighbor)

  ->

  - recompute estimates

  ->

  - if DV to any dest has changed, notify neighbors

- **Count-to-infinity 문제**
  - 부분적인 정보로만 계산을 하므로 발생
  - poisoned reverse 기법 사용 : 결정적인 역할을 한 이웃에 대해서만 무한대로 넘겨줌





### Comparison of LS and DV algorithms

시작점부터 목적지까지 최소 경로 cost를 구하는 것이 둘 다의 목적

- message complexity 메시지 복잡성
  - LS: with n nodes, E links, O(nE) msgs sent
  - DV: exchange between neighbors only
    - convergence time varies
- speed of convergence
  - LS: O(n^2) algorithm requires O(nE) msgs
    - may have oscillations
  - DV: convergence time varies
    - may be routing loops
    - count-to-infinity problem
- robustness: what happens if router malfunctions?
  - LS
    - node can advertise incorrect link cost
    - each node coputes only its own table
  - DV
    - DV node can advertise inccorect path cost
    - each node's table used by others
      - error propagate thru network



### AS: Autonomous System

### AS Numbers (ASNs)

ASNs are 16 bit values. 

64512 through 65535 are "private"

Currently over 11,000 in use



-> As 사이의 위상이 다름



### Customer and providers

- 인터넷 연결을 제공하는 as: provider
- 사용하는 as : customer

- Customer pays provider for access to the Internet (갑을관계)



### The "Peering" Relationship

- 비용지불 없이 서로 트래픽을 왕래힘



### Implementing Inter-Network Relationships with BGP

BGP : Border Gateway Protocol (AS 경계에 있는 라우터)



Emforce order of route preference

- Customer > Peer > provider





- understand priciples behind link layer services:
  - error detection, correction
  - sharing a broadcast channel: multiple access 
  - link layer addressing
  - local area networks: 이더넷, VLANs
- instantiation, implementation of various link layer technologies

- 충돌이 발생하지 않도록 하는 것(또는 충돌에 대처)이 링크계층의 일. 공유하는 매체를 통해 자원을 전달하기 때문에 충돌이 발생하지 않아야 전달됨



### Link layer:

- terminology
  - hosts and routers: nodes
  - communication channels that connect adjacent nodes along communication path: links
    - wired links
    - wireless links
    - LANs
  - layer-2 packet: frame, encapsulate datagram
- 네트워크 인터페이스 card (NIC)에 있음



### Multiple access links, protocols

- Two types of "links":
  - point-to-point
    - PPP for dial-up access
    - point-to-point link between Ethernet switch, host
  - broadcast(shared wire or medium)
    - 두 명 이상 얘기하면 섞여서 문제 발생
- 매체 접근 시 제어해서 충돌 해결: Medium Access Control (Mac Protocol) (ex 와이파이)



### 이상적인 mac 프로토콜

1. 하나의 노드가 데이터 전송을 원할 경우 R에 보낼 수 있음
2. M개의 노드가 같이 사용을 원할 경우 평균적인 R/M에 각각 보냄
3. 중앙 서버가 아닌 분산 처리 
4. 단순



### MAC protocols: taxonomy

- three broad classes:
  - channel partitioning
  - random access
  - taking turns



### channel partitioning MAC Protocols: TDMA

- 여러 사람이 접근할 수 있게 만든다

- 각 사람 별로 전송할 수 있는 타임 슬롯을 나눠서 자기 차례에 왔을 때만 사용가능하게 함

- 자원이 낭비되는 문제가 발생

### channel partitioning MAC Protocols: FDMA

- 도메인만 frequency라는 차이



### Random access protocols

- 자기가 보내고 싶을 때 보내는 것
- 충돌이 발생할 수 있음 - > 어떻게 방지하고, 어떻게 처리할지가 중요함

- ALOHA : 최초의 프로토콜



### CSMA (carrier sense multiple access)

- listen before transmit: 말하기 전에 듣는다





### Ethernet

- dominant wired LAN technology
  - cheap $20 for NIC
  - first widely used LAN technology
  - simpler, cheaper than token LANs and ATM
  - kept up with speed race: 10 Mbps - 10 Gbps



### Ethernet: Physical topology

- bus: popular through mid 90s
  - all nodes in same collision domain (can collide with each other)
- star: prevails today
  - active switch in center
  - each "spoke" runs a (separate) Ethernet protocol (nodes do not collide with each other)



### Ethernet frame structure

