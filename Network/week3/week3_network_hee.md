## Distance vector algorithm

* 예측한 정보를 토대로 거리 계산  
* 벨만 포드 알고리즘을 이용하여 최단 거리 도출  
* count-to-infinity : 라우팅 루프가 도는 현상  
  * poison reverse  

![image-20220620000411344](week3_network_hee.assets/image-20220620000411344.png)  

![image-20220620000538935](week3_network_hee.assets/image-20220620000538935.png)  

![image-20220620000813762](week3_network_hee.assets/image-20220620000813762.png)  

<br>

## routing in the Internet

* AS : Autonomous Systems
  * An autonomous system is an autonomous routing domain that has been assigned an Autonomous System Number
  * Intra-AS routing
    * 같은 AS 내부 호스트와 라우터들 간의 라우팅
    * 정책 결정이 필요없음
    * cost 기준

  ![image-20220620001704985](week3_network_hee.assets/image-20220620001704985.png)
  
  ![image-20220620001732585](week3_network_hee.assets/image-20220620001732585.png)
  
  
    * Inter-AS routing
  
  
      * 서로 다른 AS 시스템 간의 라우팅
      * cost < policy
  
  
      * BGP : Border Gateway Protocol
  
        * Policy-Based routing protocol
  

![image-20220620003654701](week3_network_hee.assets/image-20220620003654701.png)

* AS-Path : 해당 AS까지 경로 주소들을 기록
  * 새로운 AS는 왼쪽에 추가
  * 서로 다른 neighbor로부터 동일한 BGP 수신 시 AS-Path 주소에 짧은 AS 선택


![image-20220620002508690](week3_network_hee.assets/image-20220620002508690.png)

<br>

  ## Link layer

* MAC(Multiple Access links) protocols

  * point to point
  * broadcast(shared wire or medium)
  * three broad classes
    * channel partitioning
    * random access
    * taking turns

* Channel partitioning MAC protocols

  * TDMA(time division multiple access)
  * FDMA(frequency division multiple access)
  * 자원이 낭비되는 문제

* Random access protocols

  * ALOHA
  * CSMA(carrier sense multiple access)
    * listen before transmit
    * collision can still occur : propagation delay 때문

  ![image-20220620010516758](week3_network_hee.assets/image-20220620010516758.png)

  * 충돌이 감지되면 전송을 멈춘다.

  ![image-20220620011023390](week3_network_hee.assets/image-20220620011023390.png)

* "Taking turns" MAC protocols

![image-20220620011645111](week3_network_hee.assets/image-20220620011645111.png)

<br>

## LAN(Local Area Network)

* Ethernet

![image-20220620161359552](week3_network_hee.assets/image-20220620161359552.png)

![image-20220620161442067](week3_network_hee.assets/image-20220620161442067-16557092833881.png)

![image-20220620161637972](week3_network_hee.assets/image-20220620161637972.png)

<br>

* ARP(address resolution protocol)

![image-20220620163258468](week3_network_hee.assets/image-20220620163258468.png)

<br>

![image-20220620165043927](week3_network_hee.assets/image-20220620165043927.png)

![image-20220620165059631](week3_network_hee.assets/image-20220620165059631.png)

![image-20220620165125804](week3_network_hee.assets/image-20220620165125804.png)

![image-20220620165151364](week3_network_hee.assets/image-20220620165151364.png)

<br>

* switching
  * switching forward table : each switch has a switch table
  * switch learns which hosts can be reached through which interfaces
  * destination의 위치를 모르는 경우 : flood
  * destination의 위치를 아는 경우 : selectively send on just one link

![image-20220620170013698](week3_network_hee.assets/image-20220620170013698.png)

![image-20220620170645882](week3_network_hee.assets/image-20220620170645882.png)

![image-20220620171757731](week3_network_hee.assets/image-20220620171757731.png)

<br>

## data center network

![image-20220620171842466](week3_network_hee.assets/image-20220620171842466.png)

<br>

## wireless network

* infrastructure : 기지국을 통한 통신
* ad hoc : 기지국 없이 단말끼리 네트워크 구성

![image-20220620173511487](week3_network_hee.assets/image-20220620173511487.png)

![image-20220620175444047](week3_network_hee.assets/image-20220620175444047.png)

![image-20220620175510927](week3_network_hee.assets/image-20220620175510927.png)

* AP(access point) 
* BSS(basic service set) : cell,  AP가 만든 네트워크

![image-20220620175633876](week3_network_hee.assets/image-20220620175633876.png)

<br>

* 802.11 : passive / active scanning

![image-20220620175807073](week3_network_hee.assets/image-20220620175807073.png)

![image-20220620180121725](week3_network_hee.assets/image-20220620180121725.png)

![image-20220620180258192](week3_network_hee.assets/image-20220620180258192.png)

* RTS(Ready to send)
* CTS(Clear to send)

![image-20220620180425042](week3_network_hee.assets/image-20220620180425042.png)

<br>

![image-20220620180631896](week3_network_hee.assets/image-20220620180631896.png)

<br>

* 본인 addr는 제거한 frame을 전송

![image-20220620180703589](week3_network_hee.assets/image-20220620180703589.png)



