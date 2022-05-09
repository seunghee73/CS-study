# 1. CPU Scheduling

## A. 개념

### a. CPU and I/O Bursts in Program Execution

- **CPU burst** : CPU 작업만 연속적으로 실행하고 있는 단계
- **I/O burst** : I/O 작업만 실행하고 있는 단계
- 프로그램 실행
    - CPU burst와 I/O burst가 번갈아가며 실행
    - 프로그램 종류에 따라 burst의 빈도나 길이가 다르다
    - 여러 종류의 process가 섞여있기 때문에 CPU 스케줄링 필요

- **I/O bound job/process**
    - CPU를 짧게 쓰고 I/O가 자주 끼어드는 프로그램
    CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
    - CPU 빈도가 잦다
    - **interactive** job → CPU를 늦게 주면 사람이 답답해 한다
    - many short CPU bursts
- **CPU bound job/process**
    - CPU를 아주 오래 쓰고 I/O가 적게 끼어드는 프로그램
    - CPU 빈도가 잦지 않다 → 오래 많이 쓴다
    - 계산 위주의 job
    - few very long CPU bursts

### b. CPU scheduler & Dispatcher

- **CPU scheduler**
    - 어떤 프로세스에 CPU를 줄지 고르는 것
    - 운영 체제 안의 CPU scheduler 기능을 하는 코드가 존재
    - **Ready** 상태의 프로세스 중에서 CPU를 줄 프로세스를 고른다
- **Dispatcher**
    - 역시 OS kernel에 있는 코드
    - CPU scheduler에 의해 선택된 프로세스에게 CPU 제어권을 넘긴다
    - 이 과정이 바로 **context switch (문맥 교환)!**
    - 방금 전까지 돌아가던 프로그램의 context save & 새로 돌아갈 프로그램의 context를 cpu에 setting

### c. CPU 스케줄링이 필요한 경우

 → 프로세스에게 상태 변화가 있는 경우

1. **Running → Blocked**
    1. CPU를 어떤 프로세스가 잡고 있다가, 그 프로세스가 I/O 작업처럼 오래 걸리는 작업을 하러 간 경우
    2. e.g. I/O를 요청하는 시스템 콜
    3. 프로세스가 자진해서 CPU를 넘겨줌
2.  **Running → Ready**
    1. 할당 시간 만료로 timer interrupt
    2. CPU를 더 쓰기를 원하지만, 시간이 다 되어 강제로 빼앗고 다른 프로세스에게 넘긴다
3. **Blocked → Ready**
    1. I/O 작업 종료 후 ready로 바꿔주기 (CPU를 얻을 수 있는 권한 부여)
    2. ⇒ I/O 완료 후 interrupt
    3. 보통, I/O가 끝난 후에 ready가 아니라 바로 running으로 넘어가는 경우는 드물다 (다른 프로그램이 interrupt 당했기 때문에, 이 프로그램으로 돌아가는 경우가 일반적)
    4. but! 절대적 우선순위가 높은 프로세스의 I/O가 끝난 경우에는 바로 running으로 넘어올 수도 있다
4. **Terminate**
    1. 프로세스가 종료되서 더 이상 할 일이 없어, 새로운 프로세스에게 CPU를 넘겨야 하는 경우

### d. CPU 스케쥴링 종류

- **Nonpreemptive** scheduling
- **preemptive** scheduling → 대부분 선점형 스케쥴링 사용

### e. Scheduling Criteria (성능 척도)

- 여러 스케쥴링 알고리즘을 평가하는 성능 척도
- = Performance Index, Performance measure

- System 입장에서 성능 척도 : CPU하나로 최대한 일을 많이 시킬 수록 좋다

  - **CPU utilization (이용률)**
    - 전체 시간 중에서 CPU가 놀지 않고 일한 시간의 비율
    - keep the CPU as busy as possible → CPU를 최대한 일을 시키자
  - **Throughout (처리량, 산출량)**
    - 주어진 시간 동안 몇 개의 작업을 완료했는지

- Program 입장에서 성능 척도 : CPU를 빨리 얻어 빨리 끝낼 수록 좋다

  - **Turnaround time**(소요 시간, 반환 시간)

    - CPU를 쓰러 들어와서 다 쓰고 나갈 때까지 걸린 시간

    - 프로세스가 시작되서 종료될 때까지 걸린 시간 x 
  
      → I/O 갔다 온 시간 포함 안 하고 CPU한정
  
    - amount of time to execute a particular process
  
    - waiting time + 실행하고 나간 시간
  
  - **Wating time**(대기 시간)
  
     - ready 상태로 기다린 시간
    - amout of time a process has been waiting in the ready queue
    
  - **Response time**(응답 시간)
  
     - CPU를 쓰려고 들어와서 처음으로 CPU를 얻기까지 걸린 시간
  
    - preemptive scheduling에서는 CPU를 계속 뺏기고 기다리고 반복하기 때문에, 프로그램 완료까지 ready 상태로 기다리는 시간이 여러 차례 발생 → 이를 모두 합친 것이 Waiting time. Response time은 ready 상태에서 처음으로 cpu를 얻기까지 걸린 시간
    
      

## B. Scheduling Algorithm

### a. FCFS (First-Come First-Served)

- 먼저 온 순서대로 프로세스 처리 = FIFO
- nonpreemptive scheduling
- 엄청 효율적이지는 않다 → 오래 쓰는 프로그램이 CPU를 잡아버리면, interactive job을 하는 프로세스가 오래 대기 ⇒ **Convoy effect** : short process behind long process
- 어떤 프로세스가 먼저 도착하는 지에 따라 전체 걸리는 시간에 영향이 크다

### b. SJF (Shrotest-Job-First)

- CPU burst가 가장 짧은 프로그램에게 CPU를 먼저 할당

- 각 프로세스의 다음번 **CPU burst time**을 가지고 스케줄링에 활용

- Two schemes:

  - **Nonpreemptive**

    : 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지, (더 짧은 burst time을 가진 프로세스가 도착하더라도) CPU를 선점(preemption) 당하지 않는다

  - **Preemptive** : 현재 수행 중인 프로세스의 남은 burst time 보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗긴다 → 이 방법을 **Shortest-Remaining-Time-First (SRTF)으로도** 부른다 → 앞으로 남은 CPU 시간이 짧은 애한테 가장 먼저 주기 때문

- SJF is optimal

  - 주어진 프로세스들에 대해 **minimum average waiting time** 보장
  - 전체 대기 시간이 가장 짧아지는 알고리즘
  - average waiting time을 최소화 하는 것은 SJF 중에서도 preemptive

- 문제점

  - Starvation : SJF는 극단적으로 CPU 사용 시간이 짧은 프로세스 우선
    - CPU 사용 시간이 긴 프로세스는 영원히 처리되지 못할 수도
  - CPU burst time(사용 시간)을 미리 완전히 예측 불가
    - 추정 (estimate)만이 가능
    - user 인풋량, branch (if, for loop 등)
    - 그래서 추정해서 사용 → 과거의 CPU 사용 흔적으로 예측
    - **exponential averaging**
      - 실제 사용 시간과 사용 예측 시간을 일정 비율로 반영
      - 과거를 어떤 비율로 반영할 것인가 (최근에 가까울 수록 많이 반영)

### c. Priority Scheduling: 우선순위 스케쥴링

- 우선순위가 높은 프로세스에게 CPU 할당
- A **priority number (integer)** is associated with each process
- highest priority를 가진 프로세스에게 CPU 할당
  - (일반적으로) smallest integer = highest priority
  - **preemptive** : 우선 순위가 더 높은 프로세스가 왔을 때 CPU 뺏음
  - **nonpreemptive** : 우선 순위가 더 높은 프로세스가 오더라도 CPU를 뺏지 않는다
- SJF는 일종의 priority scheduling
  - **priority** = **predicted next CPU burst time**
- Problem
  - **starvation (기아 현상)** : low priority process may never execute
- Solution
  - **Aging**: as time progresses increase the priority of the process → priority가 낮더라도 기다리는 시간이 길어질 수록 priority를 높여주기

### d. ✨ Round Robin (RR) ✨

- 현대 CPU scheduling의 근간

- 프로세스에게 CPU를 줄 때 할당 시간을 세팅해서 넘기고, 할당 시간이 끝나면 뺏기는 preemptive scheduling + timer

- 각 프로세스는 **동일한 크기의 할당 시간 (time quantum)**을 가진다

  - 일반적으로 10-100 ms (milliseconds)

- 할당 시간이 지나면 프로세스는 선점(preempted) 당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다

- 장점

  - response time이 빠르다
  - 누가 CPU 오래 쓸지 예상할 필요 없이, 모두에게 금방 turn이 간다 (CPU 짧게 쓰는 process를 상대적으로 금방 처리 가능)
  - 일반적으로 SJF보다 average turnaround time은 길어지지만, response time은 더 짧다
    - 일반적으로 짧은 프로세스와 긴 프로세스가 섞여 있기 때문에, 이 때는 round robin이 낫다
    - 동일한 긴 처리 시간을 가진 프로세스들인 경우, round robin에서는 모두 거의 동시에 끝나게 되기 때문에, 하나라도 먼저 내보내는 FCFS가 나을 수도 있다

- n 개의 프로세스가 ready queue에 있고 할당 시간이 q time unit인 경우, 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다

  - **어떤 프로세스도 (n-1)q time out 이상 기다리지 않는다**
  - 대기 시간이 본인이 CPU를 사용하는 시간에 비례 → 여러 번 queue에 서야 하기 때문
  
- Performance

  - q large (할당 시간이 아주 큰 경우) ⇒ FIFO (FCFS)
  - q small (할당 시간이 아주 작은 경우) ⇒ context switch 오버헤드가 커져 전체 성능 저하 가능성

### e. Multilevel Queue

- FCFS나 RoundRobin과 달리 줄이 여러 개 존재
- 맨 위로 갈 수록 우선순위가 높고 아래로 갈 수록 우선순위가 낮다
    - system process > interactive process (사용자) > batch process ...
    - 높은 우선순위 줄에 없어야 하위 우선순위 줄에도 순서가 간다
    - 프로세스를 우선순위 별로 구분 필요
- Ready queue를 여러 개로 분할 (여기에서는 크게 두 가지로 구분)
    - foreground (**interactive**)
    - background (**batch** -no human interaction)
- 각 큐는 독립적인 스케줄링 알고리즘을 가진다
    - foreground : **round robin**
    - background : **FCFS**
        - CPU를 오랫동안 쓰고, batch이기 때문에 작업이 오래 걸린다
        - CPU를 주고 뺏는 것이 잦을 수록 overhead가 커지기 때문에 FCFS
- 큐에 대한 스케줄링이 필요
    - **Fixed priority scheduling**
        - serve all from foreground then from background (우선순위가 높은 줄이 비었을 때만 낮은 줄에 있는 프로세스 실행 가능)
        - Possibility of **starvation**
    - Time slice ← starvation에 대한 solution (background에 일정량 확보)
        - 각 큐에 CPU time을 적절한 비율로 할당
        - e.g. 80% to foreground in RR, 20% to background in FCFS

 ### f. Multilevel Feedback Queue

- Multilevel Queue와 달리 프로세스가 다른 큐로 이동 가능
- Aging을 이와 같은 방식으로 구현 가능
- Multilevel Feedback Queue를 정의하는 파라미터들
    - **Queue의 수**
    - **각 큐의 scheduling algorithm**
    - **procees를 상위 큐로 보내는 기준**
    - **process를 하위 큐로 내쫓는 기준**
    - **프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준**
        - 처음 프로세스가 큐에 들어갈 때 어느 큐에 들어갈지
        - 보통 가장 우선 순위가 높은 큐에 일단 들어가서 할당 시간을 짧게 받는다. 할당 시간 내 안 되면, 계속해서 그 다음 우선 순위로 내려가게 되는데 대신 할당 시간은 더 오래 받는다
        - **CPU 사용 시간이 짧은 프로세스에게 높은 우선순위**를 주는 방식
        - CPU 사용 시간을 미리 예측할 필요도 없다

 ### g. (특수 케이스 1) **Multiple-Processor Scheduling**

- **CPU가 여러 개**인 경우의 스케줄링
- **Homogeneous processor**인 경우
    - Queue에 한 줄로 세워서, 각 프로세서가 알아서 꺼내가게 할 수 있다 (한 줄 서기)
    - But! 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해진다
- **Load sharing**
    - 일부 프로세서에 job이 몰리지 않도록, 부하를 적절히 공유하는 메커니즘 필요
    - 별개의 큐를 두는 방법(각각의 CPU마다 별도의 줄) 
    vs. 공동 큐를 사용하는 방법
- **Symmetric Multiprocessing (SMP)**
    - 모든 CPU가 대등
    - So, 각 프로세서가 각자 알아서 스케줄링 결정
- **Asymmetric Multiprocessing**
    - 하나의 CPU가 나머지 CPU를 책임 (분배하는 역할(
    - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고, 나머지 프로세서는 거기에 따른다

### h. (특수 케이스 2) **Real-Time Scheduling**

- 반드시 Deadline 안에 처리가 끝나는 것을 보장
- 주기적으로 업데이트 되어야 하는 성격이 많다 (periodic)
- Hard real-time systems
    - 반드시 정해진 시간 내에 끝내도록 스케줄링
- Soft real-time computing
    - 일반 프로세스에 비해 높은 priority를 갖도록 하지만, deadline을 반드시 보장하지는 못한다

### f. (특수 케이스 3) **Thread Scheduling**

- Local Scheduling
    - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정
    - OS는 스레드가 나뉘어 있는지 모른다
- Global Scheduling
    - Kernel level thread의 경우 일반 프로세스와 마찬 가지로 커널의 단기 스케줄러가 어떤 thread를 스케줄할지 결정
    



## C. Algorithm Evaluation (알고리즘 평가 방법)

- **Queueing Models**
    - 이론적
    - **확률 분포**로 주어지는 arrival rate(도착율)와 service rate (처리율 - 단위 시간 당 몇 개 처리) 등을 통해 각종 preformance index (Throughput, response time 등) 값을 계산
- **Implementation (구현) & Measurement (성능 측정)**
    - 실제 시스템에 알고리즘을 구현하여, 실제 작업(workload)에 대해서 직접 성능을 측정하고 기존과 비교
    - 실측
- **Simulation (모의 실험)**
    - 알고리즘을 모의 프로그램으로 작성 후 **trace**(실제 프로그램에 들어갈 input data or 실제 프로그램에서 뽑은 데이터)를 입력으로 하여 결과 비교
    - 실제로 돌리는 것이 아니라 모의 실험



# 2. Process Synchronization (동기화)

​	**= Concurrency Control (병행 제어)**

## A. 동기화 문제

### a. 컴퓨터 시스템 내부 데이터 접근 패턴

- 데이터를 읽어와서 연산 수정하고 저장한다면, 누가 먼저 읽어서 다시 쓰냐에 따라 내용이 달라질 수 있다 (synchronization의 문제 발생)
- 여러 주체가 하나의 데이터를 동시에 접근하려 할 때 (Race Condition; 경쟁 상태) 원치 않는 값을 가질 수 있다

  - CPU가 여러 개 있는 **multiprocessor system**에서는 문제 발생 가능

- 공유 데이터(shared data)의 동시 접근(concurrent access)는 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다
- 일관성(consistency)의 유지를 위해서는 협력 프로세스 (cooperating process) 간의 실행 순서(orderly execution)를 정해주는 메커니즘 필요
- Race Condition
  - 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
  - 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라진다
- race condition을 막기 위해서는 concurrent process는 동기화 (synchronize)되어야 한다

### b. The **Critical-Section** Problem

- n 개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 **critical section**이 존재
- (문제) 하나의 프로세스가 critical section에 있을 때 다른 프로세스는 critical section에 들어갈 수 없어야 한다 (CPU를 뺏겨도 접근을 막아야 한다)

1. 소프트웨어적 해결 방법: critical section 주위로 entry section과 exit section을 두어 lock/unlock → Algorithm 1, 2, 3 (Perterson's Algorithm)
2. 하드웨어적 해결 방법: Test & Set
3. 추상 자료형 해결 방법: Semaphore, Monitor

### c. 프로그램적(소프트웨어적) 해결 법의 충족 조건

- **Mutual Exclusion (상호 배제)**

  - 배타적으로 접근
  - 어떤 프로세스가 critical section 부분을 수행 중이면 다른 모든 프로세스는 해당 부분 접근 불가

- **Progress**

  - 아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있으면, critical section에 들어가게 해준다
  - 당연하지만, 둘이 동시에 들어가는 것을 막고자, 아무도 못 들어가는 코드를 만들 수도 있기 때문

- **Bounded Waiting**

  - 대기하는 시간은 유한해야 한다. (무한 x)

  - starvation 방지

  - 프로세스가 critical section에 들어가려고 요청한 후부터 그 요청이 적용될 때까지, 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다.

  - (ex) 기존의 것들이 계속 번갈아 들어가고 새로운 것이 못 들어가는 경우

    

## B. 해결 방법 1. Software적 해결방법

### a. Algorithm 1 - turn

- **turn**이라는 동기화 변수 (synchronization) 사용 → critical section에 들어갈 차례
- int turn; initially **turn** = 0; ⇒ **Pi** can enter section **if turn == i**
- mutual exclusion은 만족하나, progress 조건은 만족 못한다
  - 반드시 교대로 turn 진행하도록 되어 있는 코드
  - 코드 별로 critical section에 접근하는 빈도가 다를 수 있다 → P0는 빈번히 들어가고 P1는 한 번만 들어가도 된다면, P0도 turn이 오지 않아 영원히 작동 못한다

### b. Algorithm 2 - flag

- **flag**(critical section에 들어가고자 하는 의중 표시)라는 동기화 변수 (synchronization) 사용
- 처음에는 flag 값이 모두 false. critical section에 들어가려면 본인의 flag를 true로 만든다 → 이후 상대방도 flag가 있는지 확인 → critical section 진입 → 나올 때 flag를 꺼서 다른 프로그램도 들어가도록 한다.
- 프로그램 A가 flagA를 들자마자 timer로 시간이 뺏겼는데, 상대방도 flagB를 들자마자 뺏기는 경우 → 둘 다 while로 상대를 체크할 때, 아무도 서로 못 들어간다 (mutual exclusion은 만족하나, progress 조건은 만족 못한다)

### c. Algorithm 3. **Peterson’s Algorithm**

- 알고리즘 1, 2의 flag와 turn 동기화 변수 모두 사용

- 상대방이 깃발을 들고 있거나 turn이 상대방이면, 나는 일단 기다리기. 들어갈 때는 flag를 올리고 나갈 때는 flag를 내림으로써, 작업이 끝나면 다른 프로세스가 들어올 수 있도록 한다.

- (특징) mutual exclusion, progress, bounded waiting 모두 충족

- (단점) busy waiting = spin lock

   (계속 CPU와 memory를 쓰면서 wait)

  - lock을 걸어 다른 프로세스가 못 들어가게 막는데, 계속 while문을 돌면서 상대가 못 들어가게 lock
  - 어떤 프로세스가 이미 critical section에 들어가 있는 상태에서 다른 프로세스가 접근하면(CPU를 얻으면), 계속 본인 할당 시간이 오면 critical section 진입을 위해 while문을 돌 것 → 하지만, 다른 프로세스가 선점 중이라 못 들어가고, 본인의 CPU 할당 시간을 while문에서만 쓰고 반납

## B. 해결 방법 2. Hardware적 해결 방법 - Test and Set

- busy waiting은 **하드웨어**적으로 고유한 인스트럭션 지원(**Test and set**)을 해 간단히 해결 가능

- 데이터를 읽으면서 쓸 수 없었기 때문에 그동안 문제 발생

- a 라는 값을 읽고 a 값을 업데이트 하는 두 가지 작업을 **atomic** (하나의 instruction처럼) 하게 동시에 하나의 작업처럼 한다

  

 ## C. 해결 방법 3. Semaphore

- 추상 자료형 (ADT) : Object + Operation
  → Semaphore는 자원을 획득하는 P연산과 반납하는 V연산으로 구성된 추상 자료형; 앞의 방식들을 추상화
- 프로그래머가 간단하게 lock하고 unlock하도록 도와준다.
  어떤 공유 자원을 획득하는 반납하는 것을 처리.
- 변수 S
  - integer variable : 자원의 개수
  - 두 가지 atomic 연산 존재
- 연산
  - **P (S)**: 공유 데이터를 획득하는 과정 (lock)
    - while (S ≤ 0) do no operation;
      S --;
    - S 값이 0 이하인 동안은 아무 것도 안하고 기다린다 (자원 없다)
      자원이 있다면 S 값을 1 빼고 자원 획득
    - **busy-wait (=spin lock)** 문제 존재: S값이 0 이하인 동안은 while만 돌면서 CPU 낭비만 하다가 반납 ⇒ busy-wait 말고 block & wakeup 방식의 구현도 존재
  - **V (S)**: 공유 데이터를 반납하는 과정 (unlock)
    - S ++;
    - 자원 반납해서 + 1
- mutex = mutual excluscion

- **Block & Wakeup (= sleep lock)** : lock을 못 얻으면 CPU를 쓰는 것이 아니라 blocked 상태 (sleep 상태)로 만든다 → CPU를 얻을 수 있는 권한 자체를 없앤다. 공유 데이터를 얻을 수 있을 때 다시 깨운다.

  - **block**
    - 커널은 block을 호출한 프로세스를 suspended 시킨다
    - 이 프로세스의 PCB를 semaphore에 대한 wait queue에 넣는다
  - **wakeup**(P)
    - block 된 프로세스 P를 wakeup 시킨다
    - 이 프로세스의 PCB를 ready queue로 옮긴다

  - 변수가 busy & wait에서는 자원의 개수를 나타냈다면, block & wakeup에서는 음수라면 자원을 기다리는 프로세스가 있고, 양수라면 자원에 여분이 있는 상황을 의미한다

  - P 연산: 자원을 획득
    - 자원에 여분이 있다면 획득, 자원에 여분이 없다면 block()
    - 일단 semaphore 변수 값을 1을 뺀다. 값이 음수라면 자원에 여유가 없기 때문에 S의 List (S.L)에 이 프로그램 연결 시킨 후 blocked 상태로 만든다

  - V 연산: 자원을 반납
    - 자원을 기다리며 잠들어 있는 프로세스가 있다면 깨우는 연산
    - 자원을 쓰고 있던 프로세스가 다 쓰면, 변수 1 증가
    - 자원을 내놓았는데도 불구하고 변수가 0 이하면, 잠들어 있는 프로세스가 있다는 소리 → wakeup

- Busy-wait vs. Block/Wakeup
  - 보통은 block/Wakeup이 효율적 (의미 있게 CPU 사용)
  - BUT! 프로세스를 ready <> wakeup 왔다갔다 하는데 overhead가 든다
    - critical sectiond의 길이가 매우 짧은 경우에는 busy-wait해도 overhead가 그리 들지 않는다. → busy-wait가 block/wakeup보다 나을 수도 있다
    - critical section의 길이가 긴 경우, while문을 도는 busy-wait는 CPU 낭비되는 시간이 많아진다 → block/wakeup이 적당
- Two Types of Semaphore
  - **Counting semaphore**
    - 도메인이 0 이상인 임의의 정수 값
    - 주로 resource counting에 사용
  - **Binary semaphore (=mutex)**
    - 0 또는 1 값만 가질 수 있는 semaphore
    - 주로 mutual exclusion (lock/unlock에 사용)
- (주의) Deadlock & Starvation
  - **Deadlock** : 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상
  - **Starvation**: indefinite blocking. 프로세스가 suspend된 이유에 해당하는 세마포어 큐에서 빠져나갈 수 없는 현상 (deadlock도 일종의 starvation이라 볼 수 있다. 여기서는 특정 프로세스 사이에서만 자원을 공유하면서 다른 프로세스는 영원히 자원을 갖지 못할 때 )

## D. 해결 방법 4. Monitor

: 동시 수행 중인 프로세스 사이에서 **ADT(Abstract Data Type)**의 안전한 공유를 보장하기 위한 high-level synchronization construct

- 프로그래밍 언어 차원에서 synchronization 문제를 monitor가 자동적으로 해결

- high-level 언어

- 공유 데이터를 접근하기 위해서는 monitor라고 정의된 내부의 procedure를 통해서만 접근 가능하게 한다

  - monitor 안에 공유데이터와 코드 작성
  - monitor가 원천적으로 내부에 있는 프로시저 여러 개가 동시에 작동하지 않도록 통제하는 권한을 주기 때문에(한 번에 하나), semaphore처럼 프로그래머가 critical section 전후로 **lock을 걸고 풀어주는 작업을 할 필요가 없다.** monitor가 알아서 접근 제어
  - 프로세스 A가 monitor 안에서 코드를 실행을 막 시작하다가 CPU를 뺏기더라도, monitor 내부에서 active하기 때문에 다른 프로세스는 monitor 내부에 접근하지 못하고 대기한다.
  - 나머지 프로세스는 프로시저를 통과하기 위해 줄을 선다 (queue)

- 모니터 내에서는 한 번에 하나의 프로세스만 활동 가능

- 프로그래머가 동기화 제약 조건(lock/unlock)을 명시적으로 코딩할 필요 없다

- 프로세스가 모니터 안에서 기다릴 수 있도록 하기 위해, **condition variable** 사용

  - **condition x, y;**

  - Semaphore에서 자원이 있으면 접근 가능하게 해주고, 없으면 접근을 막는 역할을 하는 것과 유사

  - 어떤 조건을 충족하지 못했을 때(so, 오래 기다려야 할 때), 프로세스를 잠들게 하고 줄 세우기 위한 변수

  - 자원이 여분이 있으면 바로 접근하게 해주고, 없으면 줄에 서서 기다리게 하는 wait(), 접근 후 빠져나갈 때는 signal()을 날려 기다리고 있는 다른 프로세스에게 들어오라고 알린다

  - condition variable은 wait와 signal 연산에 의해서만 접근 가능

  - **x.wait()**
    : x.wait()을 invoke한 프로세스는 다른 프로세스가 x.signal()을 invoke하기 전까지 suspend 된다

  - **x.signal()**
    : x.signal()은 정확하게 하나의 suspend된 프로세스를 resume 한다. Suspended process가 없으면 아무 일도 일어나지 않는다.

    

## E. [Classical problems of Synchronization]

1. Bounded-Buffer Problem (Producer-Consumer Problem)
2. Readers and Writers Problem
3. Dining-Philosophers Problem

### a. Bounded-Buffer Problem (Producer-Consumer Problem)

- Buffer

  : 임시로 데이터를 저장하는 공간

  - Buffer의 크기가 유한한 환경에서 producer-consumer problem
  - producer : 생산자 프로세스
    - 공유 Buffer에 데이터를 만들어서 넣는다
  - consumer : 소비자 프로세스
    - 공유 Buffer에서 데이터를 빼다 쓴다
  - producer와 cosumer가 여러 개 있는 상황

- 발생 가능한 문제

  - (문제 1) 여러 생산자가 같은 빈 공간에 데이터를 동시에 넣는 경우

    - 데이터 집어 넣을 때 다른 생산자나 소비자가 접근하지 못하도록 lock
    - 넣은 이후에는 pointer를 다음에 비어있는 위치로 옮겨둔다

    - (문제 2) 여러 소비자가 동시에 같은 데이터를 꺼내 가려고 시도하는 경우
      - 역시, 데이터를 꺼내 가려고 Buffer에 접근할 때, 다른 생산자나 소비자가 접근하지 못하도록 lock


  - (문제 3) bounded buffer (메모리에 한계가 있는 버퍼 문제)
    - buffer에 데이터가 다 찼는데, 계속해서 생산자만 오는 경우 (Full)
      - 생산자 입장에서 사용할 수 있는 자원(비어있는 buffer)이 없는 상태
      - buffer가 빌 때까지 생산자는 대기
      - buffer에 데이터가 없는데, 소비자만 오는 경우
        - 생산자가 올 때까지 소비자는 대기

- Producer & Consumer

  - Producer
    - empty buffer가 있는지 확인. 없으면 대기
    - 빈 buffer가 있다면, 공유 데이터에 lock을 걸고 buffer에 접근
    - empty buffer에 데이터 입력 및 buffer 조작
    - lock을 풀고 full buffer += 1

  - Consumer
    - full buffer 가 있는지 확인. 없으면 대기
      - full buffer가 있다면, 공유 데이터에 lock을 걸고 buffer에 접근
      - full buffer에서 데이터 꺼내고 buffer 조작
      - lock을 풀고 empty buffer 하나 증가


- Buffer & Semaphore

  - Shared data
    - buffer 자체
    - buffer 조작 변수: empty/full buffer의 시작 위치 (pointer)

  - Synchronization variables
    - **mutual exclusion (상호 배제) : need binary semaphore**
      - **resource count (자원의 수) : need integer semaphore**


### b. Readers-Writers Problem

- 한 process가 DB에 write 중일 때, 다른 process가 접근하면 안 된다
  - 읽는 프로세스(Reader)와 쓰는 프로세스(Writer)가 존재
  - 주로 DB에서 발생하는 Synchronization 문제
- read는 동시에 여러 process가 할 수 있다 write는 항상 다른 프로세스가 없을 때 배타적으로만 가능
- (solution)
  - (read, write 모두 상호 배제로 하면 구현은 쉽지만, 비효율적)
    - *read도 lock을 하긴 하지만, 다른 read는 허용하고 write는 허용 x
  - Writer가 DB에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기 중인 Reader들을 다 DB에 접근을 허용한다
    - 이때 readcount ++나 -- 을 하기 전에, 동시에 여러 reader가 접근해서, 숫자에 오류가 발생할 수 있으므로, reader 하나 당 read count 제대로 계산되기 위해, reader 간 일시적인 lock인 mutex lock을 건다
    - readcount ++, --를 하고 난 후에는 mutex unlock
  - Writer는 대기 중인 Reader가 하나도 없을 때 DB 접근이 허용된다
    - writer의 대기는 상당히 오래 걸릴 수 있다 → **starvation 발생 가능!**
    - (ex) priority queue를 두어 대기 시간이 길어질 수록 writer의 우선 순위를 높여 주는 등의 방식으로 starvation 방지 가능
  - 일단 Writer가 DB에 접근 중이면 Reader의 접근이 금지된다
  - Writer가 DB에서 빠져나가야만 Reader의 접근이 허용된다
- **Shared Data**
  - DB 자체
  - readcount; (현재 DB에 접근 중인 Reader의 수)
- **Synchronization variables**
  - mutex : 공유 변서 readcount를 접근하는 코드(critical section)의 mutual exclusion 보장 위해 사용 → readcount에 lock/unlock 용도
  - db : reader와 writer가 공유 DB 자체를 올바르게 접근하게 하는 역할 → DB 전체에 lock/unlock 용도

### c. Dining-Philosophers Problem

- 철학자의 업무
  - 생각하기
  - 배고프면 음식 먹기 → 왼쪽 오른쪽 젓가락을 하나씩 집어 젓가락 쌍을 만들어야 밥 먹을 수 있다 (젓가락 = 공유 자원)
  - 음식을 먹는 주기는 철학자마다 다르다
- Synchornization variables
  - semaphore chopstick[5]; // 젓가락은 모두 1로 초기화 되어 있다
  - 즉, 자원이 1이기 때문에 젓가락 하나 당 철학자 한 명만 가능
- (위 solution의 문제점)
  - Deadlock의 가능성
  - 모든 철학자가 동시에 배가 고파져서, 모두 왼쪽 젓가락을 하나씩 먼저 집은 경우
- (해결 방안)
  - 5자리 테이블이지만, 4명의 철학자만이 테이블에 동시에 앉힌다 (or 밥을 먹을 때만 테이블에 앉게 한다)
  - 젓가락을 두 개 모두 집을 수 있을 때만, 젓가락을 집게 한다
  - 비대칭: 짝수(홀수) 철학자는 왼쪽(오른쪽) 젓가락부터 잡도록 한다

- Semaphore의 문제점
  - 코딩하기 힘들다 (비교적 쉽게 만들어 주긴 했지만, 문제가 생겼을 때 버그 잡기가 쉽지 않기 때문에 + 순서가 상당히 중요하기 때문에, 프로그래머가 실수하면 잡기가 힘들다)
  - 정확성(correctness)의 입증이 어렵다
  - 자발적 협력(voluntary cooperation)이 필요하다
  - 한 번의 실수가 모든 시스템에 치명적 영향

## F. Semaphore vs. Monitor

Monitor

- 동시 접근 자체를 monitor에서 제어
- lock/unlock을 프로그래머가 직접 걸어줄 필요 x
- 변수가 무슨 특정한 값을 나타내기 보다는, 잠들어 있는 프로세스가 있으면 깨우고, 잠들어 있는 프로세스가 없으면 아무 역할 X (조건을 충족하는지 여부만 체크)

Semaphore

- 프로그래머가 동시 접근을 통제하기 위해 직접 P연산 V연산 필요.
- lock/unlock을 직접 프로그래머가 건다 (기본적으로 공유 데이터 접근할 때 lock이 필요)
- 값을 가지는 변수 (resource의 수)

