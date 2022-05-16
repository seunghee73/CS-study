# 1. Deadlocks

**Deadlock (교착 상태)**

: 일련의 프로세스들이 서로가 가진 자원을 기다리며 **block**된 (잠들어 있는) 상태

- Resource (자원)
  - 하드웨어, 소프트웨어 등을 포함하는 개념
  - (예) I/O device, CPU cycle, memory space, semaphore 등
  - 프로세스가 자원을 사용하는 절차 : Reqeust (자원 요청) → Allocate (획득) → Use (사용) → Release (반납)



## 1. ⭐ Deadlock 발생하는 조건 4가지 ⭐

### 1. Mutual exclusion (상호 배제)

- 매 순간 하나의 프로세스만이 자원 사용 가능

- 독점적 사용

### 2. Nopreemption (비선점)

- 프로세스는 자원을 스스로 내어 놓을 뿐, 강제로 뺏기지 않는다

### 3. Hold and wait (보유 대기)

- 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있다

### 4. Circular wait (순환 대기)

- 자원을 기다리는 프로세스 간에 사이클이 형성 되어야 한다

- 프로세스 P0, P1, ..., Pn이 있을 때

  - P0는 P1이 가진 자원을 기다린다

  - P1은 P2가 가진 자원을 기다린다

  - Pn-1은 Pn이 가진 자원을 기다린다

  - Pn은 P0가 가진 자원을 기다린다

    

## 2. **Resource-Allocation Graph (자원 할당 그래프)**

- Deadlock이 발생했는지 여부를 알 수 있는 그래프
- Cycle이 없으면 Deadlock X
- Cycle이 있으면
  - 자원 당 instance가 하나 밖에 없다면 cycle = deadlock
  - 자원에 instance가 여러 개 있는 상황에서는 deadlock 일수도 아닐 수도 있다



## 3. Deadlock 처리 방법

​	→ 위로 올 수록 더 강한 방법

- **Deadlock Prevention**
  - 미연에 방지
  - 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것
- **Deadlock Avoidance**
  - 미연에 방지
  - 자원 요청에 대한 **부가적인 정보**를 이용해서 **Deadlock의 가능성이 없어지는 경우에만** 자원 할당
  - 시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원 할당
- **Deadlock Detection and Recovery**
  - 미연에 방지하는 것은 너무 비용이 크다. 그러니 일단 발생 허용
  - Deadlock 발생은 허용하되, 그에 대한 detection 루틴을 두어 Deadlock 발견 시 recover
- **Deadlock Ignorance**
  - Deadlock을 시스템이 책임지지 않는다
  - UNIX를 포함한 대부분의 OS가 채택 → 사람이 프로세스가 느려진다든지 낌새를 느끼고 알아서 프로세스를 끈다 (Deadlock이 흔히 발생하는 상황이 아니기 때문에, Deadlock을 방지하려는 것 자체가 overhead가 클 수 있다)



### 1. Deadlock Prevention

​	: 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나를 원천 차단

- Mutual Exclusion
  - 공유해서는 안 되는 자원의 경우 반드시 성립해야
  - But! 사실 배제 가능한 조건 X (이게 안 되서 Deadlock이 발생)
- Hold and Wait
  - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다 (자원을 보유하면서 다른 자원을 요청)
  - (방법 1) 프로세스 시작 시 필요한 모든 자원을 할당받게 하는 방법 → 종료될 때 전부 반납 + 중간에 다른 자원 요청 x → 중간에 잠깐 필요한 자원까지 프로세스 실행하는 동안 보유하기 때문에 비효율적
  - (방법2) 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청 → 방법 1과 달리 필요할 때마다 자원을 요청하지만 (처음부터 한번에 얻는 것이 아니라), 요청할 때마다 기존에 가지고 있던 자원을 자진 반납
- No Preemption
  - 가지고 있는 자원을 뺏어올 수 없었기 때문에 Deadlock 발생 → 자원을 뺏어올 수 있게 하면 발생 안 한다
  - process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
  - state를 쉽게 save하고 restore할 수 있는 자원에서 주로 사용 (CPU, memory) → ex. timer로 CPU 선점 보장
  - 모든 필요한 자원을 얻을 수 있을 때, 그 프로세스는 다시 시작된다
- Circular Wait
  - 사이클 발생
  - 모든 자원 유형에 할당 순서를 정해, 순서대로만 자원 할당 (낮은 번호를 획득해야지만 높은 번호의 자원을 획득할 수 있도록 만든다)
  - 예를 들어 순서가 3인 자원 Ri을 보유중인 프로세스가, 순서가 1인 자원 Rj를 할당받기 위해서는 우선 Ri를 release해야 한다

​	⇒ Deadlock을 원천적으로 막을 수 있지만, 자원 Utilization (이용률) 저하, throughput 감소 (시스템 성능 감소), starvation 문제

​	→ Deadlock이 발생하는 빈도에 비해 너무 많은 단점

### 2. Deadlock Avoicance

​	: 자원 요청에 대한 부가 정보를 이용해, 자원 할당이 Deadlock으로부터 안전(safe)한지를 동적으로 조사한 후, 안전한 경우에만 할당

- 가장 단순하고 일반적인 모델은 프로세스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법

  - 프로세스가 시작할 때, 프로세스가 평생 쓸 자원을 미리 알고 있다고 가정
  - **여분이 있어도 Deadlock 발생할 가능성이 있다면 주지 않는다**
  
- safe state

  - 시스템 내의 프로세스들에 대한 safe sequence가 존재하는 상태

- safe sequence

  - 프로세스의 sequence <P1, P2, ..., Pn>이 safe 하려면 Pi(1≤i≤n)의 자원 요청이 “가용 자원 + 모든 Pj (j < i)의 보유 자원”에 의해 충족되어야
  - 조건을 만족하면 다음 방법으로 모든 프로세스의 수행 보장
    - Pi의 자원 요청이 즉시 충족될 수 없으면 모든 Pj (j < i)가 종료될 때까지 기다린다
    - Pi-1이 종료되면 Pi의 자원 요청을 만족시켜 수행한다
  - 가용 자원에서 현재 최대 요청인 P1에 투자. P1이 프로그램 종료되면 모든 쓰던 자원을 뱉어내고 종료. 다시 이 가용 자원을 가지고 최대 요청을 충족하는 것이 있는지 살펴보고 P2에게 주는 것을, P3, P4, P5....반복. 이렇게 순차적으로 이어지는 sequence가 하나라도 있으면 safe하다

- 시스템이 safe state에 있으면 ⇒ no deadlock 시스템이 unsafe state에 있으면 ⇒ possibility of deadlock

- Deadlock avoidance는 시스템이 unsafe state에 들어가지 않는 것을 보장

- Avoicance 알고리즘 2개

  - resouce type 당 Single instance인 경우

    - **Resource Allocation Graph algorithm (자원 할당 그래프)**

  - resouce type 당 Multiple instance인 경우

    - **Banker’s Algorithm (뱅커스 알고리즘)**

**[Resource Allocation Graph algorithm]**

- Claim edge Pi → Rj
  - 프로세스 Pi가 자원 Rj를 미래에 요청할 수 있음을 뜻한다 (점선)
    - 평생에 적어도 한 번은 이 자원을 사용할 일이 있다 (declare)
  - 프로세스가 해당 자원 요청 시 request edge로 바뀐다 (실선)
    - 자원 → 프로세스 : 해당 자원이 어떤 프로세스에 할당된 상태
    - 프로세스 → 자원 : 자원을 request 했지만, 아직 받지 못한 상태
  - Rj가 release 되면 assignment edge는 다시 claim edge로 바뀐다
- request edge의 assignment edge 변경 시 (점선을 포함하여) cycle이 생기지 않는 경우에만 요청 자원을 할당한다
- Cycle 생성 여부 조사 시 프로세스의 수가 n일 때 O(n ** 2)의 시간이 걸린다

→ (마지막 그래프) 아직 Deadlock이 아니지만(P1이 R2를 요청한다 하면 deadlock이 발생하지만, 아직은 요청하지 않았다 + R2를 쓰기 전에 R1을 반납), 자원 할당 그래프는 최악의 상황을 가정하기 때문에, deadlock이 발생할 가능성이 있으므로, P1이 R2를 요청해도 주지 않는다

**[Banker’s algorithm]**

- 가정

  - 모든 프로세스는 자원의 최대 사용량을 미리 명시
  - 프로세스가 요청 자원을 모두 할당 받은 경우 유한 시간 안에 이들 자원을 다시 반납

- 방법

  - 기본 개념: 자원 요청 시 safe 상태를 유지할 경우에만 할당
  - 총 요청 자원의 수가 가용 자원의 수보다 적은 프로세스를 선택 (그런 프로세스가 없으면 unsafe 상태)
  - 그런 프로세스가 있으면 그 프로세스에게 자원 할당
  - 할당 받은 프로세스가 종료되면 모든 자원을 반납
  - 모든 프로세스가 종료될 때까지 이러한 과정 반복

  → Available (가용 자원)과 추가로 요청 가능한 양 (Need)를 비교해서, 자원을 줄지 말지 결정

  

### 3. Deadlock Detection and Recovery

: 일단 자원을 요청하는 대로 모두 준다.

- Resource type 당 single instance인 경우 : **자원 할당 그래프**에서 cycle은 곧 deadlock의미

- Resource type 당 multiple instance인 경우 : Banker’s algorithm과 유사한 방법 활용

  - Deadlock을 banker’s와 달리 낙관적으로 본다: 현재 추가로 요청한 자원이 없는 프로세스는 곧 자신이 쓰고 있는 자원을 반납할 것이라 생각. 예정된 반납 + 가용 자원 > 요청이면 Deadlock이 발생하지 않을 것이라 예상. 하지만, 예정된 반납 + 가용 자원 < 요청이면 Deadlock

- Wait-for graph 알고리즘

  - Resource type 당 single instance인 경우
  - 자원 할당 그래프의 변형
  - 프로세스만으로 node 구성
  - Pj가 가지고 있는 자원을 Pk가 기다리고 있는 경우 Pk → Pj
  - Wait-for graph에 사이클이 존재하는 지 주기적으로 조사
  - O(n ** 2)

- Deadlock이 발견된 경우 Recovery 필요

  - Process termination

     프로세스 종료

    - (방법 1) abort all deadlocked processes 데드락에 연루된 모든 프로세스를 죽인다
    - (방법 2) abort one process at a time until the deadlock cycle is eliminated 데드락에 연루된 프로세스를 하나씩 죽여가며 데드락이 해지되는지 확인

  - Resource Preemption

     자원을 뺏기 (선점)

    - 비용을 최소화할 victim 선정하고 뺏는다

    - safe state로 rollback하여 process restart

    - starvation 문제

      - 동일한 프로세스가 계속해서 victim으로 선정되는 경우 (자원을 뺏는 패턴을 조금씩 다르게 해야 한다)

      - cost factor(비용 요소)에 rollback 횟수도 같이 고려 (rollback도 희생이 크기 때문)

        

### 4. Deadlock Ignorance

​	: Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않는다

- Deadlock이 매우 드물게 발생하므로 Deadlock에 대한 조치 자체가 더 큰 overhead일 수 있다
- 만약, 시스템에 Deadlock이 발생한 경우, 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 process를 죽이는 등의 방법으로 대처
- UNIX, Windows 등 대부분의 범용 OS가 채택





# 2. Memory Management

 ## 1. Logical & Physical Address

- Logical Address (= Virtual Address)
  - 프로세스마다 독립적으로 가지는 주소 공간 (프로그램 마다 가지는 가상의 주소 공간)
  - 각 프로세스마다 0번지부터 시작
  - CPU가 보는 주소는 logical address
    - 물리적 주소이더라도 코드 상의 주소는 logical address로 남아있기 때문
    - CPU가 메모리를 읽어 들일 때마다 logical → physical 주소 변환 필요
- Physical Address
  - 메모리에 실제 올라가는 위치 (실행될 때)
  - 물리적인 메모리는 (모든 프로세스) 통으로 0번부터 시작 가장 아래에는 Kernel이 메모리에 올라가 있다
- 주소 바인딩 : 프로그램마다 갖고 있던 논리적 주소가, 물리적인 메모리 어디로 올라갈지 주소를 결정하는 것 : Symbolic Address → Logical Address → Physical Address
  - Symbolic Address : 프로그래머는 직접 주소에 접근하지 않고, 변수를 사용해 접근 ⇒ symbol
  - compile 때 symbol이 0번지부터 가지는 독자적인 logical한 숫자 주소로 변환
  - logical에서 physical address로 바뀌는 시점은 3가지 존재

## 2. 주소 바인딩 (Address Binding)

: logical address → physical address

### 1. Compile time binding

- 물리적 메모리 주소(physical address)가 compile 단계에서 정해진다 → compile 때 0번부터 시작하는 logical address가 생성되는데, 이때 physical도 정해지게 된다 ⇒ absoulte code → physical address = logical address : 물리적 메모리의 다른 위치가 많이 비어있음에도, 항상 0번부터 올라가게 되는 비효율
- 프로그램 하나만 돌리던 컴퓨터 시절에 사용. 지금은 사용하지 않는다.
- 컴파일러는 **절대 코드 (absolute code)** 생성
- 시작 위치 변경을 원한다면 다시 컴파일 새로 해야 한다

### 2. Load time binding

- 프로그램이 실행 시작될 때 physical memory 위치 결정
- Loader의 책임 하에 물리적 메모리 주소 부여
- 컴파일러가 **재배치 가능 코드 (relocatable code)**를 생성한 경우 가능
  - compile time binding과 달리 실행 시에 어느 위치든 올라갈 수 있는 코드라는 것

### 3. Execution time binding (= Runtime binding)

- 프로그램이 실행 시작될 때 physical memory 위치 결정
- 프로그램 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있는 바인딩 방법
- 현재 사용되는 시스템은 runtime binding 지원
- CPU가 주소를 참조할 때마다 binding을 점검 필요 (address mapping table) → 계속 주소 변환이 일어나기 때문
- So, 하드웨어적인 지원이 필요 (그때그때 참조하는 주소를 변환하는 기술) (e.g. base and limit registers, **MMU**)



## 3. **Memory-Management Unit (MMU)**

: logical address를 physical address로 매핑해주는 Hardware device

- 주소 변환 기능 지원
- register 두 개를 통해서 주소 변환 : OS 및 사용자 프로세스 간 메모리 보호를 위해 사용
  1. limit register
     - 논리적 주소 범위
     - 악의적인 프로그램이 자신의 메모리 크기를 넘어서는 메모리 위치를 요구할 때
     - CPU가 주소를 요청할 때 limit register의 위치를 넘어서는 메모리 주소를 요청하면 limit register가 **trap** 발동
  2. relocation register (= base register)
     - 실제 물리적 주소에서 시작 위치 저장 (접근할 수 있는 물리적 메모리 주소 최소값)
     - limit register를 통과한 요청 대상
     - relocation register가 저장하고 있는 주소 + CPU가 요구한 logical address 주소 = physical address
- **MMU scheme** : 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해 base register (=relocation register)의 값을 더한다
- user program
  - logical address만을 다룬다
  - **실제 physical address를 볼 수 없으며 알 필요가 없다**

[**용어**]

- **Dynamic Loading**

  - Loading: 프로그램에 메모리를 올리는 것
  - 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라, 해당 루틴이 불려질 때마다 메모리에 load
  - memory utilization의 향상
  - 프로그램 메모리 전체를 사용하는 것이 아니라, 상당 부분은 오류 처리 루틴일 가능성이 높다
    - 자주 사용하는 루틴의 경우 유용
    - 오류 같은 상황이 생겼을 때 이 부분을 올리는 것
  - OS의 특별한 지원 없이 프로그램 자체에서 구현 가능 (OS는 라이브러리를 통해 지원 가능) → 사실상 OS가 관리하면 페이징 시스템, 프로그래머가 관리하면 Dynamic Loading (하지만 섞어서 쓰기도)
  - (cf) 페이징 시스템
    - 필요한 부분만 메모리에 올리고, 필요 없어진 부분은 메모리에서 쫓는 기법
    - OS가 직접 관리

- **Overlays**

  - 메모리에 프로세스 부분 중 실제 필요한 정보만 올린다
  - 작은 공간의 메모리를 사용하던 컴퓨터 초창기 시스템에서 수작업으로 프로그래머가 구현하던 것
    - dynamic loading과 overlay는 사실상 같은 개념이지만 역사 배경이 다르다
    - 워낙 메모리가 작기 때문에 프로그램 하나를 올리는 것도 힘들었고, 그래서 프로그램 하나를 수작업으로 쪼개던 것이 overlay
    - = manual overlay
    - 프로그래밍이 매우 복잡
    - dynamic loading은 OS가 라이브러리를 제공하기 때문에, 어떻게 메모리고 올리고 내리는지 세세히 알 필요 없다
  - 프로세스의 크기가 메모리보다 클 때 유용
  - 운영체제의 지원 없이 사용자에 의해 구현

- **Swapping**

  - 프로세스를 일시적으로 메모리에서 backing store으로 쫓아내는 것
  - Backing store (= swap area)
    - main memory에서 하드 디스크로 쫓겨난 것을 저장하는 곳(하드 디스크 내에 위치)
    - 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장공간
  - swap in / swap out
    - swap in (backing store에서 다시 메인 메모리로) swap out (메인 메모리에서 backing store로)
    - 일반적으로 **중기 스케줄러 (swapper)**에 의해 swap out 시킬 프로세스 선정
    - priority-based CPU scheduling algorithm
      - 우선순위 기반으로 swap
      - priority가 낮은 프로세스를 swapped out 시킨다
      - priority가 높은 프로세스를 메모리에 올려 놓는다
    - **Compile time 혹은 load time binding에서는 원래 메모리 위치로 swap in** 해야 한다 (다른 메모리 위치가 비어 있더라도)
    - **Execution time binding(Runtime binding)에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있다** → swapping이 compile time binding이나 load time binding에 비해 효율적
    - swap time은 대부분 **transfer time** (**swap되는 양에 비례하는 시간**)이다 → 많은 양을 올리고 내리기 때문에 (전부 쫓아낸다)
    - 요새는 페이징 기법에서 일부만 쫓겨나는 것도 swap out이라 섞어 쓰기도

- **Dynamic Linking**

  - linking : compile한 후 여러 군데 존재하는 compiled file을 묶어 하나의 실행 파일로 만드는 것 (ex. 내 코드 + 라이브러리 코드)

  - Linking을 실행 시간 (execution time)까지 미루는 기법

  - Static linking

    - 라이브러리가 프로그램의 실행 파일 코드에 포함된다
    - 실행 파일의 크기가 커진다
    - 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비 (e.g. printf 함수의 라이브러리 코드)

  - Dynamic linking

    - 라이브러리가 실행 시 연결(link) 된다 (compile 때는 포함 되지 않았다가, 호출될 때 link)

    - 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 **stub**(일종의 포인터)이라는 작은 코드를 둔다

    - 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어온다

    - dynamic linking 제공하는 라이브러리: shared library

    - 운영체제의 도움이 필요

      

## 4.**Allocation of Physical Memory**

- 메모리는 일반적으로 두 영역으로 나뉘어 사용
  - OS 상주 영역 : interrupt vector와 함께 낮은 주소 영역 사용
  - 사용자 프로세스 영역 : 높은 주소 영역 사용
- 사용자 프로세스 영역의 할당 방법
  - Contiguous allocation (연속 할당)
    - 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
    - 모든 메모리를 한 곳에 통째로 올리는 것
    - Fixed partition allocation
    - Variable partition allocation
  - Noncontiguous allocation (불연속 할당)
    - 하나의 프로세스가 메모리의 여러 영역에 분산해 올라갈 수 있다
    - 현대 대부분의 시스템 채택
    - page나 segment 단위로 주소 변환을 해야 하기 때문에, 주소 변환이 연속 할당에 비해 복잡하다
    - Paging
    - Segmentation
    - Paged Segmentation

### 1.**Contiguous Allocation**

- 고정 분할(Fixed Partition) 방식
  - 사용자 프로그램이 들어갈 메모리 영역을 미리 분할해 둔다
  - 물리적 메모리를 몇 개의 영구적 분할(partition)으로 나눈다
  - 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재
  - 분할 당 하나의 프로그램 적재
  - 융통성이 없이 미리 공간을 나눠 놓는다
    - 동시에 메모리에 load되는 프로그램 수 고정되어 있다
    - 최대 수행 가능 프로그램 크기 제한
  - Internal fragmentation 발생 (external fragmentation도 발생)
    - External Fragmentation (외부 조각)
      - 프로그램 크기가 분할된 영역보다 작은 경우 발생
      - 프로그램이 들어갈 수 있는 공간임에도, 올리려는 프로그램보다 공간이 작아 안 쓰이는 경우
      - 아무 프로그램이 배정되지 않은 빈 곳인데도, 프로그램이 올라갈 수 없는 분할 지역
    - Internal Fragmentaion (내부 조각)
      - 프로그램 크기보다 분할의 크기가 큰 경우
      - 하나의 분할 내부에서 발생하는 사용되지 않는 메모리 조각
      - 특정 프로그램에 배정되었지만 사용되지 않은 공간
- 가변 분할(Variable Partition) 방식
  - 사용자 프로그램이 들어갈 메모리 영역을 미리 분할해 두지 않는다
  - 프로그램의 크기를 고려해서 할당
  - 프로그램이 실행될 때마다 차곡차곡 올린다
  - 그런데, 중간에 어떤 프로그램이 먼저 종료되는 경우, 빈 공간 발생
  - → External fragmentation 발생 (프로그램의 크기가 동일하지 않기 때문에 발생)
  - 분할의 크기, 개수가 동적으로 변함
  - 기술적 관리 기법 필요

**Hole**

- 비어있는 가용 메모리 공간 ← fragment
- 다양한 크기의 hole이 메모리 여러 곳에 산발적으로 발생
- 프로세스가 도착하면 수용 가능한 hole을 할당
- 운영체제는 다음의 정보를 유지
  - 할당 공간
  - 가용 공간 (hole)

**Dynamic Storage-Allocation Problem**

: **가변 분할 방식**에서 size N인 요청을 만족하는 가장 적절한 Hole을 찾는 문제 (Hole을 어떻게 분배할지)

- First-fit
  - size가 N 이상(N보다 같거나 큰 것)인 것 중 최초로 찾아지는 hole에 할당
- Best-fit
  - size가 N 이상인 가장 작은 Hole을 찾아서 할당
  - 프로그램과 용량이 가장 비슷한 것에 넣는다
  - Hole들의 리스트가 크기 순으로 정렬되지 않은 경우 모든 Hole의 리스트를 탐색해야 한다
  - 많은 수의 아주 작은 hole이 생성된다
- Worst-fit
  - 가장 큰 hole에 할당
  - 역시 모든 리스트를 탐색해야 한다
  - 상대적으로 아주 큰 hole들이 생성된다

→ first-fit과 best-fit이 worst-fit보다 속도와 공간 이용률 측면에서 효과적인 것으로 알려진다 (실험적인 결과)

**Compaction (컴팩션)**

- external fragmentation 문제를 해결하는 방법 중 하나
- 사용 중인 메모리 영역을 한 곳으로 몰고, Hole을 다른 한 곳으로 몰아 아주 큰 block을 만드는 것
- 매우 비용이 많이 드는 방법
- 최소한의 메모리 이동으로 compaction하는 것은 아주 어려운 문제
- compaction은 프로세스의 주소가 실행 시간에 동적으로 재배치 가능(**Runtime binding**)한 경우에만 수용 가능

### 2. Noncontiguous Allocation

- paging
- segmentation



## 5. Paging

- 프로그램을 구성하는 주소 공간을 **동일한 크기의 page**로 잘라, page 단위로 물리적인 메모리에 올려놓거나 swap area에 내려 놓기

  - 보통 4KB로 자른다
  - 보통 프로그램 하나 당 100만 개 프레임 생성 → CPU register 안에 다 못 들어 간다 → page table도 100만 개라는 상당히 큰 크기인데, 프로그램 별로 존재 ⇒ memory에 page table이 들어간다

- Process의 virtual memory(주소 공간)를 동일한 사이즈의 **page** 단위로 나눈다

- **page frame**

  : physical memory도 동일한 크기의 page 단위로 분할하게 되는데, 이를 page frame이라 한다

- virtual memory의 내용을 page 단위로 noncontiuous하게 저장

- page라는 단위가 생겼기 때문에, 주소 변환이 단순히 두 개의 register로 가능하지 않고, **page table**이 필요

  - 각각의 페이지마다 물리적인 메모리 어디에 저장되어 있는지 기록해둔 배열

  - 페이지마다 주소 변환 필요

  - 페이지 개수만큼 크기(entry) 존재

  - 변환 후 물리적 메모리 어느 위치에나 갈 수 있다

  - Page table은 **main memory**에 상주

  - **Page-table base register (PTBR)**이 page table을 가리킨다

  - **Page-table limit register (PTLR)**이 테이블 크기 보관

  - 모든 메모리 접근 연산에는 **2번의** memory access 필요

  - main memory의 page table에 한 번 접근, 실제 data/instruction이 적혀있는 곳에 또 한 번 접근

  - 속도 향상을 위해 (main memory와 CPU 사이에 위치해) 

    associative register

     혹은 **translation look-aside buffer (TLB)**라 불리는 고속의 lookup hardware cache (데이터 보관과 접근을 위한 캐시 메모리처럼 주소 변환을 위한 별도의 캐시 register) 사용

    - 빈번히 접근하는 주소를 TLB에 저장
    - 주소 변환 하기 전에 먼저 TLB에 정보가 있는지 체크. TLB에 있다면 TLB에 접근하고 종료 (memory 접근 한 번)
    - 없다면 memory에 2번 접근 필요 (cache miss)
    - 그냥 페이지 register가 page/frame : offset의 쌍이었다면, TLB는 page:frame의 쌍
    - TLB는 위에서부터 순서대로 검색해야 하기 때문에, 시간이 오래 걸린다 → So, **parallel search**가 가능한 **associative register**를 사용해 구현 (TLB는 array 형태라 그렇고, page table은 hash 형태라 검색 시간에 차이 존재)

- external fragmentation은 발생하지 않지만, internal fragmentation은 발생 가능 (프로그램의 앞 부분을 페이지로 잘라내고 남은 마지막 부분이 항상 페이지 단위 크기만큼 남지 않는다)

- 페이지를 동일한 크기로 자르기 때문에 hole처럼 크기로 고민할 필요 없다

- **Associative Register**
  - parallel search가 가능
  - TLB에는 page table 중 일부만 존재
  - Address translation
    - page table 중 일부가 associative register에 보관되어 있다
    - 만약 해당 page #가 associative register에 있는 경우 곧바로 frame #을 얻는다
    - 그렇지 않은 경우 main memory에 있는 page table로부터 frame #을 얻는다
    - TLB는 context switch 때 flush (remove old entries) : 프로세스마다 주소 변환이 다르기 때문
- Effective Access Time

### 1. Two-Level Page Table (이단계 페이지 테이블)

- outer-page table, page table의 두 단계로 구성 (두 번에 걸쳐 주소 변환)
  - outer-page table 하나 당, page table이 존재
  - page table: page 하나의 크기가 **4KB** (4 * 2 ** 10 = 2 ** 12), entry 하나가 **4Byte**씩(즉, 주소 하나 당 4B), entry가 **1K**(2 ** 10)개 존재
  - page 하나 당 2 ** 12 이기 때문에, bit 수는 000000000000 12개
  - logical address (on 32-bit machine with 4KB page size)의 구성
    - 12bit의 page offset (각 주소 구분 용 자리수)
    - 20bit의 page number (32 bit - page offset) : outer page번호와 inner page 번호의 공간으로 나눈다 ↓
  - 안쪽 page table 자체가 page로 구성되어 outer page table에 들어가기 때문에 page number는
    - 10 bit의 page number(page table의 위치)
    - 10 bit의 page offset으로 구분 (각각의 page table 내에서 entry 주소들 → entry는 1K개 있으므로 1K를 구분할 수 있어야 한다 → 2 ** 10이므로 10bit 필요)
- 속도는 더 걸리거나 줄어들지 않지만
  - 페이지 테이블은 중간에 entry를 없애고 만들 수 없기 때문에 page table은 무조건 maximum logical table의 크기 만큼 만든다
  - 이단계 페이지 테이블은 바깥쪽 페이지 테이블은 전체 논리적 메모리 크기만큼 만들어지지만, 실제 사용되지 않는 메모리 영역은 null로 하고 안쪽 테이블을 만들지 않고, 사용되는 영역만 안쪽 테이블로 만든다.
  - 테이블을 두 개 만듦에도 불구하고, 프로그램 메모리에서 사용되지 않는 영역이 더 크기 때문에 가능한 공간 절약
- 현대 컴퓨터는 address space가 매우 큰 프로그램 지원
  - 32 bit address 사용 시 : 2 ** 32 (4G)의 주소 공간
    - 프로그램의 virtual memory 크기 결정
    - 주소는 0 ~ 2 ** 32 - 1 (byte)의 주소 보유 (2 ** 10 = K, 2 ** 20 = M, 2 ** 30 = G)
  - page size가 4KB 시 1 M 개의 page table entry 필요
  - 각 page entry당 4Byte 필요하므로, 프로세스 당 4M의 page table 필요
  - 그러나, 대부분의 프로그램은 4G 주소 공간 중 지극히 일부분만을 사용하므로 page table 공간이 심각하게 낭비

⇒ page table 자체를 page로 구성

- 사용되지 않는 주소 공간에 대한 outer page table의 엔트리 값은 NULL (대응하는 inner page table이 없다)

- (18) Memory Management 3



### 2. **Multilevel Paging** and Performance (다단계 페이지 테이블)

- Address space가 더 커지면(프로그램의 주소 공간이 넓다면) 다단계 페이지 테이블 필요
- 각 단계의 페이지 테이블이 메모리에 존재하므로, logical address의 physical address 변환에 더 많은 메모리 접근 필요 (접근할 때 주소 변환이 테이블 만큼 더 필요하다)
- **TLB**(ex. pararell search인 associative register로 구현) 통해 메모리 접근 시간을 줄일 수 있다
- 4단계 페이지 테이블을 사용하는 경우
  - 메모리 접근 시간이 100ns인 경우 → 원래대로라면 주소 변환에만 4단계 + 실제 값이 저장된 메모리 위치 접근 1단계 ⇒ 500ns (시간 overhead 크다)
  - But! TLB를 통해 다단계 페이지 테이블이더라도 그렇게 오래 걸리지 않는다.
  - TLB(주소 변환 전담하는 캐시 메모리)를 통해 접근한다면 TLB 접근 시간이 20ns이고, TLB hit ratio 가 98%인 경우(TLB에서 바로 주소 변환이 되는 경우), effective memory access time = 0.98 * 120 + 0.02 * 520(=TLB + 500 ns) = 128 nanoseconds (평균 소요 시간)
  - 결과적으로 주소 변환을 위해 28ns(128-100)만 추가적으로 소요

### 3. **Memory Protection**

: 페이지 테이블 내 각 entry 마다 물리적 메모리로의 주소 변환 정보만이 아니라, 부가적인 bit가 entry마다 같이 저장

- Protection bit

  - Page 연산에 대한 접근 권한 (read/ write/ read-only)
    - code → read-only : 내용이 바뀌면 안 된다 data, stack → read, write : 읽고 쓰기 가능
  - 다른 프로세스가 테이블을 못 보게 하는 것 X (페이지 테이블은 원래부터 해당 프로세스만 접근 가능)

- Valid-invalid bit

  - 프로그램의 주소 공간이 가질 수 있는 maximum size만큼 페이지 테이블 entry 생성

    - table -array- 의 특성 상 위부터 index로 접근하기 때문
    - But! 프로그램을 실행할 때 모든 code, data, stack을 사용하지 않는다
    - 사용하지 않는 부분은 physical memory에 올리지 않기 때문에, 사용하는 부분은 valid (v), 사용하지 않는 부분은 invalid(i)로 표시

  - **valid**는 해당 주소의 frame에 그 프로세스를 구성하는 유효한 내용이 있있음을 뜻한다 (접근 허용)

  - invalid는 해당 주소의 frame에 유효한 내용이 없다는 것을 뜻한다 (접근 불허)

    - 프로세스가 그 주소 부분을 사용하지 않는 경우
- 해당 페이지가 메모리에 올라와 있지 않고 swap area에 있는 경우

### 4.**Inverted Page Table**

- page table이 매우 큰 이유
  - page table 자체의 크기가 크다!
  - 모든 process 별로 그 logical address에 대응하는 모든 page에 대해 page table entry가 존재
  - 대응하는 page가 메모리에 있든 아니든 간에 page table에는 entry로 존재
- Inverted page table
  - 원래대로라면 프로세스 마다 page table 존재 Inverted page table은 시스템 안에 페이지 테이블이 딱 하나 존재 (system-wide)
  - entry가 logical memory의 page 개수만큼 존재 X physical memory의 page frame 개수만큼  entry 존재
  - 기존의 page table은 원하는 내용이 logical memory의 번호만큼 page table에서 떨어져 있는 것을 찾아 physical memory로 변환 → inverted page table은 불가능
    - 사실상 physical address → logical address를 바꾸는데 더 적합하지만, 필요로 하는 기능 X
    - 어떤 프로세스의 프레임인지 process ID와 해당 프로세스에서 몇 번째 페이지인지 페이지 번호를 같이 저장해야 한다
  - So, 단점 : 테이블 전체를 탐색해야 한다
    - 페이지 테이블의 공간을 줄이기 위해 사용. 대신 시간을 교환
  - 각 page table entry는 각각의 물리적 메모리의 page frame이 담고 있는 내용 표시 (process-id, process의 logical address)
  - 조치: **associative register** 사용 (expensive)

### 5. **Shared Page**

: 다른 프로세스와 공유할 수 있는 페이지 존재 (ex. 아래 한글 프로그램을 3개 돌린다) → 같은 프로그램이면 data, stack만 다르지 code는 동일

- Shared Code

  - Re-entrant Code (= Pure Code) : 재 진입 가능 코드

  - (조건 1) read-only로 하여 프로세스 간에 하나의 code만 메모리로 올린다

    - e.g. text editors, compilers, window systems
    - table에서 logical을 physical memory와 mapping할 때 physical memory를 모두 동일한 주소를 공유 (table은 프로세스마다 각각 생성)
    
  - Shared code는 모든 프로세스의 (조건 2) **logical address space에서 동일한 위치에 있어야** 한다
  
- Private code and data

  - 각 프로세스들은 독자적으로 메모리에 올린다
  - Private data는 logical address space의 아무 곳에 와도 무방



## 6. **Segmentation**

- 프로그램은 의미 단위인 여러 개의 segment로 구성
  - 작게는 프로그램을 구성하는 함수 하나하나를 segment로 정의
  - 크게는 프로그램 전체를 하나의 segment로 정의 가능
  - 일반적으로는 **code, data, stack** 부분이 하나씩 segment로 정의된다
- Segment의 logical unit
  - main()
  - function
  - global variables
  - stack
  - symbol table, arrays

Segmentation Architecture

- Logical address 구성

  - **segment-number (\**segment 번호: 각각의 segment 구분\**)**
  - **offset (\**segment내에서 segment 시작점부터 주소가 얼만큼 떨어져 있는지\**)**

- Segment table

  - segment별로 주소 변환이 필요하기 때문에 존재
  - 각각의 테이블 entry는 아래 두 가지 register 보유
    - **base** : segment의 physical address 시작 지점
    - **limit(length)** : segment의 길이 (프로그램이 사용하는 segment의 개수) → paging 기법과 달리 segment는 서로 길이가 동일하지 않기 때문에 기록 필요

- Segment-table base register (STBR)

  - 물리적 메모리에서의 segment table의 위치

- Segment-table length register (STLR)

  - 프로그램이 사용하는 segment의 수
  - segment number s is legal if s < STLR → 어떤 segment의 번호가 STLR (총 segment 수)보다 큰 경우는 잘못된 요청이기 때문에 trap이 걸린다 → segment 안에서 offset이 segment의 길이보다 긴 경우에도 trap 발생

- paging vs. segment

  - paging
    - 크기가 균일
    - 페이지의 크기가 offset의 크기 결정 (표현하는 bit 수 결정)
    - 프레임 크기 = 페이지 크기이기 때문에, 주소 변환 시 segment보다 간단
    - 페이지 개수가 상당히 많다 → 테이블을 위해 메모리 낭비 심하다
  - segment
    - segment 길이는 offset으로 표현할 수 있는 bit 수 이상으로 커질 수 없다
    - 프레임 크기 <> 페이지 크기이기 때문에, 주소 변환 시 paging보다 복잡 → segment가 어디서 시작되는지 정확한 byte 주소로 기록
    - 가변 기록 방식에서 hole이 생겼던 것처럼, segment도 각 프로세스마다 크기가 달라 중간중간 사용되지 않는 메모리 공간 발생 (가변 분할 방식에서와 동일한 문제점 발생) → Allocation 문제
    - 실제로 운영해보면 segment는 페이지에 비해 개수가 훨씬 적다.

- 장점

  - 의미 단위(공유, 보안)로 하는 일에 효과적
  - Protection (의미 단위의 protection)
    - 각 세그먼트 별로 protection bit가 있다
    - 각각의 entry마다 의미 단위로 valid, invalid, read, write, read-only 부여 가능 (paging은 코드와 데이터가 같이 들어갈 수도 있어 하기가 어렵다)
  - Sharing
    - 어떤 프로세스의 주소 공간을 프로세스들끼리 공유하고 싶을 때도 의미 단위이지, 균일한 크기의 페이지 X (Hole이 발생)
    - shared segment, same segment number
    - 공유를 위해서는 공유하려는 부분이 각 프로그램 내에서 같은 logical address여야 한다.

- 단점: Allocation

  - **first fit/ best fit** 사용
- external fragmentation 발생 ← segment의 길이가 동일하지 않기 때문



### 1. **Segmentation with Paging (Paged Segmentation)**

- segmentation을 여러 개의 page로 구성하는 기법
  - memory에 올라갈 때 여러 page로 쪼개서 올라간다
  - segmentation에서 발생하던 allocation 문제 (hole 발생) 해결 → 동일한 크기이므로
- pure segmentation과의 차이점
  - segment-table entry가 segment의 base address를 가지고 있는 것이 아니라, segment를 구성하는 page table의 base address를 보유
  - logical address : segment 번호와 segment 내에서 offset
  - segment table : segment의 길이(page table entry 개수)와 page table 시작 번호
  - segment 당 page table 존재 (page table 당 physical memory 어디에 올라가는지) : page 번호, page 내에서 offset
  - 주소 변환이 두 번 발생 (segment table, page table)
  - 보안 등 의미 단위로 일을 할 때는 segment 층위에서