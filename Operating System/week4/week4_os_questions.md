# ✨ week4_os_questions ✨

#### ✏ RAID의 사용 목적 두 가지

- 디스크의 처리 속도 향상 
- 신뢰성 향상

<br>

#### ✏ 디스크에 파일을 저장할 때의 방법 3가지

* Contiguous Allocation
* Linked Allocation
* Indexed Allocation

<br>

#### ✏ Paging System에서 LRU, LFU가 불가능한 이유

* CPU는 trap이 발생하기 전까지 어떤 페이지가 가장 오래 전에 혹은 가장 적게 참조되었는지 알 수 없기 때문이다.

<br>

#### ✏ 현대에서 disk scheduling 알고리즘은 어떤 알고리즘을 기본으로 하는가

* SCAN

<br>

#### ✏ Disk를 사용하는 두 가지 이유

* 메모리의 volatile한 특성(휘발성)
* 프로그램 실행을 위한 메모리 공간 부족

<br>

#### ✏ Disk Scheduling에서 Access time의 구성 세 가지를 말해주세요.

* Seek time : 헤드를 해당 실린더로 움직이는데 걸리는 시간 

* Rotational latency : 헤드가 원하는 섹터에 도달하기까지 걸리는 회전 지연 시간 
* Transfer time : 실제 데이터의 전송 시간

<br>

#### ✏ Web에서 caching이란?

* 멀리 있는 웹 서버에서는 가져올 때 사용자 브라우저에 정보를 일시 저장해두고 동일한 url에 대한 요청이 들어올 때면 웹 서버까지 가지 않고 사용자 브라우저에서 불러오는 것

<br>

#### ✏ LRU와 LFU는 어떤 알고리즘인지 설명해주세요.

* LRU:가장 오래 전에 참조된 것을 지움 
* LFU:참조 횟수가 가장 적은 페이지를 지움

<br>

#### ✏ Thrashing에서 page size를 감소시켰을 때 발생하는 현상에 대해서 설명해주세요.

* 페이지 수 증가

  * 페이지 테이블 크기 증가
  * Internal fragmentation 감소
  * Disk transfer의 효율성 감소
  * 필요한 정보만 메모리에 올라와 메모리 이용이 효율적

<br>

#### ✏ C-SCAN은 SCAN보다 이동 거리가 길어질 수 있다는 단점이 있다. 그렇다면 더 나은점은?

* 더 균일한 대기 시간을 제공할 수 있다.

<br>

#### ✏ PFF(Page-Fault Frequency) 알고리즘에 대해 간단하게 설명해주세요.

* 매번 page fault 가 직접 일어나는지 체크하며 메모리에 올라간 프로세스의 수를 조절하는 알고리즘
* Page fault rate가 상한값을 넘으면 frame을 더 할당한다.
* Page fault rate가 하한값 이하이면 할당 frame 수를 줄인다.

<br>

#### ✏ 파일 접근 권한 제한 방법에 대해서 말해주세요.

* access control matrix
* grouping
* password

<br>

#### ✏Linked Allocation의 단점에 대해 말해주세요.

* Reliability 문제 (한 sector가 고장나 pointer가 유실되면 많은 부분을 잃음)
* Pointer를 위한 공간이 block의 일부가 되어 공간 효율성을 떨어뜨림

<br>

#### ✏ Disk 구조에서 sector에 대해 말해주세요.

* Logical block이 물리적인 디스크에 매핑된 위치
* Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번재 섹터
* 각 섹터는 header + 실제 data + trailer로 구성

<br>

#### ✏ SSTF(Shortest Seek Time First)에 대해 설명해주세요

* 현재의 헤드 위치에서 제일 가까운 위치에 있는 헤드부터 처리해서 이동거리가 줄어든다. 
* starvation 문제가 발생한다는 단점이 있다.

<br>

#### ✏ Clock 알고리즘을 개선하기 위해 reference bit과 modified bit을 사용하는데 두개의  bit는 무엇을 의미하나요?

* reference bit =1은 최근에 참조된 페이지
* modified bit = 1은 최근에 변경된 페이지

<br>

#### ✏ Indexed Allocation의 단점에 대해 설명해주세요

* 파일의 크기가 작으면 공간이 낭비된다.
* 파일의 크기가 너무 크면 하나의 block으로 index를 저장하기 부족하다.

<br>

#### ✏ 페이지 교체 알고리즘 중 Optimal Algorithm(off-line algorithm)은 가장 좋은 방법이다. 그러나 실제 사용할 수는 없다. 이유는?

* 미래에 사용되는 프로세스를 알 수 없기 때문이다.

<br>

#### ✏ Replacement 알고리즘의 목표를 말해주세요.

* page-fault rate를 최소화해서 0에 가깝도록 만드는 것이 목표이다.

<br>

#### ✏ 데이터 섹터에서 에러 검출 및 교정을 위한 코드 (데이터를 해시 함수로 작게 요약)

* ECC(Error Correcting Code)

<br>