| Week 1 | Lecture 1~8 |
| :----: | :---------: |

<br/>

<br/>

# Introduction to Operating Systems

---

<br/>

### 운영체제

- 컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층
- 커널(좁은의미) + 주변 유틸리티(넓은의미)

<br/>

### 운영체제의 목적

- 컴퓨터 시스템을 편리하게 사용할 수 있는 환경을 제공

  - 동시 사용자/프로그램들이 각각 독자적 컴퓨터에서 수행되는 것 같은 환상을 제공

  - 하드웨어를 직접 다루는 복잡한 부분을 운영체제가 대행

- 컴퓨터 시스템의 자원을 효율적으로 관리 ⭐

<br/>

### 운영체제의 분류

#### ✔ 동시 작업 가능 여부에 따라

- 단일 작업: 한번에 하나의 작업만 처리
  - MS-DOS: 한 명령의 수행을 끝내기 전에 다른 명령을 수행시킬 수 없음
- 다중 작업: 동시에 두 개 이상의 작업 처리
  - UNIX, MS Windows: 한 명령의 수행이 끝나기 전에 다른 명령이나 프로그램을 수행할 수 있음

#### ✔ 사용자의 수에 따라

- 단일 사용자 ex) MS-DOS, MS Windows
- 다중 사용자 ex) UNIX, NT server

#### ✔ 처리 방식에 따라

- 일괄처리: 작업 요청을 모아 한꺼번에 처리, 작업이 완전 종료될 때까지 기다려야 함
- 시분할: 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용, 일괄처리에 비해 짧은 응답 시간
- 실시간: 데드라인이 존재, 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야 하는 실시간 시스템을 위한 OS

<br/>

### 운영체제의 예

#### ✔ 유닉스

- 대부분 C언어로 작성
- 높은 이식성, 최소한의 커널, 확장 용이, 소스 코드 공개, 프로그램 개발에 용이, 다양한 버전

#### ✔ DOS

- IBM-PC를 위해 개발
- 단일 사용자용 운영체제, 메모리 관리 능력의 한계

#### ✔ MS Windows

- MS사의 다중 작업용 GUI 기반 운영 체제
- Plug and Play, 네트워크 환경 강화
- DOS용 응용 프로그램과 호환
- 불안정성(-> 지금은 많이 해소됨)
- 풍부한 소프트웨어

<br/>

<br/>

# System Structure & Program Execution

---

<br/>

### 컴퓨터 시스템 구조

Computer(CPU + Memory) + I/O device(Disk + Keyboard + Monitor + etc...)

#### 💡 Computer

- Mode bit
  - 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호장치
  - Mode bit 을 통해 하드웨어 적으로 두가지 모드의 operation 지원
    - 1 사용자 모드: 사용자 프로그램 수행
    - 0 모니터 모드(= 커널 모드, 시스템 모드): OS 코드 수행
    - 보안을 해칠 수 있는 중요한 명령어는 0 모니터 모드 에서만 수행 가능한 특권 명령으로 규정
    - Interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 바꿈
    - 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 셋팅

- Timer
  - CPU의 Time Sharing 구현
  - 특정 프로그램이 독점하는 것으로부터 보호
- DMA controller
  - I/O 장치의 잦은 interrupt로 CPU가 방해를 받는 것을 방지

#### 💡 I/O device

- Device Controller
  - I/O device controller: 해당 I/O 장치유형을 관리하는 작은 CPU
  - I/O가 끝났을 경우 interrupt로 CPU에 알림
- 입출력의 수행
  - 시스템콜: 사용자 프로그램이 운영체제에게 I/O 요청
  - trap을 사용하여 interrupt 벡터의 특정 위치로 이동
  - 제어권이 interrupt 벡터(처리 루틴 주소)가 가르키는 interrupt 서비스 루틴으로 이동
  - 올바른 I/O 요청인지 확인 후 I/O 수행
  - I/O 완료시 제어권을 시스템콜 다음 명령으로 옮김

<br/>

### 동기식 입출력 VS 비동기식 입출력

프로세스가 입출력이 끝나고 나서 instruction을 실행하면 동기식 입출력, 입출력이 끝나기 전에 instruction을 실행하면 비동기식 입출력

<br/>

<br/>

# Process

---

<br/>

### 프로세스의 개념

Process is a program in execution

#### 💡 프로세스의 문맥

- CPU 수행 상태를 나타내는 하드웨어 문맥: Program Counter, register
- 프로세스의 주소 공간: code, data, stack
- 프로세스 관련 커널 자료 구조: Process Control Block, Kernel stack
  - Process Control Block
    - 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
    - 구성 요소
      - OS가 관리상 사용하는 정보: Process state, Process ID, scheduling information, priority
      - CPU 수행 관련 하드웨어 값: Program counter, registers
      - 메모리 관련: Code, data, stack의 위치 정보
      - 파일 관련: Open file descriptors
- 문맥 교환
  - CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
    - CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
    - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴
  - 시스템콜이나 Interrupt 발생 시 반드시 문맥교환이 일어나는 것은 아님

<br/>

### 프로세스의 상태

✔ New: 프로세스가 생성중인 상태

✔ Running: CPU를 잡고 instruction을 수행중인 상태

✔ Ready: CPU를 기다리는 상태(메모리 등 다른 조건을 모두 만족)

✔ Blocked(wait): CPU를 줘도 당장 instruction을 수행할 수 없는 상태, 요청한 event가 즉시 만족되지 않아 이를 기다리는 상태

✔ Terminated: 수행이 끝난 상태

✔ Suspended(stopped): 외부적인 이유로 프로세스의 수행이 정지된 상태, 프로세스는 통째로 디스크에 swap out 됨

- Blocked VS Suspended
  - Blocked: 자신이 요청한 event가 만족되면 Ready
  - Suspended: 외부에서 resume해 주어야 Active

<br/>

### 프로세스를 스케줄링하기 위한 큐

프로세스들은 각 큐들은 오가며 수행

✔ Job queue: 현재 시스템 내에 있는 모든 프로세스의 집합

✔ Ready queue: 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합

✔ Device queues: I/O device의 처리를 기다리는 프로세스의 집합

<br/>

### 스케줄러

✔ Long-term scheduler(장기 스케줄러 or job scheduler)

- 시작 프로세스 중 어떤 것을 ready queue로 보낼지 결정
- 프로세스에 memory(및 각종 자원)을 주는 문제
- 메모리에 올라가 있는 프로그램의 수(degree of Multiprogramming)를 제어
- time sharing system에서는 장기 스케줄러가 없음 보통 무조건 ready

✔ Short-term scheduler(단기 스케줄러 or CPU scheduler)

- 어떤 프로세스를 다음번에 running 시킬지 결정
- 프로세스에 CPU를 주는 문제
- 빠름

✔ Medium-Term scheduler(중기 스케줄러 or Swapper)

- 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄
- 프로세스에게서 memory를 뺏는 문제
- degree of Multiprogramming을 제어

<br/>

### Thread

프로세스 하나에 CPU 수행 단위만 여러개 두고 있는 것

- Thread의 구성
  - program counter
  - register set
  - stack space
- Thread가 동료 thread와 공유하는 부분
  - code section
  - data section
  - OS resources

- 장점
  - 하나의 프로세스안에 스레드를 여러개 두게 되면 스레드 하나가 blocked 상태 일 때 다른 스레드를 실행할 수 있어 빠른 처리가 가능
  - 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율, 향상된 성능
  - 병렬성 높일 수 있음
  - 응답성(Responsiveness), 자원 공유(Resource Sharing), 경제성(Economy), 멀티 프로세서 아키텍처의 경우 병렬처리(Utilization of MP Architectures)

- Kernel Threads VS User Threads

<br/>

<br/>

# Process Management

---

<br/>

### 프로세스 생성

- 부모 프로세스가 자식 프로세스를 생성
  - 부모 프로세스의 주소공간을 자식 프로세스가 복사
  - 자식은 그 공간에 새로운 프로그램 올림
    - ex) 유닉스
      - fork() 시스템 콜이 새로운 프로세스를 생성
      - 다음 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올림
- 프로세스의 트리 형성
- 프로세스는 자원을 필요로 함
- 자원의 공유(부모자식이 모든 자원을 공유하는 모델/ 일부만 공유하는 모델/ 전혀 공유하지 않는 모델)
- 수행(부모자식이 공존하며 수행되는 모델/ 자식이 종료될 때까지 부모가 기다리는 모델)

<br/>

### 프로세스 종료

- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려줌 (exit)

  - 자식이 부모에게 output data 보냄
  - 프로세스의 각종 자원들은 운영체제에 반납됨

- 부모 프로세스가 자식의 수행을 종료시킴(abort)

  - 자식이 할당 자원의 한계치를 넘어섰을 때

  - 자식에게 할당된 태스크가 더이상 필요하지 않을 때

  - 부모가 종료할 때

    

<br/>

<br/>