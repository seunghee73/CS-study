## TCP

* Flow Control
  * sender won't overflow receiver's buffer by transmitting too much, too fast

<br>

* Three way handshake
  1. client host sends TCP SYN segment to server
     * specifies initial seq #
     * no data
  2. server host receives SYN, replies with SYNACK segment
     * server allocates buffers
     * specifies sever initial seq #
  3. client receives SYNACK, replies with ACK segment, which may contain data

![image-20220612194124119](week2_network_hee.assets/image-20220612194124119.png)

![image-20220612204002639](week2_network_hee.assets/image-20220612204002639.png)

![image-20220612204117223](week2_network_hee.assets/image-20220612204117223.png)

<br>

* Congestion control
  * End-end congestion control
    * no explicit feedback from network
    * congestion inferred from end-system observed loss, delay
    * approach taken by TCP
  * Network-assisted congestion control
    * routers provide feedback to end system
    * single bit indicating congestion(SNA, DECbit, TCP/IP ECN, ATM)
    * explicit rate sender should send at

<br>

* TCP congestion control
  * Slow Start
    * Do not know bottleneck bandwidth
    * So start from zero and quickly ramp up
  * Additive increase
    * getting close to capacity
    * be conservative and increase slow
  * Multiplicative decrease
    * Packet drop

![image-20220612213134976](week2_network_hee.assets/image-20220612213134976.png)

![image-20220612213214372](week2_network_hee.assets/image-20220612213214372.png)

<br>

* TCP slow start
  * When connection begins, increase rate exponentially fast until first loss event
    * double CongWin every RTT
    * done by incrementing CongWin for every ACK received
  * **initial rate is slow but ramps up exponentially fast**

![image-20220612213530972](week2_network_hee.assets/image-20220612213530972.png)

<br>

* TCP Fairness

![image-20220612214930472](week2_network_hee.assets/image-20220612214930472.png)

![image-20220612214941725](week2_network_hee.assets/image-20220612214941725.png)

<br>

## HTTP

![image-20220612215443978](week2_network_hee.assets/image-20220612215443978-16550384849211.png)

![image-20220612215500238](week2_network_hee.assets/image-20220612215500238.png)

![image-20220612220126991](week2_network_hee.assets/image-20220612220126991.png)

<br>

## Network layer

* Network layer

  * transport segment from sending to receiving host
  * on sending side encapsulates segments into datagrams
  * on receiving side, delivers segments to transport layer
  * network layer protocols in every host, router
  * router examines header fields in all IP datagrams passing through it
  * forwarding : move packets from router's input to appropriate router output, process of getting through single interchange
  * routing : determine route taken by packets from source to dest, process of planning trip from source to dest

  ![image-20220612221817116](week2_network_hee.assets/image-20220612221817116.png)

  ![image-20220612221913153](week2_network_hee.assets/image-20220612221913153-16550399540182.png)

  ![image-20220612221931334](week2_network_hee.assets/image-20220612221931334.png)

  ![image-20220612221947662](week2_network_hee.assets/image-20220612221947662.png)

<br>

## Internet Protocol

* IP datagram format

![image-20220612222322985](week2_network_hee.assets/image-20220612222322985.png)



* IP Address (IPv4)
  * A unique 32-bit number
  * Identifies an interface (on a host, on a **router**, ...)
  * Represented in dotted-quad notation

![image-20220612222809660](week2_network_hee.assets/image-20220612222809660.png)

![image-20220612222842355](week2_network_hee.assets/image-20220612222842355.png)

![image-20220612222912179](week2_network_hee.assets/image-20220612222912179.png)

![image-20220612222937748](week2_network_hee.assets/image-20220612222937748.png)

![image-20220612222957123](week2_network_hee.assets/image-20220612222957123.png)

![image-20220612223930971](week2_network_hee.assets/image-20220612223930971.png)

![image-20220612223633003](week2_network_hee.assets/image-20220612223633003.png)

![image-20220612224125021](week2_network_hee.assets/image-20220612224125021.png)

![image-20220612224409489](week2_network_hee.assets/image-20220612224409489.png)

![image-20220612224424245](week2_network_hee.assets/image-20220612224424245.png)



![image-20220613121111542](week2_network_hee.assets/image-20220613121111542.png)

![image-20220613121248861](week2_network_hee.assets/image-20220613121248861.png)

<br>

## Dynamic Host Configuration Protocol

![image-20220613122448549](week2_network_hee.assets/image-20220613122448549.png)  

![image-20220613122734707](week2_network_hee.assets/image-20220613122734707.png)  

![image-20220613123908230](week2_network_hee.assets/image-20220613123908230-16550915490733.png)  

![image-20220613124305223](week2_network_hee.assets/image-20220613124305223.png)  

## ICMP : Internet Control Message Protocol

* ICMP
  * used by hosts & routers to communicate network-level information
    * error reporting : unreachable host, network, port, protocol
    * echo request/reply(used by ping)
  * network-layer "above" IP
    * ICMP msgs carried in IP datagrams
  * ICMP message : type, code plus first 8 bytes of IP datagram causing error

![image-20220613182752878](week2_network_hee.assets/image-20220613182752878.png)

![image-20220613182959669](week2_network_hee.assets/image-20220613182959669.png)

* 전체 네트워크의 비용 정보가 있는 경우

* 이웃 네트워크의 비용 정보만 있는 경우

  
