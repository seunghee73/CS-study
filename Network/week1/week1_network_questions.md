# ✨ week1_network_questions ✨

**✏ 인터넷 5계층은?** 

- 응용계층, 전송계층, 네트워크계층, 데이터링크계층, 물리계층



**✏ FTTH는 어떤 케이블을 사용하는가**

- 광케이블



**✏ 성능지표에서 전송률은 최대전송률을 보는게 옳은가?**

- 아니다. 공유채널일때, 과대측정 됐을 확률이 높다.



**✏ 지연시간의 딜레이 3종류**

- processing delay, queueing delay, transmisson delay



**✏ 응용 계층에 간단하게 설명하시오**

- 분산된 시스템을 하나의 통합된 응용시스템으로 묶어주는 계층



**✏ 큐잉 지연에 대해 설명하시오 **

- 기다리는데 걸리는 시간. 병목현상 파악 후 자원을 조금 늘리면 병목현상을 해결할 수 있다.



**✏ switch network 중에 시작과 끝이 명시적으로 구분되는 것은 무엇인가? **

- circuit switch network



**✏ 동축선의 특징에 대해 설명하시오 **

- 비싸고 전송률이 높다



**✏ circuit switching과 packet switching에 대해서 설명하시오**

- circuit switching은 데이터를 전송하기 위해 하나의 회선을 단독으로 할당받아 사용하는 방법이다. 회선을 점유하고 있는데 보내는 데이터가 많지 않은 경우에는 circuit이 낭비된다는 특징이 있다. packet switching은 데이터를 쪼개서 전송하는 방법으로 필요에 따라 링크를 할당받아 사용하는 방법이다. 각각의 sender가 보내는 데이터가 많지 않은 경우에는 circuit switching보다 packet switching이 유용하다.



**✏ transmission delay와 propagation delay의 차이점에 대해서 설명하시오**

- transmission delay는 버퍼에서 첫 번째 비트가 나가는 순간부터 마지막 비트가 나가는 순간까지 걸리는 시간이다. 반면 propagation delay는 마지막 비트가 링크를 통해 다음 라우터에 도착할 때까지 걸리는 시간이다.



**✏ packet error를 대비하는 4가지 방법 (일련과정)**

- error detection, feedback(ack, nak), retransmission, sequence number



**✏ UDP의 header가 보유한 것은?**

- source port #, dest port #,  checksum, length



**✏  non-persistent HTTP와 persisent HTTP에 대해서 설명하시오**

- non-persistent HTTP의 경우에는 한 번의 데이터를 받고 나면 TCP 연결을 끊어버린다. 여러 번 데이터를 전송해야 하는 경우, 매 번 TCP 연결을 다시 해야 하는 문제가 있다. persistent HTTP는 여러 개의 데이터를 보내는 경우 모든 데이터를 받고 난 뒤에 연결을 끊는다. TCP 연결을 한 번만 하면 된다는 장점이 있다



**✏ TCP(Transmission Control Protocol) 와 UDP(User Datagram Protocol)의 특징에 대해 비교하여 설명해주세요 **

- UDP의 경우에는 client와 server가 서로 연결을 하는 과정 없이 데이터를 전송하기 때문에 unreliable 합니다. 반면 TCP의 경우, 3-way handshaking을 통해 client와 server가 서로 연결이 확인된 상황에서 데이터를 전송하기 때문에 reliable합니다. 그리고 UDP의 경우에는 congestion과 flow를 조절할 수 없지만 TCP의 경우에는 조절이 가능합니다.



**✏ 패킷유실, 패킷에러 둘 다 발생한 경우의 rdt 프로토콜은?** 

- rdt 3.0



**✏ RDT프로토콜에서 timer가 짧은경우와 긴경우 장단점?**

- timer가 너무 짧으면 빠르게 유실상황을 복구할 수 있으나 네트워크에 중복된 패킷이 전송될 수 있음 
- timer가 너무 길면 유실상황에 대한 반응이 너무 느리나 네트워크에 대한 오버헤드를 예방할 수 있음



**✏ Multiplexing과 Demultiplexing은 무엇인가?**

- Multiplexing : application layer의 각 프로세스가 socket을 통해 내려보내는 메시지들을 header로 동봉해 segment로 만드는 것
- Demultiplexing : 받은 segment에 들어있는 message를 알맞은 process socket으로 올려보내는 것



**✏ go-back-N과 selective repeat 비교하시오**

- go-back-N: timeout시 window만큼 재전송 + cumulative ack 
- selective repeat: 문제가 발생한 패킷만 개별적으로 재전송 + ack를 개별 패킷별로 전송



**✏ UDP Segment에서 checksum이 하는일?**

- 데이터 에러 판단



**✏ 윈도우 사이즈가 N일 때 시퀀스 넘버는 얼마까지 필요한가?**

- N^2