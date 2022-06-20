| Week 3 | Lecture 13~18 |
| :----: | :-----------: |

<br/>

<br/>

# 네트워크계층

---

<br/>

### Distance vector algorithm

- 벨만포드 알고리즘
- 전체 노드 연결정보는 알지 못하지만 본인과 이웃 라우터의 갱신정보를 가지고 최소비용을 계산

<br/>

### autonomous systems(AS)

- 각각의 네트워크에서는 각자 어떤 라우팅 알고리즘을 사용할 것인지 결정
- link state algorithm로 구현한 프로토콜: OSPF
- distance vector algorithm로 구현한 프로토콜: RIP
- 더 큰 범위의 네트워크 끼리의 라우팅 알고리즘: Inter-AS(BGP 프로토콜 사용)

<br/>

<br/>

# 링크계층

---

<br/>

### Multiple access links, protocols(다중 접근 프로토콜)

- point-to-point
- broadcast

<br/>

### An ideal MAC protocol

- desire

  - 한 노드가 전송을 하기 위해서는 R 사용

  - M개의 노드가 전송을 하기 위해서는 R/M 사용

  - 분산처리
  - 단순함

- channel partitioning

  - 시간을 쪼개서 각 사용자가 사용할 수 있게 함
  - 자원낭비의 문제

- random access

  - 보내고자 할 데이터가 있을 때 데이터를 보냄
  - 충돌문제
  - CSMA
    - listen before transmit
    - 전송하는 노드가 있을 경우 전송하지 않음, 전송하는 노드가 없으면 전송
    - 같이 전송하는 경우가 발생할 수 있음
  - CSMA/CD
    - CSMA collision을 해결하기 위해 나온 프로토콜
    - collision이 발생하면 즉시 전송을 멈춤(데이터를 보낸 이후)
    - 사용자가 몰릴 경우 전송속도가 느려질 수 있음

- taking turns

  - Token가지고 있는 host만이 전송 가능
  - 차례로 Token 넘김
  - Token을 잃어버릴 경우 시스템 다운

<br/>

### Ethernet

- Structure
  - header:  preamble, dest address, source address, type(IP 프로토콜 정의: IPv4, IPv6)
  - CSMA/CD를 사용
  - collision이 발생했을 때 감지하지 못한다면?
    - delay로 인해 생긴 문제
    - minimun frame size 강제(64byte)

  - MAC address: 48bit(제조회사 24bit + 인터페이스 고유 숫자 24bit)


<br/>

### ARP

- IP 주소를 이용하여 MAC 주소를 찾기 위한 프로토콜
- 장치 A가 ARP Request Broadcast를 보냄
- 해당 주소에 맞는 장치 B가 ARP reply Unicast를 통해 MAC 주소 찾음

<br/>

### 스위치

- 교통정리

- 스위치 내부에는 MAC 주소 테이블 있음
- 플러딩: 수신 포트 이외의 모든 포트에서 데이터를 송신

<br/>

<br/>

# 무선이동네트워크

<br/>

### Wireless network characteristics

- A와 B가 통신중일 때 C가 carrier sense 하려고 해도 A가 보내는 데이터를 알아차릴 수 없음
  - 무선환경에서는 CSMA/CD 방식을 사용하지 못함
- Wi-Fi : CSMA/CA 사용
  - 데이터를 보낸 후 ACK를 받았을 때만 데이터 전송이 완벽하게 이루어졌다라고 판단하고 다음 데이터 준비
  - 캐리어감지: 회선이 비어있는지 판단
  - RTS(알림)/CTS(허락) 이용해 Collision 회피
  - A가 RTS를 보낸 후 AP가 보낸 CTS와 B가 보낸 RTS가 겹치면 B반경에서 노이즈
  - A는 데이터 전송을 시작했으나 B는 A가 보내고 있는줄 모르고 충돌
  - A는 ACK를 못받았으니 다시 RTS 시도
  - 무한경쟁 -> 사용자가 많으면 속도가 느려짐
  - ACK재전송은 7번까지만 -> 여기서 포기된 프레임은 돌고돌아 TCP에서 처리

<br/>

<br/>
