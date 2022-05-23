| Week 4 | Lecture 22~28 |
| :----: | :-----------: |

<br/>

<br/>

# Virtual Memory

---

<br/>

### Demand Paging

- 실제로 필요할 때 page를 메모리에 올리는 것
  - I/O양의 감소
  - Memory 사용량 감소
  - 빠른 응답 시간
  - 더 많은 사용자 수용
- Valid / Invalid bit의 사용
  - Invalid
    - 사용되지 않는 주소 영역인 경우
    - 페이지가 물리적 메모리에 없는 경우
  - 처음에는 모든 page entry가 invalid로 초기화
  - address translation 시에 invalid bit이 set되어 있으면 page fault
    - Page Fault
      - Invalid page를 접근하면 MMU가 trap을 발생시킴 (page fault trap)
      - Kernel mode로 들어가서 page fault handler가 invoke됨
      - 해당 페이지를 disk에서 memory로 읽어옴: 운영체제는 페이지 테이블을 확인하여 가상 메모리에 페이지가 존재하는지 확인하고 없으면 프로세스를 중단하고 현재 물리 메모리에 비어 있는 프레임이 있는지 찾음. 물리 메모리에도 없다면 스와핑 발동
      - 이 프로세스가 CPU를 잡고 다시 running
      - 중단되었던 instruction 재개

<br/>

### Page Replacement Algorithm

어떤 frame을 빼앗아올지 결정

곧바로 사용되지 않을 page를 쫓아내는 것이 좋음

동일한 페이지가 여러번 메모리에서 쫓겨났다가 다시 들어올 수 있음

page-fault rate을 최소화하는 것이 목표

주어진 page reference string에 대해 page fault를 얼마나 내는지 조사

- Optimal Algorithm(offline algorithm)
  - 가장 먼 미래에 참조되는 page를 replace
  - 그러나 미래에 사용되는 프로세스를 알 수는 없음 -> 사용할 수 없는 알고리즘
    - 다른 알고리즘의 성능에 대한 기준 제공
- FIFO(First In First Out) Algorithm
  - 먼저 들어온 것을 먼저 내쫓음
- LRU(Least Recently Used) Algorithm
  - 가장 오래전에 참조 된 것을 내쫓음
- LFU(Least Frequently Used) Algorithm
  - 참조 횟수가 가장 적은 페이지를 지움

<br/>

### 다양한 캐슁 환경

- 캐슁 기법

  - 한저된 빠른 공간(=캐쉬)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로부터 직접 서비스하는 방식
  - paging system외에도 cache memory, buffer caching, Web caching등 다양한 분야에서 사용

- 캐쉬 운영의 시간 제약

  - 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음

  - Buffer caching이나 Web caching의 경우

    - O(1)에서 O(log n)까지 허용

  - Paging system인 경우

    - page fault인 경우에만 OS가 관여

    - 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 OS가 알수없음

    - O(1)인 LRU의 list조작조차 불가능

      - Paging System에서 LRU, LFU 사용 가능한가?

        => 불가능: 주소 변환은 하드웨어가 하고 운영체제는 모르기 때문

        LRU는 오랜 기간동안 사용되지 않은 페이지를 찾아야 하고

        LFU는 참조 횟수가 가장 작은 페이지를 찾아야 함

        이 때 page fault가 나지 않으면 운영체제는 어떤 페이지가 몇번 사용되었는지 모름

    - Clock Algorithm(LRU 근사)

      - second-chance, Not Used Recently
      - Reference bit을 사용해서 교체 대상 페이지 선정
      - 시계방향을 돌면서 reference bit가 0인 것을 찾고 0을 찾은 순간 해당 프로세스를 교체하고 해당 부분을 1로 바꿈(이동 중 1은 모두 0으로 바꿈)
      - 한 바퀴 되돌아와서도(=second chance) 0이면 replace
      - 자주 사용되는 페이지라면 second chance가 올 때 1
      - reference bit과 modified bit 함께 사용
        - reference vit = 1: 최근에 참조된 페이지
        - modified bit = 1: 최근에 변경되 페이지(I/O를 동반하는 페이지)

<br/>

### Page Frame의 Allocation

각 process에 얼마만큼의 page framd을 할당할 것인가?

- Allocation의 필요성
  - 메모리 참조 명령어 수행시 명령어, 데이터 등 여러 페이지 동시 참조
    - 명령어 수행을 위해 최소한 할당되어야 하는 frame의 수가 있음
  - Loop를 구성하는 page들은 한꺼번에 allocate 되는 것이 유리
    - 최소한의 allocation이 없으면 매 loop마다 page fault
- Allocation Scheme
  - Equal allocation: 모든 프로세스에 똑같은 갯수 할당
  - Proportional allocation: 프로세스 크기에 비례하여 할당
  - Priority allocation: 프로세스의 priority에 따라 다르게 할당

<br/>

### Global vs Local Replacement

- Global replacement
  - Replace시 다른 process에 핟랑된 frame을 빼앗아 올 수 있음
  - Process별 할당량을 조절
  - FIFO, LRU, LFU등의 알고리즘을 global replacement로 사용시에 해당
  - Working set, PFF 알고리즘 사용
- Local replacement
  - 자신에게 할당된 frame 내에서만 replacement
  - FIFO, LRU, LFU등의 알고리즘을 process 별로 운영시에 해당

<br/>

### Thrashing

- 프로세스의 원활한 수행에 필요한 최소한의 page frame 수를 할당 받지 못한 경우
- Page fault rate가 매우 높아짐
- CPU utilization이 낮아짐
- OS는 Multiprogramming degree를 높여야 한다고 판단
- 또 다른 프로세스가 시스템에 추가됨
- 프로세스 당 할당된 frame 수가 더욱 감소
- 프로세스는 바쁘고 CPU는 한가 => 악순환 반복
- 해결 알고리즘
  - Working-Set Model
    - 프로세스의 과거 사용 이력인 지역성을 통해 결정된 페이지 집합을 만들어서 미리 메모리에 로드
    - process의 working set 전체가 메모리에 올라와 있어야 수행되고 그렇지 않은 경우 모든 frame을 반납한 후 swap out
  - PFF(Page-Fault Frequency) Scheme
    - page-fault rate의 상한값과 하한값을 만듬
      - 상한선에 도달하면 페이지를 늘리고 하한선에 도달하면 페이지를 줄임
    - 빈 frame이 없으면 일부 프로세스를 swap out

<br/>

### Page Size의 결정

- Page size를 감소시키면
  - 페이지 수 증가, 페이지 테이블 크기 증가
  - Internal fragmentation 감소, Disk transfer의 효율성 감소
  - 필요한 정보만 메모리에 올라와 메모리 이용히 효율적
    - Locality의 활용 측면에서는 좋지 않음
- Trend
  - Larger page size

<br/>

<br/>

# File Systems

---

<br/>

### File and File System

- File
  - 디스크에 저장된 데이터, 파일 서버에 제공한 데이터
  - 운영체제에서 다양한 저장 장치를 File이라는 동일한 논리적 단위로 볼 수 있게 해줌

- File attribute
  - 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들
  - open("/a/b/c")
    - 디스크로부터 파일 c의 메타데이터를 메모리로 가지고 옴
    - 이를 위하여 directory path를 search
    - Directory path의 search에 너무 많은 시간 소요
      - Open을 read/write와 별도로 두는 이유
      - 한번 open한 파일은 read / write시 directory search 불필요

    - Open file table
      - 현재 open 된 파일들의 메타데이터 보관소
      - 디스크의 메타데이터보다 몇 가지 정보 추가(Open한 프로세스의 수, File offset)

    - File descriptor
      - Open file table에 대한 위치 정보(프로세스 별)

- File System
  - 운영체제에서 파일을 관리하는 부분
  - 파일 및 파일의 메타데이터, 디렉토리 정보 등을 관리
  - 파일의 저장 방법 결정
  - 파일 보호


<br/>

### Directory and Logical Disk

- Directory
  - 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일
  - 그 디렉토리에 속한 파일 이름 및 파일 attribute들
- Partition(=Logical Disk)
  - 하나의 디스크 안에 여러 파티션을 두는게 일반적
  - 여러 개의 물리적인 디스크를 하나의 파티션으로 구성하기도 함
  - 물리적 디스크를 파티션으로 구성한 뒤 각각의 파티션에 file system을 깔거나 swapping 등 다른 용도로 사용할 수 있음

<br/>

### File Protection

각 파일에 대해 누구에게 어떤 유형의 접근(read/write/execution)을 허락할 것인가?

- Access Control 방법
  - Access control Matrix
    - Access control list: 파일별로 누구에게 어떤 접근 권한이 있는지 표시
    - Capability: 사용자별로 자신이 접근 권한을 가진 파일 및 해당 권한 표시
  - Grouping
    - 전체 user를 owner, group, public의 세 그룹으로 구분
    - 각 파일에 대해 세 그룹의 접근 권한을 3비트씩으로 표시
    - ex) UNIX
  - Password
    - 파일마다 password를 두는 방법
    - 모든 접근 권한에 대해 하나의 password: all-or-nothing
    - 접근 권한별 password: 암기 문제, 관리 문제
- Access Methods
  - 시스템이 제공하는 파일 정보의 접근 방식
    - 순차 접근
      - 카세트 테이프를 사용하는 방식처럼 접근
      - 읽거나 쓰면 offset은 자동적으로 증가
    - 직접 접근
      - LP 레코드 판과 같이 접근하도록 함
      - 파일을 구성하는 레코드를 임의의 순서로 접근할 수 있음

<br/>


<br/>

# File System Implementation

---

<br/>

### Allocation of File Data in Dist

#### ✔ Contiguous Allocation

- 단점
  - External fragmentation
  - File grow가 어려움
    - file 생성시 얼마나 큰 hole을 배당할 것인가?
    - grow 가능 vs 낭비

- 장점
  - Fast I/O
    - 한번의 seek/rotation으로 많은 바이트 transfer
    - Realtime file용으로 또는 이미 run중이던 process의 swapping용

  - Direct access 가능 


#### ✔ Linked Allocation

- 장점
  - External fragmentation 발생 안 함

- 단점
  - No random access
  - Reliability 문제
    - 한 sector가 고장나 pointer가 유실되면 많은 부분을 잃음

  - pointer를 위한 공간이 block의 일부가 되어 공간 효율성을 떨어뜨림

- 변형
  - File-allocation table 파일 시스템
    - 포인터를 별도의 위치에 보관하여 reliability와 공간효율성 문제 해결


#### ✔ Indexed Allocation

- 장점
  - External fragmentation이 발생하지 않음
  - Direct access 가능
- 단점
  - small file의 경우 공간 낭비
  - Too Large file의 경우 하나의 block로 index를 저장하기에 부족
    - 해결방안1. linked-scheme
    - 해결방안2. multi-level index

<br/>

### UNIX 파일시스템의 구조

- Boot block
  - 부팅에 필요한 정보
  - UNIX 시스템 뿐 아니라 어떤 파일 시스템이라도 가장 먼저 나옴

- Super block
  - 파일 시스템에 관한 총체적 정보 담고 있음
  - 어디까지가 빈 블록이고 어디부터 실제 사용중인 블록인지 등을 관리

- Inode list

  - 파일 하낭 inode 하나씩 할당, 파일 이름을 제외한 파일의 모든 메타데이터 저장
  - direct blocks부터 triple indirect까지 파일의 위치 정보를 저장, 크기가 작은 파일의 경우 direct blocks만으로 파일의 위치 정보를 저장, 크기가 커질수록 더 많은 indirect 블록들을 함께 사용
- Data block
  - 파일의 실제 내용 보관
  - UNIX 파일 시스템에서 data block 내의 디렉토리는 메타 데이터 중 하나인 파일의 이름, 그리고 inode 번호만을 가지고 있음

<br/>

### FAT 파일 시스템

파일의 메타 데이터 중 위치 정보만을 FAT에 보관

<br/>

### Free-Space Management

- Bit map / Bit vector
  - 비어 있으면 0, 비어 있지 않으면 1
  - bit map은 부가적인 공간을 필요로 함
  - 연속적인 n개의 free block을 찾는데 효과적
- Linked list
  - 모든 free block들을 링크로 연결
  - 연속적인 가용공간을 찾는 것은 쉽지 않음 (실제로 쓰기 어려움)
  - 공간의 낭비가 없음
- Grouping
  - linked list 방법의 변형
  - 첫 번째 free block이 n개의 pointer를 가짐
    - n-1 pointer는 free data block을 가리킴
    - 마지막 pointer가 가리키는 block은 또 다시 n pointer를 가짐
- Counting
  - 프로그램들이 종종 여러 개의 연속적인 block을 할당하고 반납한다는 성질에 착안
  - 빈 블록의 첫 번째 위치(first free block)와, 그 블록에서부터 몇 개의 블록이 비어있는지를 하나의 쌍으로 저장(연속된 n개의 블록을 찾기에 적합)

<br/>

### Directory Implementation

- Linear List
  - <file name, file의 metadata>의 list
  - 구현이 간단
  - 디렉토리 내에 파일이 있는지 찾기 위해서는 linear search 필요(time-consuming)
- Hash Table
  - linear list + hashing
  - Hash table은 file name을 이 파일의 linear list의 위치로 바꾸어줌
  - search time을 없앰
  - Collision 발생 가능
- File의 metadata의 보관 위치
  - 디렉토리 내에 직접 보관
  - 디렉토리에는 포인터를 두고 다른 곳에 보관
    - UNIX 파일 시스템의 inode, FAT 파일 시스템의 FAT 등
- Long file name의 지원
  - <file name, file의 metadata>의 list에서 각 entry는 일반적으로 고정
  - file name이 고정 크기의 entry 길이보다 길어지는 경우 entry의 마지막 부분에 이름의 뒷부분이 위치한 곳의 포인터를 두는 방법
  - 이름의 나머지 부분은 동일한 directory file의 일부에 존재

<br/>

### VFS and NFS

- Virtual File System (VFS)
  - 서로 다른 다양한 파일 시스템에 대해 동일한 시스템 콜 인터페이스(API)를 통해 접근할 수 있게 해주는 OS의 layer
- Network File System (NFS)
  - 분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있음
  - NFS는 분산 환경에서의 대표적인 파일 공유 방법

<br/>

### Page Cache and Buffer Cache

- Page Cache
  - Virtual memory의 paging system에서 사용하는 page frame을 caching의 관점에서 설명하는 용어
  - Memory-Mapped I/O를 쓰는 경우 file의 I/O에서도 page cache 사용
- Memory-Mapped I/O
  - 파일의 일부를 가상 메모리에 mapping시킴
  - 매핑시킨 영역에 대한 메모리 접근 연산은 파일의 입출력을 수행하게 함(파일을 접근할 때 read/write 콜을 하는 대신 메모리 영역에 맵핑시킨 영역을 참고하면 됨)
- Buffer Cache
  - 파일 시스템을 통한 I/O 연산은 메모리의 특정 영역인 Buffer cache 사용
  - 파일 사용의 locality를 활용
    - 한번 읽어온 block에 대한 후속 요청시 buffer cache에서 즉시 전달
  - 모든 프로세스가 공용으로 사용
  - Replacement 알고리즘이 필요 (LRU, LFU 등)
- Unified Buffer Cache
  - 최근의 OS에서는 기존의 버퍼 캐시가 페이지 캐시에 통합됨
  - Unified Buffer Cache를 이용하지 않는 파일 입출력
    - 파일을 open한 뒤, read/write 시스템 콜
      - 운영체제는 해당하는 파일 내용이 버퍼 캐시에 있는지 확인한 후 있으면 바로 전달해주고 없으면 디스크 파일 시스템에서 읽어와서 버퍼 캐시에 저장한 뒤 전달
    - Memory-Mapped I/O
      - map() 시스템 콜을 하면 자신의 주소 공간 중 일부를 파일에 매핑, 이후 내 메모리의 영역에 메모리를 읽고 쓰는 방식으로 파일 입출력
  - Unified Buffer Cache를 이용하는 파일 입출력
    - 파일을 open한 뒤, read/write 시스템 콜
    - Memory-Mapped I/O
      - Unified Buffer Cache를 이용하지 않았을 때와는 달리 페이지 캐시 자체가 사용자 프로세스의 논리적 주소 영역에 매핑되었으므로 페이지 캐시에 직접 읽고 쓸 수 있음

<br/>

<br/>

# Disk Management and Scheduling

<br/>

### Disk Structure

- Logical block
  - 디스크의 외부에서 보는 디스크의 단위 정보 저장 공간들
  - 주소를 가진 1차원 배열처럼 취급
  - 정보를 전송하는 최소 단위

- Sector
  - Logical block이 물리적인 디스크에 매핑된 위치
  - Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번재 섹터

<br/>

### Disk Management

- Physical formatting(Low-level formatting)
  - 디스크를 컨트롤러가 읽고 쓸수 있도록 섹터들로 나누는 과정
  - 각 섹터는 header + 실제 data + trailer로 구성
  - header와 trailer는 sector numver, ECC 등의 정보가 저장되며 controller가 직접 접근 및 운영
- Partitioning
  - 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
  - OS는 이것을 독립적 disk로 취급(logical disk)
- Logical formatting
  - 파일시스템을 만드는 것
  - FAT, inode, free space 등의 구조 포함
- Booting
  - ROM에 있는 small bootstrap loader의 실행
  - sector 0(boot block)을 load하여 실행
  - sector 0은 full Bootstrap loader program
  - OS를 디스크에서 load하여 실행

<br/>

### Disk Scheduling

- Access time의 구성
  - Seek time
    - 헤드를 해당 실린더로 움직이는데 걸리는 시간
  - Rotational latency
    - 헤드가 원하는 섹터에 도달하기까지 걸리는 회전지연시간
  - Transfer time
    - 실제 데이터의 전송 시간
- Disk bandwidth
  - 단위 시간 당 전송된 바이트의 수
- Disk Scheduling
  - seek time을 최소화 하는 것이 목표
  - Seek time은 seek distance에 비례

<br/>

### Disk Scheduling Algorithm

- FCFS(First Come First Service)
  - 들어온 순서대로 처리
- SSTF(Shortest Seek Time First)
  - 현재 헤드 위치에서 가장 가까운 요청부터 처리
  - starvation 문제 발생 가능
- SCAN(엘리베이터 알고리즘)
  - disk arm이 디스크의 한쪽 끝에서 다른쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
  - 다른 한쪽 끝에 도달하면 역방향으로 이동하며 오는 길목에 있는 모든 요청을 처리하며 다시 반대쪽 끝으로 이동
  - 문제점: 실린더 위치에 따라 대기 시간이 다름
- C-SCAN
  - 헤드가 한쪽 끝에서 다른쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
  - 다른쪽 끝에 도달했으면 요청을 처리하지 않고 곧바로 출발점으로 다시 이동
  - SCAN보다 이동 거리는 길어질 수 있지만 균일한 대기 시간을 제공
- N-SCAN
  - SCAN 변형
  - 일단 arm이 한 방향으로 움직이기 시작하면 그 시점 이후에 도착한 job은 가는 도중에 처리하지 않고 되돌아올 때 service
- LOOK and C-LOOK
  - SCAN이나 C-SCAN은 언제나 헤드가 디스크 끝에서 끝으로 이동하지만 LOOK과 C-LOOK은 헤드가 진행 중이다가 그 방향에 더 이상 기다리는 요청이 없으면 헤드의 이동 방향을 즉시 반대로 이동
- Disk-Scheduling Algorithm의 결정
  - 현재 SCAN에 기반한 알고리즘을 주로 씀
  - SCAN, C-SCAN 및 그 응용 알고리즘인 LOOK, C-LOOK 등이 일반적으로 디스크 입출력이 많은 시스템에서 효율적인 것으로 알려져 있음
  - 파일의 할당 방법에 따라 디스크 요청이 영향을 받음
  - 디스크 스케줄링 알고리즘은 필요할 경우 다른 알고리즘으로 쉽게 교체할 수 있도록 OS와 별도의 모듈로 작성되는 것이 바람직

<br/>

### Swap-Space Management

- Disk를 사용하는 두가지 이유
  - memory의 volatile한 특성
  - 프로그램 실행을 위한 memory 공간 부족
- Swap-space
  - Virtual memory system에서는 디스크를 memory의 연장 공간으로 사용
  - 파일시스템 내부에 둘 수도 있으나 별도 partition 사용이 일반적
    - 공간 효율성 보다는 속도 효율성이 우선
    - 일반 파일보다 훨씬 짧은 시간만 존재하고 자주 참조됨
    - block의 크기 및 저장 방식이 일반 파일시스템과 다름

<br/>

### RAID

여러 개의 디스크를 묶어서 사용하는 것

- RAID의 목적
  - 디스크의 처리 속도 향상
  - 신뢰성 향상

<br/>

<br/>
