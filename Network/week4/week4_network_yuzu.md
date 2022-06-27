| Week 4 | Lecture 19~23 |
| :----: | :-----------: |

<br/>

<br/>

# 무선이동네트워크

---

<br/>

### RTS-CTS

- 어차피 충돌이 생길거라면 더 작은단위에서 충돌테스트를 해서 충돌이 안나면 안전하게 데이터를 전송할 수 있게끔 함

<br/>

### frame: addressing

- Address1: 유선프레임을 직접적으로 받는 인터페이스의 MAC address
- Address2: 프레임을 전송하는 인터페이스의 MAC address
- Address3: IP패킷을 처리할 Router의 MAC address
- Address4

<br/>

### mobility within same subnet

- Connection
  - switch가 switch table을 참조하여 목적지 구함
  - h1이 이동해도 connection 유지됨(서버의 IP와 Port 주소가 바뀌지 않기 때문에)
  - 이동시 AP가 바뀔 때 더미프레임을 보내서 switch table정보 갱신

<br/>

### cellular networks

- FDMA: frequency 기준
- TDMA: time 기준
- CDMA: 사용자들에게 각기 다른 코드를 줘서 그 코드에 수학적 연산을 가함(자기 신호는 잘들리고 남의 신호는 노이즈 즈 처리가 되도록)
- 2G, 3G, 4G는 데이터 전송 속도로 나뉘어짐

<br/>

### Mobility

- 같은지점에서 인터넷 사용 > TCP 연결을 끊고 이동 후 다시 새로 TCP연결 > TCP 연결을 유지한 채 이동
- 이동하는 대상과 어떻게 연결할 것인가/ 이동할 때 어떻게 연결을 유지시킬 것인가
  - 이동할때마다 home network(permanent)에게 위치를 알려줌

<br/>

<br/>

# 멀티미디어네트워크

---

<br/>

### Audio

- sampling: 아날로그 신호 -> 디지털 신호
- 주기가 짧을수록 비트가 많을수록 좋음

<br/>

### Video

- 이미지의 연속 ,각 픽셀에 어떤 색 value가 나타나는가
- 이웃한 픽셀은 비슷한 색깔이 저장되기때문에 중복되는 것에대해 compact하게 압축해서 저장할 수 있음

<br/>

### Multimedia networking 3 application types

- streaming, stored
- conversational
- streaming live

<br/>

### DASH

- Streaming stored
  - UPD사용하면 네트워크상황을 너무 고려하지 않음
  - TCP사용하면 네트워크상황에 너무 의존적임

- Dynamic Adaptive Streaming over HTTP
  - 하나의 데이터를 작은 단위로 쪼갬
  - 각 단위별로 여러 버전의 인코딩을 해놓음
  - 처음에 사용자가 영상을 시청할 때 낮은 버전 부터 틀어주고 네트워크 상황이 괜찮으면 점점 버전을 높여서 틈


<br/>

<br/>

# 네트워크보안

<br/>

### network security

- confidentiality
- authentication
- message integrity
- access and availability

<br/>

### Cryptography

- symmetric key

  - 암호화 key 필요

  - 두 사람이 같은 key를 공유해야하므로 사전 합의하는 작업이 필요

- public key

  - 모든사람이 각자 2가지 종류의 키를 가짐
  - 하나는 모든 사람에게 공개되는 public key
  - 하나는 자신만 알수있는 private key

- Authentication

  - symmetric or RSA 방식을 이용하면 데이터를 다른 사람이 알아차리지 못하게 상대방에게만 전달할 수 있음 하지만 검증과정 필요
  - 앨리스가 데이터를 보내고 밥이 받으면 앨리스인지 확인하기 위해 랜덤한 숫자R을 보냄
  - 앨리스는 그 숫자를 암호화에서 밥에게 보내고 밥이 해독한 후 R과 같으면 앨리스라고 확인

- SSL and TCP/IP

  - SSL: 보안을 위해 Application내에 존재하는 층
  - 웹 보안 표준: TCP connection -> handshake -> certificate -> MS -> 메세지 교환

<br/>

<br/>

