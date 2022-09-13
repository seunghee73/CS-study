## IP 주소

- ARP(Address Resolution Protocol)
  
  - IP주소를 실제 주소인 MAC주소로 변환
  
  - RARP로 MAC 주소를 IP 주소로 변환하기도 함



- 브로드캐스트
  
  - 송신 호스트가 전송한 테이터가 네트워크에 연결된 모든 호스트에 전송되는 방식
    
    

- 유니캐스트
  
  - 고유 주소로 식별된 하나의 네트워크 목적지에 1:1로 데이터를 전송하는 방식
  
  

- 홉바이홉(hop by hop) 통신
  
  - IP 주소를 통해 통신하는 과정
  
  - 통신 장치에 있는 '라우팅 테이블'의 IP를 통해 시작 주소부터 시작하여 다음 IP로 계속해서 이동하는 '라우팅' 과정을 거쳐 패킷이 최종 목적지까지 도달하는 통신



- 라우팅 테이블
  
  - 송신지에서 수신지까지 도달하기 위해 사용되며 라우터에 들어가 있는 목적지 정보들과 그 목적지로 가기 위한 방법이 들어 있는 리스트
  
  - 게이트웨이와 모든 목적지에 대해 해당 목적지에 도달하기 위해 거쳐야 할 다음 라우터의 정보를 가지고 있음
    
    

- 게이트웨이
  
  - 서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는 관문 역할을 하는 컴퓨터나 소프트웨어
  
  - 서로 다른 네트워크 상의 통신 프로토콜을 변환해주는 역할을 하기도 함
  
  - 라우팅 테이블을 통해 볼 수 있고 라우팅 테이블은 `netstat -r`을 통해 확인할 수 있다.

## 

- IP 주소 체계
  
  - IPv4 : 32비트를 8비트 단위로 점을 찍어 표기
  
  - IPv6 : 64비트를 16비트 단위로 점을 찍어 표기
  
  - 네트워크의 첫 번째 주소는 네트워크 주소
  
  - 네트워크의 마지막 주소는 브로드캐스트용 주소



- DHCP(Dynamic Host Configuration Protocol)
  
  - 네트워크 장치의 IP 주소를 자동으로 할당



- NAT(Network Address Translation)
  
  - 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소 정보를 수정하여 IP 주소를 다른 주소로 매핑하는 방법
  
  - 사설 IP를 공인 IP로 변환하거나 공인 IP를 사설 IP로 변환하는데 사용
  
  - ICS, RRAS, Netfilter
  
  - NAT를 이용하여 보안 가능
    
    NAT는 여러 명이 동시에 인터넷을 접속하게 되므로 실제로 접속하는 호스트 숫자에 따라 접속 속도가 느려질 수 있다는 단점
    
    

## HTTP

- HTTP/1.0
  
  - 한 연결 당 하나의 요청
  
  - 서버로부터 파일을 가져올 때마다 TCP의 3wayhandshake를 해야하기 때문에 RTT 증가의 문제



- RTT 
  
  - 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간이며 패킷 왕복 시간



- RTT 증가를 해결하기 위한 방법
  
  - 이미지 스플리팅 : 많은 이미지를 다운 받게 되면 과부하가 걸리기 때문에 많은 이미지가 합쳐 있는 하나의 이미지를 다운로드 받고, 이를 기반으로 background-image의 position을 이용하여 이미지를 표기하는 방법
  
  - 코드 압축 : 코드를 압축해서 개행 문자, 빈칸을 없애서 코드의 크기를 최소화하는 방법
  
  - 이미지 Base64 인코딩 : 이미지 파일을 64진법으로 이루어진 문자열로 인코딩하는 방법으로 서버와의 연결을 열고 이미지에 대해 서버 HTTP 요청을 할 필요가 없지만 크기가 더 커지는 문제

- 인코딩
  
  - 정보의 형태나 형식을 표준화, 보안, 처리 속도 향상, 저장 공간 절약 등을 위해 다른 형태나 형식으로 변환하는 처리 방식



- HTTP/1.1
  
  - 한 번 TCP 초기화를 한 뒤 여러 개의 파일 전송 가능
  
  - 문서 안에 포함된 다수의 리소스를 처리하려면 요청할 리소스 개수에 비례해서 대기 시간이 길어지는 단점
  
  - HOL Blocking(Head Of Line Blocking)
    
    - 네트워크에서 같은 큐에 있는 패킷이 그 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상
  
  - 무거운 헤더 구조



- HTTP/2
  
  - HTTP/1.x보다 지연 시간을 줄이고 응답 시간을 더 빠르게
  
  - 멀티 플렉싱 
    
    - 여러 개의 스트림을 사용하여 송수신, 특정 스트림의 패킷이 손실되어도 해당 스트림에만 영향을 미치고 나머지 스트림은 멀쩡하게 동작
    
    - 단일 연결을 사용하여 병렬로 여러 요청을 받을 수 있고 응답을 줄 수 있음 → HOL Blocking 문제 해결
  
  - 헤더 압축
    
    - 허프만 코딩 알고리즘 사용
    
    - 허프만 코딩 : 문자열을 문자 단위로 쪼개 빈도수를 세어 빈도가 높은 정보는 적은 비트 수를 사용하여 표현하고 빈도가 낮은 정보는 비트 수를 많이 사용하여 표현해서 전체 데이터의 표현에 필요한 비트양을 줄이는 원리
  
  - 서버 푸시
    
    - 클라이언트 요청 없이 서버가 바로 리소스 푸시 가능
  
  - 요청의 우선 순위 처리 지원



- 스트림
  
  - 시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 흐름

- HTTPS
  
  - 애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 SSL/TLS 계층을 넣은, 신뢰할 수 있는 HTTP 요청 → 통신을 암호화
  
  - HTTP/2는 HTTPS 위에서 동작
  
  - SSL/TLS 
    
    - 전송 계층에서 보안을 제공하는 프로토콜
    
    - 클라이언트와 서버가 통신할 때 제 3자가 메시지를 도청하거나 변조하지 못하도록 함
    
    - 보안 세션을 기반으로 데이터를 암호화하며 보안 세션이 만들어질 때 인증 메커니즘, 키 교환 암호화 알고리즘, 해싱 알고리즘 사용
    
    - 보안 세션 : 보안이 시작되고 끝나는 동안 유지되는 세션으로 SSL은 handshake를 통해 보안 세션을 생성하고 이를 기반으로 상태 정보 등을 공유한다.
    
    - 세션 : 운영 체제가 어떤 사용자로부터 자신의 자산을 이용을 허락하는 일정 기간
    
    - 클라이언트에서 cypher suites를 서버에 전달하면 서버는 받은 cypher suites의 암호화 알고리즘 리스트를 제공할 수 있는지 확인 → 제공할 수 있으면 서버에서 클라이언트로 인증서를 보내는 인증 메커니즘 시작, 이후 해싱 알고리즘 등으로 암호화된 데이터의 송수신 시작
    
    - cypher suites : 프로토콜, AEAD 사이퍼 모드, 해싱 알고리즘이 나열된 규약
    
    - AEAD 사이퍼 모드 : 데이터 암호화 알고리즘
  
  - 인증 메커니즘
    
    - CA(Certificate Authorities)에서 발급한 인증서를 기반
    
    - CA에서 발급한 인증서는 공개키를 클라이언트에 제공하고 사용자가 접속한 서버가 신뢰할 수 있는 서버임을 보장
    
    - 인증서는 서비스 정보, 공개키, 지문, 디지털 서명 등으로 이루어져 있음
  
  - 암호화 알고리즘
    
    - 대수곡선 기반의 ECDHE
    
    - 모듈식 기반의 DHE
    
    - 디피 헬만 키 교환 암호화 알고리즘 
  
  - 해싱 알고리즘
    
    - SHA-256 알고리즘 : 해쉬 함수의 결괏값이 256비트인 알고리즘으로 해싱 해야할 메시지에 전처리를 하고 전처리된 메시지를 기반으로 해시 반환



- HTTP/3
  
  - QUIC 위에서 동작하고 UDP 기반
  
  - 멀티 플렉싱
  
  - 초기 연결 설정 시 지연 시간 감소
    
    - 3way handshake를 거치지 않음
    
    - 순방향 오류 수정 메커니즘(FEC, Forward Error Correction) → 전송한 패킷이 손실되었다면 수신 측에서 에러를 검출하고 수정



- HTTPS 구축 방법
  
  - 직접 CA에서 구매한 인증키를 기반으로 HTTPS 서비스 구축
  
  - 서버 앞단의 HTTPS를 제공하는 로드 밸런서
  
  - 서버 앞단에 HTTPS를 제공하는 CDN









