Chapter 2. 네트워크

---

section 1. 네트워크의 기초

- 네트워크: 노드(node)와 링크(link)가 서로 연결되어 있거나 연결되어 있지 않은 집합체를 의미
- 좋은 네트워크: 많은 처리량을 처리할 수 있으며 지연 시간이 짧고 장애 빈도가 적으며 좋은 보안을 갖춘 네트워크
- 네트워크 토폴로지(network topology): 노드와 링크가 어떻게 배치되어 있는지에 대한 방식이자 연결 형태 -> 병목 현상을 찾을 때 중요한 기준이 됨(어떤 토폴로지를 갖는지, 어떠한 경로로 이루어져 있는지를 알아야 병목 현상을 올바르게 해결 가능)
  - 트리 토폴로지: 계층형 토폴로지라고 하며 트리 형태로 배치한 네트워크 구성
  - 버스 토폴로지: 중앙 통신 회선 하나에 여러 개의 노드가 연결되어 공유하는 네트워크 구성. 근거 리 통신망(LAN)에서 사용. 설치비용 적음. 신뢰성 우수. 중앙 통신 회선에 노드 추가 삭제 용이. 스푸핑이 가능한 문제점
  - 스타 토폴로지: 중앙에 있는 노드에 모두 연결된 네트워크 구성
  - 링형 토폴로지: 각각의 노드가 양 옆의 두 노드와 연결하여 전체적으로 고리처럼 하나의 연속된 길을 통 해 통신을 하는 망 구성 방식
  - 메시 토폴로지: 망형 토폴로지, 그물망처럼 연결되어 있는 구조
- 네트워크 분류(규모)
  - LAN(Local Area Network)
  - MAN(Metropolitan Area Network)
  - WAN(Wide Area Network)
- 병목 현상의 주된 원인
  - 네트워크 대역폭
  - 네트워크 토폴로지
  - 서버 CPU, 메모리 사용량
  - 비효율적인 네트워크 구성

---

section 2. TCP/IP 4계층 모델

- 애플리케이션 계층
  -  FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜 계 층이며 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층
- 전송 계층
  - 송신자와 수신자를 연결하는 통신 서비스를 제공하며 연결 지향 데이터 스트림 지원, 신뢰성, 흐름 제어를 제공하며, 애플리케이션과 인터넷 계층 사이의 데이터가 전달될 때의 중계 역할
  - TCP: 패킷 사이 순서 보장, 연결지향 프로토콜 사용해서 연결하여 신뢰성을 구축하여 수신 여부를 확인. '가상회선 패킷 교환 방식' 사용
  - UDP: 순서 보장 x, 수신 여부 확인 x, 단순히 데이터만 주는 '데이터그램 패킷 교환 방식' 사용
  - 가상회선 패킷 교환 방식: 각 패킷에는 가상회선 식별자가 포함되며 모든 패킷을 전송하면 가상회선이 해제되 고 패킷들은 전송된 ‘순서대로’ 도착하는 방식
  - 데이터그램 패킷 교환 방식: 패킷이 독립적으로 이동하며 최적의 경로를 선택하여 가는데, 하나의 메시지에 서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있으며 도착한 ‘순서가 다를 수’ 있는 방식
  -  3-웨이 핸드셰이크: TCP가 신뢰성을 확보하기 위해 진행하는 작업
    - SYN 단계: 클라이언트는 서버에 클라이언트의 ISN을 담아 SYN을 보냄. ISN은 새로운 TCP 연결의 첫 번째 패킷에 할당된 임의의 시퀀스 번호. 장치마다 다를 수 있음
    - SYN + ACK 단계: 서버는 클라이언트의 SYN을 수신하고 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN + 1을 보냄
    - ACK 단계: 클라이언트는 서버의 ISN + 1한 값인 승인번호를 담아 ACK를 서버에 보냄
  - 4-웨이 핸드셰이크: TCP가 연결을 해제할 때 발생하는 과정
- 인터넷 계층
  - 장치로부터 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용되는 계층
- 링크 계층(네트워크 접근 계층)
  - 전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달하며 장치 간에 신호를 주고받는 ‘규칙’을 정하는 계층
- PDU (Protocol Data Unit): 네트워크의 어떠한 계층에서 계층으로 데이터가 전달될 때 한 덩어리의 단위
  - 제어 관련 정보들이 포함된 ‘헤더’, 데이터를 의미하는 ‘페이로드’로 구성

---

section 3. 네트워크 기기

---

- 네트워크 기기의 처리 범위

  - 애플리케이션 계층: L7 스위치

  - 인터넷 계층: 라우터, L3 스위치

  - 데이터 링크 계층: L2 스위치, 브리지

  - 물리 계층: NIC, 리피터, AP