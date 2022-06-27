# Network 4

## 1. Wireless and Mobile Networks

### 1. 802.11 (Wifi)

 	1. 허가를 받지 않고도 사용 가능한 주파수 대역 사용
 	2. 각 AP마다 채널 선택
 	3. 같은 채널을 선택한 AP가 있는 경우 간섭 발생 → CSMA/CA + RTS/CTS 사용
 	4. 프레임에 세 개의 주소가 필수로 들어간다
     	1. AP Mac Addr.
     	2. Source Mac Addr.
     	3. Router Mac Addr.

### 2. Cell Tower

1. 전체 지역을 cell 단위로 나누어 cell마다 기지국 설치
2. 기지국간 무선 통신 방식 : channel partitioning (FDMA / TDMA) 





## 2. Multimedia Networking

1. 대표적인 multimedia
   1. audio
      1. analog
      2. analog를 digital 신호로 바꾸기 위한 sampling 작업 필요
      3. 샘플링 데이터 크기와 주기에 따라 원본에 가까운 정도가 달라진다
   2. video
      1. 이미지의 연속 (프레임)
      2. 서로 주위에 있는 픽셀은 유사하다는 점을 이용해 압축 (레코딩)
2. multimedia networking type
   1. jitter (network delay)가 발생하기 때문에 네트워크로 받은 데이터를 바로 출력하면 안 된다
   2. 어느 정도 buffer에 차기를 기다렸다가 재생 (buffering)
   3. buffering이 길면 길수록 미리 받아 놓을 수 있는 양이 많아지지만, 소비자의 인내심도 고려 필요
   4. buffer에 들어오는 속도보다 나가는 속도가 빠르면 "buffering" 발생
3. 영상 스트리밍 서비스 (e.g. Youtube)
   1. UDP가 속도가 빠르긴 하지만, 대용량 전송하기 때문에 그만큼 유실되는 리스크도 크다.
   2. 그래서 TCP 사용. 하지만, TCP는 속도를 UDP처럼 원하는 대로 보낼 수 없다.
      1. DASH (Dynamic adaptive streaming over Http)
         1. 큰 영상을 작은 단위의 chunk로 나눈다
         2. 각 chunk를 여러 버전으로 코딩하고, 이 버전 별로 url 보유
         3. 클라이언트가 네트워크 상황에 맞춰 네트워크 속도를 조절할 수 있도록, 이 url이 적힌 manifest file 제공
      2. CDN (Contents Distribution Network)
         1. CP (contents provider)에게 집중되는 트래픽을 분리하기 위해 사용
         2. 콘텐츠가 저장된 storage를 전 세계 곳곳에 두고, source IP를 보고 DNS를 통해 근처 CDN으로 보낸다.



## 3. Network Security

### 1. Network Security

1. Confidentiality : sender와 reciever 외에는 message 내용에 대해 알지 못하도록 보안 유지 (암호화)
2. authentication : identity 확인
3. message integrity : 메시지가 변경되지 않고 원본 그대로 전달
4. access & availability : 사용자에게 항상 24시간 서비스 제공

### 2. 암호화

1. Symmetric Key
   1. sender와 receiver가 가진 키가 동일
   2. 빠르고 효율적
   3. 같은 키를 공유해야 하기 때문에 사전 작업이 필요
   4. (Diffie) public key(모두가 아는 키), private key(각 host만 아는 키) 개념 제시
2. RSA
   1. public key, private key로 암호화 하는 방법을 소인수 분해를 이용하여 구현
   2. 구현한 Rivest, Shamir, Adleman 3명의 이름을 따서 명명
   3. 하지만, 큰 수의 소인수 분해는 CPU 자원을 많이 먹기 때문에, 보통 symmetric key 사용을 위한(같은 키를 공유하기 위한) 사전 작업으로 사용 
   4. private key로 암호화하면 public key로 풀 수 있고, public key로 풀면 private key로 풀 수 있다.
   5. 보내는 사람의 public key인지는 인증서를 통해 확인
3. authentication
   1. 상대의 신원을 확인하기 위해 난수를 private key로 암호화해서 보내면, 그걸 받아 상대의 public key로 풀어봄으로써 확인
4. Message Integrity
   1. digital signature : 메시지를 암호화 해서 보내는 것 (private key 적용)
   2. 전체 메시지를 암호화 하기 보다 효율을 위해 해시 값을 암호화해 사용



### 3. HTTP & HTTPS

1. SSL (secure socket layer) = TLS (Transport layer security)
   1. app layer에서 사용하는 라이브러리
   2. TCP로 내려보내기 전에 보안을 거치도록 만든다
2. 이렇게 SSL(TLS)를 거치면 HTTP가 아니라 HTTPS가 된다
3. 방법
   1. TCP 기반이니 TCP 연결 선행 (handshake)
   2. 상대가 public key를 인증서와 함께 보낸다
   3. 받은 public key를 인증 센터에서 확인하고 message를 public key로 암호화해 보낸다.
   4. 이때 받은 public key로 symmetric key를 4개 생성
      1. sender 암호화용
      2. receiver 암호화용
      3. sender 메시지의 data integrity 확인용
      4. receiver 메시지의 data integrity 확인용
   5. message를 보낼 때 데이터 뿐만이 아니라 MAC (Message authentication code)를 함께 보낸다
      1. MAC은 해시 함수 안에 데이터, intiegrity확인용 symmetric key + 레코드 순서 + type(FIN 구별용)을 포함한다.











