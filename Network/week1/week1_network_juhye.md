# Chapter 1. 컴퓨터네트워크 기본 1



### A closer look at network structure

- network edge
  - applicataions and hosts

- network core
  - routers
  - Network of networks

- access networks, physical media
  * communication links



#### network edge

- end systems (hosts):

  * run application programs

  * eg. Web, email

- client / server model

  * client host requests, receives service from always-on server

    클라이언트는 서버로부터 정보를 가져오는 것. 서버는 24시간 연결되어 있어서 언제 들어올지 모르는 클라이언트의 요청을 기다리는 것

  * eg. Web browser/server, email client/server

- peer-peer model

  * minimal use of dedicated servers
  * eg. Skype, BitTorrent, KaZaA



#### Network edge: connection-oriented service

인터넷에서 제공하는 통신 서비스

- 목표: data transfer between end systems
  - Connection: prepare for data transfer ahead of time
    - Request/Respond
    - set up "state" in two communicating hosts
  - TCP - Transmission Control Protocol
    - Internet's connection-oriented service
- **TCP service**
  - Reliable, in-order byte-stream data transfer (신뢰성있는 데이터를 순서대로 전송)
    - loss: acknowledgements and retransmissions
  - Flow control (속도에 맞춰서 전달): 
    - sender won't overwhelm receiver (리시버의 속도에 맞춰서 센더의 속도를 조절)
  - congestion control:
    - senders "slow down sending rate" when network congested (네트워크 상황에 맞춰서 전달)

#### Network edge: connectionless service

- 목표: data transfer between end systems
  - Same as before
- UDP - User Datagram Protocol (아무것도 안함)
  - connectionless
  - Unreliable data transfer (신뢰성 보장 x)
  - no flow control
  - no congestion control
- App's using TCP:
  - HTTP (web), FTP(file transfer), Telnet(remote login), SMTP(email)
- App's using UDP:
  - streaming media, teleconferencing, DNS, Internet telephony

-> TCP와 UDP는 인터넷에서 제공하는 통신 서비스로, 둘 중에 선택해서 사용하면 됨

보이스, 음성은 유출되어도 사람들의 귀가 민감하지 않아서 알아채지 못해서 UDP 사용 가능. 네이버 기사와 같은 대부분의 경우에는 TCP 사용



### What's a protocol?

a human protocol and a computer network protocol

대화의 약속

All communication in Internet coordinated by protocols

프로토콜이 통하는 네트워크끼리 통할 수 있음



### The Network Core 네트워크 코어

- mesh of interconnected routers 
- the fundamental question: how is data transferred through net? 어떻게 메시지를 전달하는가?
  - Circuit switching: dedicated circuit per call: telephone net (출발지에서 목적지까지 가는 길을 미리 예약해두고 특정 사용자만 사용하도록 한 것 (유선전화))
  - **Packet-switching**: data sent thru net in discrete "chunks" (인터넷에서 사용하는 방식) (사용자가 보내는 메세지를 패킷 단위로 받아서 들어오는대로 전송)



#### Packet switching vs Circuit Switching

패킷 스위칭이 더 많은 사용자가 네트워크를 사용할 수 있게 함

- 1 Mb/s link
- Each user: 100 kb/s when "active", active 10% of time
- circuit switching: 10 users
- packet switching: with 35 users, probability > 10 active less then .0004

-> 패킷 스위치이기 때문에 delay가 생김



### Four sources of packet delay

- nodal processing: 패킷이 들어와서 검사하는 시간. 

  라우터 성능 개선을 통해 delay를 줄일 수 있음

- queueing: 패킷이 buffer/queue에서 순서 대기하는 시간

  사람들의 사용 패턴이기 때문에 사람들이 몰리는 것을 조절할 수 없어서 딜레이를 줄이기 가장 힘듦. (고속도로 하이패스, 차선 확장을 해도 도로 정체 발생)

  큐의 크기가 무한대일 수 없고 한계가 있을 수 밖에 없음. 큐의 크기보다 더 많이 들어오면 넘치는 packet은 버려짐 -> packet loss(유실) 발생

  TCP는 처음부터 재전송 (dumb core는 단순 전달만)

- transmission delay: 첫 비트부터 마지막 비트까지 링크에 나가는데 걸리는 시간. = Packet length / link bandwidth (링크길이/비트속도)

  Cable 성능 개선을 통해 delay를 줄일 수 있음

- propagation delay: 마지막 비트가 다음 라우터까지 도달하는데 걸리는 시간



-> 이 딜레이를 줄이는 것이 좋음



---

# Chapter 2. 컴퓨터네트워크 기본 2

### Client-server architecture

- server (네트워크): 바뀌지 않고 고정된 IP 주소를 가져야 함
  - Always-on host
  - Permanent IP address
  - Data centers for scaling

- clients (웹브라우저):  IP 주소를 가져야하지만 고정되지 않아도 됨
  - communicate with server
  - may be intermittently connected
  - may have dynamic IP address
  - do not communicate directly with each other


클라이언트-서버 간 의사소통(통신)을 위한 인터페이스 - 소켓

소켓의 주소 역할을 하는 인덱싱이 필요함 -> IP address와 port (다른 컴퓨터의 위치한 프로세스를 지칭)

모든 웹서비스는 모두 공통된 port 80번으로 통일해서 사용함. 도메인 주소가 다르므로 번거로움을 피하기 위해 포트나마 통일함





### What transport service does an app need? 

TCP는 제공하고 UDP는 제공하지 않음

- data integrity
- timing
- throughput
- security



### HTTP overview

- Hypter Text Transfer Protocol: 하이퍼텍스트를 전송하는 프로토콜
- uses TCP:
  - Client initiates TCP connection (creates socket) to server, port 80
  - server accepts TCP connection from client
  - HTTP messages (application-layer protocol message) exchanged between browser (HTTP client) and Web server (HTTP server)
  - TCP connection closed

- HTTP is "stateless"
  - server maintains to information about past client requests (클라이언트의 과거 요청에 대해 기억하지 않음)




HTTP가 TCP를 사용하는 방식에 따라 두 가지로 나뉨

- non-persistent HTTP (response time 연결을 끊음)
  - At most one object sent over TCP connection
    - Connection then closed
  - downloading multiple objects required multiple connections
- Persistent HTTP (끊지 않고 유지하면서 재사용)
  - multiple objects can be sent over single TCP connection between client, server



---

# Chapter 3. 애플리케이션계층 1

### Socket

클라이언트와 서버의 통신

OS와 애플리케이션 프로그램 사이의 인터페이스

os에서 제공하는 프로세스 간 통신을 위한 API

소켓 타입 - TCP Socket / UDP Socket

#### SOCK_STREAM

- TCP
- Reliable delivery
- In-order guranateed
- Connection-oriented
- bidirectional



#### SOCK_DGRAM

- UDP
- Unreliable delivery
- No order guarantees
- no notion of 'connection'-opp indicates dest. for each pocket
- can send or receive



### Sockets API

- Creation and Setup
- Establishing a Connection (TCP)
- Sending and Receiving Data
- Tearing Down a Connection (TCP)



### Big picture: Socket Functions (TCP case)

- Socket : 소켓 생성

  파라미터가 세개 들어가는데, 가장 중요한 것은 두번째 파라미터

  소켓의 리턴 값으로 소켓의 아이디(인덱스값)

- Bind()

  방금 생성한 소켓의 아이디를 사용해서 특정 어드레스에 바인드하겠다

- listen()

  방금 생성한 이 소켓을 리슨 용도로 사용할 것이며, 순서대로 큐에 담아놓고 처리하겠다.

- Accept()

  모든 준비가 끝났으므로 클라이언트의 요청을 기다리겠다.

  클라이언트가 요청 할 때 까지 블락된 상태 (blocking)

- connect

  파라미터에 서버의 주소와 프로토콜 넘버가 들어감

- Read / write

  - Write is blocking

- close

  연결을 끊음

  방금 사용했던 소켓을 release 시켜줌



### Recap: TCP socket connection setup



- server

```c
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <string.h>
#include <sys/types.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <sys/wait.h>
#define PORT 3490 /*포트는 3490번을 사용*/
#define BACKLOG 10 /*최대 접속 가능한 큐 크기*/

main()
{
	int sockfd, new_fd; /*listen on sock_fd, new connection on new_fd */
    struct sockaddr_in my_addr; /*my address*/
    struct sockaddr_in their_addr; /*connecttor addr*/
    int sin_size;
    
    /*
    	sockaddr_in 구조:
        struct sockaddr_in {
        	short sin_family;
            u_short sin_port;
            struct in_addr sin_addr;
        };
        sin_family = AT_INET
        sin_port: port #(0-65535)
        sin_addr: IP-address
    */
    
    if((sockfd = socket(PF_INET, SOCK_STREAM, 0))==-1){
    	perror("socket");
        exit(1);
    }
    
    my_addr.sin_family = AF_INET;
    my_addr.sin_port = htons(MYPORT); /*short, network byte order */
    my_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    /* INADDR_ANY allows clients to connect to any one of the host's IP address*/
    
    if(bind(sockfd,(struct sockaddr *)&my_addr, sizeof(struct sockaddr))==-1){
    	perror("bind");
        exit(1);
    }
    
    if(listen(sockfd, BACKLOG) == -1) {
    	perror("listen");
        exit(1);
    }
    while(1) { /*main accept() loop*/
    	sin_size = sizeof(struct sockaddr_in);
        /*accept()가 실행되면 클라이언트에서 요청이 올때까지 block 상태가 됨*/
        if((new_fd = accept(sockfd, (struct sockaddr*)&their_addr,&sin_size))==-1){
        	perror("accept");
            continue;
        }
        /*접속한 클라이언트 정보 출력*/
        printf("server: got connection form %s\n". inet_ntoa(their_adddr.sin_addr));
    }
}
```



- 클라이언트

```
if((sockfd = socket(PF_INET, SOCK_STREAM, 0))==-1){
	perror("socket");
    exit(1);
}

their_addr.sin_family = AF_INET;
their_addr.sin_port = htons(Server_Portnumber);
their_addr.sin_addr = htonl(Servel_IP_address);

if(connect(sockfd,(struct sockaddr*)&their_addr, sizeof(struct sockaddr))==-1){
	perror("connect");
    exit(1);
}
```



### Multiplexing and Demultiplexing

- 전송계층이 기본적으로 제공하는 기능

- Multiplexing: sender의 전송계층이 어플리케이션 계층의 수많은 프로세스들이 보내는 메세지를 하나의 segment로 묶어서 아래 계층에게 전달하는 기능
- Demultiplexing: receiver의 전송계층에서 받은 segment를 애플리케이션 계층에 있는 수많은 프로세스 중 정확한 소켓에 전달하는 기능



### How demultiplexing works

Receiver는 segment의 Header에 데이터에 대한 정보가 있으므로 정확한 목적지의 소켓에 전달할 수 있다.

segment format = data + header (source port # + dest port #)



---

# Chapter 4. 애플리케이션계층 2

### Principles of Reliable Data Transfer



### Let's Build simple Reliable Data Transfer Protocol

- Incrementally develop sender, receiver sides of reliable data transfer protocol(rdt)
- Stop-and-wait protocol 만 고려
- use finite state machines (FSM) to specify sender, receiver



### Rdt1.0: Data Transfer over a perfect Channel

에러, 유실이 발생하지 않는다면(no packet errors, no packet loss), 딱히 할 일 없음 (비현실적 상황)



### Rdt2.0: channel with packet errors (no loss)

- Error detection : 에러 감지 판단이 필요
- Feedback : 패킷에 에러가 있었다면 ? 다시 보내달라고 요청하거나 하는 피드백 필요 (신뢰성 있는 의사소통)
- retransmission : 재전송



#### Can this completely solve errors ?

- 피드백에 에러가 있는 경우 완전히 처리되지 않음
- The receive packet is new or duplicate ? Seq #



#### Handling Duplicate Packets

- sequence number를 모든 패킷에 붙인다
- 재전송
- Duplicate packet 버림



### rdt2.1: examples



### rdt2.1: summary for packet error

- sender :
  - Seq # added to pkt
  - must check if received ACK/NAK corrupted
  - Retransmit of NAK or corrupted feedback
- Receiver : 
  - must check if received packet is duplicate
  - send NAK if received packet is corrupted
    - send ACK otherwise



### Rdt2.2 : a NAK-free protocol

- ACKs만을 사용, rdt2.1과 기능적으로 같음



### rdt3.0: channel with loss & packet errors

Timer

sender는 패킷을 보낼 때마다 타이머를 실행해서 일정 시간이 지난 뒤엔 유실됨을 판단하고 재전송

타이머 시간을 reasonable하게 설정해야 함



=> 한번의 하나의 패킷밖에 보내지 못해서 너무 단순함

한꺼번에 여러 개의 데이터를 보내고 받는 방식을 사용 = Pipelined protocols



---

# Chapter 5. 전송 계층 1

전송 단위: segment

### Pipelined protocols

- 파이프라인 프로토콜의 두 형태 : go-Back-N, selective repeat



### Pipelining: increased utilization



### Go-Back-N

- 한꺼번에 많은 패킷을 보냄 (버퍼 크기 기준: window size = N)
- window size만큼은 피드백을 받지 않고 보냄

- cumulative(누적되는 방식) ACK
- ACK를 받으면 해당 패킷을 window에서 제거하고 새 패킷을 window에 넣음
- 에러나 loss로 패킷의 타임아웃이 발생하면 재전송함 (go back n)
- 단 하나의 패킷이 유실되더라도 윈도우 전체를 재전송해야하는 단점.



#### GBN in action



### Selective Repeat

- GBN의 단점을 해결

- GBN은 문제가 있을 때, 다 보내줌. selective repeat은 문제가 있을 때 재전송을 해주되, 선택적으로 없어진 애들만 재전송
- receiver가 받은 패킷을 임시저장할 수 있는 버퍼가 있어서, 순서에 맞게 들어온 패킷이 아니면 우선 버퍼에 저장한다.
- 버퍼에 넣고 sender가 재전송한 것을 받으면 버퍼 안의 모든 패킷을 애플리케이션에 올리고 ACK를 보냄



#### Selective repeat: dilemma

- 윈도우사이즈가 N일 때, 시퀀스 넘버가 N+1이면 안됨 

- Seq #를 늘리면 패킷 수만큼 커질 수 있으므로 최소한의 seq 넘버를 가지고 반복해서 사용

  

---

# Chapter 6. 전송 계층 2

### TCP

- Point-to-point 
  - One sender, one receiver 1대 1 연결
- reliable, in-order byte stream
  - No "message boundaries" 전송간 데이터 에러와 유실이 없음
- pipelined
  - TCP congestion and flow control set window size 한꺼번에 다수의 패킷 전송
- send & receive buffers 패킷 임시보관 버퍼 
- full duplex data
  - Bi-directional data flow in same connection 양방향 통신
  - MSS: maximum segment size
- connection-oriented
  - handshaking (exchange of control msgs) init's sender, receiver state before data exchange 커넥션을 유지하기 위한 3-way handshaking, 4-way handshaking
- flow controlled
  - sender will not overwhelm receiver
  - 리시버의 소화 능력에 맞게 데이터를 보내야 한다.



### TCP segment structure



### TCP seq. #'s and ACKs

TCP 전송방식



### TCP reliable data transfer

- 파이프라인 segments
- Cumulative acks
- 단일의 타이머 사용



### TCP: retransmission scenarios (재전송 시나리오)

- sender: 타임아웃되면 재전송
- Receiver: 보낸 ack가 유실되어 중복된 패킷 재전송 - 제일 최신 ack 전송
- 타임아웃 / 중복된 ack 3개를 더 받을 경우 sender는 재전송 (fast retransmit)