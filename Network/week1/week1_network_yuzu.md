| Week 1 | Lecture 1~6 |
| :----: | :---------: |

<br/>

<br/>

# 컴퓨터네트워크 기본

---

<br/>

### Network structure

- network edge: 서비스를 이용하는 애플리케이션과 호스트
- network core: 라우터

<br/>

### TCP VS UDP

- TCP
  - 데이터를 전송한 그대로 받을 수 있게 신뢰성 보장
  - 속도가 느림
  - 네트워크 자원 소모 큼
- UDP
  - 순서대로 전송되지 않고 신뢰성이 보장되지 않음

<br/>

### How is data transferred through net?

- circuit switching
  - 미리 데이터가 전송될 경로를 예약해놓고 전송
  - 일대일
- packet switching
  - 데이터를 전송하는 동안만 네트워크 자원 사용
- packet delay
  - nodal processing: 각 네트워크 노드에서 데이터를 받았을 때 제대로 전달되었는지 확인하는 과정에서 생기는 지연
  - queueing: 패킷데이터를 큐에 순서대로 넣고 다른 패킷이 모두 전송될때까지 대기하는데 걸리는 시간
  - transmission delay: 첫 비트부터 마지막 비트까지 링크에 올리는데 걸리는 시간
  - propagation delay: 다음 라우터까지 전달되는데 걸리는 시간

<br/>

### Client-server architecture

- server: 바뀌지 않는 고정된 IP 주소를 가져야 함
- client: IP 주소를 가지고 있어야하나 고정되지 않아도 됨

<br/>

### Port를 통일하는 이유

- 매번 Port를 입력하기는 번거롭기때문에 80으로 통일


<br/>

### What transport service does an app need?

- data integrity
- timing
- throughput
  - timing은 개별적인 시간에 대해 throughput은 양에 대해
- security

<br/>

### HTTP overview

- Hypter Text Transfer Protocol: 하이퍼텍스트를 전송하는 프로토콜

- Application Layer 프로토콜
- Client의 상태를 저장하지 않음
- Connection
  - Persistent: 추가적으로 요청할 오브젝트가 있을 수 있기 때문에 소켓 연결을 끊지 않고 바로 다시 요청 할 수 있음 병렬요청
  - Non-Persistent: 하나의 Request에 대한 Response 후 접속을 끊음 소켓 연결이 알아서 끊김


<br/>

<br/>

# 애플리케이션계층

---

<br/>

### Socket Programming

- 컴퓨터 네트워크를 경유하는 프로세스 간 통신의 종착점
- UDP소켓
- TCP소켓
  - 서버생성
    - socket(): 소켓 생성 및 사용
    - bind(): 소켓을 서버의 IP 주소와 포트 번호와 결합
    - listen(): 연결 요청 대기
    - accept(): 연결 요청 수락
  - 클라이언트 접속
    - socket() -> connect()
    - three-way handshaking
  - 데이터입출력
    - write(): 데이터 전달
    - read(): 데이터 읽기
  - 소켓닫기
    - close()

<br/>

### Multiplexing 

데이터를 하나로 묶음

<br/>

### Demultiplexing

전송받은 세그먼트의 데이터를 적절한 소켓에 전달

- Segment: 전송계층에서 다중화하여 만들어진 데이터
  - Header: 포트(전달하는 쪽 포트 source port, 전달받는 쪽 포트 dest port)
  - Data: 메시지
  - 헤더의 크기 < 데이터의 크기
  - IP 주소는 전송영역이 아닌 네트워크 단의 헤더에 저장되어 여기서는 존재하지 않음

- UDP
  - 서버는 하나의 소켓으로만 통신, 하나로 모든 클라이언트와 자유롭게 통신 가능
  - Segment
    - header 4개
    - source port
    - dest port
    - length
    - checksum: 데이터 에러 판단
- TCP
  - 하나의 클라이언트 소켓에 하나의 서버 소켓이 매핑(네트워크 자원 소비 큼)

<br/>

### Reliable Data Transfer Protocol

패킷에러, 패킷유실 현상 없이 정상적으로 패킷이 전송되었는지 확인하고 제대로 전송되었다면 다음 패킷을 전송

- RDT프로토콜: TCP에서의 신뢰성 보장
  - RDT 1.0: 완벽하게 데이터가 전송됨
  - RDT 2.0: 패킷 에러만 발생
    - 에러감지
    - 피드백
    - 재전송
  - RDT 2.1: RDT 2.0 보완
    - RDT 2.0일 때 피드백 신호에서 에러가 발생할 수도 있음
    - 패킷 재전송을 통해 해결
    - 재전송하는 패킷은 복제 패킷이므로 메시지에 시퀀스 넘버를 붙여 복제 패킷인지 아닌지 구별할 수 있도록 함
  - RDT 2.2: NAK을 없앰
    - RDT 2.1에서 무조건 Receiver가 ACK를 보내도록 함
  - RDT 3.0: 패킷유실, 패킷에러 둘 다 발생
    - 패킷을 보낼 때 timer를 작동시켜서 일정 시간 동안 응답이 안 오면 timeout으로 간주 이후 데이터 재전송
      - timer가 너무 짧으면 빠르게 유실상황을 복구할 수 있으나 네트워크에 중복된 패킷이 전송될 수 있음
      - timer가 너무 길면 유실상황에 대한 반응이 너무 느리나 네트워크에 대한 오버헤드를 예방할 수 있음
    - RDT 3.0을 한 패킷씩 전송하며 패킷 낭비 발생 -> 한번에 데이터를 전송하고 피드백도 한번에 받아서 패킷 낭비 줄일 수 있음

<br/>

<br/>

# 전송계층

---

<br/>

### Pipelined protocol

기존의 RDT 3.0은 메시지를 하나의 링크를 따라 보내므로 하나의 패킷이 도착하고 ACK가 돌아올 때까지 해당 소켓은 아무런 메시지를 보낼 수 없음 -> 패킷도 동시에 보내고 ACK 패킷도 동시에 받는다면 네트워크 활용률을 높일 수 있음

- Go-Back-N
  - timeout시 윈도우 내 모든 패킷 재전송
  - cumulative 방식으로 ACK 전송
  - 윈도우에 들어있는 데이터는 buffer에 저장하고 있어야 함
  - 정상 전송된 패킷이 버려지고 재전송 될 수도 있음
- Selective repeat
  - Go-Back-N 개선
  - 패킷이 유실되어 재전송 할 때 선별적으로 패킷 재전송
  - 패킷을 받을 때마다 각각 ACK를 전송시켜 같은 패킷을 두 번 전송할 필요가 없게 함
  - 윈도우 내의 모든 패킷마다 타임를 설정해야 함 -> 윈도우 내에서 대표 timeout 타이머를 설정하여 문제 해결
- 윈도우 사이즈가 N일 때 시퀀스 번호는 N**2 정도가 필요

<br/>

### TCP

- point-to-point: sender하나에 receiver 하나
- reliable, in-order byte
- pipelined: ACK가 돌아올 때까지 기다리지 않고 다음 데이터 전송
- full duplex data: 양방향
- sender & receiver buffer
- flow controlled
- segment
  - port: 각각 16비트씩 32비트로 구성
  - sequence number
  - acknowledgement number(ACK)
  - checksum
  - receive window
- timeout
  - EstimatedRTT에 특정한 수를 더해 timeout 판별
- RDT
  - 기존 rdt와 약간의 차이 있음
  - 파이프라인 방식으로 세그먼트 전송
  - 누적 확인 응답
  - TCP는 재전송을 위해 단일 타이머를 사용함

<br/>

<br/>