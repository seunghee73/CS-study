# 1. Virtual Memory

## 1. **Demand Paging**

- 실제로 필요할 때(요청이 있을 때) page를 메모리에 올리는 기법 사용

  - 필요하지 않는 부분은 swap area에 내려가 있다
  - I/O 양의 감소 → 프로그램에서 빈번히 사용되는 부분은 적기 때문에, 필요한 것만 올리면 I/O로 읽어 들이는 양 자체가 줄어든다
  - memory 사용량 감소
  - 빠른 응답 시간 → 남는 공간에 더 많은 프로그램 수용 가능 (각각 자주 쓰는 부분만 올라오기 때문에, response time이 systemwide하게 보면 빨라지게 된다)
  - 더 많은 사용자 수용

- Valid/Invalid bit 사용 (page entry마다 존재하는 부가적인 bit)

  - invalid 의미

    - 사용되지 않는 주소 영역인 경우 → 주소 공간이 프로그램보다 더 크게 지원되어 버린 경우
    - 페이지가 물리적 메모리에 없는 경우 → physical memory가 아니라 swap area에 내려가 있는 경우

  - 처음에는 모든 page entry를 invalid로 초기화

  - address translation (주소 변환) 시 페이지 테이블에 invalid bit이 set 되어 있으면, (해당하는 page가 memory에 올라와 있지 않으면) “**page fault**” (페이지 결손, 결함)

  - Page fault

    : 요청한 페이지가 memory에 올라와 있지 않은 경우

    - I/O 작업이 필요하다는 소리와 같기 때문에, OS로 CPU가 넘어가게 된다
    - 일종의 software interrupt (trap)
    - page fault에 대한 처리 루틴이 OS에 적혀 있다

## 2. **Page Fault**

- **invalid page**를 접근하면 **MMU**(hardware의 일종; memory management unit; logical memory → physical memory 변환하는 장치)가 **trap**을 발생시킨다 **→ page fault trap**

- Kernel mode로 들어가서(CPU가 OS로 넘어간다), **page fault handler가 invoke** 된다

- 처리 루틴

  - invalid reference ? (잘못된 페이지 요청이 아닌지 체크)

    - e.g. bad address, protection violation

    ⇒ **abort process**

  - Get an empty page frame

    - memory에 새 공간이 없으면 다른 페이지의 프레임 공간을 뺏어온다; **replace**

  - 해당 페이지를 disk(backing area)에서 memory로 읽어온다.

    - disk I/O가 끝나기까지 이 프로세스는 CPU를 **preempt** 당한다 (**blocked**) + OS는 disk에서 읽어오는 작업을 시키고 다른 ready 상태의 process에게 CPU를 넘긴다.
    - disk read가 끝나면 **page tables entry** 기록
    - valid/invalid bit = **valid**로 setting
    - **ready queue**에 process를 insert → **dispatch** later

  - 이 프로세스가 CPU를 잡고 다시 running

  - 아까 중단되었던 instruction 재개

- page fault rate (0 ~1)

  - disk에서 읽어오는 작업은 오래 걸리는 작업
  - page fault가 나는 비율은 메모리 접근하는 총 시간에 크게 영향
  - 비율이 0이면 페이지 fault가 한 번도 안나고, 1이면 항상 fault 발생
  - 보통 0.0x 정도의 비율

## 3. **Page Replacement**

- OS가 page replacement 수행

- 빈 page frame이 없는 경우

  - memory에서 어떤 frame을 쫓아낼지 결정
  - 곧바로 사용되지 않을 page를 쫓아내는 것이 좋다
  - 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 들어올 수 있기 때문
  - page를 내쫓을 때 해당 frame에 기존과 다른 변경 사항이 있었다면, 이를 backing store에도 반영 필요
  - 내쫓은 페이지는 invalid bit 표시, 새로 들여온 페이지는 valid bit 표시

- Replacement Algorithm

  - page-fault rate 최소화가 목표

  - 알고리즘의 평가

    - 주어진 page reference string에 대해 page fault를 얼마나 내는지 조사

    - reference string

      (시간 순서에 따라 페이지가 참조된 순서 나열)의 예

      - 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

### **1. Optimal Algorithm (Offline Algorithm, Offline Optimal Algorithm)**

- = **Belady’s optimal algorithm, MIN, OPT**
- 페이지 repacement algorithm 중 가장 좋은 알고리즘
  - page fault를 가장 적게 하는 알고리즘
  - **가장 먼 미래에 참조되는 page를 replace** (쫓아낸다)
- 실제 시스템에서는 어떤 페이지가 참조 될 지 알 수 없다
  - But! 미래에 참조되는 페이지를 모두 안다고(page reference string을 안다) 가정 → offline algorithm
  - 실제 시스템에서 사용 불가
- 다른 알고리즘의 성능에 대한 upper bound 제공
  - 어떤 알고리즘을 만들어도 optimal algorithm 이상 좋은 성능을 낼 수 없다

### **2. FIFO (First In First Out) Algorithm**

- 메모리에 먼저 들어온 페이지를 먼저 내쫓는 알고리즘
- FIFO Anomaly (Belady’s Anomaly)
  - 메모리 크기(page frame 수)를 늘려주면 일반적으로는 성능이 더 좋아지지만, FIFO는 성능이 더 나빠질 수 있다



### **3. LRU (Least Recently Used) Algorithm ⭐**

- 가장 오래 전에 참조된 페이지를 내쫓는 알고리즘
  - 가장 덜 최근에 사용된 것을 지운다
- 과거의 기록을 가지고 미래를 예측하는 알고리즘
- 페이지를 double-linked-list 형태로 참조된 시간 순서대로 연결해 관리
  - **O(1)** : 쫓아내기 위해서 비교하지 않기 때문에, 가장 오래된 맨 끝 노드만 지우거나, 시간이 갱신 되면 reference를 끊고 맨 앞으로 연결해주거나
  - 비교가 전혀 필요 없기 때문에 가능

### **4. LFU (Least Frequently Used) Algorithm**

- 가장 덜 빈번하게 사용된 알고리즘을 쫓아내는 알고리즘
  - 참조 횟수(reference count)가 가장 적은 페이지를 지운다
- 과거의 기록을 가지고 미래를 예측하는 알고리즘
- 최저 참조 횟수인 page가 여럿 있는 경우 (동일한 횟수 보유)
  - LFU 알고리즘 자체에서는 딱히 명시되어 있지 않고, 그냥 여러 page 중 임의로 지울 페이지 선정
  - 성능 향상을 원한다면, 그중 가장 오래 전에 참조된 페이지 삭제하도록 구현
- 참조 횟수 별로 한 줄로 줄 세우기 불가
  - linked-list인 경우 참조 횟수가 1이 늘어났다고 해서, 바로 줄의 맨 앞으로 갈 수 있는 것이 아니라, 다른 노드들과 참조 횟수를 비교해 봐야 한다
    - **O(n)** complexity
  - So, **heap**으로 구현 → **O(log n)**
- 장단점
  - LRU처럼 직전 참조 시점만 보는 것이 아니라, 장기적인 시간 규모를 보기 때문에 **page의 인기도를 좀 더 정확히 반영 가능**
  - 참조 시점의 **최근성**을 반영하지 못한다. (이제 막 여러 참조를 시작하는 경우) - 반대로 LRU는 빈도를 반영하지 못한다
  - LRU보다 구현이 복잡하다

### # Paging System에서 LRU, LFU가 가능한가

- 주소 변환 = 기본적으로 하드웨어 (OS x)
- page fault 발생 → I/O 필요 → trap → OS가 작동
  - 이때 자리가 부족하다면 replacement algorithm을 가동하게 되는데,
  - 결손이 나기 전까지 OS는 physical memory에 어떤 페이지가 언제 올라왔는지 알 수 없다 (CPU 제어권이 없기 때문)
  - OS는 trap이 발생한 시점부터 어떤 페이지가 접근했는지 알 수 있다
- LRU에서 물리적 메모리에 올라와 있는 페이지가 다시 쓰이는 경우
  - 가장 최근에 쓰였으니 맨 앞으로 reference를 옮겨야 한다
  - page fault가 발생하지 않으므로 OS에게 CPU가 넘어가지 않는다
  - 그런데, reference를 앞으로 옮겨주는 일은 OS가 해야한다
- OS는 어떤 페이지가 최근에 참조 됐는지, 가장 자주 참조됐는지 알 수 없다
- 즉, virtual memory에서는 LRU, LFU가 불가능하다
- 대신, buffer caching이나 web caching에서는 쓰일 수 있다
- ⇒ paging system에서는 **Clock Algorithm**이 쓰이게 된다

 ### **# Caching**

- paging system은 일종의 caching 기법인데, 이는 paging system에서만 사용되는 것이 아니다
- **Caching** 기법
    - 한정된 빠른 공간(= cache)에 요청된 데이터를 저장해 두었다가 후속 요청 시 cache로부터 직접 서비스하는 방식
    - paging system 외에도 cache memory, buffer caching, web caching 등 다양한 분야에서 사용
        - (예) paging system가 caching인 이유:
        main memory = 한정된 빠른 공간, 느린 공간 = backing store
        - CPU와 main memory 사이 cache memory 존재
        - buffer caching : 파일 시스템에 대한 read write 요청을 메모리에서 빠르게 제공 (main memory 빠른 공간, disk 느린 공간으로 paging system과 유사하지만, paging system은 disk 중에서도 swap area, buffer caching은 disk의 파일 시스템
        - web caching : 멀리 있는 웹 서버에서 가져오는데, 동일한 url에 대해 요청한다면, 통째로 다시 읽어오기보다는 사용자 브라우저에 일시적으로 저장해두고 필요할 때마다 다시 불러오기
- cache 운영의 **시간 제약**
    - paging system과 달리 시간 제약이 존재
    - 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없다
    - Buffer caching이나 web caching의 경우
        - **O(1)에서 O(logn) 정도**까지 허용
        - (cf) paging system의 LRU는 O(1), LFU는 O(logn)으로 시간 제약 만족
    - paging system인 경우
        - page fault인 경우에만 OS가 관여
        - 페이지가 이미 메모리에 존재하는 경우 참조 시각 등의 정보를 OS가 알 수 없다
        - O(1)인 LRU의 list 조작조차 불가능

 ### 5. **⭐ Clock Alogrithm ⭐**

- LRU의 근사(approximation) 알고리즘
- Paging 알고리즘에서 일반적으로 사용
- = **second chance algorithm**
  = **NUR (Not Used Recently)** 또는 **NRU (Not Recently Used)**
    - 최근에 사용되지 않은 페이지를 쫓아내지만(가장 오래된 페이지를 쫓지는 못한다), 기회를 두 번 준다
- reference bit을 사용해서 교체 대상 페이지 선정 (circular linked-list)
    - 페이지를 참조하면 reference bit 1로 setting해 참조되었다는 것을 표시
    - **하드웨어**가 작업
    - OS가 나중에 하드웨어가 세팅해둔 값을 참조해 쫓아내게 된다
- OS는 reference bit가 0인 것을 찾을 때까지 포인터를 하나씩 앞으로 이동
- 포인터 이동하는 중에 reference bit 1은 모두 0으로 바꾼다
- reference bit이 0인 것을 찾으면 그 페이지를 교체
- 한 바퀴 되돌아 와서도 (=second chance) 0이면 그때는 replace 당한다
- 자주 사용되는 페이지라면 second chance가 올 때 1
- 개선
    - reference bit과 **modified bit (dirty bit)** 함께 사용
        - 페이지는 read로 참조될 수도, write로 참조될 수도 있다
        - write → modified bit
    - reference bit = 1 : 최근에 참조된 페이지
    - modified bit = 1 : 최근에 변경된 페이지 (I/O를 동반하는 페이지)
        - modified bit이 1이라면 메모리에 올라온 이후로 CPU에서 적어도 한 번은 내용 수정했으므로, 그냥 지우지 못하고 backing store에 업데이트 한 후에 쫓아내야 한다
        - 가능하면 modified bit이 1인 것보다 0인 것을 먼저 쫓아내면 개선 가능 (backing store에 갔다 오는 것 자체가 효율 감소)

## 4. Page Frame의 Allocation

- Allocation problem : 각 프로세스에 얼마 만큼의 page frame을 할당할 것인가?
- Allocation의 필요성
    - 메모리 참조 명령어 수행 시 명령어, 데이터 등 여러 페이지 동시 참조
        - 명령어 수행을 위해 **최소한 할당되어야 하는 frame 수**가 있다
    - Loop를 구성하는 page들은 한꺼번에 allocate 되는 것이 유리
        - 최소한의 allocation이 없으면 매 loop마다 page fault
        - 프로세스에 할당할 때, loop에 필요한 페이지 수보다 부족한 프레임을 할당하는 경우, 계속 page fault가 발생할 것
- **Allocation** scheme
    - **Equal allocation** : 모든 프로세스에 똑같은 개수 할당 (비효율적)
    - **Proportional allocation** : 프로세스 크기에 비례하여 할당
    - **Priority allocation** : 프로세스의 priority(CPU 우선 순위가 높은가)에 따라 다르게 할당
- **Global Replacement**
    - 굳이 미리 allocation 하지 않고, 필요할 때마다 빼앗아서 쓰겠다는 방법 → global replacement, local replacement
    - replace 시 다른 process에 할당 된 frame을 빼앗아 올 수 있다
    - process 별 할당량을 조절하는 또 다른 방법
    - FIFO, LRU, LFU 등의 알고리즘을 **global replacement로 사용 시** 해당
    - working set, PFF 알고리즘 사용 (최소한 필요한 프레임을 주는 일종의 할당 효과를 낼 수 있는 알고리즘)
- **Local replacement**
    - 자신에게 할당된 frame 내에서만 replacement
    - FIFO, LRU, LFU 등의 알고리즘을 **process 별**로 운영시 해당

 ## 5.**Thrashing (쓰레싱)**

: 프로세스의 원활한 수행에 필요한 **최소한의 page frame 수를 할당 받지 못해 page fault가 자주 발생**하는 경우

- Page fault rate가 매우 높아진다
- CPU utilization이 낮아진다
- OS는 **MPD (Multiprogramming Degree)**를 높여야 한다고 판단
    - CPU는 한가 (I/O를 하러 가면 CPU는 쉬게 된다)
    - 그래서 일반적으로는 그동안 CPU가 놀지 않도록, 프로세스를 메모리에 더 올리는 것이 CPU utilization을 높여준다
- 또 다른 프로세스가 시스템에 추가 된다 (higher MPD)
- 프로세스 당 할당된 frame 수가 더욱 감소 → page fault 더 자주 발생
- 프로세스는 page의 swap in/ swap out (메모리에 올리고 내리고 하는 작업)으로 매우 바쁘다
- 반대로 CPU는 한가
- low throughout
- 막기 위한 방법
    - **multiprogramming degree 조절**
    - 프로그램이 메모리 확보를 하도록 도와준다
    - **working-set algorithm, PFF (page-fault-frequency algorithm)**

### 1. Working-set Model

- Locality of reference
    - 프로세스는 **특정 시간** 동안 **특정 장소만**을 **집중적으로 참조**
        - (ex) for loop
    - **집중적으로 참조되는 해당 page 들의 집합을 locality set**이라 한다
- Working-set model
    - **working set** : **locality에 기반**하여 프로세스가 일정 시간 동안 원활하게 수행되기 위해 **한꺼번에 메모리에 올라와 있어야 하는 page들의 집합**
    - working set 모델에서는 process의 working set 전체가 메모리에 올라와 있어야 수행되고, 그렇지 않을 경우 모든 **frame을 반납한 후 suspended (swap out)**
        - Multiprogramming degree 조정
        - Threashing 방지
        
    
- Working set의 결정
    - working set window를 통해 알아낸다
    - window size가 △인 경우
        - **과거 △(델타) 시간(=window 사이즈 만큼) 동안 참조된 페이지는 메모리에서 쫓아내지 않고 유지** → working set
        - 시각 ti에서 working set WS(ti)
            - Time Interval [ti -△, ti] 사이에 참조된 서로 다른 페이지들의 입합
        - workingg set에 속한 page는 메모리 유지, 속하지 않는 것은 버린다
        - 참조된 후, △ 시간 동안 해당 page를 메모리에 유지한 후 버린다
        → 정해진 working set 은 델타 시간 동안만 유지된다
- Working-set Algorithm
    - process들의 working set size의 합이 page frame의 수보다 큰 경우
        - 일부 프로세스를 swap out 시켜 남은 process의 working set을 우선적으로 충족시켜 준다 (MPD 줄인다)
        - Working set을 할당하고도 page frame이 남는 경우
            - swap out 되었던 프로세스에게 working set 할당 (MPD를 키운다)
- Window size △ (델타)
    - working set을 제대로 탐지하기 위해서는 window size를 제대로 결정해야 한다
    - △ 값이 너무 작으면 locality set을 모두 수용하지 못할 우려
    - △ 값이 크면 여러 규모의 locality set 수용
    - △ 값이 무한대이면 전체 프로그램을 구성하는 page를 working set으로 간주

### 2. **PFF (Page-Fault-Frequency) Scheme**

- 역시 Multiprogramming degree 조정 + threashing 방지
- working-set을 추정하는 방식이 아니라, 직접 page fault가 있는지 보며 체크
- page fault 비율에 따라 해당 프로세스가 적절한 프레임양을 할당 받았는지 체크
- page-fault rate의 상한값과 하한값을 둔다
    - page fault rate가 상한 값을 넘으면(빈번 → working set이 제대로 보장 안 되는 상황)  frame을 더 할당
    - page fault rate가 하한값 이하이면(너무 allocation을 많이 한 것) 할당 frame 수를 줄인다
- allocation을 하려고 봤더니(page fault rate가 상한 값을 넘었는데, 더 줄 메모리가 없는 경우) 줄 빈 프레임이 없으면 일부 프로세스를 swap out

 Page Size 결정

- page size를 감소시키면
    - 페이지 수 증가
    - 페이지 테이블 크기 증가 (entry 수 증가)
    - Internal fragmentation 감소 (페이지 내에서 사용되지 않는 부분)
    - disk transfer의 효율성 감소
        - seek/rotation vs. transfer
            - disk는 기본적으로 회전하면서 헤더가 원하는 값을 찾아야 한다(seek) → 상당히 오래 걸리는 작업
            - 페이지 수가 작으면 fault가 날 확률(비슷한 내용은 연달아 한꺼번에 올라와야 fault날 일이 적다 → 큰 페이지 사이즈에서 가능)도 높아서 I/O에 접근할 일도 많아진다
    - 필요한 정보만 메모리에 올라와 메모리 이용이 효율적
        - 페이지 사이즈가 크면, 이만큼 정보가 필요하지 않는데도 다른 정보까지 한꺼번에 가져와야 한다
        - locality 활용 측면에서는 좋지 않다
            - 프로그램은 locality가 있다. 예를 들어 어떤 함수를 하나 실행시키면 다른 함수들도 연달아 이어지는 경우
- Larger page size가 인기 많다

# 2.  File System

## 1. **File & File System**

- File
  - a **named** collection of related information
  - 하드 디스크에 저장하는 단위
    - main memory에서는 주소로 접근했다면
    - 하드 디스크는 **이름**으로 접근
  - 일반적으로 비 휘발성의 **보조 기억 장치**에 저장
  - 운영체제는 다양한 저장 장치를 file이라는 동일한 논리적 단위로 볼 수 있게 해준다
    - 디스크에 저장하는 단위도 파일이지만,
    - OS 상에서는 여러 장치를 관리하기 위해, 장치 별로 파일을 두고 관리 → device file
  - Operation
    - **create, read, write, reposition (Iseek-영어 L-), delete, open, close** 등
    - reposition : 파일을 읽거나 쓰면 위치를 가리키는 포인터 존재. 기본적으로는 처음부터 읽지만, 다음에 읽을 때는 직전에 읽었던 부분부터 읽는다. 이때 포인터 위치를 원하는 곳으로 옮기는 연산 (파일 접근 위치 수정)
    - open : 디스크에서 메모리로 내용을 올려 놓는 것이 아니라, 메타 데이터를 메모리로 올려 놓는 것
- File attribute (= 파일의 metadata)
  - 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들
    - 파일 이름, 유형, **저장된 위치(위치 포인터)**, 파일 사이즈
    - 접근 권한 (읽기, 쓰기, 실행), 시간 (생성, 변경, 사용), 소유자 등
- File System
  - 운영체제에서 파일을 관리하는 부분 (소프트웨어)
  - 파일 및 파일의 메타데이터, 디렉토리 정보 등을 관리
    - 루트 디렉토리부터 계층 형태로 관리
  - 파일의 저장 방법 결정
  - 파일 보호 등
- Directory
  - 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일
    - 파일 중 하나
    - 디렉토리 밑에 어떤 파일이 있는지
  - 그 디렉토리에 속한 파일 이름 및 파일 attribute들
  - operation
    - search for a file, create a file, delete a file
    - list a directory (파일 목록 나열), rename a file, traverse the file system (파일 시스템 전체 탐색)
- Partition (= Logical Disk)
  - 운영체제가 보는 디스크 = 논리적 디스크 = 파티션
  - (ex) C 드라이브, D 드라이브
  - 하나의 **(물리적) 디스크 안에** **여러 파티션** 두는 것이 일반적
  - 여러 개의 물리적인 디스크를 하나의 파티션으로 구성하기도
  - (물리적) 디스크를 파티션으로 구성한 뒤 각각의 파티션에 **1) file system을 깔거나 2) swapping(virtual memory swap area)** 등 다른 용도로 사용 가능

## 2. **open(”/a/b/c”)**

- 사용자 프로그램이 파일을 열겠다고 **system call ! →** OS로 CPU가 넘어간다 → OS 메모리에 있는 파일이라면 그냥 바로 전달 없으면 디스크에서 읽어오기 시작 →
- OS가 root directory의 metadata는 알고 있다 → metadata를 통해 root directory에 접근 → 루트 디렉토리에는 나머지 파일의 메타데이터 보유 → 이걸 다시 커널 메모리로 올린다 →
- 이를 위하여 directory path를 search
  - 루트 디렉토리 “/”를 open하고 그 안에서 파일 “a”의 위치 획득
  - 파일 “a”를 open한 후 read하여 그 안에서 파일 “b”의 위치 획득
  - 파일 “b”를 open한 후 read하여 그 안에서 파일 “c”의 위치 획득
  - 파일 “c”를 open → 파일을 바로 사용자 프로그램에게 주는 것이 아니라 OS가 자신의 메모리 공간에 먼저 read해 놓는다. 이후 사용자 프로그램에게 copy해서 전달 ⇒ **buffer caching** (동일한 파일 요청하면, OS에 저장해둔 파일을 준다)
  - buffer caching에서는 **LRU, LFU 알고리즘 사용 가능** (파일이 메모리에 올라와 있든 안 올라와 있든 일단 무조건 OS가 직접 관리하기 때문)
- directory path의 search에 너무 많은 시간 소요
  - open을 read/write와 별도로 두는 이유이다
  - 한 번 open한 파일은 read/write시 directory search 불필요
- 커널이 파일 시스템 위해 유지하는 테이블
  - 메모리에 올리는 순간 기존의 파일 메타데이터 + 파일 오프셋이 필요 (프로그램마다 별도로 존재)
  - per-process file descriptor table
    - 프로세스마다 가지고 있는 file descriptor table
    - 메타데이터의 위치만 가리킨다
  - open file table
    - open된 파일의 목록을 system-wide하게 한꺼번에 관리
    - file descriptor들 보관
    - 현재 open된 파일들의 메타데이터 보관소 (in memory)
    - 디스크의 메타데이터보다 몇 가지 정보가 추가
      - open한 프로세스의 수
      - file offset : 파일 어느 위치 접근 중인지 표시 (별도 테이블 필요) → open file table 내에서도 프로세스와 무관하게 하나로 관리하는 것과, 프로세스 별로 파일 오프셋 관리하는 테이블로 분리하는 것이 일반적
- File Descriptor (file handle, file control block)
  - 오픈된 파일의 위치 정보 (fd) → 커널 메모리의 프로세스 PCB에 존재
  - 즉, open은 file descriptor를 return하고, read(fd)를 해야 읽어오는 작업까지 수행
  - open file table에 대한 위치 정보 (프로세스 별)



## 3. **File Protection**

- (cf) 메모리는 프로세스마다 별도로 가지고 있기 때문에, 결국 자기 자신만 볼 수 있다. 프로세스의 페이지 별로 read, write, read-only만 걸어주면 됐다

- 파일은 여러 사용자가 같이 사용

  - 접근 권한이 누구에게 있는지
  - 어떤 연산(read/write/execution)을 허락할 것인지

- Access Control 방법

  - **Access control matrix**

    - 상당히 sparse한 matrix가 될 수 밖에 없다 → linked-list로 구현
    - linked-list로는 아래와 같이 두 종류로 구현 가능하지만, 여전히 오버헤드가 크기 때문에, 일반적으로는 access control matrix 보다 grouping 사용

    - (종류 1) access control list
      - **각각의 파일 별**로 누구에게 어떤 접근 권한이 있는지 표시
      - 파일을 주체로 파일 별 접근 권한이 있는 사람을 linked-list로 묶는다
    - (종류 2) **capability** : **사용자 별로** 자신이 접근 권한을 가진 파일 및 해당 권한 표시 (역시 linked-list)

  - **Grouping ⭐**

    - 전체 user를 owner(파일 소유주), group(사용자와 동일 그룹), public(other; 전체 공개)의 세 그룹으로 구분
    - 각 파일에 대해 세 그룹의 접근 권한 (rwx)을 3비트씩으로 표시
    - 파일에 대해 접근 권한을 표시하기 위해 9개 bit만 필요

  - **Password**

    - 모든 파일마다 password를 두는 방법 (or 디렉토리별로 password)
    - 모든 접근 권한에 대해 하나의 password : all-or-nothing
    - 접근 권한(read-write-execution) 별 password : 암기 문제, 관리 문제

## 4. File system의 **Mounting** 연산

- logical disk는 파티션 존재, 파티션 별로 파일 시스템 존재
- 다른 파티션의 시스템에 접근하려면 ? → Mounting 연산
- 루트 디렉토리에 다른 디렉토리의 루트를 연결

파일 접근하는 방법

- 시스템이 제공하는 파일 정보의 접근 방식
  - 순차 접근 (sequential access)
    - 카세트 테이프를 사용하는 방식처럼 접근
    - 읽거나 쓰면 offset은 자동적으로 증가
    - A→B→C 순서로 되어 있다면 B를 건너뛰지 못한다
  - 직접 접근 (direct access, random access)
    - LP 레코드 판과 같이 접근
    - 파일을 구성하는 레코드를 임의의 순서로 접근
    - A→B→C 순서로 되어 있다면 B를 건너뛸 수 있다



- 직접 접근이 가능한 매체라더라도 데이터 관리 방식에 따라 순차 접근만 가능한 경우도 있다

## 5. [Allocation of File Data in Disk (디스크에 파일 저장 방법)] - 이론적

: 파일은 크기가 균일하지 않지만, 저장할 때는 동일한 크기의 sector (논리적인 block) 단위로 저장

1. Contiguous Allocation (연속 할당)
2. Linked Allocation (링크로 연결)
3. Indexed Allocation (인덱스로 두는 방법)

### 1. Contiguous Allocation

- 하나의 파일이 디스크 상에서 연속해서 저장

- (장점)

  - **빠른 I/O 작업**

    - 파일 접근 시간의 대부분은 디스크 헤드의 접근 시간
    - 한 번의 seek/rotation으로 많은 바이트 transfer

    1. **파일 시스템** 용
    2. **Realtime file** 용 : file 중 데드 라인이 있는 realtime 용 → 데드라인을 지키기 위해 빠른 I/O 필요
    3. 이미 run 중이던 process의 **swapping** 용(프로세스의 주소 공간 일부를 올리고 내리는 → 빠르게 올리고 내릴 수 있는 것이 중요 → 공간보다 시간이 중요)

  - **Direct Access (= random access)** 가능

- (단점)

  - external fragmentation

     (외부 조각 발생 가능)

    - 각각의 파일마다 크기가 다르기 때문 + 연속 할당
    - 비어있는 공간이 있어도 빈 공간의 크기가 파일 크기만큼 크지 않다면 들어갈 수 없다

  - File grow가 어렵다

    - 파일은 중간에 크기가 변경될 가능성 존재
    - 파일의 **크기를 수정하는데 제약**이 존재 ← 연속 할당
    - file 생성 시 얼마나 큰 hole을 배당할 것인가 (파일이 커질 것을 대비해 미리 할당해두는 공간 존재 → **internal fragmentation**)
    - grow 가능 vs. 공간의 낭비 (internal fragmentation)

### 2. Contiguous Allocation

- 디렉토리는 파일의 시작 위치만 기록
- linked-list 같은 형태 (연결된 메모리의 주소를 같이 기록)
- (장점)
  - external fragmentation이 발생하지 않는다
  - 파일의 데이터를 연속적으로 배치하지 않고, 빈 공간이라면 아무 곳에나 저장 가능
- (단점)
  - **random access X (직접 접근이 되지 않는다)**
  - 순차 접근만 가능 → 계속 seek (디스크 헤드가 이동) → 시간이 오래 걸린다
  - reliability 문제
    - 하나의 sector가 고장나 pointer가 유실되면 많은 부분을 잃는다
  - 디스크에서 하나의 sector는 보통 512 bytes, 디스크에 저장하려는 단위는 512의 배수
    - 그런데, linked allocation에서는 pointer 저장으로 4 바이트 추가 요구
    - 그래서 파일이 공간이 부족해 나뉘어 저장될 가능성 존재
    - pointer를 위한 공간이 block의 일부가 되어 공간 효율성을 떨어트린다
- 변형
  - File-Allocation Table (FAT) 파일 시스템
    - linked allocation을 변형
    - 포인터를 별도의 위치에 보관하여 reliability와 공간 효율성 문제 해결

### 3. Indexed Allocation

- random access이 가능하게 하기 위해, 디렉토리가 index 정보를 담고 있는 block을 가리키게 한다
- index block에 파일들이 어디에 있는지 정보 저장
- (장점)
  - external fragmentation이 발생하지 않는다
  - direct access 가능
- (단점)
  - 아무리 작은 파일이라 하더라도 블럭이 최소 두 개 필요 (인덱스용 + 파일 저장용) → 공간 낭비
  - 엄청 큰 파일인 경우에 하나의 index block으로 모든 파일들의 위치를 저장 불가
    - **linked scheme** : 인덱스 마지막에 파일이 아닌 또 다른 인덱스 위치 저장
    - **multi-level index** : 하나의 index block이 다른 index 파일을 가리키는 경우 (이단계 페이지 시스템처럼)

## 6. [실제 파일 시스템]

### 1. UNIX file system의 구조 → 파일 시스템의 기본 구조

- UNIX 파일 시스템은 partition 내부에 1. boot block 2. super block 3. inode list 4. data block이 순서대로 들어가 있다
- Boot block
  - 부팅에 필요한 정보 (**bootstrap loader**)
  - 메모리의 0번 블럭에 존재
  - 파일 시스템에서 OS의 위치를 찾아 메모리에 올린다
- Super block
  - 파일 시스템에 관한 총체적인 정보를 담고 있다
  - 어디가 빈 블록이고, 어디가 사용 중인 블럭인지 관리
  - 어디가 Inode list이고 어디부터 Data block인지 관리
- Inode list
  - 파일 이름을 제외한 파일의 모든 메타 데이터 저장
  - file의 metadata는 이론적으로는 directory가 보관하지만, 실제로는 일부만 directory가 보관하고, 나머지는 Inode에서 관리
  - 파일 하나 당 inode(아이노드) 하나 할당
  - (ex) 파일 소유주, 접근 권한, 최종 소유주, 위치 정보 (indexed allocation 사용)
    - inode 크기는 일관적 but 굉장히 큰 파일의 모든 위치 정보를 저장해야 한다
    - direct blocks, single indirect, double indirect, triple indirect로 위치 정보 구성
    - 파일 크기가 작으면 direct index만 가지고 표현 → 대부분의 파일은 크기가 작기 때문에, 이렇게 크기 별로 나눠서 저장하는 것이 효율적
    - 파일이 커질 수록 single indirect, double indirect, triple indirect로 저장 (인덱스의 인덱스 층위에 따라 singlie, double, triple)
  - directory는 파일의 이름고 inode 번호만 직접적으로 보유
- Data Block
  - 파일의 실제 내용을 보관

### 2. FAT File System

- MS DOS 만들 때 만들어진 시스템; 윈도우 계열, 일부 모바일 기기

- Boot Block

  - 부팅과 관련된 정보

- FAT

  - 파일의 메타 데이터 중 일부 보유

  - 위치 정보만

  - 나머지 정보는 디렉토리가 보유 (파일의 첫 번째 위치조차 디렉토리가 보유)

  - linked-allocation

    의 단점을 보완하여 사용

    - 블럭의 다음 블럭의 위치 정보를 저장
    - FAT table만 쭉 따라가서 정보를 찾으면, 실제 데이터 블록에서는 직접 접근이 가능
    - 데이터 블록과 FAT은 분리가 되어 있기 때문에 한 쪽의 포인터가 유실되더라도 괜찮다 + FAT은 중요한 정보이기 때문에 복사가 다른 곳에 되어 있다
    - 포인터를 따로 저장하기 때문에 포인터 크기로 인해, 한 파일이 쪼개지는 일이 없다

- Root Directory

- Data Block

### Free-Space Management

- 파일 시스템에서 빈 공간 관리하는 시스템

1. Bit map (bit vector)
   - 파일에 할당된 공간이면 1, 파일이 비어있는 공간이면 0으로 표시
   - Bit map은 디스크의 부가적인 공간을 필요로 한다.
   - (장점) 연속적인 n개의 free block을 찾는데 효과적 (연속적인 빈 블럭)
2. Linked List
   - 모든 빈 공간을 linked-list로 연결. 빈 블록의 첫 번째 위치만 포인터로 보유
   - bit map처럼 추가적인 공간 낭비 x
   - 연속적인 가용 공간을 찾는 것은 쉽지 않다
   - 실제로 사용하기는 쉽지 않다
3. Grouping
   - linked-list 방법의 변형
   - 첫 번째 빈 공간이 n개의 pointer를 보유
   - pointer 중 n - 1개는 빈 공간 블럭을 가리킨다
   - 마지막 포인터가 가리키는 블럭은 다시 n개의 pointer를 보유
   - 여전히 연속적인 빈 블럭을 찾기에는 비효율적
4. Counting
   - 연속적은 빈 블럭을 표시하기 위해, 빈 블럭의 첫 번째 위치와 첫 번째 블록부터 몇 개까지가 빈 블록(개수)인지 함께 관리
   - 프로그램들이 종종 여러 개의 연속적인 block을 할당하고 반납한다는 성질에서 착안

## 7. Directory 구현

- linear list
  - <파일 이름, 파일 메타데이터>로 구성된 list
  - 구현이 간단
  - 파일 이름과 메타데이터에 각각 고정된 크기 할당
  - 디렉토리 내에 파일이 있는지 찾기 위해서는 linear search 필요 → 찾는데 시간이 걸린다
- hash table
  - linear list + hashing
  - hash table은 파일의 이름을 이 파일의 linear list가 있는 위치로 변환해준다
  - search time을 없앤다
  - collision 발생 가능 (서로 다른 파일 이름에 대해 같은 entry로 mapping되는 현상)
- 파일의 metadata 보관 위치
  - 디렉토리 내에 직접 보관
  - 디렉토리에는 포인터에 두고 다른 곳에 보관 → inode, FAT 등
- Long file name의 지원
  - <파일 이름, 파일 메타데이터>의 list에서 각 entry는 일반적으로 고정 크기
  - 파일 이름이 고정 크기의 entry 길이보다 길어지는 경우 entry의 마지막 부분에 이름의 뒷 부분이 위치한 곳의 포인터를 두는 방법
  - 이름의 나머지 부분은 동일한 directory file의 일부에 존재

## 8. **VFS & NFS**

- Virtual File System (VFS)
  - 서로 다른 다양한 file system에 대해 동일한 시스템 콜 인터페이스 (API)를 통해 접근할 수 있게 해주는 OS의 layer
  - 없다면 개별 파일 시스템마다 다른 system call이 필요
- Network File System (NFS)
  - 분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있다
  - NFS는 **분산 환경**에서의 대표적인 파일 공유 방법 (원격에 저장된 파일 시스템)

## 9. Page Cache and Buffer Cache

- Page Cache
  - **virtual memory**의 paging sytem에서 사용하는 **page frame**을 **caching의 관점**에서 설명하는 용어 → swap area(backing store)보다 빠르기 때문
  - memory-mapped I/O를 쓰는 경우 file의 I/O에서도 page cache 사용
  - 운영체제에 주어지는 정보 부족
  - 단위: page
- Buffer Cache
  - **파일 시스템**을 통한 **I/O 연산**(ex. read system call)은 메모리의 특정 영역인 buffer cache 사용
  - 운영체제가 메모리의 일부를 buffer cache에 저장
    - 운영체제에게 정보가 다 주어진다
    - 읽어온 내용을 사용자에게 copy해서 넘겨준다
    - 동일한 내용의 요청이 들어오면 disk까지 갈 필요 없이 I/O에서 전달
  - file 사용의 locality 활용
    - 한번 읽어온 block에 대한 후속 요청시 buffer cache에서 즉시 전달
  - 모든 프로세스가 공용으로 사용
  - replacement algorithm필요 (**LRU, LFU** 등)
  - 단위: disk의 논리적인 block = sector (512 byte)
- Unified Buffer Cache
  - 최근의 OS에서는 기존의 **buffer chace가 page cache에 통합**됨
  - 버퍼 캐시를 **페이지 단위(4KB)**로 관리
  - 미리 buffer cache 영역과 page cache 영역으로 미리 나눠두지 않고, 페이지 단위로 필요할 때마다 할당
- Memory-Mapped I/O
  - **파일 시스템 → memory-mapped file**
  - 원래는 파일을 읽을 때 file을 open해서 read, write 연산(system call)을 통해 파일 접근 → memory-mapped i/o는 한 번 연결되면 메모리에서 파일을 읽고 쓰는 것을 가능하게 한다
  - file을 virtual memory(프로세스 주소 공간 일부)에 mapping 시킨다
  - 매핑시킨 영역에 대한 메모리 접근 연산은, 파일의 입출력으로 바로 연결된다
  - 한 번의 copy만으로 I/O 접근 가능
  - 일관성 문제 발생 가능 → physical memory에서 똑같은 내용을 참조할 경우 (memory-mapped가 아니라면 OS가 중재하고 한 번 더 copy하기 때문에 문제 발생 x)

- Unified buffer cache를 사용하지 않는 경우

  - read(), write() system call 사용:

     파일 open한 후 

    read(), write() system call

     → 운영체제가 파일 시스템에 있는 내용을 OS 내의 buffer caching로 읽어온다 (buffer cache) → 읽어온 내용을 사용자 프로그램에 전달 → page cache (프로세스가 페이지로 버퍼 캐시에 있는 내용 copy 해와 사용)

    - 내용이 buffer cache에 있든 없든 항상 OS에 요청을 해서 받아와야 한다
    - 무조건 파일 입출력 하려면 buffer cache를 통과해야 한다

  - memory-mapped I/O 사용

     : 파일 open한 후 read(), write() system call → 운영체제가 자신의 주소 공간 중 일부를 파일에 매핑 → 파일 시스템에 있는 내용을 OS 내의 buffer caching로 읽어온다 (buffer cache) → page cache로 copy → 읽어온 내용을 프로세스의 주소 영역과 매핑 → 이후에는 디스크에 접근하려는 시도가 (디스크에 가는 행위로 이어지지 않고) 바로 기억해둔 내용으로 접근해 읽고 쓰는 것으로 연결된다 (OS의 간섭 x)

    - 내용이 일단 page cache에 올라온 내용은 **OS의 도움을 받지 않고,** 자신의 메모리 영역 접근하듯이 I/O 접근 가능
    - read(), write()와 memory-mapped는 무조건 최소 한 번은 buffer cache를 통과

- **Unified Buffer Cache** **사용** : 운영체제가 공간을 따로 나눠 놓지 않고 필요한 경우에만 할당

  - read-write 시스템 콜 → OS에 CPU 제어권이 넘어가고, buffer cache에 내용이 있으면, 그걸 복사해서 사용자 프로그램에게 전달하고, 없으면 파일 시스템에서 buffer cache로 복사해 온 후, 다시 복사해 사용자에게 전달
  - memory-mapped I/O : 한 번 buffer memory로 읽어오고 나면 buffer memory =page cache가 되기 때문에, 버퍼 메모리에 있는 내용을 페이지 캐시에 복사해 올 필요 없이 바로 사용 가능

- virtual memory 중 code 부분은 read-only

  - physical memory가 부족해 프로세스가 쫓겨 나더라도, code 부분은 swap area로 갈 필요가 없다
  - read-only라 바뀐 내용이 없기 때문 + 원본 그대로 file system에 존재하기 때문
  - memory-mapped I/O를 쓰는 대표적인 예시
  - 코드 부분은 별도의 swap area를 존재하지 않고 파일 시스템이 그대로 프로세스 주소에 매핑

- virtual memory에서 data 부분은 read or write

  - memory mapped I/O로 쓸 때, physical memory에서 쫓겨나면 swap area가 아니라 file system의 file에다가 수정하고 쫓아내야 한다

# 3. Disk Management and Scheduling

## 1. Disk Structure

- logical block

  - 디스크 **외부**에서 디스크를 접근하는 최소 단위
  - 디스크 외부에서 보는 디스크 단위 정보 저장 공간들
  - logical block은 물리적인 sector와 mapping되어 있다
  - 주소를 가진 1차원 배열처럼 취급 → 디스크 logical block을 가져올 때 배열 인덱스로 호출
  - 정보를 전송하는 최소 단위

- Sector

  - Disk **내부**에서 Disk를 관리하는 최소 단위

  - Logical block이 물리적인 디스크에 매핑된 위치

  - Sector 0

    는 최외곽 실린더의 첫 트랙에 있는 첫 번째 섹터

    - sector 0는 logical block에서도 0번에 해당
    - 0번은 booting과 관련된 정보 저장

  - Sector에서 데이터를 읽고 쓰는 요청은 Disk Controller가 관리

## 2. Disk Management

- physical formatting (low-level formatting)
  - 디스크를 컨트롤러가 읽고 쓸 수 있도록 sector들로 나누는 과정
  - 각 sector는 **header + 실제data**(logical block; 보통 512 bytes) **+ trailer**로 구성
  - header와 trailer는 sector number(주소 mapping을 위한 sector 번호), ECC (Error-Correcting Code) 등의 정보가 저장되며 controller가 직접 접근 및 운영
    - ECC : 데이터를 작게 요약한 코드 (축약본) → 512 bytes에 대한 해시 함수 결과(축약한 결과; 2 byte)
    - 읽을 때 data와 ECC가 같다면 bad sector 없이 잘 저장되어 있던 것
    - ECC의 규모에 따라 에러를 수정할 수도 있다
- Partitioning
  - 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
  - sector 영역으로 묶어 logically 하나의 독립적 디스크로 만드는 것
  - OS는 이것을 **독립적 disk**로 취급 (logical disk)
  - (ex) C 드라이브, D 드라이브
  - 각각의 partition을 file system 용도(→ logical formatting), swap area 용도 등으로 사용 가능
- logical formatting
  - 파일 시스템을 만드는 것
  - (파일 시스템 내부에는 booting에 관한 파일도 포함)
  - **FAT, inode,** free space 등의 구조 포함
- Booting
  - ROM에 있는 “small bootstrap loader”의 실행
    - CPU는 메모리만 접근 가능. 디스크에는 직접 접근 불가
    - 디스크에서도 전원이 나가더라도 내용이 유지되는 소량의 부분: ROM
    - ROM에 부팅에 관련된 loader가 저장
    - 전원을 키면 CPU 제어권이 ROM의 주소를 가리키고, ROM에 있는 loader가 실행
  - sector 0 (**boot block**)을 main memory에 load하여 실행
  - sector 0는 “full Bootstrap loader program”
  - boot block은 OS를 디스크에서 memory로 load하여 실행
  - OS가 memory에 올라가 부팅 시작

## 3. Disk Scheduling

- Access time

  의 구성 (디스크 접근 시간)

  - seek time
    - 디스크 헤드가 해당 실린더(원판에서 같은 **트랙**에 위치하는 sector들; 원통형)로 움직이는데 걸리는 시간
    - 내부에 있는 트랙을 읽으려면 헤드가 안쪽으로 이동, 외부에 있는 트랙을 읽으려면 헤드가 바깥쪽으로 이동 → 이때 걸리는 시간 = seek time
    - 디스크 접근 시간 중 가장 큰 시간 차지 (기계 장치가 직접 이동)
  - rotational latency
    - 헤드가 원하는 섹터에 도달하기까지 걸리는 회전 지연 시간
    - 헤드가 (원하는 데이터가 있는 트랙에 도착한 후) 디스크가 회전을 해 트랙에서 원하는 sector에 도달하기까지 걸리는 시간
    - seek time의 약 10분의 1
  - transfer time
    - 실제로 헤드가 데이터를 읽고 쓰는 시간
    - 실제 데이터의 전송 시간
    - 접근 시간에서 아주 적은 시간만 차지
    - 한 번의 seek로 많은 데이터 양을 읽고 쓰는 것이 이득

- Disk bandwidth

  - 디스크 성능 지표
  - 단위 시간 당 전송된 바이트의 수
  - 가능한 seek time을 줄이는 것이 좋다

- Disk Scheduling

  - So, seek time을 최소화 하는 것이 목표
  - seek time = seek distance
  - 요청이 들어온 순서대로 하면 디스크 헤드의 이동이 커지기 때문에 비효율적

## 4. Disk Scheduling Algorithm

- Disk Scheduler는 disk가 아닌 OS에 위치
- So, 디스크 헤드, 실린더가 물리적으로 어느 위치에 있는지 정확히는 모를 수 있다. Disk Scheduler는 logical block을 통해 해결

[현재 Queue에 다음과 같은 실린더 위치의 요청이 존재하는 경우, 디스크 헤드 53번에서 시작할 때, 각 알고리즘의 수행 결과는?] (실린더 위치는 0 - 199)

→ 98, 183, 37, 122, 14, 124, 65, 67

### 1. FCFS

- first come first service
- 들어온 순서대로 처리
- 이동 거리가 아주 길어져 비효율적

### 2. SSTF

- shortest seek time first
- 요청 들어온 순서와 상관 없이, 현재 head 위치에서 가장 가까운 요청부터 처리
- 53, 65, 67, 37, 14, 98, 122, 124, 183, 199
  - 총 head의 이동:  236 cylinders
- **starvation** 문제 : 멀리 있는 주소는 영원히 처리가 안 될 수도 있다

### 3. SCAN

- = 엘리베이터 스케쥴링
- **Disk arm이 디스크의 한쪽 끝에서 다른 쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리**한다.
- 다른 한쪽 끝에 도달하면 역방향으로 이동하며 오는 길목에 있는 모든 요청을 처리하며 다시 반대쪽 끝으로 이동한다
- (장점)
  - 비교적 공정
  - 디스크 헤더 이용 거리 측면에서 효율적
- 문제점: 실린더 위치에 따라 대기 시간이 다르다
  - 가운데는 기대 시간이 짧다(최악의 경우 반 바퀴)
  - 양 끝은 기대 시간이 모 아니면 도

### 4. C-SCAN

- circular scan
- SCAN의 단점 보완
- 헤드가 한쪽 끝에서 다른 쪽 끝으로 이동하며 가는 길목에 있는 모든 요청 처리
- 다른 쪽 끝에 도달했으면 요청을 처리하지 않고 곧바로 출발점으로 다시 이동
- 이동거리는 다소 길어질 수 있지만, SCAN 보다는 균일한 대기 시간 제공

### 5. Other Algorithms

- N-SCAN
  - SCAN의 변형 알고리즘
  - 출발을 하면서 이미 큐에 들어와 있는 것은 이번 round에 처리하고, 이번 round에 가는 도중 들어온 요청은 다음에 처리
  - 일단 arm이 한 방향으로 움직이기 싲가하면 그 시점 이후에 도착한 job은 되돌아올 때 service
  - 조금 더 대기 시간의 편차를 줄일 수 있다
- LOOK and C-LOOK
  - SCAN과 C-SCAN의 비효율적인 면 개선
  - SCAN이나 C-SCAN은 요청이 있든 없든 헤드가 디스크 끝에서 끝으로 이동
  - LOOK(SCAN 개선)과 C-LOOK(C-SCAN 개선)은 헤드가 진행 중이다가 그 방향에 더 이상 기다리는 요청이 없으면 헤드의 이동 방향을 즉시 반대로 이동한다.

### 6. Disk-Scheduling Algorithm의 결정

- 현대 디스크 시스템에서는 **SCAN**에 기반한 알고리즘 사용 (헤드의 이동 거리 줄이기 위해)

- SCAN, C-SCAN 및 그 응용 알고리즘은 LOOK, C-LOOK 등이 일반적으로 디스크 입출력이 많은 시스템에서 효율적인 것으로 알려져 있다

- File의 할당 방법에 따라 디스크 요청이 영향을 받는다

  - 연속 할당 (이동 거리 감소), 불연속 할당(헤드 이동 거리 길어진다)

- 개별적인 요청을 처리하기 보다는, 여러 요청을 모아 한꺼번에 처리(merge)가 효율적

- 디스크 스케줄링 알고리즘은 필요할 경우 다른 알고리즘으로 쉽게 교체할 수 있도록 OS와 별도의 모듈로 작성되는 것이 바람직

## Swap-Space Management

- 하드 디스크(보조 기억 장치)를 사용하는 두 가지 이유

  - memory의 휘발성(volatile) → 파일 시스템은 영속성이 필요 (비휘발성)
  - 프로그램 실행을 위한 memory 공간 부족 → swap space, swap area (메모리의 연장 공간)

- Swap-space (swap area)

  - Virtual memory system에서는 디스크를 memory의 연장 공간으로 사용

  - 파일 시스템 내부에 둘 수도 있으나 별도 partition(physical disk → logical disk → os가 partition으로 구분된 디스크를 별개의 disk로 인지) 사용이 일반적

    - 공간 효율성보다는 속도 효율성이 우선

      - (cf) 파일 시스템은 공간 효율성 중시(빨리 메모리로 올리고 내리는 것이 중요)
  - 어차피 프로세스가 종료되거나 전원이 없으면 비게 될 공간
      
- 일반 파일보다 훨씬 짧은 시간만 존재하고 자주 참조된다
    
    - 따라서, block의 크기 및 저장 방식이 일반 파일 시스템과 다르다

      - seek time을 줄이기 위해서 한 번에 많은 양을 모아서 한다 → 512 KB
  - 짧은 시간에 빠른 서비스 제공을 위해

## RAID

: Redundant Array of Independent Disks

- 여러 개의 디스크(특히 저렴한 디스크)를 묶어서 사용

- 목적

  - 디스크 처리 속도 향상

    - 여러 디스크에 block의 내용을 **분산 저장**
    - **병렬적**으로 읽어온다 (**interleaving, striping**)

  - 신뢰성(reliability)

     향상

    - 동일 정보를 여러 디스크에 중복 저장
    - 하나의 디스크가 고장(failure)시 다른 디스크에서 읽어온다 (**Mirroring, shadowing**)
    - 단순한 중복 저장하는 방법도 있지만, 일부 디스크에 parity를 저장하여 공간의 효율성을 높일 수 있다(오류가 발생했는지 알아차리고 복구할 수 있을 정도로만 중복 저장)