# ✨ week2_os_questions ✨

#### ✏ Semaphore의 P 연산과 V 연산에 대해서 간단히 설명하세요.

- P 연산
  - 자원을 획득하는 연산
  - 자원이 없는 경우 프로세스를 suspended를 시킨다.
- V 연산
  - 자원을 반납하는 연산
  - 자원에 여분이 있는 경우 block 상태의 프로세스를 wakeup 시킨다.

<br>

#### ✏ busy-wait와 block & wakeup 둘 중에서 더 효율적인 것은?

- Block & Wakeup
- block & wakeup 이 보통 더 효율적이지만, critical section의 길이가 너무 짧으면 Block&wakeup 오버헤드가 busy-wait 오버헤드보다 더 커지기 때문에, busy-wait가 더 효율적인 방법이 될 수도 있다.

<br>

#### ✏ CPU scheduling에서 preemptive와 non-preemptive의 차이에 대해 설명하세요.

- preemptive : 우선순위가 더 높은 프로세스가 오면, CPU를 중간에 다른 프로세스에게 뺏긴다.
- non-preemptive :  우선순위가 더 높은 프로세스가 오더라도, CPU를 할당된 시간이 종료되기 전까지 뺏기지 않는다.

<br>

#### ✏ Thread scheduling에서 운영체제가 thread의 존재를 아는 scheduling은?

- Global scheduling

<br>

#### ✏ 현대 CPU scheduling에서 보편적으로 쓰는 CPU scheduling 방식과 그 방식의 장점은?

- RR (Round Robin)
-  response time이 짧다. waiting time도 프로세스가 n개, 할당 시간이 q인 경우 (n-1)q를 넘지 않는다.

<br>

#### ✏ scheduling을 평가하는 기준 5 가지

- CPU Utilization
- throuout
- turnaround time
- response time
- waiting time

<br>

#### ✏ CPU scheduling에서 Race Condition이란?

 	: 여러 프로세스들이 동시에 공유데이터에 접근하는 경우

- 커널모드 running 중 interrupt가 발생하는 경우
- process가 system call을 하여 kernel mode로 수행 중인데 문맥 교환이 일어나는 경우
- Multiprocessor에서 공유 메모리 내의 커널 데이터에 접근하는 경우)

<br>

#### ✏ Process가 자발적인 종료를 하는 경우는?

- 마지막 statement 수행 후 exit() 시스템 콜을 통해 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어준다.
- 개발자가 명시적으로 exit()을 써 넣는 경우

<br>

#### ✏ Semaphore와 monitor의 차이점

- Monitor는 lock/unlock 대신 condition variable을 사용한다.

<br>

#### ✏ CPU scheduling이 필요한 이유

- 프로세스간 I/O burst와 CPU burst time이 다르기 때문에, CPU를 효율적으로 가동하기 위해서 필요하다.

<br>

#### ✏ fork()로 프로세스 복제할 때 발생 가능한 문제점과 해결 방안

- 단순히 fork()만 실행한다면, 복제된 프로세스와 원본 프로세스 간에 구별이 이뤄지지 않는다. 그렇기 때문에 fork라는 시스템콜을 통해 운영체제가 복제본을 만들어 줄 때 부모 프로세스는 Process ID를 양수로, 자식 프로세스는 Process ID 0으로 부여하여 둘을 구분한다.
- Process 복제를 하는 부분을 복제된 자식 프로세스에서 계속 반복할 우려가 있기 때문에, PCB의 pointer도 같이 넘겨서 해결한다.

<br>

#### ✏ Critical-Section Problem 해결법의 충족 조건에 대해서 설명해주세요.

- mutual exclusion

  : 프로세스 Pi가 critical section 부분을 수행 중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안 된다

- progress

  : 아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있으면 critical section에 들어가게 해주어야 한다.

- bounded waiting

  : 프로세스가 critical section에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 시간에 한계가 있어야 한다.

<br>

#### ✏ priority가 있는 scheduling에서 starvation을 막기 위해 구현 가능한 방법은?

- Aging
- time slice : 우선 순위가 낮은 큐에 시간을 강제로 일정량 할당

<br>

#### ✏ Peterson's Algorithm에서 busy-waiting 현상을 해결하는 방법

- 하드웨어 적으로 test_and_set(test and modify)을 사용해서 read와 write를 동시에 atomic하게 수행하도록 하면 된다.

<br>

#### ✏ Deadlock이란?

- 둘 이상의 프로세스가 다른 프로세스가 점유하고 있는 자원을 서로 기다릴 때 무한 대기에 빠지는 상황

<br>

#### ✏ CPU bound job과 I/O bound job의 차이점

- CPU bound job은 CPU를 길게 사용
- I/O bound job의 경우에는 CPU를 짧은 시간동안 높은 빈도로 사용

<br>

#### ✏ thread 와 process의 차이점에 대해서 설명하세요.

- https://youtu.be/1grtWKqTn50 
- thread: process에서 실행 unit이 여러 개인 경우
- process: 일종의 단일 thread로 볼 수 있다

<br>

#### ✏ spin lock과 sleep lock의 차이점

- spin lock : busy-wait
- sleep lock : block & wakeup

<br>

#### ✏ Dispatcher에 대해서 설명하세요.

- OS 내에서 CPU 제어권을 넘기기 위해, 현재 프로세스의 PCB를 저장하고, CPU를 넘겨받을 프로세스의 PCB를 불러오는 코드

<br>

#### ✏ IPC(Interprocess Communication)에 대해서 설명하세요.

- message passing

  : 커널을 통해 메시지 전달, 프로세스 사이에 공유 변수를 일체 사용하지 않고 통신하는 시스템

  - direct communication: 통신하려는 프로세스의 이름을 명시적으로 표시, 마찬가지로 커널을 통해 메시지 전달
  - indirect communication: mailbox(또는 port)를 통해 메시지를 간접 전달, 경우에 따라서 특정 프로세스가 아니라 아무 프로세스에 전달될 수 있음

- shared data

  : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘

<br>

#### ✏ 프로세스를 비자발적으로 종료하는 경우에 대해서 설명해주세요

- 자식 프로세스가 한계치를 넘어서는 자원을 요청
- 자식에게 할당된 태스크가 더이상 필요하지 않아서 부모 프로세스가 자식 프로세스를 강제 종료시키는 경우
- 키보드로 kill, break를 친 경우 부모가 종료되는 경우 (부모 프로세스가 종료하기 전에 자식들이 먼저 종료)

<br>

#### ✏ FCFS(First-Come-First-Served)의 특징에 대해 설명해주세요.

- 먼저 들어온 프로세스를 먼저 수행하는 알고리즘
- 비선점형 스케줄링 자주 사용되지는 않음 앞에 온 프로세스에 따라서 waiting time이 크게 달라진다.
- Convoy effect : waiting time이 길어진 상황

<br>

#### ✏ SJF(Shortest-Job-First)의 특징에 대해 설명해주세요.

- 앞으로 CPU burst time이 짧게 남은 프로세스에게 우선적으로 CPU를 할당하는 일종의 priority scheduling
- Preemptive하게 구현된 경우
  - 현재 수행중인 프로세스의 낮은 burst time보다 더 높은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗긴다.
  - 이 방법을 Shortest-Remaining-Time-First(SRTF)라고도 부른다.
- (장점) 가장 적은 average waiting time을 보장한다. 특히 SJF 중에서도 preemptive일 때 가장 적다.
- (단점) CPU burst time을 미리 예측할 수 없기 때문에, 과거의 CPU burst time을 통해 들어온 프로세스의 CPU burst time 예측
- (단점) starvation 발생 가능

<br>

#### ✏ Priority Scheduling의 특징에 대해 설명해주세요.

- 가장 높은 priority를 가진 프로세스에게 CPU를 우선적으로 할당한다.
- 일반적으로 우선 순위는 높을 수록 작은 정수값을 갖는다. 
- preemptive한 경우, 현재 프로세스보다 우선 순위가 높은 프로세스가 들어오는 경우 CPU를 뺏긴다. 그래서 우선 순위가 낮을 수록 starvation할 확률이 높다.

<br>

#### ✏ Symmetric Multiprocessing(SMP)과 Asymmetric multiprocessing에 대해 설명해주세요.

- Symmetric Multiprocessing : 모든 processor가 동등하기 때문에, 각 processor가 알아서 processor scheduling 알고리즘을 고른다.
- Asymmetric Multiprocessing : 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따른다.

<br>

#### ✏ Process Synchronization 문제에 대해서 설명해주세요.

- 공유 데이터(shared data)에 동시 접근(concurrent access)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다. 일관성(consistency) 유지를 위해서는 협력 프로세스(cooperating process)간의 실행 순서(orderly execution)를 정해주는 메커니즘 필요

<br>