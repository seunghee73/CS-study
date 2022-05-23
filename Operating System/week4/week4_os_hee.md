## Demand Paging

* 실제로 필요할 때 page를 메모리에 올리는 것
  * I/O 양의 감소
  * Memory 사용량 감소
  * 빠른 응답 시간
  * 더 많은 사용자 수용
* Valid / Invalid bit의 사용
  * Invalid의 의미 
    * 사용되지 않는 주소 영역인 경우
    * 페이지가 물리적 메모리에 없는 경우
  * 처음에는 모든 page entry가 invalid로 초기화
  * address translation 시에 invalid bit이 set 되어 있으면 **page fault**

<br>

## Page Fault

* Invalid page를 접근하면 MMU가 trap을 발생시킴 (page fault trap)
* Kernel mode로 들어가서 page fault handler가 invoke됨
* 다음과 같은 순서로 page fault 처리
  * Invalid reference ? (eg. bad address, protection violation) ⇒ abort process
  * Get an empty page frame(없으면 뺏어온다 : replace)
  * 해당 페이지를 disk에서 memory로 읽어온다
    * disk I/O가 끝나기까지 이 프로세스는 CPU를 preempt 당함(block)
    * Disk read가 끝나면 page tables entry 기록, valid / invalid bit = "valid"
    * ready queue에 process를 insert ⇒ dispatch later
  * 이 프로세스가 CPU를 잡고 다시 running
  * 아까 중단되었던 instruction을 재개

<br>

## Free Frame이 없는 경우

* Page replacement
  * 어떤 frame을 빼앗아올지 결정해야 함
  * 곧바로 사용되지 않을 page를 쫓아내는 것이 좋음
  * 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 들어올 수 있음
* Replacement Algorithm
  * page-fault rate를 최소화하는 것이 목표
  * 알고리즘의 평가
    * 주어진 page reference string에 대해 page fault를 얼마나 내는지 조사
* Optimal Algorithm
  * MIN(OPT) : 가장 먼 미래에 참조되는 page를 replace
  * 미래의 참조를 어떻게 아는가? Offline algorithm
  * 다른 알고리즘의 성능에 대한 upper bound 제공
* FIFO(First In First Out) Algorithm
  * 먼저 들어온 것을 먼저 내쫓음
  * FIFO Anomaly(Belady's Anomaly)
    * more frames ⇒ more page faults
* LRU(Least Recently Used) Algorithm
  * 가장 오래 전에 참조된 것을 지움
* LFU(Least Frequently Used) Algorithm
  * 참조 횟수(reference count)가 가장 적은 페이지를 지움
  * 최저 참조 횟수인 page가 여럿 있ㅈ는 경우
    * LFU 알고리즘 자체에서는 여러 page 중 임의로 선정한다.
    * 성능 향상을 위해 가장 오래 전에 참조된 page를 지우게 구현할 수도 있다.
  * 장단점
    * LRU처럼 직전 참조 시점만 보는 것이 아니라 장기적인 시간 규모를 보기 때문에 page의 인기도를 좀더 정확히 반영할 수 있음
    * 참조 시점의 최근성을 반영하지 못함
    * LRU보다 구현이 복잡함 
    * LRU : O(1) complexity
    * LFU : O(log n) complexity (heap을 사용한 경우)

<br>

## 다양한 캐싱 환경

* 캐싱 기법
  * 한정된 빠른공간(=캐쉬)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로부터 직접 서비스하는 방식
  * paging system 외에도 cache memory, buffer cachinh, web caching 등 다양한 분야에서 사용
* 캐쉬 운영의 시간 제약
  * 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음
  * Buffer caching이나 Web caching의 경우
    * O(1) 에서 O(log n)정도까지 허용
  * Paging system인 경우
    * page fault인 경우에만 OS가 관여
    * 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 OS가 알 수 없음
    * O(1)인 LRU의 list 조작조차 불가능
* Clock Algorithm
  * LRU의 근사(approximation) 알고리즘
  * 여러 명칭으로 불림 : Second chance algorithm, NUR(Not Used Recently), NRU(Not Recently Used)
  * Reference bit를 사용해서 교체 대상 페이지 선정(circular list)
  * Reference bit가 0인 것을 찾을 때까지 포인터를 하나씩 앞으로 이동
  * 포인터 이동하는 중에 Reference bit 1은 모두 0으로 바꿈
  * Reference bit이 0인 것을 찾으면 그 페이지를 교체
  * 한 바퀴 되돌아와서도(=second chance) 0이면 그 때는 replace
  * 자주 사용되는 페이지라면 second chance가 올 때 1
  * Clock Algorithm의 개선
    * Reference bit과 modified bit(dirty bit)을 함께 사용
    * Reference bit = 1 : 최근에 참조된 페이지
    * Modified bit = 1 : 최근에 변경된 페이지 (I/O를 동반하는 페이지, 쫓아낼 때 disk에 변경된 내용을 써줘야함...)
* Page Frame의 allocation
  * Allocation problem : 각 process에 얼마만큼의 page frame을 할당할 것인가?
  * Allocation의 필요성
    * 메모리 참조 명령어 수행시 명령어, 데이터 등 여러 페이지 동시 참조
      * 명령어 수행을 위해 최소한 할당되어야하는 frame의 수가 있음
    * Loop를 구성하는 page들은 한꺼번에 allocate되는 것이 유리함
      * 최소한의 allocation이 없으면 매 loop마다 page fault
  * Allocation Scheme
    * Equal allocation : 모든 프로세스에 똑같은 갯수 할당
    * Proportional allocation : 프로세스 크기에 비례하여 할당
    * Priority allocatin : 프로세스의 priority에 따라 다르게 할당
* Global  vs Local Replacement
  * Global Replacement
    * Replace 시 다른 process에 할당된 frame을 빼앗아 올 수 있다.
    * Process별 할당량을 조절하는 또 다른 방법
    * FIFO, LRU, LFU 등의 알고리즘을 Global replacement로 사용시에 해당
    * Working set, PFF 알고리즘 사용
  * Local Replacement
    * 자신에게 할당된 frame 내에서만 replacement
    * FIFO, LRU, LFU 등의 알고리즘을 process별로 운영 시

## Thrashing

* Thrashing
  * 프로세스의 원활한 수행에 필요한 최소한의 page frame 수를 할당받지 못한 경우 발생
  * Page Fault rate가 매우 높아짐
  * CPU utilization이 낮아짐
  * OS는 MPD(Multiprogramming degree)를 높여야 한다고 판단
  * 또 다른 프로세스가 시스템에 추가됨(higher MPD)
  * 프로세스 당 할당된 frame의 수가 더욱 감소
  * 프로세스는 page의 swap in / swap out으로 매우 바쁨
  * 대부분의 시간에 CPU는 한가함
  * low throughput

* Working-Set Model

  * Locality of reference
    * 프로세스는 특정 시간 동안 일정 장소만을 집중적으로 참조한다.
    * 집중적으로 참조되는해당 page들의 집합을 locality set이라 함
  * Working-set Model
    * Locality에 기반하여 프로세스가 일정 시간 동안 원활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야 하는 page들의 집합을 Working Set이라 정의
    * Working Set 모델에서는 process의 working set 전체가 메모리에 올라와있어야 수행되고 그렇지 않을 경우 모든 frame을 반납한 후 swap out (suspend)
    * Thrashing을 방지
    * Multiprogramming degree를 결정
  * Working set의 결정
    * Working set window를 통해 알아냄

* PFF(Page-Fault Frequency) Scheme

  * page fault rate의 상한값과 하한값을 둔다
    * Page fault rate가 상한값을 넘으면 frame을 더 할당한다.
    * Page fault rate가 하한값 이하이면 할당 frame 수를 줄인다.
    * 빈 frame이 없으면 일부 프로세스를 swap out

* Page size의 결정

  * Page size를 감소시키면
    * 페이지 수 증가
    * 페이지 테이블 크기 증가
    * Internal fragmentation 감소
    * Disk transfer의 효율성 감소
      * Seek/rotation vs transfer
    * 필요한 정보만 메모리에 올라와 메모리 이용이 효율적
      * Locality의 활용 측면에서는 좋지 않음
  * Trend
    * Lager page size

  

## File and File System

* File
  * A named collection of related information
  * 일반적으로 비휘발성의 보조기억장치에 저장
  * 운영체제는 다양한 저장 자이를 file이라는 동일한 논리적 단위로 볼 수 있게 해줌
  * Operation
    * create, read, write, reposition, (lseek) delete, open, close
* File attribute(혹은 파일의 metadata)
  * 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들
    * 파일 이름, 유형, 저장된 위치, 파일 사이즈
    * 접근 권한(읽기 / 쓰기 / 실행), 시간(생성/ 변경 / 사용), 소유자 등
* File system
  * 운영체제에서 파일을 관리하는 부분
  * 파일 및 파일의 메타데이터, 디렉토리 등을 관리
  * 파일의 저장 방법 결정
  * 파일 보호 등
* Directory
  * 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일
  * 그 디렉토리에 속한 파일 이름 및 파일 attribute 들
  * Operation
    * search for a file, create a file, delete a file
    * list a directory, rename a file, traverse the file system
* Partition(=Logical Disk)
  * 하나의 (물리적) 디스크 안에 여러 파티션을 두는 게 일반적
  * 여러 개의 물리적인 디스크를 하나의  파티션으로 구성하기도 함
  * (물리적) 디스크를 파티션으로 구성한 뒤 각각의 파티션에 file system을 깔거나 swapping등 다른 용도로 사용할 수 있음
* open("a/b/c")
  * 디스크로부터 파일 c의 메타데이터를 메모리로 가지고 옴
  * 이를 위하여 directory path를 search
    * 루트 디렉토리 '/'를 open하고 그 안에 파일 'a'의 위치 획득
    * 파일 'a'를 open한 후 read하여 그 안에서 파일 'b'의 위치 획득
    * 파일 'b'를 open한 후 read항 그 안에서 파일 'c'의 위치 획득
    * 파일 'c'를 open
  * Directory path의 search에 너무 많은 시간 소요
    * Open을 read / write와 별도로 두는 이유
    * 한 번 open한 파일은 read / write 시 directory search 불 필요
  * Open file table
    * 현재 open된 파일의 메타데이터 보관소(in memory)
    * 디스크의 메타데이터보다 몇 가지 정보가 추가
      * open한 프로세스의 수
      * File offset : 파일 어느 위치에 접근 중인지 표시(별도 테이블 필요)
  * File descriptor(file handle, file control block)
    * Open file table에 대한 위치 정보(프로세스 별)
* File protection
  * 각 파일에 대해 누구에게 어떤 유형의 접근(read/write/execution)을 허락할 것인가?
  * Access Control 방법
    * Access control list : 파일별로 누구에게 어떤 접근 권한이 있는지 표시
    * Capability : 사용자별로 자신이 접근 권한을 가진 파일 및 해당 권한 표시
  * Grouping
    * 전체 user를 owner, group, public의 세 그룹으로 구분
    * 각 파일에 대해 세 그룹의 접근 권한(rwx)을 3비트 씩으로 표시
  * Password
    * 파일마다 password를 두는 방법 (디렉토리 파일에 두는 방법도 가능)
    * 모든 접근 권한에 대해 하나의 password : all or nothing
    * 접근 권한별 password : 암기 문제, 관리 문제
* Access Methods
  * 시스템이 제공하는 파일 정보 접근 방식
    * 순차 접근 (sequential access) : 카세트 테이프를 사용하는 방식처럼 접근, 읽거나 쓰면 offset은 자동적으로 증가
    * 직접 접근 (direct access, random access) : LP 레코드 판과 같이 접근하도록 함, 파일을 구성하는 레코드를 임의의 순서로 접근할 수 있음

<br>

## Allocation of File Data in Disk

* Contiguous Allocation
  * 단점
    * External fragmentation
    * File grow가 어려움 (file 생성 시 얼마나 큰 hole을 할당할 것인가? / grow 가능 vs 낭비 internal fragmentation)
  * 장점
    * Fase I/O (한번의 seek rotation으로 많은 바이트 transfer / Realtime file 용으로, 또는 이미 run 중이던 process의 swapping 용)
    * Direct access(=random access) 가능

* Linked Allocation
  * 단점
    * No random access
    * Reliability 문제 (한 sector가 고장나 pointer가 유실되면 많은 부분을 잃음)
    * Pointer를 위한 공간이 block의 일부가 되어 공간 효율성을 떨어뜨림
  * 장점
    * External fragmentation 발생하지 않음
  * 변형
    * File Allocation Table(FAT) 파일 시스템 (포인터를 별도의 위치에 보관하여 reliability와 공간효율성 문제 해결)
* Indexed Allocation
  * 단점
    * small file일 경우 공간 낭비(실제로 많은 file들이 small)
    * Too Large file인 경우 하나의 block으로 index를 저장하기에 부족(해결 방안 : linked scheme, multi-level index)
  * 장점
    * External fragmenation이 발생하지 않음
    * Direct access 가능

<br>

## UNIX 파일 시스템의 구조

* Boot block
  * 부팅에 필요한 정보 (Bootstrap loader)
* Superblock
  * 파일 시스템에 관한 총체적인 정보를 담고 있다.
* Inode
  * 파일 이름을 제외한 파일의 모든 메타 데이터를 저장
* Data block
  * 파일의 실제 내용을 보관

<br>

## FAT File System

* Boot block
* FAT
* Root directory
* Data block

<br>

## Free Space Management

* Bit map or bit vector
  * 부가적인 공간을 필요로 함
  * 연속적인 n개의 free block을 찾는데 효과적
* Linked list
  * 모든 free block들을 링크로 연결(free list)
  * 연속적인 가용공간으 찾는 것은 쉽지 않다
  * 공간의 낭비가 없다
* Grouping
  * linked list 방법의 변형
  * 첫번째 free block이 n개의 pointer를 가짐
    * n-1 pointer는 free data block을 가리킴
    * 마지막 pointer가 가리키는 block은 또 다시 n pointer를 가짐
* Counting
  * 프로그램들이 종종 여러 개의 연속적인 block을 할당하고 반납한다는 성질에 착안
  * (first free block, # of contiguous free blocks)을 유지

<br>

## Directory Implementation

* Linear list
  * <file name, file의 metadata>의 list
  * 구현이 간단
  * 디렉토리 내에 파일이 있는지 찾기 위해서는 linear search 필요 (time-consuming)
* Hash table
  * linear list + hashing
  * Hash table은 file name을 이 파일의 linear list의 위치로 바꾸어줌
  * search time을 없앰
  * Collision 발생 가능

* File의 metadata의 보관 위치
  * 디렉토리 내에 직접 보관
  * 디렉토리에는 포인터를 두고 다른 곳에 보관 (inode, FAT 등)
* Long file name 지원
  * <file name, file의 metadata>의 리스트에서 각 entry는 일반적으로 고정 크기
  * file name이 고정 크기의 entry 길이보다 길어지는 경우 entry의 마지막 부분에 이름의 뒷부분이 위치한 곳의 포인터를 두는 방법
  * 이름의 나머지 부분은 동일한 directory file 일부에 존재

<br>

## VFS and NFS

* Virtual File System(VFS)
  * 서로 다른 다양한 file system에 대해 동일한 시스템 콜 인터페이스(API)를 통해 접근할 수 있게 해주는 OS의 layer
* Network File System(NFS)
  * 분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있음
  * NFS는 분산 환경에서의 대표적인 파일 공유 방법

<br>

## Page Cache and Buffer Cache

* Page Cache
  * Virtual memory의 paging system에서 사용하는 page frame을 caching의 관점에서 설명하는 용어
  * Memory-Mapped I/O를 쓰는 경우 file의 I/O에서도 page cache 사용
* Memory-Mapped I/O
  * File의 일부를 virtual memory에 mapping 시킴
  * 매핑시킨 영역에 대한 메모리 접근 연산은 파일의 입출력을 수행하게 함
* Buffer Cache
  * 파일시스템을 통한 I/O 연산은 메모리의 특정 영역인 buffer cache 사용
  * file 사용의 locality 활용
    * 한 번 읽어온 block에 대한 후속 요청 시 buffer cache에서 즉시 전달
  * 모든 프로세스가 공용으로 사용
  * Replacement algorithm 필요(LRU, LFU 등)
* Unified Buffer Cache
  * 최근의 OS에서는 기존의 buffer cache가 page cache에 통합

<br>

## Disk Structure

* logical block
  * 디스크의 외부에서 보는 디스크의 단위 정보 저장 공간들
  * 주소를 가진 1차원 배열처럼 취급
  * 정보를 전송하는 최소 단위
* Sector
  * logical block 이 물리적인 디스크에 매핑된 위치
  * Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번째 섹터이다.

<br>

## Disk Management

* physical formatting(Low level formatting)

  * 디스크를 컨트롤러가 읽고 쓸 수 있도록 섹터들로 나누는 과정
  * 각 섹터는 header + 실제data + trailer로 구성
  * header와 trailer는 sector number, ECC 등의 정보가 저장되며 controller가 직접 접근 및 운영
  
* Partitioning
  * 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
  * OS는 이것을 독립적 disk로 취급(logical disk)

* Logical formatting
  * 파일시스템을 만드는 것
  * FAT, inode, free space 등의 구조 포함

* Booting
  * ROM에 있는 small bootstrap loader의 실행
  * sector 0 (boot block)을 load하여 실행
  * sector 0는 full Bootstrap loader program
  * OS를 디스크에서 load하여 실행


<br>

## Disk Scheduling

* Access time의 구성
  * seek time
    * 헤드를 해당 실린더로 움직이는데 걸리는 시간
  * rotational latency
    * 헤드가 원하는 섹터에 도달하기까지 걸리는 회전지연시간
  * transfer time
    * 실제 데이터의 전송 시간
* Disk bandwidth
  * 단위 시간 당 전송된 바이트 수
* Disk Scheduling
  * seek time을 최소화하는 것이 목표

<br>

## Disk Scheduling Algorith

* FCFS (First Come First Service)
* SSTF (Shortest Seek Time First)
  * starvation 문제
* SCAN
  * disk arm이 디스크의 한쪽 끝에서 다른쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
  * 다른 한쪽 끝에 도달하면 역방향으로 이동하며 오는 길목에 있는 모든 요청 처리, 다시 반대쪽 끝으로 이동
  * 문제점 : 실린더 위치에 따라 대기 시간이 다르다
* C-SCAN
  * 헤드가 한쪽 끝에서 다른쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
  * 다른쪽 끝에 도달했으면 요청을 처리하지 않고 곧바로 출발점으로 다시 이동
  * SCAN보다 균일한 대기시간 제공
* N-SCAN
  * 일단 arm이 한 방향으로 움직이기 시작하면 그 시점 이후에 도착한 job은 되돌아올 때 service
* LOOK and C-LOOK
  * SCAN이나 C-SCAN은 헤드가 디스크 끝에서 끝으로 이동
  * LOOK과 C-LOOK은 헤드가 진행 중이다가 그 방향에 더 이상 기다리는 요청이 없으면 헤드의 이동방향을 즉시 반대로 이동

<br>

## Disk-Scheduling Algorithm의 결정

* SCAN, C-SCAN 및 그 응용 알고리즘은 LOOK, C-LOOK 등이 일반적으로 디스크 입출력이 많은 시스템에서 효율적인 것으로 알려져 있음
* File 할당 방법에 따라 디스크 요청이 영향을 받음
* 디스크 스케줄링 알고리즘은 필요할 경우 다른 알고리즘으로 쉽게 교체할 수 있도록 OS와 별도의 모듈로 작성되는 것이 바람직하다

<br>

## Swap-Space Management

* Disk를 사용하는 두 가지 이유
  * memory의 volatile(휘발)한 특성 ⇒ file system
  * 프로그램 실행을 위한 memory 공간 부족 ⇒ swap space(swap area)
* Swap-space
  * Virtual memory system에서는 디스크를 memory의 연장 공간으로 사용
  * 파일 시스템 내부에 둘 수도 있으나 별도의 partition 사용이 일반적
    * 공간효율성보다는 속도 효율성이 우선
    * 일반 파일보다 훨씬 짧은 시간만 존재하고 자주 참조됨
    * 따라서, block의 크기 및 저장 방식이 일반 파일 시스템과 다름
* RAID(Redundant Array of Independent Disks)
  * 여러 개의 디스크를 묶어서 사용
* RAID의 사용 목적
  * 디스크 처리 속도 향상
    * 여러 디스크에 block의 내용을 분산 저장
    * 병렬적으로 읽어 옴
  * 신뢰성 향상
    * 동일 정보를 여러 디스크에 중복 저장
    * 하나의 디스크가 고장(failure) 시 다른 디스크에서 읽어옴
    * 단순한 중복 저장이 아니라 일부 디스크에 parity를 저장하여 공간의 효율성을 높일 수 있다

