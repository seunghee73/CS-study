# ✨ week3_network_questions ✨

**✏ 이더넷 header에서 type은 어떤 역할?**

- IP 프로토콜 정의: IPv4, IPv6

<br>

**✏ Link-State & Distance-Vector 의 공통점**

- 시작점부터 목적지까지 최소 경로 cost를 구하는 것을 목적으로 가진다

<br>

**✏ Channel Partitioning & Random Access  차이점**

- channel partitioning 프로토콜은 사용자가 많으면 효율성이 좋지만 사용자가 적을 경우 낭비되는 자원 발생
- random acces 프로토콜은 사용자가 적으면 효율성이 좋지만 사용자가 많을 경우 delay 시간이 길어짐

<br>

**✏  무선 센서 네트워크에서 CSMA/CD를 사용할 수 없게된 이유에 대해서 두 가지만 설명해주세요.**

- Hidden terminal problem
- Signal attenuation

<br>

**✏  CSMA/CD에서 Collision이 발생했을 때 감지하기 위한 방안?**

- minimun frame size 데이터 전송 강제 (64byte)

<br>

**✏ AS의 경로 설정 정책 **

- Customizer > Peer > Provider

<br>

**✏ Router 와 Switch의 차이점 **

- router : network layer, routing algorithm + IP address
- switch : link layer, mac address, flood, self-learning

<br>

**✏ Bellman-Ford 알고리즘에서 경로에 __값을 주는 방법을 poisoned reverse 방법이라고 한다. **

- `∞`

<br>

**✏ Switch에서 Flood란? **

- 수신 포트 이외의 모든 포트에서 데이터를 송신
- broadcast

<br>

**✏ Network relation ship: Peering?**

- 비용 지불 없이 서로 트래픽을 주고 받을 수 있다.

<br>

**✏ CSMA와 CSMA/CD의 차이점**

- collision detection시 CSMA는 멈추지 않지만, CSMA/CD는 멈춘 후에 다시 랜덤한 시간이 지난 후 재전송

<br>

**✏ CSMA/CA와 CSMA/CD의 차이점**

- CSMA/CD는 충돌을 감지하는 방식이고 CSMA/CA는 충돌을 피하는 방법이다. 
- CSMA/CD의 경우에는 충돌을 감지한 경우, 우선 전송을 멈춘다. 그리고 binary backoff를 진행하고 다시 전송을 시도한다. 
- CSMA/CA의 경우에는 데이터를 전송하기 전에 receiver에 RTS를 보낸다. 이 때. 충돌이 발생되면 binary backoff를 진행한다. 그리고 다시 RTS를 보낸다. RTS를 수신한 receiver가 데이터를 수신할 수 있는 상황이면 CTS를 보내 데이터를 수신중이라는 것을 이웃에게 알린다. CTS를 받은 RTS를 보낸 sender는 데이터를 전송한다.

<br>

**✏ IP 주소를 이용하여 MAC 주소를 찾기 위한 프로토콜은?**

- ARP (Address Resolution Protocol)

<br>

**✏ wireless & mobility란?**

- wireless : 무선
- mobility : 네트워크 혹은 AP가 변경되는 것

<br>

**✏ Link Layer의 목표**

- 충돌 방지 (collision detection, avoidance, recovery)

<br>

**✏Distance vector algorithm에 발생할 수 있는 문제와 이를 해결하기 위한 방법에 대해서 설명해주세요.**

- count-to-infinity가 발생할 수 있다. 이는 라우팅 루프가 도는 현상으로 벨만 포드 알고리즘에 사이클이 존재하는 경우 발생할 수 있는 문제이다. 이를 해결하기 위한 방법으로는 poison reverse가 있다. A에서 B 노드를 갈 때 C 노드를 거쳐간다면 C 노드에게 B로 가는 경로의 cost가 무한대라고 하는 방법이다.

<br>

**✏ Link-state 알고리즘으로 구현한 프로토콜**

- OSPF 프로토콜

<br>

**✏ MAC protocol 중 channel partitioning 방법에 대해서 두 가지 이상 설명해주세요.**

- TDMA, FDMA 방식이 있다. TDMA는 시간을 분할하는 방법이고 FDMA는 주파수를 분할하는 방법으로 자원이 낭비된다는 문제가 있다.

<br>

**✏ taking turns에 속하는 Mac Protocol 두 가지**

- polling & token passing

<br>

**✏Distance vector algorithm에 대해서 설명해주세요.**

- 이웃의 cost에 대해 예측한 정보를 토대로 거리를 계산하는 알고리즘으로 벨만 포드 알고리즘을 이용하여 cost가 최소가 되는 경로를 도출한다.

<br>

**✏CSMA에 대해서 설명해주세요.**

- CSMA는 데이터를 전송하기 전에 우선 destination으로 보내지는 데이터가 있는지 확인(listen)하는 방법이다. 없다고 판단된 경우 데이터를 전송하지만 propagation delay에 의해 여전히 충돌은 일어날 수 있다.

<br>