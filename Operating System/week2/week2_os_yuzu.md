| Week 2 | Lecture 9~14 |
| :----: | :----------: |

<br/>

<br/>

# Process Management

---

<br/>

### 프로세스 생성

Copy-on-write

- 부모 프로세스가 자식 프로세스를 생성
  - 부모 프로세스의 주소공간을 자식 프로세스가 복사
  - 자식은 그 공간에 새로운 프로그램 올림
    - fork() 시스템 콜이 새로운 프로세스를 생성: 부모 프로세스와 fork로 생성된 자식 프로세스는 return Value가 다름 (부모프로세스 양수, 자식프로세스 0)
    - 다음 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올림 (exec 시스템콜이 꼭 fork 뒤에만 쓰이는 것은 아님)
- 프로세스의 트리 형성
- 프로세스는 자원을 필요로 함
- 자원의 공유(부모자식이 모든 자원을 공유하는 모델/ 일부만 공유하는 모델/ 전혀 공유하지 않는 모델)
- 수행(부모자식이 공존하며 수행되는 모델/ 자식이 종료될 때까지 부모가 기다리는 모델)

<br/>

### 프로세스와 관련한 시스템 콜

#### ✔ fork()

create a child(copy)

- 새로운 프로세스를 생성

#### ✔ exec()

overlay new image

- 새로운 프로그램을 메모리에 올림

#### ✔ wait()

sleep until child is done

- child가 종료될 때까지 부모 프로세스를 sleep시킴 (block)
- 자식 프로세스가 종료되면 부모 프로세스를 깨움(ready)

#### ✔ exit()

frees all the resources, notify parent

- 프로세스의 종료
  - 자발적인 종료
    - 마지막 문장 수행 후 exit 시스템 콜을 통해 종료
  - 비자발적인 종료 (외부)
    - 부모 프로세스가 자식 프로세스를 강제 종료 (한계치를 넘어서는 자원요청, 자식에게 할당된 태스크가 더이상 불필요)
    - 키보드로 kill, break 등을 친 경우
    - 부모가 종료하는 경우

<br/>

### 프로세스 간 협력

#### ✔ 독립적 프로세스

- 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함

#### ✔ 협력 프로세스

- 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음
  - 프로세스 간 협력 메커니즘 (Interprocess Communication)
    - message passing: 메세지를 전달하는 방법, 커널을 통해 메세지 전달 (다른 프로세스와 연결되어있는 공유 변수 같은 것이 없으므로 운영체제 커널을 통해 메시지를 보내 통신)
      - Direct Communication: 통신하려는 프로세스의 이름을 명시적으로 표시
      - Indirect Communication: mailbox를 통해 메시지를 간접 전달
    - shared memory: 주소 공간을 공유하는 방법, 일부 주소 공간을 공유
      - thread: thread는 사실상 하나의 프로세스이므로 프로세스간 협력으로 보기 어려우나 동일한 process를 구성하는 thread들간에는 주소 공간을 공유하므로 협력이라 볼 수있음

<br/>

<br/>

# CPU Scheduling

---

<br/>

### CPU-burst Time의 분포

- I/O bound job은 CPU를 짧게 쓰는데 빈도가 잦음, CPU bound job이 CPU를 더 많이 사용
- 여러 종류의 job이 섞여 있기 때문에 CPU스케줄링이 필요
- 사람과 소통을하는 Interactive job에게 우선적으로 적절한 response를 제공해야 함

<br/>

### CPU Scheduler & Dispatcher

- CPU Scheduler: Ready 상태의 프로세스 중에서 CPU를 줄 프로세스를 고름
- Dispatcher: CPU Scheduler에 의해 선택된 프로세스에게 CPU 제어권을 넘김(=> 문맥교환)
- CPU 스케줄링이 필요한 경우
  - Running -> Blocked (ex: I/O 요청) //nonpreemptive (= 강제로 빼앗지 않고 자진 반납)
  - Running -> Ready (ex: 할당시간만료 timer interrupt) //preemptive (= 강제로 빼앗음)
  - Blocked -> Ready (ex: I/O 완료후 인터럽트) //preemptive (= 강제로 빼앗음)
  - Terminate //nonpreemptive (= 강제로 빼앗지 않고 자진 반납)

<br/>

### Scheduling Criteria

CPU 성능척도 방법: 시스템입장(CPU utilization, Throughput)/ 프로세스입장(Turnaround time, Waiting time, Response time)

- CPU utilization(이용료): Keep the CPU as busy as possible
- Throughput(처리량): # of processes that complete their execution per time unit
- Turnaround time(소요시간, 평균시간): amount of time to execute a particular process, CPU 처리시간의 총합
- Waiting time(대기 시간): amount of time a process has been waiting in the ready queue
- Response time(응답 시간): amount of time it takes from when a request was submitted until the first response is produced, not output

<br/>

### Scheduling Algorithms

#### ✔ FCFS(First-come First-Served)

- 프로세스 도착 순서대로 처리 (nonpreemptive 비선점형)
- 앞에 긴 프로세스가 존재하여 뒤의 짦은 프로세스가 처리되지 못하는 문제점 발생

#### ✔ SJF(Shortest-Job-First)

- 각 프로세스와 다음번 CPU burst time을 가지고 스케줄링
- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄
- Nonpreemptive 비선점형: CPU를 잡으면 이번 CPU burst가 완료될때까지 CPU를 빼앗기지 않음
- Preemptive 선점형: 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김(SRTF Shortest-Remaining-Time-First)
- 주어진 프로세스에 대해 minimum average waiting time을 보장
- CPU 사용시간을 미리 알 수 없다는 문제점이 있음 but 과거의 CPU burst time을 이용해 추정은 가능함

#### ✔ Priority Scheduling

- A priority number(integer) is associated with each process
- 가장 높은 우선순위를 가진 프로세스에게 CPU 할당
- SJF도 priority scheduling
- low priority processes may never execute 문제점이 있음
- 아무리 우선순위가 낮은 프로세스라 하더라도 시간이 오래되면 우선순위를 높여주는 식으로 해결(Aging 기법)

#### ✔ Round Robin

- 각 프로세스는 동일한 크기의 할당 시간을 가짐
- 할당 시간이 지나면 프로세스는 선점 당하고 ready queue와 제일 뒤에 가서 다시 줄을 섬
- n개의 프로세스가 ready queue에 있고 할당시간이 q time unit인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻음
- 응답 시간이 빨라짐
-  CPU가 길게 필요한 프로세스는 길게 기다리고 짧게 필요한 프로세스는 짧게 기다림
- q large -> FCFS
- q small -> 문맥교환이 많이 일어나 오버헤드가 커짐
- 일반적으로 SJF보다 average turnaround time이 길지만 response time은 더 짧음

#### ✔ Multilevel Queue

- Queue가 여러줄이며 우선순위가 높은 큐의 프로세스가 CPU 우선권을 가짐
- Ready Queue를 여러 개로 분할
  - foreground queue
  - background queue
- 각 큐는 독립적인 스케줄링 알고리즘을 가짐
  - foreground queue- RR
  - background queue- FCFS
- 큐에 대한 스케줄링이 필요
  - Fixed priority scheduling: starvation
  - Time slice: 각 큐에 CPU time을 적절한 비율로 할당 - 80% to foreground in RR, 20% to background in FCFS

#### ✔ Multilevel Feedback Queue

- 프로세스가 다른 큐로 이동 가능

- 프로세스 처리가 끝나면 바로 나감, 처리가 끝나지 못하면 두번째 큐로 이동, 또 처리가 되지 못했다면 맨 밑의 큐로 이동

  처리가 짧은 프로세스에게 우선권을 먼저 준다

- Aging을 이와 같은 방식으로 구현 가능

#### ✔ Multiple-Processor Scheduling

- CPU가 여러개인 경우

- Homogeneous processor

  - Queue에 한 줄로 세워서 각 프로세서가 알아서 꺼내가도록 할수있음

  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우 문제

- Load sharing

  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
  - 별개의 큐를 두는 방법 VS 공동 큐를 사용하는 방법

- Symmetric Multiprocessing

  - 각 프로세서가 각자 알아서 스케줄링 결정

- Asymmetric multiprocessing

  - 하나의 프로세서가 전체적인 컨트롤을 책임짐

#### ✔ Real-Time Scheduling

- Hard real-time systems: deadline 보장
- Soft real-time computing: 일반 프로세스에 비해 높은 우선순위를 갖도록 함

#### ✔ Thread Scheduling

- Local Scheduling(유저레벨): 로컬 내부에서 어떤 thread를 스케줄링할지 결정
- Global Scheduling(커널레벨): 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 thread를 스케줄할지 결정

<br/>

### Algorithm Evaluation

- Queueing models: 확률 분포로 주어지는 도착률(arrival rate)와 처리율(service rate) 등을 통해 각종 performance index 값 계산
- Implementation(구현) & Measurement(성능 측정): 실제 시스템에 알고리즘을 구현하여 실제 작업에 대해서 성능을 측정 비교
- Simulation(모의 실험): 알고리즘을 모의 프로그램으로 작성한 후 trace를 입력으로 하여 결과 비교

<br/>

<br/>

# Process Synchronization

---

<br/>

### Race condition

여러 프로세스들이 동시에 공유데이터에 접근

race condition을 막기 위해 concurrent process는 synchronize 되어야 함

#### 💡 OS에서 race condition이 발생하는 경우

##### ✔ interrupt handler v.s. kernel

- 커널모드 running 중 interrupt가 발생하여 인터럽트 처리루틴이 수행
- 해결책: 처리가 완료되기 전까지 interrupt를 받아주지 않음

##### ✔ preempt a process running in kernel

- process가 system call을 하여 kernel mode로 수행 중인데 문맥교환이 일어나는 경우
- 해결책: 커널모드에서 수행중일 때는 CPU를 preempt하지 않고 커널모드에서 사용자모드로 돌아갈 때 preempt

##### ✔ multiprocessor

- Multiprocessor에서 공유 메모리 내의 커널 데이터에 접근하는 경우

- 해결책: 커널 내부에 있는 각 공유데이터에 접근할 때마다 lock/unlock, 한번에 하나의 CPU만이 커널에 들어갈 수 있게 함

<br/>

### The Critical-Section Problem

- n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 critical section이 존재
- 문제점: 하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 함
  - 해결방법
    - 먼저 충족되어야 할 조건
      - Mutual Exclusion(상호 배제): 프로세스 Pi가 critical section 부분을 수행중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안됨
      - Progress(진행): 아무도 critical section에 들어가있지 않은 상태에서 들어가고자 하는 프로세스가 있으면 들어가게 해주어야 함
      - Bounded Waiting(유한 대기): Starving 현상이 없어야 함
    - Algorithm1
      - Process0이 한 번 수행으로 끝이난 경우 Process1은 다시 들어갈 수 없음
      - 다른 프로세스가 turn을 내 값으로 바꿔줘야만 들어갈 수 있음(-> 과잉양보)
    - Algorithm2
      - flag: critical section에 들어가고자 하는 의향
      - critical section에 들어가고자 할때 나 자신이 flag를 true로 바꿈
      - 상대방의 flag를 체크 -> 상대방이 true면 기다리고 false면 내가 진입 -> 나오면서 나를 다시 false로 바꿔 상대방이 기다리지 않게끔 함
      - flag가 false로 변하는 행위가 critical section을 통과해야지만 일어날 수 있기 때문에 아무도 못들어가는 상황이 발생할 수 있음
    - Algorithm3 (Peterson's Algorithm)
      - 아무도 들어가 있지 않으면 turn에 관계없이 들어갈 수 있음
      - Mutual Exclusion, Progress, Bounded waiting을 모두 만족
      - Busy waiting(=spin lock) 문제 발생
        - 해결: 하드웨어적으로 Test와 Modify를 atomic하게(하나의 instruction으로) 수행할 수 있도록 지원

<br/>

### Semaphores

임계영역 문제를 해결하기 위한 알고리즘들을 추상화

- Semaphore S 세마포어 자료형
  - P연산: 자원을 가져감
  - V연산: 반납
- busy-wait(=spin lock)
- block-wakeup(=sleep lock)
  - block: 커널은 block을 호출한 프로세스를 suspend 시킴. 이 프로세스의 PCB를 semaphore에 대한 wait queue에 넣음
  - wakeup: block된 프로세스 P를 wakeup 시킴. 이 프로세스의 PCB를 ready queue로 옮김
- Critical section 길이가 긴 경우 Block/wakeup 방식 사용
- Critical section 길이가 매우 짧은 경우 Block/wakeup 오버헤드가 busy-wait 오버헤드보다 더 커질 수도 있음
- 일반적으로 Block/wakeup 방식이 더 좋음
- Two Types of Semaphores
  - Counting semaphore: 도메인이 0 이상인 임의의 정수값, resource counting에 사용
  - Binary semaphore: 0 또는 1만 값으로 가짐, mutual exclusion에 사용
- Deadlock: 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상
- Starvation: 프로세스가 suspend된 이유에 해당하는 세마포어 큐에서 빠져나갈 수 없는 현상

<br/>

### Classical Problems of Synchronization

##### ✔ Bounded-Buffer Problem(Produce-Consumer Problem 생산자-소비자 문제)

- 발생할 수 있는 문제
  - 다수의 생산자가 동시 접근할 때: Buffer에 lock을 걸고 데이터를 입력한 후 Lock을 품
  - 다수의 소비자가 동시 접근할 때: Buffer에 lock을 걸고 데이터를 꺼낸 후 Lock을 품
  - 버퍼가 꽉 차있는데 생산자가 접근할 때: 비어있는 buffer가 생길 때까지 기다려야함
  - 버퍼가 비어있는데 소비자가 접근할 때: 생산자가 buffer에 데이터를 넣을 때 까지 기다림

##### ✔ Readers-Writers Problem

- 한 process가 DB에 write중일 때 다른 process가 접근하면 안됨, read는 가능
- 해결방법
  - writer가 DB에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기중인 Reader들을 다 DB에 접근하게 해줌
  - writer는 대기 중인 reader가 하나도 없을 때 DB접근이 허용 됨
  - writer가 DB에 접근 중이면 reader들은 접근이 금지 됨
  - writer가 DB에서 빠져나가야만 reader의 접근이 허용 됨


<br/>

### Semaphore의 문제점

- 코딩하기 어려움
- 정확성 입증이 어려움
- 자발적 협력이 필요
- 한번의 실수가 모든 시스템에 치명적 영향

<br/>

### Monitor

- 동시 수행중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한 high-level synchronization construct

- 모니터 내에서는 한번에 하나의 프로세스만 활동 가능
- 프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요 없음
- 프로세스가 모니터 안에서 기다릴 수 있도록 condition variable 사용

<br/>

<br/>