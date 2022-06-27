# ✨ week4_network_questions ✨

**✏ DASH란?**

- Dynamic Adaptive Streaming over HTTP - 하나의 데이터를 작은 단위로 쪼갬 - 각 단위별로 여러 버전의 인코딩을 해놓음 - 처음에 사용자가 영상을 시청할 때 낮은 버전 부터 틀어주고 네트워크 상황이 괜찮으면 점점 버전을 높여서 틈

<br>

**✏ cell tower (기지국 간) 무선 통신 방식은?**

- channel partitioning(FDMA/TDMA)

<br>

**✏ streaming 서비스에 UDP와 TCP를 그대로 사용하는 것이 어려운 이유에 대해 설명해주세요.**

- UDP는 네트워크 상황을 고려하지 않기 때문에 적합하지 않고 TCP는 네트워크가 좋지 않은 상황에 전송이 원활하지 않다.

<br>

**✏ cellular networks에서 CDMA에 대해 설명하세요**

- 사용자들에게 각기 다른 코드를 줘서 그 코드에 수학적 연산을 가함(자기 신호는 잘들리고 남의 신호는 노이즈 즈 처리가 되도록)

<br>

**✏ 네트워크 보안의 4요소는 무엇인가요?**

- 1. Confidentially 기밀성: sender와 receiver만 알아야 함
  2. authentication 인증: 상대방이라는 것을 확신할 수 있어야 함. 
  3. message integrity: 보내는 사람이 변하지 않고 받는 사람이 그대로 받아야 함.
  4. access and availability: 서비스를 제공하는 사람은 24시간 내내 사용자에게 서비스를 제공할 수 있어야 함.

<br>

**✏ HTTPS와 HTTP의 차이점**

- HTTPS는 HTTP와 달리 메시지를 TCP로 내려보낼 때 SSL (TLS) - 보안 요소-를 거친다.

<br>

**✏ 보안을 위해 Application내에 존재하는 층**

- SSL

<br>

**✏ RSA 암호화 방식에 대해 설명해주세요**

- 소인수 분해 방식을 사용해 public key, private key 암호화. Key의 순서는 상관이 없으나 더 많은 시간이 소요됨.

<br>

**✏ MAC(Message Authentication Code)에 대해서 설명해주세요. **

- 송신자를 보장하고 메시지의 내용이 변조되지 않았는지 확인하기 위한 코드로 Hash를 사용하여 만든다.

<br>

**✏ 스트리밍  서비스에서 많은 이용자에 의한 트래픽 요청을 처리하기위해 저장하는 서버 네트워크는? (지리적 제약 없이 전세계 사용자에게 콘텐츠를 전송할 수 있는 방식)**

- CDN

<br>

**✏ 버퍼링이 발생하는 이유는?**

- buffer에 들어오는 속도보다 나가는 속도가 빠를 때 발생하는 딜레이

<br>

**✏ Mobility via indirect routing에 대해 설명해주세요.**

- 우선 host는 home network와 home network에서 사용되는 permanent address를 갖게 된다. host가 다른 네트워크로 이동한 상황에서 외부 노드가 home agent로 요청을 보내면 home agent가 다시 visited network에 있는 host에게 전송하는 라우팅을 의미한다.