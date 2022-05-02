# (1) 운영체제 개론

# 

Q. 실시간과 시분할의 차이점

 **OS (Operating System; 운영체제)**

: 컴퓨터 **Hardware 바로 위에 설치되어**, 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 **소프트웨어 계층**

- 협의(= **Kernel**)
    - 운영체제의 핵심적인 부분
    - 전원이 켜져있는 동안(부팅이 되어 있는 동안) 메모리에 상주
- 광의
    - 커널 뿐 아니라 각종 주변 시스템 유틸리티를 포함한 개념
    - 메모리에 상주하지 않는 독립적인 프로그램이지만, 운영체제의 범주에 속한다.
    - (ex) file 복사하는 소프트웨어

운영 체제의 목표

- **컴퓨터 시스템을 편리하게 사용할 수 있는 환경 제공**
    - OS는 동시 사용자/프로그램들이 각각 독자적 컴퓨터에서 수행되는 것 같은 인상을 제공 
     (or 여러 프로그램이 하나의 컴퓨터에서 동시에 실행되는 것처럼)
      → 컴퓨터가 한 대 이더라도 마치 혼자 컴퓨터를 사용하는 것 같은 느낌
    - 하드웨어를 직접 다루는 복잡한 부분을 운영체제가 대행
- **컴퓨터 시스템의 자원을 효율적으로 관리 ⭐** (자원 관리자)
    - 프로세서, 기억 장치, 입출력 장치 등의 효율적 관리
        - 주어진 자원으로 최대한의 성능을 내도록
        - + 형평성 관리(사용자 간 형평성 있는 자원 분배)
        → 특정 프로그램이나 특정 사용자가 너무 자원을 적게 받아도 안 된다.
    - 사용자 및 운영 체제 자신을 보호
    - resource (자원) : 컴퓨터 안의 cpu, memory, 입출력 장치, 보조 기억 장치 (하드웨어 자원) + 프로세스, 파일, 메시지 (소프트웨어)  등을 일컫는 말
    - (ex) CPU → 컴퓨터 안에서도 가장 빠른 자원
    ⇒ (운영체제의 역할) 사람들이 각자 컴퓨터를 혼자 사용하는 듯한 환경을 제공하기 위해, 실행 중인 프로그램들에게 짧은 시간 간격으로 번갈아 CPU 할당
    - (ex) 메모리 ⇒ (운영체제의 역할) 여러 프로그램이 동시에 실행되어야 하기 때문에, 프로그램마다 메모리 공간 적절히 할당
      

[운영 체제의 분류]

1. 동시 작업 가능 여부
2. 사용자의 수
3. 처리 방식

 운영 체제의 분류 

**1. 동시 작업 가능 여부**

- **단일 작업 (single tasking)**
    - 한 번에 하나의 작업만 처리
    - (ex) MS - DOS (마이크로 소프트 사의 DOS), 예전의 휴대폰(전화 받는 기능밖에 없을 때), 엘레베이터 (단일 작업에 집중)
    - 프롬프트 상에서 한 명령의 수행을 끝내기 전에 다른 명령을 수행시킬 수 없다
- **다중 작업 (multitasking)**
    - 동시에 두 개 이상의 작업 처리
    - (ex) 현대 대부분의 운영 체제 → UNIX, MS Windows(마이크로 소프트 윈도우), 스마트폰
    - 한 명령의 수행이 끝나기 전에 다른 명령이나 프로그램 수행 가능

1. **사용자의 수**
   
    : 한 컴퓨터를 여러 사용자가 동시에 접속해서 실행이 가능한지 여부
    
- 단일 사용자 (single user)
    - (ex) MS - DOS, MS Windows (예전; 여러 계정 만들어 원격에서 서버 기능을 추가 시켜 동시 접근을 허용하기도)
- 다중 사용자 (multi user)
    - (ex) UNIX, 리눅스, NT server
    - 각 사용자의 파일이나 작업을 보지 못하게 하는 보안이나 자원 형평성 문제가 단일 사용자에 비해 중요

1. **처리 방식**
- **일괄 처리 (batch processing)**
    - 작업 요청의 일정량 (batch)를 모아서 한꺼번에 처리
        - interactive 하지 않다
        - 오류가 나면 하루가 늦어지는 경우가 많으므로, 오류 없이 코드 작성하는 것이 중요했다
        - *(ex) DB에 접근하는 것이 오래 걸린다면? → DB에 갈 일을 줄여서 한 번에 처리하는 것이 좋다*
    - 작업이 완전 종료될 때까지 기다려야 한다
    - 현대에서는 찾아보기 어렵다 (역사 속)
    - (ex) 초기 Punch Card 처리 시스템 (OMI 카드...)
    
- **시분할 (Time Sharing)**
    - 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 (엄청 작은) 시간 단위로 분할하여 사용
        - 기본적으로 CPU 기준으로 시간을 나눠서 분배
        - 사람이 느끼기에 빠른 속도 보장(interactive) + 주어진 자원 최대한 활용 (정확한 시간을 지켜주는 것이 목표는 아니다 → 시간 분배는 사용자와 프로그램에 따라 달라진다)
        - (ex) 프로그램 하나의 점유율이 높아지면 다른 것들이 끊기거나 느려지면 시분할을 쓰는 OS 케이스
    - 일괄 처리 시스템에 비해 짧은 응답 시간을 가지는 특성
    - **interactive**한 방식 : 컴퓨터의 결과가 바로바로 나온다 (일정 시간 대기 후 나온다면 batch processing)
    - (ex) UNIX + 현대 OS의 대부분 (아이폰, 안드로이드 OS)
      
    
- **실시간 (Realtime OS)**
    - **정해진 시간 안에 어떠한 일이 반드시 종료(처리)됨이 보장**(Dead Line 존재)되어야 하는 실시간 시스템을 위한 OS
    - 시분할이 일반적인 범용 컴퓨터에 사용된다면, 실시간은 특정한 목적을 가진 컴퓨터에 사용된다
    - (ex) 원자로/공장 제어, 미사일 제어, 반도체 장비, 로봇 제어
      
        → (오류가 나지 않기 위해) 시간을 철저하게 지켜야 하는 것 (시분할처럼 CPU가 그때그때마다 분배하는 시간이 바뀌면 안 되는 것)
         → 반도체: 공정마다 정해진 시간이 있고, 이 공정들이 연달아 이어져 있는 경우 (하나 밀리면 제품 출시 자체가 밀리는 경우)
        
    - 실시간 시스템의 개념 확장
        - **Hard realtime system** (경성 실시간 시스템) 
          : (원래 실시간 시스템) deadline을 철저히 지키는 시스템
          정확히 지키지 않으면 치명적 결과 야기 가능
        - **Soft realtime system** (연성 실시간 시스템)
          : (최근 생긴 시스템) deadline이 있지만 조금 어겨도 큰 문제가 없는 시스템
           (ex) 영화 1s당 24 프레임씩 읽어와야 하는 경우 
                → 시간에 오차가 나면 영화가 끊긴다
          
             (ex) 블랙박스 사고 포착, 네비게이션 정확한 지시 필요 → 점점 실시간성을 요구하는 범용 운영체제가 중요해 지는 중
          
    

비슷한 말 다른 관점

: 아래 용어들은 컴퓨터에서 여러 작업을 동시에 수행하는 것을 뜻하지만, 강조하는 관점이 다르다.

**Multitasking** : CPU에서는 매 시간 하나의 작업만 수행하지만, 짧은 시간 단위로 돌아가면서 하기 때문에 동시에 수행하는 것처럼 보이는 것

**Multiprogramming**: 메모리에 여러 프로그램이 동시에 올라가는 것 → 당연히 **multitasking**을 하려면 multiprogramming이 되어야 한다 (But! 메모리 측면 강조)

**Time Sharing** : 시분할 → CPU를 강조한 측면

**Multiprocess** : 여러 프로그램이 동시에 실행된다 (프로그램 측면)

(cf) **Multiprocessor** (다중 처리기):  하나의 컴퓨터에 CPU (processor)가 여러 개 붙어 있는 것 의미 

→ 여러 프로그램이 여러 CPU에 들어가 있을0 수 있다

→ multiprocessor와 위의 용어들이 다른 점은, 위의 용어는 하드웨어 적으로 cpu가 하나라도 가능

 운영 체제 예시

1. **Unix (유닉스)**

- 대형 컴퓨터를 위해 만들어진 OS
    - multitasking, 동시 사용자 지원
- 코드의 대부분을 C 언어로 작성
    - 초기에는 기계어에 가까운 assembly 언어로 만듦
     → 복잡하고 어려운 코드
    - So, Unix 운영체제를 만들기 위해 C 언어 개발
    - 대부분의 커널 코드가 C 언어로 되어있다.
    - 초창기에는 OS 소스 코드 전부 공개 → 프로그램 개발 용이, 공부 용이
- 높은 이식성 : portability 
 → 다른 컴퓨터에 이식하기가 쉽다 (기계어와는 다른 독립적인 C 언어이기 때문에, compile을 다시 하기만 하면 된다)
- 최소한의 커널 구조: 메모리에 올라가는 OS 메모리량 줄였다
- 확장 용이
- 다양한 버전
    - System V, FreeBSD, SunOS, Solaris → open source x
    - **Linux**
        - open source (여러 사용자도 가능하고, 개인용으로도 가능)
        - 안드로이드도 linux kernel 사용 중

**2. DOS (Disk Operating System)**

- PC (개인용 컴퓨터) 대상 OS
- 단일 사용자용 운영 체제로 출발
- MS에서 1981년 IBM-PC를 위해 개발
    - 초창기에는 PC가 용량이 작고 단일 프로그램만 지원
    - So, MS가 단일 사용자용 운영 체제로 출발
    - 메모리 관리 능력의 한계
        - 640KB까지만 지원하도록 개발
        - 하드웨어의 발전으로 640KB를 훨씬 뛰어 넘는 메모리들 만들어졌다
        - 이걸 보완하느라 코드가 점점 복잡해짐
        

**3. MS Windows**

- 처음에 DOS 위에서 실행 → 후에 독자적인 운영체제로 변모
    - DOS용 응용 프로그램과 호환성 제공
- 다중 작업 용
- GUI 기반 운영체제
- 초창기 불안정성 (프로그램이 갑자기 꺼지는 등)
- OS 소스 코드 공개 X
- 풍부한 지원 소프트웨어

**4. Handheld device(소형 디바이스) 를 위한 OS**

- PalmOs, Pocket PC (WinCE), Tiny OS

 운영체제 기능

- CPU 스케쥴링 : CPU 할당
    - 먼저 온 순서대로 처리 X
    - cpu가 아무리 빠르다 한들 하나의 프로그램이 cpu 점유하면서 놓지 않으면, 뒤에 들어온 프로그램은 아무리 기다려도 처리가 되지 않기 때문
    - 오히려 걸리는 시간이 짧은 프로그램이 먼저 들어가는 것이 낫다
- 메모리 관리: 한정된 메모리를 어느 프로그램에 얼만큼 할당?
    - 너무 많이 할당해도, 적게 할당해도 안 된다 → cpu에게 원활하게 실행되기 위해 필요한 정도 (workingset model)
    - 메모리가 부족할 때 어떤 프로그램을 메모리에서 내릴 것인가
- 파일 관리: 디스크에서 어떻게 파일을 보관할지 → 디스크 헤더의 이동을 줄이자 (엘리베이터처럼 현재 위치와 가장 가까운 순서대로 처리)
- 입출력 관리 : 각기 다른 입출력 장치와 컴퓨터 간에 어떻게 정보를 주고 받을 것인가
    - 입출력 장치가 워낙 느리기 때문에 발생하는 이슈
    - 인터럽트 기반 관리 → 입출력 장치가 cpu에게 인터럽트를 걸어서 멈춘다
- 프로세스 관리 (sw)
    - 프로세스의 생성과 삭제
    - 자원 할당 및 반환
    - 프로세스 간 협력
- 보호 시스템, 네트워킹, 명령어 해석기 (command line interpreter)

---

**Computer** : CPU, Memory

- CPU : 기계어 실행, 매 클럭 사이클 마다 메모리에서 기계어(instruction)를 읽고 interrupt가 들어왔는지 체크
    - **register** : memory보다 더 빠르고 memory보다는 작은 저장공간
        - Program Counter Register : 다음에 실행할 명령어 주소(메모리 주소)를 기억하고 있는 CPU 레지스터. CPU는 Program Counter가 가리키는 명령을 CPU는 실행한다. 메모리에 있는 명령어를 가져와 저장한다. 순차적으로 명령 실행하는데 도움
    - **mode bit** : CPU에서 현재 실행되는 것이 운영체제인지 사용자 프로그램인지 구분
    - **Interrupt line** : memory에 있는 instruction만 실행하는 CPU. 디스크에서 읽어오거나, 키보드에서 입력이 들어오는 등 I/O와 상호 작용이 일어나야 할 때, 잠시 CPU가 memory하고만 일하던 걸 멈추고 I/O device에 접근 → 그렇다고 직접 I/O device에 접근하지는 않는다 → device controller에게 시킨다
        - I/O에서 interrupt가 들어오면 사용자 프로그램에 할당되어 있던 CPU가 OS로 넘어와 왜 interrupt가 들어왔는지 체크
        - CPU는 명령어를 시행한 후 인터럽트가 들어왔는지 체크하는 것을 반복
    - A 프로그램에서 더 이상 할 일이 없으면 바로 B로 넘어간다 (ex. 사용자 입력 없이는 더 진행이 안 될 때)
    - 무한 루프 → 계속 CPU 점유하는 프로그램 : time sharing 구현 불가
- **timer** : 특정 프로그램이 CPU를 독점하는 것을 막기 위해 존재
    - 처음 부팅했을 때 OS 실행. 이후 사용자 프로그램으로 넘어가는데, 각각 프로그램마다 timer에 시간이 같이 할당되며 넘어간다. 프로그램 A에서 프로그램 B로 넘어갈 때도 timer setting된다.
    - setting된 시간이 되면 timer가 cpu에 interrupt를 걸어 시간이 끝났다고 알려주게 된다. CPU는 사용자 프로그램에서 OS로 넘어간다.
- Memory : CPU의 작업 공간

**I/O device** : Input/Output device

- Input : 키보드, 마우스
- Output : 프린트, 모니터
- 하드 디스크 (보조 기억 장치) → input(데이터 제공), output(처리 결과 저장) 모두 수행 가능

**device controller** 

- 각각의 I/O device에 붙어 전담하는 작은 컨트롤러 (CPU의 역할을 수행하지만 CPU가 하지 않는다)
- CPU와 I/O device의 속도가 차이 나기 때문에, 분리할 수 밖에 없다

**local buffer** : CPU가 memory가 필요하듯이 device controlloer도 작업 공간이 필요

 Mode Bit

- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치
- CPU에서 명령을 실행할 때 mode bit이 0인지 1인지 보고 실행
- Mode bit을 통해 하드웨어적으로 두 가지 모드의 operation 지원
    - interrupt나 exception 발생 시 하드웨어가 mode bit을 0으로 바꾼다
    - 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 바꾼다.
- CPU가 OS를 실행하고 있는지, 사용자 프로그램을 실행중인지 표시
    - **커널 모드** (= 모니터 모드, 시스템 모드)
        - **mode bit = 0**
        - OS가 실행중
        - memory 접근, I/O device 접근 등 모든 instruction 허용 → **특권 명령 (Privileged instruction)**
        - 보안을 해칠 수 있는 중요한 명령어는 커널 모드에서만 수행 가능
    - **사용자 모드**
        - **mode bit = 1**
        - 사용자 프로그램이 실행 중
        - 제한된 instruction만 CPU가 실행 가능 → 보안 상의 목적 (일반 명령)
        - memory, I/O device 접근 불가

 Timer

- 특정 프로그램이 CPU를 독점하는 것을 막기 위해, 프로그램마다 정해진 시간을 할당
    - 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킨다
    - 타이머는 매 클럭 틱 때마다 1씩 감소
    - 타이머 값이 0이 되면 타이머 인터럽트 발생
- time sharing 구현하는데 이용
- 현재 시간을 계산하는데도 이용

 Device Controller

- I/O device controller
    - I/O 장치를 관리하는 일종의 작은 CPU
    - 제어 정보를 위해 control register, status register 보유
        - CPU가 일을 시킬 때 register를 통해 제어 지시
    - local buffer (일종의 data register)
        - (입력 후/ 출력 전) 데이터를 일시적으로 담는 공간
        - local buffer에 쌓인 값을 cpu는 memory에 복사해 와 일을 하게 된다
- I/O는 실제 device와 local buffer 사이에서 일어난다
- device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림
    - CPU가 장비마다 interrupt 걸리니 너무 많이 걸린다 → CPU가 비효율적이 될 수 있다
    - So, DMA controller : 직접 메모리를 접근할 수 있는 controller
        - 메모리를 CPU와 DMA가 접근 가능
        - memory controller가 CPU와 DMA가 동시에 접근하지 않도록 접근 관리
        - I/O device들이 CPU에 interrupt에 직접 걸게 하지 않고, local buffer에 작업이 끝나면 DMA가 local buffer로부터 모아서 CPU에 한 번에 전달

**device driver (장치 구동기)**
 : OS 코드 중 각 장치별 처리 루틴 (각 디바이스에 접근해 처리할 수 있게 하는 인터페이스) → software

 - Disk 안의 firmware 명령어로 작동

**device controller (장치 제어기)**
 : 각 장치를 통제하는 일종의 작은 CPU → hardware

 입출력(I/O)의 수행

- 모든 입출력 명령은 특권 명령 → OS를 통해서만 가능
- 사용자 프로그램은 어떻게 I/O를 하는가
    - **시스템 콜(system call)** : 사용자 프로그램은 디스크에서 읽어와야 할 때(ex. 파일 읽어오기) 운영체제에게 I/O 요청 (kernel 호출 → 함수 호출)
        - 사용자 프로그램이 운영체제의 서비스(ex. I/O)를 받기 위해 커널 함수를 호출하는 것
        - 사용자 프로그램이 직접 OS로 접근 못하기 때문 (mode bit)
        - 프로그램이 소프트웨어적으로 인터럽트를 CPU에 건다(software interrupt: 프로그램이 직접 **의도적으로 interrupt line setting**)
    - trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
    - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    - 올바른 I/O 요청인지 확인 후 I/O 수행
    - I/O 완료 시 제어권을 시스템 콜 다음 명령으로 옮김

 Interrupt

- 인터럽트(Interrupt)
    - 인터럽트 당한 시점의 레지스터와 **program counter**를 **save**한 후, CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
- Interrupt
    - **Interrupt** (하드웨어 인터럽트: 좁은 의미)
        - 하드웨어가 발생시킨 인터럽트
        - **timer, I/O controller**
    - **Trap** (= 소프트웨어 인터럽트)
        - 넓은 의미의 interrupt: interrupt + trap
        - 프로그램이 os에게 trap을 걸어 요청
        - **Exception** : 프로그램이 오류를 범한 경우
            - (ex) zero division, 메모리에 접근하려는 시도 등
            - 사용자 프로그램에서 OS로 넘어가게 되고, 프로그램을 강제 종료 시키는 등의 행위를 수행
        - **System call**: 프로그램이 커널 함수를 호출하는 경우
- 순서
    - 프로그램A에서 I/O를 요청하기 위해 OS에 system call (trap: software interrupt) → cpu가 해당하는 I/O device controller에 명령 전달 후, 다른 프로그램B으로 넘어가 작업 시작 → I/O쪽에서 일을 다 끝내면 하드웨어 인터럽트 → CPU에게 작업이 끝났음을 알린다
    - I/O 요청: 소프트웨어 인터럽트
    I/O 종료: 하드웨어 인터럽트
    한 프로그램의 CPU 독점 방지: timer의 하드웨어 인터럽트
- 인터럽트 관련 용어
    - **인터럽트 벡터**
        - 해당 인터럽트의 처리 루틴 주소를 가지고 있음 (번호-주소의 쌍)
        - 각각 들어온 인터럽트 요청마다 다른 처리가 필요 (다양한 인터럽트의 종류와 처리 방식) → 각각의 인터럽트 종류마다 해야하는 일 = 루틴 → 운영체제 코드에 루틴이 정의되어 있다.
        - *django의 urls.py처럼 생각하면 될듯. 적절한 루틴으로 보내기*
    - **인터럽트 처리 루틴**
        - Interrupt Service Routine, 인터럽트 핸들러
        - 해당 인터럽트를 처리하는 커널 함수
        - *django의 views.py처럼 생각하면 될듯. 들어온 요청마다 적절한 처리 방식*

---

동기식 입출력 & 비동기식 입출력

- 동기식 입출력 (synchronous I/O)

  - 서로 시간적으로 맞추는 것 → synch...

  - I/O 요청 후 

    입출력 작업이 완료된 후에야, 제어가 사용자 프로그램에 넘어간다

    - I/O 장치까지 직접 가서 결과를 보고 오는 것 → I/O 작업을 하는 동안 대기
    - I/O 장치를 동시에 여러 곳에서 접근 가능한데, 모두가 접근이 완료된 후에야 제어가 사용자 프로그램에 넘어가는 경우

  - 구현 방법 1 : I/O가 끝날 때까지 CPU를 낭비

    - I/O를 요청한 프로그램이 계속 CPU 점유
    - 매 시점 하나의 I/O만 일어날 수 있다 → I/O도 낭비

  - 구현 방법 2 → CPU 낭비 x, I/O가 여러 작업 동시 실행

    - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 뺏는다.
    - I/O 처리를 기다리는 줄에 그 프로그램 줄 세운다.
    - 다른 프로그램에게 CPU를 넘겨 준다

- 비동기식 입출력 (asynchronous I/O)

  - I/O가 시작된 후 **입출력 작업이 끝나기를 기다리지 않고, 제어가 사용자 프로그램에 즉시 넘어간다.**
  - CPU는 **해당 프로그램이 하는 일 중 I/O와 무관한 작업을 찾아 계속 수행**

⇒ 즉, 동기식은 프로그램 A의 I/O가 수행되는 동안, CPU가 아무 일을 하지 않고 계속 점유하거나, 아예 다른 프로그램 B, C ...로 넘기지만, 비동기식은 I/O가 수행되는 동안 다시 프로그램 A를 작업

- read는 기본적으로 synchronous가 논리적으로 적합하고(정보가 들어올 때까지 무슨 작업을 할 수 없다), write는 asynchronous가 적합

⇒ 두 경우 모두 I/O의 완료를 인터럽트로 알린다.

DMA (Direct Memory Access)

- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용

  - (CPU 외) 메모리를 접근할 수 있는 장치

- CPU의 중재 없이 device controller가 device의 buffer sotrage의 내용을 **메모리에 block 단위로 직접 전송**

- 바이트 단위가 아니라 

  block 단위

  로 인터럽트 발생시킨다.

  - CPU가 작은 일 하나하나에 인터럽트 걸리지 않도록
  - I/O를 block 단위로 모았을 때 인터럽트 발생

서로 다른 입출력 명령어

- (좌측) I/O를 수행하는 special instruction에 의해

  - 메모리에 접근하는 instruction이 있는 memory address와 I/O에 접근하는 instruction이 있는 device address가 별개로 정의
  - 일반적

- (우측) 

  Memory Mapped I/O

  에 의해

  - I/O device에도 메모리 주소를 매겨서, 메모리 접근하는 instruction 통해서 device에 접근하는 경우

저장장치 계층 구조

- primary(executable) → CPU가 직접 접근 (byte 단위 접근 가능 + 실행)
  - register (CPU)
  - cache memory (CPU, SRAM)
  - main memory (DRAM) → byte 단위로 주소 기억
- secondary : (ex) hard disk → byte가 아니라 sector 단위 접근 → CPU가 I/O에 부탁해서 읽어들인다 → 하드 디스크
  - magnetic disk
  - optical disk
  - magnetic tape

→ 위로 갈 수록 빠른 속도, 비싼 가격, 적은 용량

→ pirmary는 휘발성(volatile), secondary 비휘발성

- caching

  : 빠른 매체로 정보를 읽어 들여 쓰는 것

  - 재사용 목적

프로그램의 실행 (memory load)

- 프로그램이 컴퓨터에서 어떻게 실행되는가?

- 평소에 프로그램은 실행 파일로 file system (보조 기억 장치)에 저장됨

- 실행시키면 memory(주 기억 장치: 커널 영역은 항상 존재)로 올라가 실행

  - 주 기억 장치로 바로 올라가는 것이 아니라 virtual memory (가상 메모리)를 거친다

  - 프로그램 실행하면 

    프로그램만의 address space(0번부터 시작하는 독자적인 주소 공간)

     형성

    - 실제로 있는 것은 아니다. (실제로는 physical memory와 swap area에 분배 되어 존재)
    - code(cpu에서 실행할 코드) / data (전역 변수, 자료 구조)/ stack(함수를 쌓고 return)으로 구성 → virtual memory
    - 이 address space를 memory에 올리게 된다.

  - physical memory에 모두 올려놓는 것이 아니라, 당장 실행하는 것만 올린다. 당장 쓰지 않는 내용은 disk의 swap area로 내려놓는다.

    - swap area: (메모리의 한계로) 메인 메모리의 연장 공간으로 사용, 당장 쓰이지 않는 것들을 임시 보관해두는 곳
    - swap area(전원 나가면 유지 x)와 file system(전원 나가도 유지) 모두 하드 디스크이지만, 다른 용도

  - virtual memory는 0번부터 시작하지만 physical memory는 그렇지 않기 때문에, 둘을 변환해주는 작업도 필요

커널 주소 공간의 내용

- kernel도 하나의 프로그램이기 때문에 code/data/stack 구조 보유
- code: kernel이 하는 일
  - 자원 효율적 관리
  - 편리한 서비스 제공
  - 시스템 콜, 인터럽트 처리 코드 (인터럽트가 올 때 os 다시 가동)
- data
  - 하드웨어 관리 + 하드웨어 관리하기 위한 자료구조
  - 프로세스 관리 (어떤 프로그램이 CPU 얼마나 쓰는지, 메모리는 얼마나 할당해야 할지) → PCB (Process Control Block: 프로세스 별로 관리하기 위한 자료구조)
- stack
  - 여러 프로그램이 CPU 코드를 요청해 실행 가능 (system call)
  - 어떤 사용자 프로그램이 어떤 코드를 실행 중인지
  - 프로그램마다 kernel stack을 별도로 보유

사용자 프로그램이 사용하는 함수

- 사용자 정의 함수 : 자신의 프로그램에서 정의한 함수 → address space
- 라이브러리 함수 : 자신의 프로그램에서 정의하지 않고 가져다 쓴 함수; 프로그램의 실행 파이렝 포함되어 있다. → address space
- 커널 함수 : 운영체제 프로그램의 함수 → kernel address space
  - 커널 함수의 호출 = 시스템 콜

프로그램의 실행

- user mode
  - program이 직접 cpu를 잡고 있을 때
  - 사용자 정의 함수 사용 or library 함수 사용
- kernel mode
  - system call로 user mode → kernel mode
  - kernel 함수 실행 & return

---

**Process**

- Process is **a program in execution : 실행 중인 프로그램**

- 프로세스의 문맥(

  context

  ) : 프로세스가 현재 어느 상태에 와 있는지 규명

  - CPU 수행 상태를 나타내는 하드웨어 문맥 : 현재 시점에 instruction을 어디까지 실행했는가
    - **Program Counter**
       → 프로그램 카운터가 어디를 가리키고 있는지 → 코드의 어느 부분까지 실행했는가
    - 각종 **register** → 메모리에 어떤 내용을 담고 있는가 → (ex) stack에 쌓인 함수와 변수 → (ex) register에 어떤 명령어가 쌓여 있는가
  - 프로세스의 주소 공간
    - 프로그램은 실행되면 독자적인 주소 공간을 가진다
    - **code, data, stack**
  - 프로세스 관련 커널 자료 구조 → 운영 체제는 프로세스 관리
    - **PCB (Process Control Block)** : 프로세스가 하나 시작될 때 마다 OS는 각 프로그램마다 PCB를 하나씩 부여하며, 메모리나 CPU를 얼마나 할당하고 있는지, 어떻게 평가하고 있는지
    - **Kernel stack** : 프로그램 실행 도중 system call이 발생하면, OS로 넘어오게 되고, 커널 코드를 실행하게 되고, 프로그램 마다 (프로세스 마다) 함수가 쌓이는 kernel stack을 별도로 둔다.

- 대부분의 컴퓨터가 시분할이기 때문에, 문맥을 파악해 백업하고 있어야, 다시 프로그램 A로 돌아왔을 때 적절한 곳에서 다시 실행 가능

프로세스의 상태 (Process State)

- 프로세스는 상태(state)가 변경되며 수행된다

  - 운영체제에서 프로세스 상태 관리하기 위한 명칭
  - 운영체제 자신에게는 적용되지 않는 개념

- **Running** : CPU를 잡고 instruction을 수행 중인 상태

- Ready

   : CPU를 기다리는 상태

  - **메모리** - 물리적인 메모리에 올라가기 - 등 다른 조건을 모두 만족한 상태)
  - ready 상태인 것과 running 상태인 프로세스가 시분할로 왔다갔다
  - (ex) Timer interrupt

- Blocked (wait, sleep)

  - CPU를 주어도 당장 instruction을 수행할 수 없는 상태
  - Process 자신이 요청한 event(ex. I/O)가 즉시 만족되지 않아 이를 기다리는 상태 → 오래 걸리는 작업 중
  - (ex) 메모리에 올라와 있지 않고 디스크에 내려가 있을 때
  - (ex) 디스크에서 file을 읽어와야 하는 경우

- Suspended (stopped)

  - 외부적인 이유로 프로세스의 수행이 정지된 상태
    - (ex) midterm scheduler
  - 메모리를 통째로 뺏은 상태 → 하던 일 멈추고 중지
  - 프로세스는 통째로 디스크로 swap out이 된다.
  - (ex) 사용자 프로그램을 일시 정지 시킨 경우 (break key) → 종료는 아니지만 정지 → 사람이 프로세스를 재개 시켜줘야만 다시 메모리로 올라갈 수 있다. (ex) 시스템이 여러 이유로 프로세스를 잠시 중단시킨다 (메모리에 너무 많은 프로세스가 올라와 있을 때)
  - **blocked: 자신이 요청한 event(\**ex. I/O 요청\**)가 만족되면 ready** **suspended: 외부에서 resume해줘야 active (inactive ↔ active)**
  - CPU관점에서는 suspended되면 아무 일도 못하는 것이 맞지만, 원래 I/O 작업중이던 프로세스가 suspended되면, 그 와중에서도 (CPU가 아니기 때문에) I/O를 계속 수행해서 완료 시킬 수 있다. **suspended blocked** → **suspended ready**

- New : 프로세스가 생성 중인 상태

- Terminated: 수행(execution)이 끝난 상태 → 프로세스 자체는 끝났지만, 끝나고 정리하는 작업이 약간 남아 있는 상태

- 각 큐는 실제로는 OS kernel이 kernel address space의 data 영역에 각 영역의 queue를 생성해서 관리
- 공유 데이터
  - 소프트웨어 자원 중 하나
  - 여러 프로세스가 동시에 접근하면, (하나의 프로세스가 접근하던 도중에, CPU를 timer로 뺏기고 다른 프로세스도 이에 접근한다면), 일관성이 깨질 가능성 존재
  - 데이터를 쓰고 있는 프로세스가 데이터를 사용하면 안 되기 때문에, 자원에 대해서도 줄 서야 한다
- **프로그램이 운영체제로 system call을 해도, 운영체제가 running하는 것이 아니라, 프로그램이 커널 모드에서 running하고 있다** (user mode → monitor mode, kernel mode)
  - 외부의 다른 요인으로 인해 interrupt가 들어와도, 여전히 직전까지 작업중이던 프로그램이 커널 모드에서 running하고 있다고 간주

**Process Control Block (PCB)**

- 운영체제가 각 프로세스를 관리하기 위해 각 프로세스 당 유지하는 정보

- 구성 요소 (구조체)

  - OS

    가 관리 상 사용하는 정보

    - **Process state**(ready, blocked, running), **Process ID** (각 프로세스 당 번호 부여)
    - **scheduling information, priority** (프로세스에 CPU를 주기 위한 우선 순위 정보) → CPU는 사실 우선순위 큐로 관리

  - CPU

     수행 관련 하드웨어 값 (문맥)

    - **Program counter, registers**

  - 메모리 관련

    - **code, data, stack**이 메모리 어디에 위치하고 있는지

  - 파일 관련

    - **open file descriptors** ... (프로세스가 오픈하고 있는 파일에 대한 정보)

      

**Context Switch (문맥 교환)** ⭐

- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정

- 뺏기던 시점의 문맥을 기억해 두고 교환 필요

- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행

  - CPU를 내어주는 프로세스 상태를 그 프로세스의 PCB에 저장
  - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어온다. (하드웨어에 복원)

- System call

  이나 

  interrupt

   발생 시 반드시 

  context switch

  가 일어나는 것은 아니다.

  - 사용자 프로세스 A로부터 사용자 프로세스 B로 넘어갈 때만 context switch 확실
  - 사용자 프로그램에서 OS에 넘어가는 것 자체는 context switch가 아니다
  - OS 전후로 프로그램이 동일하면 context switch가 아니고(kernel mode로 잠깐 들어오기만 한 것), OS가 실행되기 이전과 이후의 프로그램이 다르면 context switch
  - (1)의 경우에도 CPU 수행 정보 등 context의 일부를 PCB에 save 해야하긴 하지만, 문맥 교환을 하는 (2)의 경우 그 부담이 훨씬 크다
    - overhead가 훨씬 적다
    - (ex) cache memory flush → 사용자 프로그램 간 문맥 교환이 발생하는 경우, 프로세스 A가 사용하던 캐시 메모리를 지워야 한다. (overhead가 큰 작업)

프로세스를 스케쥴링하기 위한 Queue

- 정확하게는 프로세스를 줄 세우는 것이 아니라 OS가 **PCB의 pointer**를 줄 세운다.
- **Ready Queue** : 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 집합
- **Device Queue** : I/O device의 처리를 기다리는 프로세스의 집합
- **Job Queue** : 현재 시스템 내에 있는 모든 프로세스의 집합
  - ready queue, device queue에 포함되어 있는 프로세스가 모두 job queue에도 존재
  - ready queue와 device queue는 서로 겹치지 않는다.

스케쥴러 (scheduler)

- Short-term scheduler (단기 스케쥴러 / CPU scheduler)

  - 아주 짧은 시간 단위로 스케쥴링
  - 어떤 프로세스를 다음 번에 **running** 시킬지 결정 (CPU 줄지 결정)
  - 프로세스에 **CPU**를 주는 문제
  - 충분히 빨라야 한다 (millisecond 단위)

- Long-term scheduler (장기 스케쥴러 / job scheduler)

  - 시작 프로세스 (new) 중 어떤 것들을 

    ready queue

    로 (메모리를 줄지) 보낼지 결정

    - memory에 올라가는 것 : admitted → 허락해줘야 CPU를 얻을 수 있는 상태가 되기 때문 → longterm scheduler가 결정

  - **memory(및 각종 자원)**를 어떤 프로세스(프로그램)에 줄지 결정하는 문제

  - degree of multiprogramming

    을 제어

    - mutliprogramming : 메모리에 여러 프로그램을 동시에 올리는 것
    - 즉, 메모리에 올라가는 프로세스 수 제어
    - 너무 적게 줘도 I/O에 계속 갔다와야하고, 너무 많아도 프로그램 전체 성능 저하

  - time sharing system에는 보통 장기 스케쥴러가 없다 (무조건 ready)

    - 일단 모두 프로세스가 생성되면 다 ready 상태로 들어간다
    - medium-term scheduler

- Medium-Term Scheduler (중기 스케쥴러 / Swapper)

  - time sharing system에서는 프로그램이 시작되면 무조건 ready 상태가 되기 때문에(메모리를 부여하기 때문에), 메모리에 동시에 너무 많은 프로그램이 올라갈 가능성 → 중기 스케쥴러로 관리
  - **여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄**
  - 프로세스에게서 **memory**를 뺏는 문제
  - **degree of multiprogramming**을 제어
  - 현대 스케쥴러에는 long-term이 아니라 medium-term scheduler이기 때문에 추가된 process state 존재 → **suspended**

---

**Thread**

- A **thread** (or **lightweight process**) is a basic unic of CPU utilization
- process 내부에 **CPU 수행 단위가 여러 개 있는 경우** thread라 부른다
- process가 주어지면, code data, stack으로 구성된 주소 공간이 프로세스마다 생성 + 프로세스를 관리하기 위해 운영체제 내부에 PCB
- 같은 일을 하는 프로세스를 여러 개 띄우고 싶을 때
  - 주소 공간을 메모리에 하나만 띄워두고 (여러 개 띄우지 않고)
  - PCB에서 코드의 어느 부분을 실행하고 있는지 (**Program Counter**)와 **register**에 어느 값을 뒀는지 여러 개 두어 분리
  - **stack**도 thread 마다 별도로 부여
- 메모리 주소, 프로세스 상태, 각종 자원 공유 & CPU 수행 관련 정보(CPU related inforamtion)는 분리 (program counter, register, stack)
  - PCB는 하나만 만들어지만 CPU 관련 정보만 여러 개로 copy되어 각 스레드마다 별도로 보유

- Thread의 구성

  - **program counter**
  - **register set**
  - **stack space**

- Thread가 동료 thread와 공유하는 부분 (=

  task

  )

  - **code section**
  - **data section**
  - **OS resources**

- 전통적인 개념의 **heavyweight process**는 하나의 thread만 가지고 있는 task로 볼 수 있다.

Thread의 장점

- 다중 스레드로 구성된 태스크 구조에서는 **하나의 서버 스레드가 blocked(waiting) 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행(running)**되어 빠른 처리 가능

  - (ex) 클라이언트가 서버에 요청해서 문서를 받아오는 것 → I/O작업
    - 오래 걸리기 때문에 웹 브라우저 blocked 상태가 된다 → 사용자 입장에서는 답답
    - 웹 브라우저를 여러 개의 스레드를 사용해서 만들어 둔다면, 먼저 읽어온 것부터 display → 별도의 작업 가능 → 빠른 응답성

- 동일한 일을 수행하는 다중 스레드가 협력하여, 높은 처리율(

  throughput

  )과 성능 향상을 얻을 수 있다.

  - 같은 일을 처리하는 작업을 별도의 프로세스로 만들면 메모리 낭비 심함 ← 독자적인 주소 공간
  - 자원 절약
  - (ex) 웹 브라우저 여러 개 띄우기, 아래 한글에 파일 여러 개 띄우기

- 스레드를 사용하면 병렬성을 높일 수 있다

  - CPU 여러 개 갖고 있는 컴퓨터에만 해당되는 사항
  - CPU가 하나라면 순차적으로 실행했어야 하는데, CPU가 여러 개라면 스레드를 CPU에 각각 분리해 실행 가능 (병렬)

1. Responsiveness

    (응답성)

   1. (ex) multi-threaded Web : if one thread is blocked (e.g. network), another thread continues (e.g. display) → thread가 분리되면 network쪽만 분리된다 ⇒ asynchronous I/O

2. Resource Sharing (자원 공유) : n threads can share binary code, data, resource of the process

3. Economy (경제성)

   1. creating

       & 

      CPU switching thread

       (rather than a process)

      1. 프로세스 하나를 만드는 것은 overhead가 크지만, 스레드를 추가하는 것은 적다
      2. 스레드 간 문맥 교환은 프로세스 간 문맥 교환보다 overhead가 적다 (cpu switching) → 주소를 공유하고 있기 때문

   2. Solaris(Unix 계열 OS)의 경우 위 두 가지 overhead가 각각 30배(creating), 5배(CPU switching)

4. Utilization of MP Architectures

    (멀티 프로세서의 병렬성)

   1. MP: Multiprocessor : CPU 여러 개 인 architecture
   2. each thread may be running in **parallel on a different processor**

Implementation of Threads

- Kernel Threads

  : OS kernel의 지원 받는 thread

  - 스레드가 여러 개 있다는 사실을 커널이 인지
  - 하나의 스레드에서 다른 스레드로 CPU가 넘어가는 것을 커널이 CPU 스케쥴링 하듯이 넘겨준다.

- User Threads

  : library의 지원 받는 thread

  - 프로세스 안에 스레드가 여러 개 있다는 사실을 OS는 모른다
  - 라이브러리 통해 다중 스레드 관리
  - 구현 상의 제약이 있을 수 있다

- **real-time thread** : real-time 기능을 지원하는 thread도 존재