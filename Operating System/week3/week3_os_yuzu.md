| Week 3 | Lecture 15~21 |
| :----: | :-----------: |

<br/>

<br/>

# Process Synchronization

---

<br/>

### Monitor

- 동시 수행중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한 high-level synchronization construct

- 모니터 내에서는 한번에 하나의 프로세스만 활동 가능
- 프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요 없음
- 프로세스가 모니터 안에서 기다릴 수 있도록 condition variable 사용

<br/>

<br/>

# Deadlocks

---

<br/>

### The Deadlock Problem

- Deadlock: 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태
- Resource: 하드웨어, 소프트웨어 등을 포함하는 개념/ 프로세스가 자원을 사용하는 절차

<br/>

### Deadlock 발생의 4가지 조건

- Mutual Exclusion(상호 배제)
  - 매 순간 하나의 프로세스만이 자원을 사용할 수 있음

- No preemption(비선점)
  - 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음

- Hold and wait(보유대기) 
  - 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음

- Circular wait(순환대기)
  - 자원을 기다리는 프로세스간에 사이클이 형성되어야함

- Deadlock이 발생했는지 알아보기 위해 자원할당 그래프 이용
  - 그래프에 cycle이 없으면 deadlock이 아님
  - 그래프에 cycle이 있으면
    - 자원에 인스턴스가 하나밖에 남지 않는다면 deadlock
    - 여러 자원이 있다면 deadlock일수도 아닐수도


<br/>

### Deadlock의 처리 방법

- Deadlock Prevention - 미연에 방지
  - 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것
  - Mutual Exclusion
    - 공유해서는 안되는 자원의 경우 반드시 성립해야 함

  - Hold and Wait
    - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 함
    - 방법 1. 프로세스 시작 시 모든 필요한 자원을 할당 받게하는 방법
    - 방법 2. 자원이 필요한 경우 보유 자원을 모두 놓고 다시 요청

  - No Preemption
    - Process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
    - 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작
    - State를 쉽게 Save하고 restore할 수 있는 자원에서 주로 사용(CPU, memory)

  - Circular Wait
    - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당

  - Utilization 저하, throughput 감소, starvation 문제

- Deadlock Avoidance - 미연에 방지
  - 자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당
  - 가장 단순하고 일반적인 모델은 프로세스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법
  - safe state
    - 시스템 내의 프로세스들에 대한 safe sequence가 존재하는 상태

  - safe sequence
    - 프로세스의 sequence가 safe하려면 프로세스의 자원 요칭이 "가용 자원 + 모든 프로세스들의 보유 자원"에 의해 충족되어야 함

  - 2가지 avoidance 알고리즘
    - Single instance per resource types: Resource Allocation Graph algorithm
      - cycle이 생기지 않는 경우에만 요청 자원을 할당

    - Multiple instance per resource types: Banker's Algorithm
      - 가정: 모든 프로세스는 자원의 최대 사용량을 미리 명시, 프로세스가 요청 자원을 모두 할당받은 경우 유한 시간 안에 이들 자원을 다시 반납
      - 방법: 자원 요청시 safe 상태를 유지할 경우에만 할당, 총 요청 자원의 수가 가용 자원의 수보다 적은 프로세스를 선택, 그런 프로세스가 있으면 그 프로세스에게 자원을 할당, 할당받은 프로세스가 종료되면 모든 자원을 반납, 모든 프로세스가 종료될 때까지 반복
      - 최악의 상황을 가정해서 남는 자원을 할당하지 않기 때문에 비효율적

- Deadlock Detection and Recovery
  - Deadlock Detection
    - Resource type 당 single instance인 경우
      - 자원할당 그래프에서의 cycle이 곧 deadlock

    - Resource type 당 multiple instance인 경우
      - Banker's algorithm과 유사한 방법 사용

  - Wait for graph 알고리즘
    - Resource type 당 single instance인 경우

  - Recovery
    - Process termination
      - deadlock에 연루된 모든 프로세스 사살

    - Resource Preemption
      - deadlock에 연루된 프로세스를 하나씩 죽여봄

- Deadlock Ignorance

<br/>

<br/>

# Memory Management

---

<br/>

### Logical vs Physical Address

#### ✔ Logical address

- 프로세스마다 독립적으로 가지는 주소 공간
- 각 프로세스마다 0번지부터 시작
- CPU가 보는 주소는 logical address임

#### ✔ Physical address

- 메모리에 실제 올라가는 위치

#### ✔ 주소 바인딩

주소를 결정하는 것

Symbolic Address -> Logical Address -> Physical Address

- compile time binding
  - 물리적 메모리 주소가 컴파일시에 알려짐
  - 시작 위치 변경시 재컴파일
  - 컴파일러는 절대 코드 생성
- Load time binding
  - Loader의 책임하에 물리적 메모리 주소 부여
  - 컴파일러가 재배치가능코드를 생성한 경우 가능
- Execution time binding(=Run time binding)
  - 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
  - CPU가 주소를 참조할 때 마다 binding을 점검
  - 하드웨어적인 지원이 필요

##### 💎 Dynamic Relocation

Logical address를 physical address로 변환하는 방식 중 하나

Memory Management Unit(MMU)를 사용하여 Relocation 구현

운영체제 및 사용자 프로세스 간의 메모리 보호를 위해 사용하는 레지스터 필요

- Limit register: 논리적 주소의 범위

- Relocation register: 접근할 수 있는 물리적 메모리 주소의 최소값

#### ✔ Some Terminologies

- Dynamic Loading

  - 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 대 메모리에 load하는 것

  - memory utilization의 향상

  - 가끔씩 사용되는 많은 양의 코드의 경우 유용

  - 운영체제의 특별한 지원 없이 프로그렘 자체에서 구현 가능

- Overlays

  - 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림

  - 프로세스의 크기가 메모리보다 클 때 유용

  - 운영체제의 지원 없이 사용자에 의해 구현

  - 작은 공간의 메모리를 사용하던 초창기 시스템에서 수작업으로 프로그래머가 구현

- Swapping

  - 프로세스를 일시적으로 메모리에서 backing store(디스크: 많은 사용자의 프로세스 이미지를 담을만큼 충분히 빠르고 큰 저장공간)로 쫓아내는 것

  - Swap in / Swap out
    - 일반적으로 중기 스케줄러에 의해 swap out 시킬 프로세스 선정
    - 우선순위가 낮은 프로세스를 swap out
    - compile time 혹은 load time binding에서는 원래 메모리 위치로 swap in
    - execution time binding에서는 추후 빈 메모리 영역 아무곳에나 올릴 수 있음
    - swap time은 대부분 transfer time(swap되는 양에 비례하는 시간)

- Dynamic Linking
  - Linking을 실행 시간까지 미루는 기법
  - Static linking
    - 라이브러리가 프로그램의 실행 파일 코드에 포함
    - 실행 파일의 크기가 커짐
    - 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비
  - Dynamic linking
    - 라이브러리가 실행시 연결됨
    - 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 코드를 둠
    - 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
    - 운영체제의 도움이 필요

<br/>

### Allocation of Physical Memory

#### ✔ 메모리는 일반적으로 두 영역으로 나누어 사용

- OS 상주 영역: interrupt vector와 함께 낮은 주소 영역
- 사용자 프로세스 영역: 높은 주소 영역 사용

#### ✔ 사용자 프로세스 영역의 할당 방법

##### Contiguous allocation: 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것

- Fixed partition 방식

  - 물리적 메모리를 몇 개의 영구적 분할로 나눔

  - 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재

  - 분할당 하나의 프로그램 적재

  - 융통성이 없음(동시에 메모리에 load되는 프로그램의 수가 고정됨, 최대 수행 가능 프로그램 크기 제한)

  - Internal fragmentation 발생(external도)

- Variable partition 방식

  - 프로그램의 크기를 고려해서 할당

  - 분할의 크기, 개수가 동적으로 변함

  - 기술적 관리 기법 필요

  - External fragmentation 발생

- Hole

  - 가용 메모리 공간

  - 다양한 크기의 hole들이 메모리 여러 곳에 흩어져 있음

  - 프로세스가 도착하면 수용가능한 hole을 할당

  - 운영체제는 할당공간, 가용공간 정보를 유지

- Dynamic Storage-Allocation Problem

  - 가변분할 방식에서 size n인 요청을 만족하는 가장 적절한 hole을 찾는 문제

  - First-fit: Size가 n이상인 것 중 최초로 찾아지는 hole에 할당

  - Best-fit: Size가 n이상인 가장 작은 hole을 찾아서 할당, 정렬되지 않은 경우 모든 hole리스트를 탐색해야함, 많은 수의 작은 hole 생성

  - Worst-fit: 가장 큰 hole에 할당, 모든 리스트 탐색, 아주 큰 hole들 생성

- compaction

  - external fragmentation 문제를 해결하는 방법

  - 사용중인 메모리 영역을 한군데로 몰고 hole들을 다른 한곳으로 몰아 큰 block을 만듬

  - 비용이 많이 드는 방법

  - 최소한의 메모리 이동으로 compaction하는 방법(매우 복잡한 문제)

  - 프로세스의 주소가 실행 시간에 동적으로 재배치 가능한 경우에만 수행될 수 있음

##### Noncontiguous allocation: 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있는 것

- Paging

  - 프로세스의 virtual memory를 동일한 사이즈의 page 단위로 나눔

  - Virtual memory의 내용이 page 단위로 noncontiguous하게 저장됨

  - 일부는 backing storage에 일부는 physical memory에 저장

  - Basic Method

    - physical memory를 동일한 크기의 frame으로 나눔
    - logical memory를 동일 크기의 page로 나눔
    - 모든 가용 frame 관리
    - page table을 사용하여 logical address를 physical address로 변환
    - external fragmentation은 발생하지 않으나 internal fragmentation은 발생 가능성 있음

  - Implementation of Page Table

    - Page table은 main memory에 상주
    - 앞서 MMU에서 봤던 base register, limit register가 사용되는데, Page-table base register(PTBR)가 페이지 테이블을 가리키고 Page-table length register(PTLR)가 테이블 크기를 보관
    - 모든 메모리 접근 연산에는 2번의 메모리 엑세스가 필요 (page table 접근 1번, 실제 data 접근 1번)
    - 속도 향상을 위해 associative register 혹은 translation look-aside buffer(TLB: 고속의 lookup hardware cache) 사용

  - Paging Hardware with TLB ⭐

    - 주소변환을 위해 접근 속도가 빠른 공간이 필요해서 사용
    - 자주 참조되는 몇개의 entry를 캐싱하고 있기 때문에 메인 메모리보다 접근속도가 빠름
    - 페이지 번호와 프레임 번호를 쌍으로 가짐
    - TLB에서 p가 있는지 없는지 확인하려면 전체를 다 확인해야함
    - 전체 탐색을 막기 위해  Associative register를 이용해서 parallel search 가능
    - TLB는 context switch 때 flush (remove old entries)

  - Effective Access Time

    - TLB에 접근하는 시간 e, 메인메모리에 접근하는 시간 1, associative register에서 찾아지는 비율을 a라고 하면 EAT = (e+1)a + (e+2)(1-a) = 2+e-a

  - Two-Level Page Table

    - 현대의 컴퓨터는 address space가 매우 큰 프로그램 지원
      - 32bit address 사용시 2^32B(4GB)의 주소 공간
      - 대부분의 프로그램은 4GB 주소 공간 중 일부만 사용하므로 page table 공간이 심하게 낭비됨 -> 2단계 테이블 사용으로 해결해보자
    - logical address (32-bit CPU에서 하나의 페이지 크기 4KB)
      - 20bit의 page number, 12bit의 page offset
    - page table 자체가 하나의 page로 구성되기 때문에 page number는 다음과 같이 나뉨
      - 10bit의 page number, 10bit의 page offset
      - => p1(douter page table index)=10 | p2(outer page table 변위)=10 | d=12

    - 안쪽 테이블 하나의 크기는 페이지 하나(4KB) -> d의 12bit
    - entry(하나당 4B) 1K개 집어 넣을 수 있음 -> p2의 10bit, 자동으로 p1은 10bit
    - 64bit 주소체계 에서는 -> 43|9|12
    - Two-Level Page Table을 사용하면 바깥쪽 테이블은 logical memory크기만큼 만들어지지만 안쪽테이블에서는 실제로 사용되지 않는 것들은 만들어 지지 않기 때문에 페이지 테이블의 공간을 줄일 수 있음

  - Multilevel Paging and Performance

    - Address space가 더 커지면 다단계 페이지 테이블 필요
    - 각 단계의 페이지 테이블이 메모리에 존재하므로 logical address의 physical address 변환에 더 많은 메모리 접근 필요
    - TLB를 통해 메모리 접근 시간을 줄일 수 있음

  - Valid/ Invalid Bit in a Page Table

    - 사용되지 않는 영역을 위해서도 entry는 만들어져야함(인덱스를 통한 순차적 접근이 가능해야하기 때문에)
    - 대신 사용하지 않는 것을 invalid로 표시, 물리메모리에 올라와 있지 않고 swap area에 있는 경우에도 invalid
    - protection bit: 접근 권한(read, wrtie, read-only)

  - Inverted Page Table

    - page table이 매우 큰 이유: 모든 프로세스 별로 그 논리적 주소에 대응하는 모든 page에 대해 page table entry가 존재하기 때문에, 대응하는 page가 메모리에 있든 없든 간에 page table에는 entry로 존재 => 이런것들을 막아보자는 취지에서 나온 것이 Inverted Page Table
    - 시스템안에 page table이 딱 하나 존재함
    - 단점: 테이블 전체를 탐색해야함 => 조치: associative register를 사용해서 병렬적 탐색이 가능하도록 하면 시간적 overhead를 줄일 수 있음
    - cpu가 pid|p|d 로지컬 주소를 주면 pid, p에 해당하는 것을 찾아서 그것으로 f를 찾음

  - Shared Page

    - 페이지들중에는 다른 프로세스들과 공유할 수 있는 페이지가 존재
    - shared code(read-only로 세팅)에 대해서는 1copy만 올림
    - private code는 독자적으로 메모리에 올림
- Segmentation
  - 프로그램은 작은 의미 단위인 여러개의 segment로 구성
    - 작게는 프로그램을 구성하는 함수 하나하나를 세그먼트로 정의
    - 크게는 프로그램 전체를 하나의 세그먼트로 정의
    - code, data, stack 부분이 하나씩의 세그먼트로 정의됨
  - segment는 다음과 같은 logical unit
    - main, function, global variables, stack, symbol table, arrays ...
  - Logical address의 구성: segment-number, offset
  - segment table
    - segment별로 주소변환을 해야하므로 segment table을 가짐
    - entry 정보
      - base: 물리메모리에의 세그먼트 시작위치 저장
      - limit: 세그먼트의 길이
  - Segment-table base register(STBR): 물리 메모리에서의 segment table의 위치
  - Segment-table length register(STLR): 프로그램이 사용하는 segment의 수
  - CPU가 논리주소(s|d)를 주면 세그먼트 테이블에서 검색
  - 페이징 기법에서는 시작주소가 프레임 번호로 주어지면 되지만 세그먼테이션에는 세그먼트 크기가 다 다르기 때문에 세그먼트가 어디에서 시작되는지 정확한 바이트단위의 주소로 주어져야함
  - segment는 의미 단위이기때문에 공유와 보안에 있어 paging보다 효과적(<->paging은 물리적 메모리 조각이 발생하는 일이 없다는 것이 장점)
  - 단점: segment의 길이가 동일하지 않기 때문에 연속할당의 가변분할 방식의 hole들이 생기는 문제가 발생, external fragmentation 발생, first-fit, best-fit
- Paged Segmentation(Segmentation with paging)
  - pure segmentation과의 차이점
    - segment-table entry가 segment의 base address를 가지고 있는 것이 아니라 segment를 구성하는 page table의 base address를 가지고 있음
    - Allocation 문제가 생기지 않음(페이지 단위로 자르기 때문에 hole이 생기지 않음)


<br/>

<br/>
