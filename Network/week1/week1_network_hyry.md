# Computer Network 1 ~ 6

## 1. Network

### 1. Network 구조

1. Network Edge

   1. 사람들이 일반적으로 사용하는 부분

   2. 각 host에서 application program을 돌린다

   3. client-server model

   4. 각 network edge 간 제공되는 통신 방법

      1. **UDP** (Connectionless service; User Datagram Protocol) 

         1. connectionless라는 것은 서로 쌍방향으로 대화가 일어나지 않는다는 것을 의미
         2. no flow control
         3. unreliable data transfer 
         4. no congestion control
         5. 유실 가능성 존재하지만, 비용이 TCP에 비해 상대적으로 낮다.
         6. sender 마음대로 전송이 가능해 빠르다

      2. **TCP** (Connection-oriented service)

         1. reliabe data transfer + 패킷의 전송 순서 보장

         2. flow control

         3. congestion control

         4. 대신 UDP에 비해 비용이 크다는 단점이 존재

            

2. Network Core

   1. router들의 집합

   2. router는 빠른 전송만 담당 (dumb core)

   3. router 간 데이터가 전송되는 방식

      1. circuit switching: 유선 전화선처럼 특정 사용자만 회선을 사용 가능하도록 가는 길을 미리 예약

      2. **packet switching**: (길이 예약되어 있지 않아 여러 sender의 packet이 혼재) 유저가 보내는 메세지를 packet 단위로 받아 전송

         → 현재 인터넷에서 사용하는 방식

      3. packet swtiching을 쓰는 이유

         1. 더 많은 사용자가 네트워크 사용 가능

         2. 인터넷같은 경우 사용자가 네트워크를 사용 안 하는 시간이 사용하는 시간보다 길기 때문에, circuit switching처럼 계속 한 사용자가 네트워크를 점유하고 있으면 효율이 떨어진다. 

            

3. Packet Delay가 발생하는 4가지 원인

   1. **Processing Delay**
      1. 새로운 패킷을 받으면 패킷을 검사 & 최종 목적지 확인
      2. 라우터 성능 개선하면 개선 가능
   2. **Queueing Delay**
      1. packet이 빠져나가는 속도보다 들어오는 속도가 더 빠른 경우
      2. 패킷들이 임시 buffer나 queue에서 대기
      3. 사용자가 얼마나 몰리느냐에 달려있기 때문에 개선이 어렵다.
      4. 큐가 넘치게 되면 packet loss 발생 → TCP는 처음 시작점에서부터 재전송
   3. **Transmission Delay**
      1. packet이 출발하는 순간 걸리는 delay
      2. packet의 첫 bit부터 마지막 bit까지 나가는데 걸리는 시간
      3. cable 성능 개선으로 개선 가능 (bandwitdh)
   4. **Propogation Delay**
      1. 마지막 bit까지 link에 올라온 후 다음 라우터까지 가는데 걸리는 시간
      2. 링크 길이 / 빛의 속도
      3. 빛이 속도에 달려있기 때문에 개선이 어렵다



## 2. Application Layer

### 1. Client-Server Architecture

1. 같은 host 내 프로세스 간 통신은 OS가 제공하는 interprocess-communication 이용

2. 다른 host 간 프로세스 통신은 **socket**을 통해 **message**를 교환 : client-server communication

3. Client process : 커뮤니케이션을 시작하는 프로세스

   Server process : client에서 접촉해오기를 기다리는 프로세스

   → 즉, application layer는 network edge에만 존재

4. client-server Commmunication을 위해 필요한 것

   1. IP 주소
   2. Port 번호

5. Server

   1. 언제든지 request를 받을 수 있도록 24시간 대기 상태
   2. 클라이언트가 찾아올 수 있도록 항상 동일한 IP 주소
   3. port도 편의를 위해 항상 동일한 번호

6. Client

   1. IP 주소 변화 가능
   2. 서버와 통신



### 2. HTTP Protocol

1. Application Layer의 HTTP protocol에 따르면, 프로세스 간 보내는 message는 **request**와 **response**, 두 가지만 존재
2. HTTP = HyperText Transfer Protocol: 하이퍼 링크를 통해 다른 문서(페이지)로 이동 가능
3. 각 문서가 보유한 resources(object) = HTML file, JPEG image, audio file 등
4. 이 자원에 접근하기 위해서는 URL을 통해 자원을 보유한 host 명과 자원의 위치를 지정
5. 특징
   1. TCP 이용 → message를 교환하려면 그 이전에 TCP connection이 필요
      1. non-persistent HTTP
         1. object 하나를 보내고나면 연결을 바로 끊는다
         2. request-response가 한 번 이루어지고 난 이후에도 계속 끊지 않고 재사용 (multiple objects can be sent over single TCP connection)
      2. **persistent HTTP** (현재 사용하는 방식)
   2. **stateless** : 요청이 들어오면 응답은 하지만, 상대방의 과거 정보나 상태를 기억하지 않는다.



### 3. Sockets

1. socket은 각 프로세스에서 메시지를 내려 보내고 갖고 올라오는 문과 같은 역할 수행
2. application layer와 network layer 사이의 interface (OS에 구현되어 있다)
3. application layer가 이용하는 하위 계층의 서비스
   1. data integrity (transport layer에서 제공)
   2. timing : interactive할 수 있도록 제 시간에 데이터 도착
   3. throughput : 보내는 용량
   4. security
4. application에서 생성하는 socket의 type에 따라 network layer에서 TCP를 사용할지 UDP를 사용할지 결정
5. server에서 socket을 생성하는 과정
   1. socket() : 소켓 생성. 이때 TCP인지 UDP인지 type 결정. 연결할 도메인 설정. 만들어진 소켓의 ID 번호 return
   2. bind() : 소켓을 local IP address와 port #에 연결
   3. listen() : 소켓을 다른 호스트에 직접 연결하는 용도가 아닌, 연결이 오기를 대기하는 상태로 만든다. + 한 번에 몇 개의 request까지 수용할지 결정
   4. accept() : 요청이 오면 받아들이고, clinet의 IP 주소와 port # 기억; client에서 요청이 오기 전까지 blocking 상태
   5. 클라이언트에서 socket()으로 소켓을 생성하고 connect()로 원하는 서버에 연결
   6. 이후 read(), write()를 오고가며 반복
   7. 통신이 끝나면 close()
   8. (cf) UDP 같은 경우 connection 개념 없이 바로 전송



## 3. Transport Layer

1. Multiplexing & Demultiplexing

   1. **Multiplexing** : application layer의 각 프로세스가 socket을 통해 내려보내는 메시지들을 header로 동봉해 segment로 만드는 것. 각각의 header에는 source port #와 dest port # 포함

   2. **Demultiplexing** : 받은 segment에 들어있는 message를 알맞은 process socket으로 올려보내는 것

      1. UDP의 경우 dest IP와 dest Port만을 사용해 올려 보낼 socket 결정

      2. TDP의 경우 source IP, source Port, dest IP, destPort 모두 사용해 올려보낼 socket 결정

         1. 즉, TCP는 각 클라이언트마다 별도의 socket 생성하기 때문에, 자원을 많이 소모한다.

            

2. UDP의 header

   1. source port # (16bit)

   2. dest port # (16 bit)

   3. checksum → UDP도 최소한 (de)multiplexing기능과 에러가 없는지 체크해 올려 보낸다. (있다면 버린다.)

   4. length

      

3. reliable data transfer를 위해 필요한 것

   1. application layer가 하나도 유실되지 않고 전달

   2. 하지만, router를 통해 가면서 packet error와  packet loss는 항상 발생

   3. packet error

      1. error detection 기능 : checksum
      2. feedback 기능 : 잘 받았는지 신호를 보내기 (ACKs, NAKs)
      3. Sequence Number : feedback조차 오류날 경우를 대비해 packet에 붙이는 번호
      4. retransmission : NAK가 오는 경우 재 전송
      5. NAK 대신 ACK와 마지막으로 잘 받은 sequence #(또는 TCP에서는 다음으로 오기를 기대하는 번호)를 함께 보내기도

   4. packet loss

      1. Timer
         1. 짧은 경우 recovery가 빠르지만 network에 overhead 발생할 가능성
         2. 느린 경우 recovery가 느리게 진행

   5. pipelined protocols (error나 loss 발생시 재전송하는 방법)

      1. go-back-N

         1. cumulative ACK : 잘 받은 번호까지를 ACK에 함께 전송
         2. ACK가 온 번호는 send buffer(window)에서 제외
         3. timeout시 window만큼 재전송
         4. (특징) receiver는 하는 역할이 원하는 번호의 packet이 오는지 기다리고 ack를 보내는 것 밖에 없다
         5. (단점) 하나의 패킷만 유실되더라도 전체 window를 한꺼번에 다시 보낸다.

      2. selective repeat

         1. 문제가 발생한 패킷만 개별적으로 재전송
         2. cumulative ack x 개별 패킷에 대한 ack를 받는다
         3. receiver는 순서에 맞지 않게 들어와도, 일단 기다려보며 receive buffer에 저장

      3. TCP는 go-back-N과 selective repeat 혼용

         

4. TCP

   1. 특징

      1. point-to-point 
      2. reliable data transfer
      3. flow control
      4. congestion control
      5. in-order
      6. pipelined 
      7. full duplex data
      8. connection-oriented
      9. send & receive buffer

   2. sequence Number는 message의 맨 앞에 붙은 번호

      Ack는 다음에 오길 기대하는 번호

   3. Timer는 누적된 RTT (round trip time)을 기반으로 계속 현재 상황을 반영한 시간 + margin 값 (유실이 확실할 때 timer가 터지도록)

   4. 권고 사항

      1. delayed ack : ack는 조금 기다렸다가 보내기 (같이 보낼 데이터 발생 가능)
      2. fast retransmit : timer 시간이 기니 duplicate ack를 3번 받으면 loss로 판단하고 재전송